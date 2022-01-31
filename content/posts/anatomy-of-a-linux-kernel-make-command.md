---
title: "The anatomy of a Linux kernel `make` command"
date: 2022-01-31T15:19:23-07:00
draft: true
---

## Introduction

A few years ago, I wrote [a Linux kernel build guide aimed at Android](https://forum.xda-developers.com/t/reference-how-to-compile-an-android-kernel.3627297/). Looking back through it now, I did not really do a great job of explaining what each command was doing; instead, I just expected the reader to follow along and eventually they would end up with a kernel. This is not a great way to teach, as it is about the end result, rather actually understanding exactly what is being done. In this sort of "addendum" to that guide, I would like to explain how a Linux kernel `make` command works, so that newcomers can understand what each command is doing and hopefully allow them to understand other guides that may come up in the future, as mine is fairly outdated at this point and I am not going to have any time soon to update it (I barely have time to right this...).

## A typical Linux kernel `make` command

A stereotypical Linux kernel `make` command will look like:

```
$ make <flags> <variables> <targets>
```

For example, this command builds an arm64 `defconfig` Linux kernel (on a non-arm64 system):

```bash
$ make -skj"$(nproc)" ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- mrproper defconfig all
```

Another example: this command builds an x86_64 `allmodconfig` Linux kernel (on an x86_64 system)

```bash
$ make -skj"$(nproc)" LLVM=1 mrproper allmodconfig all
```

I will break down exactly what these commands do in the coming sections.

## `make` flags

In the above example, we pass a series of "flags" or "arguments" to `make`: `-skj"$(nproc)"`. These are some of the most common flags but [there are plenty of others](https://linux.die.net/man/1/make). This post is not meant to be an in-depth look at `make` but instead, a brief overview for a beginner.

* `-s`: This tells `make` that we do not want to see the commands that are being run. In the Linux kernel, you will see something like:

  ```
    HOSTLD  scripts/mod/modpost
    CC      kernel/bounds.s
    CALL    scripts/atomic/check-atomics.sh
    UPD     include/generated/timeconst.h
    UPD     include/generated/bounds.h
    CC      arch/x86/kernel/asm-offsets.s
    UPD     include/generated/asm-offsets.h
    CALL    scripts/checksyscalls.sh
    LD      /home/nathan/cbl/src/linux/tools/objtool/objtool-in.o
    LINK    /home/nathan/cbl/src/linux/tools/objtool/objtool
    CC      init/main.o
   ```

  if you leave this flag off, which describes what is being built. This is perfectly fine but without this flag, it makes finding errors a lot harder when they occur; with this flag, you will only see the error.

* `-k`: This tells `make` that we do not want to "fail fast"; instead, we want `make` to continue to build until it cannot anymore. This is useful for uncovering as many build errors as possible, which is useful when first compiling a kernel because you will not have to play "whack-a-mole" with errors but can fix all of them in one go.

* `-j"$(nproc)"`: This tells `make` to build with all the logical cores on our system (we run `nproc` in a subshell with `$()` then use the result with `-j`), which will make the build much quicker. Without this option, `make` will execute everything serially (for all intents and purposes), which will be quite slow. Using all cores might not be desireable on an interactive/desktop system, so you could use one or two less than the number of cores. For example, if running `nproc` in your shell outputs `8`, you might consider only using `6` logical cores (i.e., `-j6`).

## `make` variables

There are several different `make` variables that are commonly used when building the Linux kernel.

* `ARCH=`: This variable tells the Linux kernel build system which architecture we want to compile for. If this variable is omitted, the host's architecture is used, which can be viewed by running `uname -m`; it will likely be `x86_64`, as that is the most common laptop and desktop architecture at the time of using this. You can view a general list of architectures that can be targeted by running:

  ```bash
  $ ls -D arch
  alpha  arc  arm  arm64  csky  h8300  hexagon  ia64  m68k  microblaze  mips  nds32  nios2  openrisc  parisc  powerpc  riscv  s390  sh  sparc  um  x86  xtensa
  ```

  For example, `ARCH=arm64` will build an arm64 kernel, which is the most common Android architecture.

* `CC=` and `LD=`: These variables control which C compiler and linker are used. Most often, you will see `CC=clang` and `LD=ld.lld`, which tells the Linux kernel build system that you would like to build all C code with `clang` and link all binary objects with the LLVM linker, `ld.lld`. By default, these variables are `$(CROSS_COMPILE)gcc` and `$(CROSS_COMPILE)ld`. We'll touch on `CROSS_COMPILE` in a second. These variables can either be the binary name as it appears in your PATH (e.g., `CC=clang`) or an absolute path to the binary that you would like to use (e.g., `CC=$HOME/bin/clang`). If the binary cannot be found or the absolute path to it is wrong, you will get weird build failures, so always double check.

* `CROSS_COMPILE=`: This variable serves two different purposes, depending on what compiler is being used.

  * When compiling with `gcc` (the default), this is typically a target triple (such as `aarch64-linux-gnu-`), which is prefixed onto `gcc` ([code](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Makefile?h=v5.17-rc2#n455)) In other words, this is used to describe a cross compiler, which builds code for an architecture other than the host architecture. In the first example above, I used `CROSS_COMPILE=aarch64-linux-gnu-`, which means that the C compiler (`$(CC)`) is going to become `aarch64-linux-gnu-gcc` (`$(CROSS_COMPILE)gcc`). As with `$(CC)` and `$(LD)` above, this can either be just the target triple such that `$(CROSS_COMPILE)gcc` is in your PATH (e.g., `CROSS_COMPILE=aarch64-linux-gnu-`, which means `aarch64-linux-gnu-gcc` will be called from PATH) or an absolute path (e.g., `CROSS_COMPILE=$HOME/bin/aarch64-linux-gnu-`, if `$HOME/bin/aarch64-linux-gnu-gcc` is intended to be used).

  * When compiling with `clang` (`CC=clang` OR `LLVM=1`, more on that later), this is still a target triple that is used as a prefix for tools as above but it is also used as the value for the `--target=` flag, which tells `clang` to emit code for the requested target. For example, if you were to use `CC=clang CROSS_COMPILE=aarch64-linux-gnu-`, `--target=aarch64-linux-gnu` will be passed to `clang` so that arm64 code is generated. As with above, this can be either the target triple or it can be an absolute path.

* `CROSS_COMPILE