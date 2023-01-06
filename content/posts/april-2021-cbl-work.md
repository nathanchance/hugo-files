---
title: "April 2021 ClangBuiltLinux Work"
date: 2021-04-30T21:14:13-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Another month down! Feels like I just wrote the March 2021 post... but I suppose that is what happens when your project moves super quickly :)

## Linux kernel patches

* [`[PATCH] ACPI / CPPC: Replace cppc_attr with kobj_attribute`](https://lore.kernel.org/r/20210407213048.940498-1-nathan@kernel.org/): I discovered this issue as part of my testing of Sami Tolvanen's Control Flow Integrity series (more on that below). There are probably [many more of these lurking](https://lore.kernel.org/r/202006112217.2E6CE093@keescook/) because this pattern is hard to spot without runtime testing. As I have time amongst all of the other issues and such, I will be testing the x86 variant of CFI on several different servers, trying to flush out all of the different issues there.

* [`[PATCH] block: Disable -Walign-mismatch for blk-mq.c`](https://lore.kernel.org/r/20210408194458.501617-1-nathan@kernel.org/): A new warning in clang pointed this out. Unfortunately, the alignment mismatch is expected so I initially sent this patch silencing the warning but it seems like there might be a [better solution](https://lore.kernel.org/r/20210429150940.3256656-1-arnd@kernel.org/).

* [`[PATCH] MIPS: generic: Update node names to avoid unit addresses`](https://lore.kernel.org/r/20210409192128.3998606-1-nathan@kernel.org/): The latest version of U-Boot caused all generic MIPS configurations to stop building, which disrupted my local testing. To get back to a working state, I send this patch.

* [`[PATCH] crypto: arm/curve25519 - Move '.fpu' after '.arch'`](https://lore.kernel.org/r/20210409221155.1113205-1-nathan@kernel.org/): Debian's clang, which we use for testing in our continuous-integration, includes an out of tree patch that turns off NEON in armv7 by default, causing a build error when trying to build ARCH=arm allmodconfig. Moving the `.fpu` after `.arch` fixes the issue because `.arch` overrides the previous `.fpu` directive.

* [`[PATCH] arm64: alternatives: Move length validation in alternative_{insn,endif}`](https://lore.kernel.org/r/20210414000803.662534-1-nathan@kernel.org/): More integrated assembler fixes. This is similar to another fix that went into the tree last year, this should make it so we never see this issue again, hopefully...

* [`[PATCH 1/2] x86/events/amd/iommu: Fix sysfs type mismatch`](https://lore.kernel.org/r/20210415001112.3024673-1-nathan@kernel.org/) and [`[PATCH 2/2] perf/amd/uncore: Fix sysfs type mismatch`](https://lore.kernel.org/r/20210415001112.3024673-2-nathan@kernel.org/): More type mismatches from CFI testing. These should have been found and fixed during commit [`ebd19fc372e3`](https://git.kernel.org/linus/ebd19fc372e3) but it does not look like anyone has tested CFI on a more recent AMD CPU.

* [`[PATCH] drm/tegra: Fix shift overflow in tegra_shared_plane_atomic_update`](https://lore.kernel.org/r/20210415152913.1363964-1-nathan@kernel.org/): Just a small clang warning patch.

* [`[PATCH] kbuild: Add $(KBUILD_HOSTLDFLAGS) to 'has_libelf' test`](https://lore.kernel.org/r/20210422201914.3682494-1-nathan@kernel.org/): During a regular kernel build, compiling and linking happen in two separate phases, using the variables `$(CC)` and `$(LD)`, so it is easy to ensure that you are using only the tools that you want. An ultimate goal of ClangBuiltLinux is to be able to build the kernel in an environment free of GCC and binutils and these two variables make it easy to ensure that happens. However, during the host tools building, compiling and linking happens through only the compiler, like a more standard userspace project, and that means that the compiler's default linker is used, unless `-fuse-ld=` is provided via the command line. If a project is not able to change their default linker but still wants to test in a hermetic environment, `-fuse-ld=lld` needs to work properly but without this patch, it does not. This came up during testing from Google, where they have a Docker image that only includes the tools needed for a kernel build so if there is a call to a tool that does not exist, the build errors.

* [`[PATCH] powerpc: Avoid clang uninitialized warning in __get_user_size_allowed`](https://lore.kernel.org/r/20210426203518.981550-1-nathan@kernel.org/): Clang's static analysis pointed out a false positive, which was fixed in one place but not another, causing a build failure in our CI due to `-Werror` in `arch/powerpc`.

* [`[PATCH] Makefile: Move -Wno-unused-but-set-variable out of GCC only block`](https://lore.kernel.org/r/20210429012350.600951-1-nathan@kernel.org/): Clang's implementation of this warning is coming down the pipeline so this disables it like GCC in a default build. Ideally, we want to enable rather than disable warnings but this is a special case because it is also disabled for GCC by default.

* [`[PATCH] x86: Enable clang LTO for 32-bit as well`](https://lore.kernel.org/r/20210429232611.3966964-1-nathan@kernel.org/): LTO was merged in 5.12 for arm64 and x86_64. In my testing, I noticed that 32-bit x86 works fine and there is [some interest](https://github.com/ClangBuiltLinux/linux/issues/1163) in this.

## Patch review and input

When possible, I link directly to my response but sometimes, the link is to the main post and my response can be seen inline.

* [`Re: [PATCH v5 00/18] Add support for Clang CFI`](https://lore.kernel.org/r/20210402195823.huphtlhluqjgrw26@archlinux-ax161/)

* [`overlay: Exit rcS script so it only runs once`](https://github.com/ClangBuiltLinux/boot-utils/pull/41#issuecomment-812055354)

* [`Re: [PATCH 3/3] kbuild: fix false-positive modpost warning when all symbols are trimmed`](https://lore.kernel.org/r/20210402211552.dzcxxs5scz7ddxtt@Ryzen-9-3900X.localdomain/)

* [`[PATCH 0/2] gcov: re-fix clang-11 support`](https://lore.kernel.org/r/20210407185456.41943-1-ndesaulniers@google.com/)

* [`Re: [PATCH v4 12/12] exec: Fix overlap of PAGE_ANON and PAGE_TARGET_1`](https://lore.kernel.org/r/20210407!213337.5zzpfs5epzdp75sj@archlinux-ax161/)

* [`Re: [PATCH v9] pgo: add clang's Profile Guided Optimization infrastructure`](https://lore.kernel.org/r/20210407214747.kaemm45tut3jw5in@archlinux-ax161/)

* [`Re: [PATCH] arm64: vdso32: drop -no-integrated-as flag`](https://lore.kernel.org/r/YHYlQnFRMNdn%2FCDp@archlinux-ax161/)

* [`Re: [PATCH v2] arm64: vdso32: drop -no-integrated-as flag`](https://lore.kernel.org/r/YHeGJzhIhSFJLprr@archlinux-ax161/)

* [`[Aarch64] handle "o" inline asm memory constraints`](https://reviews.llvm.org/D100412#2690236)

* [`Re: [PATCH] KVM: x86: Fix implicit enum conversion goof in scattered reverse CPUID code`](https://lore.kernel.org/r/YIBcd+5NKJFnkTC1@archlinux-ax161/)

* [`Re: [PATCH] x86/boot/compressed: enable -Wundef`](https://lore.kernel.org/r/YIIVlahVlJAsaE9W@archlinux-ax161/)

* [`Re: [PATCH] stack: replace "o" output with "r" input constraint`](https://lore.kernel.org/r/YIIcoz4fHjVjWHTI@archlinux-ax161/)

* [`Hexagon CI coverage`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/125)

* [`[PowerPC] Handle inline assembly clobber of link regsiter`](https://reviews.llvm.org/D101657)

* [`Assertion `MI->getOperand(0).isReg()' failed`](https://github.com/ClangBuiltLinux/linux/issues/1363)

## Issue triage and reporting

A lot more issues in upstream LLVM this month and I ran into two regressions with external tools. Thankfully, those were quick to be resolved so testing could get back on track. As with previous months, the link is usually to the main post/issue and my input can be seen throughout.

* [`qemu-aarch64-static "Illegal instruction" with debootstrap`](https://bugs.launchpad.net/qemu/+bug/1922617)

* [`CFI violation in drivers/infiniband/core/sysfs.c`](https://lore.kernel.org/r/20210402195241.gahc5w25gezluw7p@archlinux-ax161/)

* [`mkimage regression when building ARCH=mips defconfig Linux kernel`](https://lore.kernel.org/r/20210408182253.6m3j6lhmh3iflquz@archlinux-ax161/)

* [`inconsistent ORC unwind table entries in file: vmlinux in /scripts/sorttable vmlinux`](https://github.com/ClangBuiltLinux/linux/issues/1341)

* [`-Wframe-larger-than= in sound/core/control_led.c`](https://github.com/ClangBuiltLinux/linux/issues/1344)

* [`perl spam when running tuxmake without a runtime`](https://gitlab.com/Linaro/tuxmake/-/issues/133)

* [`clang-nightly arm toolchain outdated?`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/118)

* [`undefined symbol: __compiletime_assert_177 in firmware/stratix10-rsu.o`](https://github.com/ClangBuiltLinux/linux/issues/1346)

* [`ppc44x_defconfig has stopped booting on next`](https://github.com/ClangBuiltLinux/linux/issues/1345)

* [`"error: expected assembly-time absolute expression" in arch/arm64/kernel/entry.S`](https://github.com/ClangBuiltLinux/linux/issues/1347)

* [`arch/arm64/kvm/perf.c:58:36: error: implicit declaration of function 'perf_num_counters'`](https://lore.kernel.org/r/20210413200057.ankb4e26ytgal7ev@archlinux-ax161/)

* [`Re: [PATCH 2/3] habanalabs: support legacy and new pll indexes`](https://lore.kernel.org/r/YHhK4twu3feLG3AR@archlinux-ax161/)

* [`s390: Issue around the ALTERNATIVE macro with integrated-as`](https://github.com/ClangBuiltLinux/linux/issues/1356)

* [`error: couldn't allocate output register for constraint 'x'`](https://github.com/ClangBuiltLinux/linux/issues/1358)

* [`-Wsometimes-uninitialized in arch/powerpc/include/asm/uaccess.h`](https://github.com/ClangBuiltLinux/linux/issues/1359)

* [`Re: [PATCH v7] powerpc/irq: Inline call_do_irq() and call_do_softirq()`](https://lore.kernel.org/r/YIcLcujmoK6Yet9d@archlinux-ax161/)

* [`[Passes] Add relative lookup table converter pass`](https://reviews.llvm.org/D94355#2717531)

* [`[DebugInfo] Use variadic debug values to salvage BinOps and GEP instrs with non-const operands`](https://reviews.llvm.org/D91722#2724321)

* [`"Not a NOP" warning when booting i386_defconfig`](https://github.com/ClangBuiltLinux/linux/issues/1361)

* [`Generalize getInvertibleOperand recurrence handling slightly`](https://reviews.llvm.org/D100884#2724725)

* [`chkobjdump.awk: no default action when using llvm-objdump`]()

* [`Formatting of llvm-objdump trips up x86 insn_decoder_test`](https://github.com/ClangBuiltLinux/linux/issues/1364)

* [`Re: Very slow clang kernel config ..`](https://lore.kernel.org/r/YItU3YrFi8REwkRA@archlinux-ax161/)

* [`5.12 boot failure when ThinLTO is enabled`](https://github.com/ClangBuiltLinux/linux/issues/1365)

* [`-Wconstant-conversion in drivers/platform/surface/surface_aggregator_registry.c`](https://github.com/ClangBuiltLinux/linux/issues/1366)

## Tooling improvements

A little all over the place this month, no major work now that the continuous integration setup is stable and the other tools are working well.

* [`build-llvm.py: Build compiler-rt builtins`](https://github.com/ClangBuiltLinux/tc-build/pull/146)

* [`buildroot: Check init.d action to avoid double version print out`](https://github.com/ClangBuiltLinux/boot-utils/pull/42)

* [`debian/build.sh: qemu-debootstrap -> debootstrap`](https://github.com/ClangBuiltLinux/boot-utils/pull/43)

* [`Make it green as of 4-26-2021`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/123)

* [`boot-qemu.sh: Do not set earlycon for arm32_v6`](https://github.com/ClangBuiltLinux/boot-utils/pull/44)

* [`Enable LLVM_IAS=1 for arm32 allmodconfig on android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/126)

## Behind the scenes

Three things that were more behind the scenes this month:

* ClangBuiltLinux did a bug scrub on two days, where we sat down in Google Meet and worked on a shared Google Doc to try and see if we could reproduce old issues. I think we ended up touching somewhere around 100 issues, closing several issues that are no longer relevant. This will allow us to stay focused and work through a clean backlog as we have time in the future. It is the balance of dealing with new issues while finding time to work on the old or getting more people involved with the project ;)

* I did a lot of testing of the Control Flow Integrity series on both arm64 (which I mention above) and x86_64. Unfortunately, the x86_64 testing became moot when the x86 maintainers basically [NAK'd](https://lore.kernel.org/r/CALCETrV6WYx7dt56aCuUYsrrFya==zYR+p-YZnaATptnaO7w2A@mail.gmail.com/) the series in its current form. I will continue testing in May, hoping to tease out any failures so that those are all resolved by the time the series is ready for a new revision.

* I received a [BeagleV board](https://beagleboard.org/beaglev) as part of their beta program for the sake of testing clang built kernels on it. I messed around with it a little bit already, I will continue this in May.

## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security)!
