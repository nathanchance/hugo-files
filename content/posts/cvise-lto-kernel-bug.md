---
title: "Reducing an LTO Linux kernel bug with cvise"
date: 2021-11-29T10:07:16-07:00
toc: false
images:
tags:
  - clang
  - clangbuiltlinux
  - linux
  - llvm
  - lto
---

My co-maintainer Nick Desaulniers wrote [a great post](https://nickdesaulniers.github.io/blog/2019/01/18/finding-compiler-bugs-with-c-reduce/) about taking a several thousand line C file that exposed a compiler bug down to 12 lines with `creduce`. I thought I would do the same thing with a bug that only happens with link time optimization (LTO) in the Linux kernel, which is a bit of a different beast. Hopefully this post can help others reduce their own bugs and think about the best way to triage a bug.

## Make sure the bug is reproducible

Any time that I receive a bug report on [our GitHub issue tracker](https://github.com/ClangBuiltLinux/linux/issues), I want to make sure it is reproducible for me so that I can investigate it in my environment with my tools. The report in question is [issue #1215](https://github.com/ClangBuiltLinux/linux/issues/1215) and [issue #1516](https://github.com/ClangBuiltLinux/linux/issues/1516). The second issue has a set of configurations that trigger the issue with either `ARCH=x86_64` or `ARCH=arm64`; as I am working on an x86_64 server, it is probably going to be easier to work with the native architecture.

I like to use [`tuxmake`](https://gitlab.com/Linaro/tuxmake) for initially reproducing bugs with Debian `clang` versions, as they are prepacked in Docker images. I won't get into the specifics of how to use `tuxmake` in this post, its documentation is pretty good as it is and I am only going to use it for initially reproducing the issue. The reporter of issue #1516 mentions they are using the latest stable branch (`linux-5.15.y`) but I want to reproduce on mainline, as that is going to change how I approach this bug. If the bug is fixed on mainline and broken on stable, it means there was a fix somewhere that needs to be found and backported, which is much simpler than fixing an issue that is still present in the development tree.

```
$ git show -s
commit d58071a8a76d779eedab38033ae4c821c30295a5 (HEAD, tag: v5.16-rc3, origin/master)
Author: Linus Torvalds <torvalds@linux-foundation.org>
Date:   Sun Nov 28 14:09:19 2021 -0800

    Linux 5.16-rc3

$ echo "CONFIG_FTRACE_MCOUNT_USE_RECORDMCOUNT=n
CONFIG_KASAN=n
CONFIG_GCOV_KERNEL=n
CONFIG_COMPILE_TEST=n
CONFIG_LTO_CLANG_FULL=y
CONFIG_TRIM_UNUSED_KSYMS=y
CONFIG_CFI_CLANG=y
CONFIG_CFI_CLANG_SHADOW=y
CONFIG_SHADOW_CALL_STACK=y
CONFIG_INIT_STACK_ALL_ZERO=y" >kernel/configs/repro.config

$ tuxmake \
    -a x86_64 \
    -k allmodconfig \
    -K repro.config \
    -r podman \
    -t clang-13 \
    LLVM=1 LLVM_IAS=1 default
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM13.0.1' Reader: 'LLVM 13.0.1')
make[3]: *** [/home/nathan/cbl/worktrees/cbl-1215/scripts/Makefile.build:307: fs/overlayfs/overlay.lto.o] Error 1
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM13.0.1' Reader: 'LLVM 13.0.1')
make[3]: *** [/home/nathan/cbl/worktrees/cbl-1215/scripts/Makefile.build:307: fs/xfs/xfs.lto.o] Error 1
...
```

Okay, so the bug is reproducible locally. Now, I know that some of these configuration options are not needed to reproduce the issue. `CONFIG_CFI_CLANG` and `CONFIG_CFI_CLANG_SHADOW` do not currently work for x86_64. `CONFIG_SHADOW_CALL_STACK` is an arm64 only feature. `CONFIG_FTRACE_MCOUNT_USE_RECORDMCOUNT` is not a user selectable Kconfig value (it is just `bool`, rather than `bool "..."`). `CONFIG_INIT_STACK_ALL_ZERO` and `CONFIG_TRIM_UNUSED_KSYMS` are a little suspect but let's see if we can reproduce this bug without them to keep our scope more limited (fewer variables can make the bug easier to fix down the road).

`CONFIG_LTO_CLANG_FULL` appears critical to the problem and `CONFIG_KASAN`, `CONFIG_GCOV_KERNEL`, and `CONFIG_COMPILE_TEST` all need to be disabled for it to be selected. So:

```
$ echo "CONFIG_COMPILE_TEST=n
CONFIG_GCOV_KERNEL=n
CONFIG_KASAN=n
CONFIG_LTO_CLANG_FULL=y" >kernel/configs/repro.config

$ tuxmake \
    -a x86_64 \
    -k allmodconfig \
    -K repro.config \
    -r podman \
    -t clang-13 \
    LLVM=1 LLVM_IAS=1 default
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM13.0.1' Reader: 'LLVM 13.0.1')
make[3]: *** [/home/nathan/cbl/worktrees/cbl-1215/scripts/Makefile.build:307: fs/overlayfs/overlay.lto.o] Error 1
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM13.0.1' Reader: 'LLVM 13.0.1')
make[3]: *** [/home/nathan/cbl/worktrees/cbl-1215/scripts/Makefile.build:307: fs/xfs/xfs.lto.o] Error 1
...
I: config: PASS in 0:00:07.230375
I: default: FAIL in 0:09:36.347980
I: build output in /home/nathan/.cache/tuxmake/builds/273
```

Alright, we have a decently minimal configuration (`allmodconfig` with full LTO) to work off of. Take note of the `.../.cache/tuxmake/builds/273`, that will come in handy later.

## Reproduce the bug with the latest compiler

We reproduced the bug with the latest Linux kernel development release but we used a released version of `clang` (13). For the same reason as above, we want to make sure the bug is not already fixed with the development branch, as duplicate reports waste developers' time. [apt.llvm.org](https://apt.llvm.org/) is a great resource for quickly getting access to a close to tip of tree `clang` to test with, if you are on Debian/Ubuntu or willing to work out of a Docker image. For the purpose of this tutorial, we are just going to build it from source. I developed a Python LLVM build script (`build-llvm.py`) as part of [tc-build](https://github.com/ClangBuiltLinux/tc-build) to help with situations like this.

I will explain the flags below:

* `--build-folder`/`--llvm-folder`: By default, the script keeps everything self-contained; it builds the toolchain in `build/` and it downloads the source to `llvm-project/`. I have a separate tree that I use for development so I provide these flags to point those folders to that tree. These will not be necessary under normal circumstances.
* `--assertions`: Turns on assertions in the LLVM source, which can help figure out the cause of the bug sooner, but it will make the compiler slower; it might not be worth the trade off depending on what you are reducing.
* `--build-stage1-only`: Shortens the build time, as the script normally builds a compiler and uses that compiler to build the final compiler (a two stage build). When using the toolchain for more than a triage, this should be omitted, as a two stage build can produce a more optimized compiler.
* `--check-targets`: Runs the check targets requested (`clang` will become `check-clang`) to make sure there are no current obvious issues with the development tree.
* `--projects`: Limits what we build to `clang` and `ld.lld`, as we do not need any other projects to reproduce this.
* `--targets`: Limits the available backends to just `X86`, which is the only one we need to reproduce the issue, to help speed up the build.

```
$ $CBL_GIT/tc-build/build-llvm.py \
    --assertions \
    --build-folder $CBL_SRC/llvm-project/build \
    --build-stage1-only \
    --check-targets clang ll{d,vm{,-unit}} \
    --llvm-folder $CBL_SRC/llvm-project \
    --projects "clang;lld" \
    --targets X86
...

LLVM build duration: 0:06:02

LLVM toolchain installed to: /home/nathan/cbl/src/llvm-project/build/stage1

To use, either run:

    $ export PATH=/home/nathan/cbl/src/llvm-project/build/stage1/bin:${PATH}

or add:

    PATH=/home/nathan/cbl/src/llvm-project/build/stage1/bin:${PATH}

to the command you want to use this toolchain.

Version information:

ClangBuiltLinux clang version 14.0.0 (https://github.com/llvm/llvm-project 8d474f1d157530577f06ce3ef9187e1aaf31a59e)
Target: x86_64-unknown-linux-gnu
Thread model: posix
InstalledDir: /home/nathan/cbl/src/llvm-project/build/stage1/bin

LLD 14.0.0 (compatible with GNU linkers)
```

After a little bit of time (or maybe a lot of time, depending on how fast your machine is), the script will spit out some information like above, which explains how we can use it.

One nice thing about `tuxmake` is that it generates a `config` file that we can use to reproduce issues later (that is why I mentioned taking note of the folder that `tuxmake` generates at the end). Thus, with the new toolchain and the old config, we can see if the issue is still reproducible:

```
$ cp $HOME/.cache/tuxmake/builds/273/config .config

$ PATH=$CBL_SRC/llvm-project/build/stage1/bin:$PATH \
    make -skj"$(nproc)" LLVM=1 LLVM_IAS=1 olddefconfig all
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM14.0.0git' Reader: 'LLVM 14.0.0git')
make[3]: *** [scripts/Makefile.build:307: fs/overlayfs/overlay.lto.o] Error 1
...
drivers/gpu/drm/i915/intel_pm.c:3065:12: error: use of bitwise '|' with boolean operands [-Werror,-Wbitwise-instead-of-logical]
        changed = ilk_increase_wm_latency(dev_priv, dev_priv->wm.pri_latency, 12) |
                  ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
drivers/gpu/drm/i915/intel_pm.c:3065:12: note: cast one or both operands to int to silence this warning
drivers/gpu/drm/i915/intel_pm.c:3065:12: error: use of bitwise '|' with boolean operands [-Werror,-Wbitwise-instead-of-logical]
        changed = ilk_increase_wm_latency(dev_priv, dev_priv->wm.pri_latency, 12) |
                  ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
                                                                                  ||
drivers/gpu/drm/i915/intel_pm.c:3065:12: note: cast one or both operands to int to silence this warning
2 errors generated.
...
drivers/power/reset/ltc2952-poweroff.c:162:28: error: expression requires  'long double' type support, but target 'x86_64-unknown-linux-gnu' does not support it
        data->wde_interval = 300L * 1E6L;
                                  ^
drivers/power/reset/ltc2952-poweroff.c:162:21: error: expression requires  'long double' type support, but target 'x86_64-unknown-linux-gnu' does not support it
        data->wde_interval = 300L * 1E6L;
                           ^
drivers/power/reset/ltc2952-poweroff.c:163:41: error: expression requires  'long double' type support, but target 'x86_64-unknown-linux-gnu' does not support it
        data->trigger_delay = ktime_set(2, 500L*1E6L);
                                               ^
3 errors generated.
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM14.0.0git' Reader: 'LLVM 14.0.0git')
make[3]: *** [scripts/Makefile.build:307: fs/xfs/xfs.lto.o] Error 1
...
```

So the issue reproduces but we picked up a couple new issues by the nature of going to a development build of LLVM. These are not going to impact our triage but it is still good to fix them. Thankfully, patches to fix these issues exist on the mailing list:

```
$ curl -LSs https://lore.kernel.org/lkml/20211014211916.3550122-1-nathan@kernel.org/raw | patch -Np1
patching file drivers/gpu/drm/i915/intel_pm.c
Hunk #1 succeeded at 3062 (offset 12 lines).

$ curl -LSs https://lore.kernel.org/lkml/20211105152049.2522250-1-nathan@kernel.org/raw | patch -Np1
patching file drivers/power/reset/ltc2952-poweroff.c
```

Running the above command again should just show the two errors now.

## Reproduce the issue outside of the build system

The next step is getting the files that we need to reproduce the issue separated from the build system. We can choose to look at `fs/overlayfs/overlay.lto.o` or `fs/xfs/xfs.lto.o`; I am going to choose the former, as `fs/overlayfs/` is a smaller directory than `fs/xfs/`.

The Linux kernel's build system has a variable `V=1` to see every build step, which we can dump to a text file for easy inspection and searching. Note: the `-s` flag needs to be dropped from `make` to get the full output.

```
$ PATH=$CBL_SRC/llvm-project/build/stage1/bin:$PATH \
    make -kj"$(nproc)" LLVM=1 LLVM_IAS=1 olddefconfig clean all V=1 &>build.log
```

First, we need to see how `fs/overlayfs/overlay.lto.o` is generated.

```
$ rg fs/overlayfs/overlay.lto.o build.log
6011:  ld.lld -m elf_x86_64 -mllvm -import-instr-limit=5   -r -o fs/overlayfs/overlay.lto.o  --whole-archive fs/overlayfs/overlay.o  ; ./tools/objtool/objtool orc generate  --module  --no-fp  --no-unreachable  --retpoline  --uaccess  --mcount fs/overlayfs/overlay.lto.o
6133:make[3]: *** [scripts/Makefile.build:307: fs/overlayfs/overlay.lto.o] Error 1
```

It comes from `fs/overlayfs/overlay.o` so we need to see how that is generated.

```
$ rg fs/overlayfs/overlay.o build.log
29599:  rm -f fs/overlayfs/overlay.o.symversions                  ; rm -f fs/overlayfs/overlay.o; llvm-ar cDPrsT fs/overlayfs/overlay.o fs/overlayfs/super.o fs/overlayfs/namei.o fs/overlayfs/util.o fs/overlayfs/inode.o fs/overlayfs/file.o fs/overlayfs/dir.o fs/overlayfs/readdir.o fs/overlayfs/copy_up.o fs/overlayfs/export.o
29634:  ld.lld -m elf_x86_64 -mllvm -import-instr-limit=5   -r -o fs/overlayfs/overlay.lto.o  --whole-archive fs/overlayfs/overlay.o  ; ./tools/objtool/objtool orc generate  --module  --no-fp  --no-unreachable  --retpoline  --uaccess  --mcount fs/overlayfs/overlay.lto.o
```

It comes from running `llvm-ar` on a bunch of `.o` files in `fs/overlayfs/`. Now we need to figure out which `.o` has the issue. This can be done by running the `ld.lld` command with a single `.o` files from the `llvm-ar` command (`llvm-ar` is only necessary for combining multiple `.o` files into one). For example, with `fs/overlayfs/super.o`:

```
$ rm fs/overlayfs/overlay.o

$ $CBL_SRC/llvm-project/build/stage1/bin/ld.lld -m elf_x86_64 -mllvm -import-instr-limit=5 -r -o fs/overlayfs/overlay.lto.o --whole-archive fs/overlayfs/super.o

$ echo $?
0
```

So the issue is not with `fs/overlayfs/super.c`. Trying all the other files, we end up seeing the error in `fs/overlayfs/file.c`:

```
$ rm fs/overlayfs/overlay.o

$ $CBL_SRC/llvm-project/build/stage1/bin/ld.lld -m elf_x86_64 -mllvm -import-instr-limit=5 -r -o fs/overlayfs/overlay.lto.o --whole-archive fs/overlayfs/file.o
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM14.0.0git' Reader: 'LLVM 14.0.0git')
```

Okay, we have a single C file that can reproduce the issue, which we can reduce from. The Linux kernel allows us to generate a preprocessed file (`.i`) from source files (`.c`) directly, which is important for reduction purposes as it will be one file that can be built anywhere, rather than being tied to a build system and headers. The other thing we need is the build command for the object file (`.o`) so we know what flags to use for reproducing the issue. The kernel's build system generates `.cmd` to show the command used to build the object file but these only show up when the object file can be successfully built. If you are reducing a build error on the object file, you can use the `V=1` trick above to see the command. So, putting that all together:

```
# Generate the '.i' and '.o' files
$ PATH=$CBL_SRC/llvm-project/build/stage1/bin:$PATH \
    make -kj"$(nproc)" LLVM=1 LLVM_IAS=1 olddefconfig clean fs/overlayfs/file.{i,o}

# Get the compiler command from the .cmd file
$ head -1 fs/overlayfs/.file.o.cmd
cmd_fs/overlayfs/file.o := clang -Wp,-MMD,fs/overlayfs/.file.o.d -nostdinc -I./arch/x86/include -I./arch/x86/include/generated  -I./include -I./arch/x86/include/uapi -I./arch/x86/include/generated/uapi -I./include/uapi -I./include/generated/uapi -include ./include/linux/compiler-version.h -include ./include/linux/kconfig.h -include ./include/linux/compiler_types.h -D__KERNEL__ -Qunused-arguments -fmacro-prefix-map=./= -Wall -Wundef -Werror=strict-prototypes -Wno-trigraphs -fno-strict-aliasing -fno-common -fshort-wchar -fno-PIE -Werror=implicit-function-declaration -Werror=implicit-int -Werror=return-type -Wno-format-security -std=gnu89 --target=x86_64-linux-gnu -fintegrated-as -Werror=unknown-warning-option -Werror=ignored-optimization-argument -mno-sse -mno-mmx -mno-sse2 -mno-3dnow -mno-avx -fcf-protection=none -m64 -falign-loops=1 -mno-80387 -mno-fp-ret-in-387 -mstack-alignment=8 -mskip-rax-setup -mtune=generic -mno-red-zone -mcmodel=kernel -DCONFIG_X86_X32_ABI -Wno-sign-compare -fno-asynchronous-unwind-tables -mretpoline-external-thunk -fno-delete-null-pointer-checks -Wno-frame-address -Wno-address-of-packed-member -O2 -Wframe-larger-than=2048 -fstack-protector-strong -Werror "-Wimplicit-fallthrough" -Wno-gnu -mno-global-merge -Wno-unused-but-set-variable -Wno-unused-const-variable -ftrivial-auto-var-init=pattern -fno-stack-clash-protection -pg -mfentry -DCC_USING_NOP_MCOUNT -DCC_USING_FENTRY -fno-lto -flto -fvisibility=hidden -falign-functions=64 -Wdeclaration-after-statement -Wvla -Wno-pointer-sign -Wno-array-bounds -fno-strict-overflow -fno-stack-check -Werror=date-time -Werror=incompatible-pointer-types -Wno-initializer-overrides -Wno-format -Wno-sign-compare -Wno-format-zero-length -Wno-pointer-to-enum-cast -Wno-tautological-constant-out-of-range-compare  -fsanitize=array-bounds -fsanitize=shift -fsanitize=integer-divide-by-zero -fsanitize=object-size -fsanitize=bool -fsanitize=enum  -fsanitize-coverage=trace-pc -fsanitize-coverage=trace-cmp  -DMODULE  -DKBUILD_BASENAME='"file"' -DKBUILD_MODNAME='"overlay"' -D__KBUILD_MODNAME=kmod_overlay -c -o fs/overlayfs/file.o fs/overlayfs/file.c
```

We can take that `clang` command and use it to generate `fs/overlayfs/file.o` from `fs/overlayfs/file.i` to make sure the issue is completely reproducible outside of the kernel's build system. To do so, I like to copy the files into a separate folder and work within there.

```
$ tmp_dir=$(mktemp -d)

$ cp fs/overlayfs/file.i "$tmp_dir"

$ cd "$tmp_dir"
```

Now, the clang command has several flags in it that we do not need:

* Any of the flags prior to `-Wall` and define flags (`-D...`) since those are all preprocessor flags (the file has already been preprocessed at this stage).
* All of the warning flags (`-W...`) are pointless, as they cannot cause code generation to change.
* The `--target` flag is redundant, as we are on an x86_64 Linux platform, along with `-fintegrated-as`, as that is the default for x86_64 on Linux (it is only present in the command to make figuring out the assembler easier for the Kconfig stage).
* Lastly, we need to adjust the paths of the source files and output to be relative to our new temporary directory.

So, we end up with:

```
clang -fno-strict-aliasing -fno-common -fshort-wchar -fno-PIE -std=gnu89 --target=x86_64-linux-gnu -fintegrated-as -mno-sse -mno-mmx -mno-sse2 -mno-3dnow -mno-avx -fcf-protection=none -m64 -falign-loops=1 -mno-80387 -mno-fp-ret-in-387 -mstack-alignment=8 -mskip-rax-setup -mtune=generic -mno-red-zone -mcmodel=kernel -Wno-sign-compare -fno-asynchronous-unwind-tables -mretpoline-external-thunk -fno-delete-null-pointer-checks -O2 -fstack-protector-strong -mno-global-merge -ftrivial-auto-var-init=pattern -fno-stack-clash-protection -pg -mfentry -flto -fvisibility=hidden -falign-functions=64 -fno-strict-overflow -fno-stack-check -fsanitize=array-bounds -fsanitize=shift -fsanitize=integer-divide-by-zero -fsanitize=object-size -fsanitize=bool -fsanitize=enum -fsanitize-coverage=trace-pc -fsanitize-coverage=trace-cmp -c -o file.o file.i
```

Now, we can put the commands that we need to reproduce the issue into a simple shell script. For the `ld.lld`, we do not care about the location of the output (`-o ...`) because we are not analyzing it, so I just dump it to `/dev/null`. I also know the `-mllvm -import-instr-limit=5` is not critical for reproducing this bug so we can drop it here.

```
$ cat test.sh
#!/usr/bin/env bash

llvm_bin=$CBL_SRC/llvm-project/build/stage1/bin

$llvm_bin/clang -fno-strict-aliasing -fno-common -fshort-wchar -fno-PIE -std=gnu89 --target=x86_64-linux-gnu -fintegrated-as -mno-sse -mno-mmx -mno-sse2 -mno-3dnow -mno-avx -fcf-protection=none -m64 -falign-loops=1 -mno-80387 -mno-fp-ret-in-387 -mstack-alignment=8 -mskip-rax-setup -mtune=generic -mno-red-zone -mcmodel=kernel -Wno-sign-compare -fno-asynchronous-unwind-tables -mretpoline-external-thunk -fno-delete-null-pointer-checks -O2 -fstack-protector-strong -mno-global-merge -ftrivial-auto-var-init=pattern -fno-stack-clash-protection -pg -mfentry -flto -fvisibility=hidden -falign-functions=64 -fno-strict-overflow -fno-stack-check -fsanitize=array-bounds -fsanitize=shift -fsanitize=integer-divide-by-zero -fsanitize=object-size -fsanitize=bool -fsanitize=enum -fsanitize-coverage=trace-pc -fsanitize-coverage=trace-cmp -c -o file.o file.i

$llvm_bin/ld.lld -m elf_x86_64 -r -o /dev/null --whole-archive file.o

$ chmod +x test.sh

$ ./test.sh
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM14.0.0git' Reader: 'LLVM 14.0.0git')
```

Before we move on, we should trim down the compiler flags, as some of them may not be necessary to reproduce the issue. Dropping flags after this point is definitely possible but doing it now can help `creduce` or `cvise` really tease out the source code that triggers the problem, as your interesting test can be written in a clearer manner (more on that later). There is not really any art to this; I will typically remove four to five flags at a time to see if I can reproduce the issue. If it does, the flags were not important but if it doesn't, the flag needs to stick around (the Linux kernel is no longer buildable without optimizations so `-Os`/`-O2`/`-O3` will almost always be needed). After running through that process with this issue, we end up with:

```
$ cat test.sh
#!/usr/bin/env bash

llvm_bin=$CBL_SRC/llvm-project/build/stage1/bin

$llvm_bin/clang -O2 -flto -fsanitize=object-size -c -o file.o file.i

$llvm_bin/ld.lld -m elf_x86_64 -r -o /dev/null --whole-archive file.o

$ ./test.sh
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM14.0.0git' Reader: 'LLVM 14.0.0git')
```

Sweet! We have a single preprocessed file that we can use to reproduce the issue with two simple commands. Now, we can reduce this input using either `creduce` or a more recent Python implementation called [`cvise`](https://github.com/marxin/cvise). They work the same but I prefer `cvise`, as it appears to be faster for me and it produces more readable C code (as it keeps function and variable names from the original source).

## Writing a good interestingness test and running the reduction

NOTE: This is distilled down from [the official "Using C-Reduce" documentation](https://embed.cs.utah.edu/creduce/using/). If I say something that contradicts the documentation, assume the documentation is correct (but drop me a note so I can tell).

`creduce` and `cvise` work by running a shell script against the input and checking the return code of the script to determine if the bug is still present (so the source can be reduced further) or not (so it can undo the test it just tried). 0 is considered "interesting" and non-zero is not (source is invalid, other build errors, etc).

So in this case, we might want our interestingness test to look something like:

```
#!/usr/bin/env bash

# Location of your built toolchain's bin/ folder
llvm_bin=$CBL_SRC/llvm-project/build/stage1/bin

# If the source cannot be compiled with LTO and '-fsanitize=object-size', it is not interesting (we know the source builds at the beginning)
$llvm_bin/clang -O2 -flto -fsanitize=object-size -c -o file.o file.i || exit

# If "error: Never resolved function from blockaddress" is present, the test is interesting (grep returns 0 when the string is found)
$llvm_bin/ld.lld -m elf_x86_64 -r -o /dev/null --whole-archive file.o |& grep "error: Never resolved function from blockaddress"
```

Running this, we get:

```
$ ./test.sh
...
ld.lld: error: Never resolved function from blockaddress (Producer: 'LLVM14.0.0git' Reader: 'LLVM 14.0.0git')

$ echo $?
0
```

Now, we want to make sure `creduce`/`cvise` gives us code that stresses the error under these specific conditions and not others. For this reason, I typically write two other tests within my interestingness test. The first is making sure that `creduce`/`cvise` does not introduce new warnings under either GCC and `clang`; this also makes sure the code is accepted by two compilers, which improves its quality. This directly contradicts [note #3 in the `cvise` README](https://github.com/marxin/cvise#notes) but I have seen some dumb looking code generated from these tools so keeping it somewhat warning clean makes that less likely. The one downside of this is it typically makes the reduction take longer so keep that in mind if you decide to add it.

First, let's try to build the source with `gcc`:

```
$ gcc -c -o /dev/null file.i
...
./arch/x86/include/asm/jump_label.h:27:2: error: impossible constraint in ‘asm’
```

The Linux kernel pretty much expects `-O2` or higher.

```
$ gcc -O2 -c -o /dev/null file.i
...

$ echo $?
0
```

Adding `-Werror` will turn all of the warnings that we currently see into errors so we need to add `-Wno-error=...` for the warnings that do show up. For `gcc`, I only see `-Wattributes`.

```
$ gcc -O2 -Werror -Wno-error=attributes -c -o /dev/null file.i
...

$ echo $?
0
```

Next, we want to test with `clang` in a similar manner, which we end up with:

```
$ clang -O2 -Werror -Wno-error={address-of-packed-member,unused-value} -c -o /dev/null file.i
...

$ echo $?
0
```

Lastly, I want GCC to try and catch any instances of `-Waddress-of-packed-member` or `-Wunused-value` that might crop up in various passes, which we cannot catch with `clang` because the source is not currently clean (we could try to fix that in the actual source file but this is a little bit easier).

Now, we can throw these lines into our interestingness test:

```
$ cat test.sh
#!/usr/bin/env bash

# Location of your built toolchain's bin/ folder
llvm_bin=$CBL_SRC/llvm-project/build/stage1/bin

# New warnings or invalid source are not interesting (smoke test)
gcc -O2 -Werror -Werror={address-of-packed-member,unused-value} -Wno-error=attributes -c -o /dev/null file.i || exit
$llvm_bin/clang -O2 -Werror -Wno-error={address-of-packed-member,unused-value} -c -o /dev/null file.i || exit

# If the source cannot be compiled with LTO and '-fsanitize=object-size', it is not interesting (we know the source builds at the beginning)
$llvm_bin/clang -O2 -flto -fsanitize=object-size -c -o file.o file.i || exit

# If "error: Never resolved function from blockaddress" is present, the test is interesting (grep returns 0 when the string is found)
$llvm_bin/ld.lld -m elf_x86_64 -r -o /dev/null --whole-archive file.o |& grep "error: Never resolved function from blockaddress"

$ ./test.sh
...

$ echo $?
0
```

The second test I like to add is only relevant if the issue is reproducible under a combination of flags. In these cases, I like to make sure the issue is not reproducible when using a subset of the flags, as that will sometimes help show the issue a little bit clearer (as I mentioned before when reducing down compiler flags). For example, the bug we are looking into is only reproducible with `-flto` when it is used in combination with `-fsanitize=object-size` so I want to make sure the issue does not show up when only using `-flto` or `-fsanitize=object-size`, as that would be a different bug than the one we are reducing. It would still be worth looking into but the reduced file would not be stressing the bug in the best way.

Adding these tests would look something like:

```
$ cat test.sh
#!/usr/bin/env bash

# Location of your built toolchain's bin/ folder
llvm_bin=$CBL_SRC/llvm-project/build/stage1/bin

# New warnings or invalid source are not interesting (smoke test)
gcc -O2 -Werror -Werror={address-of-packed-member,unused-value} -Wno-error=attributes -c -o /dev/null file.i || exit
$llvm_bin/clang -O2 -Werror -Wno-error={address-of-packed-member,unused-value} -c -o /dev/null file.i || exit

# If the source cannot be compiling and linked with '-fsanitize=object-size' without LTO, it is not interesting
$llvm_bin/clang -O2 -fsanitize=object-size -c -o file.o file.i || exit
$llvm_bin/ld.lld -m elf_x86_64 -r -o /dev/null --whole-archive file.o || exit

# If the source cannot be compiled and linked with LTO without '-fsanitize=object-size', it is not interesting
$llvm_bin/clang -O2 -flto -c -o file.o file.i || exit
$llvm_bin/ld.lld -m elf_x86_64 -r -o /dev/null --whole-archive file.o || exit

# If the source cannot be compiled with LTO and '-fsanitize=object-size', it is not interesting (we know the source builds at the beginning)
$llvm_bin/clang -O2 -flto -fsanitize=object-size -c -o file.o file.i || exit

# If "error: Never resolved function from blockaddress" is present, the test is interesting (grep returns 0 when the string is found)
$llvm_bin/ld.lld -m elf_x86_64 -r -o /dev/null --whole-archive file.o |& grep "error: Never resolved function from blockaddress"

$ ./test.sh
...

$ echo $?
0
```

Now that we have a fully formed interestingness test, we can run `cvise`. Depending on how many tests it runs in parallel and the speed of your machine, this might take a while.

```
$ cvise test.sh file.i
00:00:02 INFO ===< 13 >===
00:00:02 INFO running 32 interestingness tests in parallel
00:00:02 INFO INITIAL PASSES
00:00:02 INFO ===< IncludesPass >===
00:00:02 INFO ===< UnIfDefPass >===
00:00:02 INFO ===< CommentsPass >===
00:00:05 INFO (0.0%, 2217001 bytes, 31799 lines)
...
Runtime: 5975 seconds
Reduced test-cases:

--- /home/nathan/cbl/creduce-files/cbl-1215/file.i ---
struct fd {
  int file;
} kmem_cache_alloc(), ovl_write_iter_real;
struct {
  int dep_map;
} * percpu_rwsem_release_sem;
struct {
  void *ki_complete;
} * is_sync_kiocb_kiocb;
void *ovl_write_iter___trans_tmp_4;
_Bool ovl_write_iter___trans_tmp_3;
void lock_release(int *, long);
static void percpu_rwsem_release(long ip) {
  lock_release(&percpu_rwsem_release_sem->dep_map, ip);
}
int file_remove_privs();
long ovl_write_iter() {
  long ret = file_remove_privs();
  if (ret)
    goto out_unlock;
  ret = (&ovl_write_iter_real)->file;
  if (ret)
    goto out_unlock;
  ovl_write_iter___trans_tmp_3 = is_sync_kiocb_kiocb->ki_complete == 0;
  if (ovl_write_iter___trans_tmp_3) {
    kmem_cache_alloc();
    if (ovl_write_iter___trans_tmp_4)
      goto out;
    percpu_rwsem_release(({
      __here:
        (long)&&__here;
    }));
  }
out:
out_unlock:
  return ret;
}
```

So the reduction took a little over an hour and a half and now it is a lot easier for an LLVM developer to take that small C program, transform it into LLVM IR, and figure out what is going wrong here. The difference is crazy (0.08% the original size!):

```
$ wc -l file.i{,.orig}
     37 file.i
  44372 file.i.orig
  44409 total
```

## Conclusion

Providing developers a small, concise reproducer is important so that all their time can spent debugging and fixing the issue; otherwise, they are less likely to respond to the report, as it will take more work on their part than another issue where the reproducer is ready and available. Hopefully this post was informative! If there are any issues, comments, or improvements, feel free to reach out to me via [email](mailto:nathan@kernel.org) or [Twitter](https://twitter.com/nathanchance).
