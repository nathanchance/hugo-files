---
title: "March 2021 ClangBuiltLinux Work"
date: 2021-04-01T10:25:53-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Another month, another post about the work that I have done as a kernel/compiler developer! One of the highlights this month is that I got LLVM commit access so I can merge my own patches into LLVM, which I did twice so far.

## Linux kernel patches

* [`powerpc/fadump: Mark fadump_calculate_reserve_size as __init`](https://lore.kernel.org/r/20210302195013.2626335-1-nathan@kernel.org/): LLVM 13 switched over to the [New Pass Manager (NPM)](https://blog.llvm.org/posts/2021-03-26-the-new-pass-manager/), which has impacted some inlining decisions, which in turn exposed some bugs in section annotations. The kernel places certain functions and variables into specific sections that are discarded and freed after init. If a non-init function calls an init function, that is technically a use-after-free so the kernel warns when this happens. In this particular case, `identical_pvr_fixup()` was not marked as `__init` and it was not getting inlined so the calls to `identify_cpu()` and `of_get_flat_dt_prop()` resulting in the modpost warnings. In practice, this is not an issue because `identical_pvr_fixup()` is only called from `__init` context but it is important to get these things right so that real warnings can easily be caught.

* [`powerpc/prom: Mark identical_pvr_fixup as __init`](https://lore.kernel.org/r/20210302200829.2680663-1-nathan@kernel.org/): Same situation as above :)

* [`Makefile: Remove '--gcc-toolchain' flag`](https://lore.kernel.org/r/20210309205915.2340265-1-nathan@kernel.org/) and [`Makefile: Only specify '--prefix=' when building with clang + GNU as`](https://lore.kernel.org/r/20210309205915.2340265-2-nathan@kernel.org/): These two patches clean up our clang specific compiler flags. I noticed that we specify `--prefix=` with a full path after commit [ca9b31f6bb9c](https://git.kernel.org/linus/ca9b31f6bb9c6aa9b4e5f0792f39a97bbffb8c51) ("Makefile: Fix GCC_TOOLCHAIN_DIR prefix for Clang cross compilation"), meaning that `--gcc-toolchain=` is unnecessary because the kernel uses nothing within the sysroot usually provided by GCC. As a follow up, specifying `--prefix=` is only necessary when we are using the GNU assembler.

* [`ARM: Make UNWINDER_ARM depend on ld.bfd or ld.lld 11.0.0+`](https://lore.kernel.org/r/20210311005418.2207250-1-nathan@kernel.org/): A workaround for a bug in all LLD versions prior to 11.0.0. The kernel's minimum supported version of LLVM is 10.0.1, meaning that if something is broken with LLVM 10.0.1+, it must be fixed or worked around. KernelCI has run into this as their testing plan is now the minimum supported version of LLVM and the latest stable version.

* [`riscv: Fix CONFIG_FUNCTION_TRACER with clang`](https://lore.kernel.org/r/20210325223807.2423265-1-nathan@kernel.org/): This series fixes build errors on RISC-V when `CONFIG_FUNCTION_TRACER` is set. This was a rather fun series to develop but at the same time, incredibly frustrating because of how it evolved. Unfortunately, it requires a kind of ugly workaround due to an LLVM bug (more on that below). I am hoping that it will be picked up before Linux 5.12 is released but it is not like it cannot be backported to stable.

* [`riscv: Use $(LD) instead of $(CC) to link vDSO`](https://lore.kernel.org/r/20210325215156.1986901-1-nathan@kernel.org/): In traditional projects, the linker is never directly invoked; instead, the compiler is called and it calls the linker implicitly. For the kernel, it is the other way around: the compiler is only used to build `.o` files and the linker is directly invoked on them, except in certain vDSO Makefiles. We have been working on getting rid of this and making the behavior consistent over the kernel so that `ld.lld` can be used in more places, as `clang` by default defauls to calling `ld.bfd`.

* [`Fix cross compiling x86 with clang`](https://lore.kernel.org/r/20210326000435.4785-1-nathan@kernel.org/): Back in December, someone reported that x86 was not cross compilable due to a lack of the `--target=` flag. This is not a very common task as x86 is the dominant architecture but arm64 is starting to become more common, especially with Apple's M1 chip. This series adds the `--target=` flag to specific subdirectories that have hand rolled `KBUILD_CFLAGS` (which is an antipattern that should really be avoided but simple fix for now).

## LLVM patches

I contributed a few small patches to LLVM this month:

* [`debuginfo-tests: Fix check-gdb-mlir-support build after MLIR API change in a4bb667d831c`](https://reviews.llvm.org/D98613): A MILR API change that was not reflected back in the MILR debuginfo-tests.

* [`[RISCV] Fix mcount name`](https://reviews.llvm.org/D98881): This is the LLVM side of the RISC-V function tracing series above. Somehow, LLVM used a different name for the mcount symbol than GCC did, which needed to be fixed. Thankfully, LLVM already had the plumbing inside of it for this, I just had to hook into it for RISC-V and add some tests.

* [`libc: Default LIBC_INSTALL_PREFIX to ${CMAKE_INSTALL_PREFIX}`](https://reviews.llvm.org/D99636): While working on the full toolchain tc-build series below, I ran into this issue while running `ninja install`.

## Patch review and input

I feel like I gave more input/review this month than I have before, which is never a bad thing :) Being a quick reviewer for others incentivizes them to give you a quick review in return. For some of the links below, I linked the top message/patch; my full input can be seen at the bottom by clicking on my name.

* [`[PATCH] Fix ld-version.sh script if LLD was built with LLD_VENDOR`](https://lore.kernel.org/r/20210302221211.1620858-1-bero@lindev.ch/)

* [`[PATCH 1/4] kbuild: remove LLVM=1 test from HAS_LTO_CLANG`](https://lore.kernel.org/r/20210303183333.46543-1-masahiroy@kernel.org/)

* [`[PATCH] MIPS: Add comment about CONFIG_MIPS32_O32 in loongson3_defconfig when build with Clang`](https://lore.kernel.org/r/1614820544-10686-1-git-send-email-yangtiezhu@loongson.cn/) and [`Re: [PATCH v2] MIPS: Make MIPS32_O32 depends on !CC_IS_CLANG`](https://lore.kernel.org/r/20210304064552.xzjqbhltghmd5vfa@archlinux-ax161/)

* [`Re: [PATCH] KVM: arm64: Don't use cbz/adr with external symbols`](https://lore.kernel.org/r/20210307043545.pyxy22z3rktgof4m@archlinux-ax161/)

* [`Re: [PATCH] kbuild: dummy-tools: adjust to scripts/cc-version.sh`](https://lore.kernel.org/r/20210309173745.lbinnmjrmamld2rs@archlinux-ax161/)

* [`[PATCH] gcov: fix clang-11+ support`](https://lore.kernel.org/r/20210312192139.2503087-1-ndesaulniers@google.com/)

* [`Re: [PATCH] kbuild: fix ld-version.sh to not be affected by locale`](https://lore.kernel.org/r/20210312194956.vsikqyaya676qvcu@archlinux-ax161/)

* [`Thumb2 ias`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/101)

* [`Gcov`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/107)

* [`spin down LTO builds of Sami's LTO tree`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/114)

* [`Add Android ARCH=arm LLVM_IAS=1 5.4+ coverage`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/115)

* [`Arm android cleanup`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/116)

* [`debian: build.sh: add missing quotes to MODULES_FOLDER`](https://github.com/ClangBuiltLinux/boot-utils/pull/37)

* [`fix -g`](https://github.com/ClangBuiltLinux/boot-utils/pull/39)

## Issue triage and reporting

Healthy dose of LLVM regressions and Linux kernel build failures due to new functionality.

* [`[mlir] Simplify various pieces of code now that Identifier has access to the Context/Dialect`](https://reviews.llvm.org/D97431#2598012)

* [`[mlir][IR] Refactor the internal implementation of Value`](https://reviews.llvm.org/D97804#2625161)

* [`[AST] Add generator for source location introspection`](https://reviews.llvm.org/D93164#2630203)

* [`[RISCV] Add basic cost modelling for fixed vector gather/scatter.`](https://reviews.llvm.org/D99142#2651729)

* [`(continuous-integration2) incomplete logs?`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/105)

* [`Build failures with LLVM 13.0.0-++20210330111113+6d2fb3cefba6-1~exp1~20210330091825.3874`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/117)

* [`clang-12: error: unsupported option '-fpatchable-function-entry=8' for target 'riscv64-unknown-linux-gnu'`](https://github.com/ClangBuiltLinux/linux/issues/1268)

* [`Issue around hyp_panic while building aarch64 allmodconfig with ThinLTO`](https://github.com/ClangBuiltLinux/linux/issues/1317)

* [`Faild to build qcacld-3.0 on msm-5.4 kernel`](https://github.com/ClangBuiltLinux/linux/issues/1323)

* [`-Walign-mismatch in include/linux/efi.h`](https://github.com/ClangBuiltLinux/linux/issues/1327)

* [`a0e2bf7cb7006b5a58ee81f4da4fe575875f2781 in -next breaks compiling with CFI`](https://github.com/ClangBuiltLinux/linux/issues/1330)

* [`mcount problem on RISC-V`](https://github.com/ClangBuiltLinux/linux/issues/1331)

* [`ld.lld: error in arch/arm64/kernel/vmlinux.lds`](https://github.com/ClangBuiltLinux/linux/issues/1336)

* [`Re: [PATCH 3/4] certs: Add ability to preload revocation certs`](https://lore.kernel.org/r/20210303181121.ii7ofii65lh337ln@archlinux-ax161/)

* [`-Walign-mismatch in block/blk-mq.c`](https://lore.kernel.org/r/20210310182307.zzcbi5w5jrmveld4@archlinux-ax161/)

* [`Re: [PATCH 2/2] zonefs: Fix O_APPEND async write handling`](https://lore.kernel.org/r/20210315170855.tguqrsl7wsbjojib@archlinux-ax161/)

* [`Re: Clang: powerpc: kvm/book3s_hv_nested.c:264:6: error: stack frame size of 2480 bytes in function 'kvmhv_enter_nested_guest'`](https://lore.kernel.org/r/20210319170154.oe5sa6ohjbyucbws@archlinux-ax161/)

* [`Re: [PATCH v8 09/13] phy: tegra: xusb: Add wake/sleepwalk for Tegra210`](https://lore.kernel.org/r/20210325202616.xgjkdg7zev6apofe@archlinux-ax161/)

* [`8ec7ea3ddce7379e13e8dfb4a5260a6d2004aa1c causes more stack usage on PowerPC`](https://llvm.org/pr49610)

## Tooling patches

I focused on streamlining our boot utilities, increasing the build coverage, and improving the toolchain build scripts.

* [`buildroot: Speed up build times`](https://github.com/ClangBuiltLinux/boot-utils/pull/38)

* [`boot-qemu.sh: Add support for earlycon`](https://github.com/ClangBuiltLinux/boot-utils/pull/40)

* [`Enable LTO testing on mainline and -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/94)

* [`Drop CONFIG_UBSAN_UNSIGNED_OVERFLOW from all*configs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/97)

* [`github: Add check-clang.sh workflow`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/99)

* [`generate_workflow.py: Drop usedForSecurity in hashlib call`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/100)

* [`Change MIPS base config`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/102)

* [`utils.py: Fix get_cbl_name() for mipsel`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/103)

* [`Move Android back to ThinLTO for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/106)

* [`Add support for arm64 trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/108)

* [`Test allmodconfig with LTO and defconfig with full LTO`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/109)

* [`Enable i386 builds on older trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/110)

* [`Disable pseries_defconfig on LLVM 12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/111)

* [`boot-utils: Update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/112)

* [`Completely spin down LTO builds on Sami's tree`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/114)

* [`Support for Clear Linux`](https://github.com/ClangBuiltLinux/tc-build/pull/141)

* [`Update to Linux 5.11.3 and bump the known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/142)

* [`Simplify the flags in build-binutils.py`](https://github.com/ClangBuiltLinux/tc-build/pull/143)

* [`build-binutils.py: Remove '--with-sysroot' configuration flag`](https://github.com/ClangBuiltLinux/tc-build/pull/144)

* [`Make script more friendly to being used system-wide/LLVM development and add more kernel PGO options`](https://github.com/ClangBuiltLinux/tc-build/pull/145)

## Conclusion

When dealing with various issues all at once, it is hard to see any progress being made so doing these status reports is a great way to help keep myself grounded and realize that progress is indeed being made. There are some cool things in development such as Sami Tolvanen's [Control Flow Integrity series](https://lore.kernel.org/r/20210331212722.2746212-1-samitolvanen@google.com/), which is high priority for testing on arm64. Stay tuned for more on that!