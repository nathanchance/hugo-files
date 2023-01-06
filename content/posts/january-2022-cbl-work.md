---
title: "January 2022 ClangBuiltLinux Work"
date: 2022-01-31T00:18:00-07:00
toc: false
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Stable patches: The stable trees are the trees that most users consume so keeping them building and as warning and issue free as reasonable is important.

  * `[PATCH 5.4] Input: touchscreen - Fix backport of a02dcde595f7cbd240ccd64de96034ad91cffc40` ([`v1`](https://lore.kernel.org/r/20220103192935.3438038-1-nathan@kernel.org/))
  * `[PATCH RFC 4.9 0/5] Fix booting arm64 big endian with QEMU 5.0.0+` ([`v1`](https://lore.kernel.org/r/20220107194335.3090066-1-nathan@kernel.org/))
  * `[PATCH 4.4,4.9] power: reset: ltc2952: Fix use of floating point literals` ([`v1`](https://lore.kernel.org/r/20220109185902.1097931-1-nathan@kernel.org/))
  * [`Patches for clang and CONFIG_WERROR (arm64/x86_64)`](https://lore.kernel.org/r/YeCpzLnfA+g+u3Id@archlinux-ax161/)

* `-Wpointer-bool-conversion`: This warning is usually harmless, as it is typically just a developer checking if an array in the middle of a structure is `NULL`, which is not possible if the structure is not `NULL`, which would likely cause issues further up a call chain. In this one case, the fix was just to remove the check.

  * `clk: visconti: Remove pointless NULL check in visconti_pll_add_lookup()` ([`v1`](https://lore.kernel.org/r/20220107183303.2337676-1-nathan@kernel.org/))

* Other fixes: Yet another instance of an unsupported compiler flag causing issues with `cc-option`... I hope that we can get all instances of these cleaned up and have a flag turned on that will prevent these weird failures but that will probably be a release or two out.

  * `MIPS: Loongson64: Clean up use of cc-ifversion`, `MIPS: Loongson64: Wrap -mno-branch-likely with cc-option` ([`v1`](https://lore.kernel.org/r/20220120214001.1879469-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20220125221925.3547683-1-nathan@kernel.org/))



## LLVM patches

* Android LLVM: Android's version of LLVM is used by the Android kernel team so it is important to keep it working for them, as they are one of the largest consumers of this work.

  * [`Adjust location of d7e089f2d6a5cd5f283a90ab29241d20d4fc3ed1 in PATCHES.json`](https://android-review.googlesource.com/c/toolchain/llvm_android/+/1938498)

  * [`[patches] Cherry pick CLS for: ARCH=arm Linux kernel builds`](https://android-review.googlesource.com/c/toolchain/llvm_android/+/1938499)

* Upstream LLVM: This single patch was not accepted, as it was a workaround, rather than addressing the root cause of the issue.

  * [`[clang] Ignore -fconserve-stack`](https://reviews.llvm.org/D117717)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v13 2/2] x86/sgx: Add an attribute for the amount of SGX memory in a NUMA node`](https://lore.kernel.org/r/YdIzSGjxvgiWwkXu@archlinux-ax161/)

* [`Re: [PATCH 0000/2297] [ANNOUNCE, RFC] "Fast Kernel Headers" Tree -v1: Eliminate the Linux kernel's "Dependency Hell"`](https://lore.kernel.org/r/YdM4Z5a+SWV53yol@archlinux-ax161/)

* [`Re: [GIT PULL for v5.17-rc1] New year's media updates`](https://lore.kernel.org/r/YdNEQbgpBJP2lIiP@archlinux-ax161/)

* [`Re: [PATCH] [v3] x86/sgx: Fix NULL pointer dereference on non-SGX systems`](https://lore.kernel.org/all/YdSduKj1%2F4nmb5QF@archlinux-ax161/)

* [`Re: [PATCH 0/2] *** Fix reformat_objdump.awk ***`](https://lore.kernel.org/r/Ydc8wUjX4hnHg7ZE@archlinux-ax161/)

* [`[Clang][CFG] check children statements of asm goto`](https://reviews.llvm.org/D116059)

* [`Re: [PATCH] objtool: prefer memory clobber & %= to volatile & __COUNTER__`](https://lore.kernel.org/r/YeHx8qaHiJie3xdL@archlinux-ax161/)

* [`Re: [PATCH 1/1] script/sorttable: fix some initialization problems`](https://lore.kernel.org/r/YeX0BQOQYKI+95eP@archlinux-ax161/)

* [`Re: [PATCH] Makefile: Fix build with scan-build`](https://lore.kernel.org/r/YeiAa%2FeCxVZC+QbS@archlinux-ax161/)

* [`Re: [PATCH] lib/crypto: blake2s: avoid indirect calls to compression function for Clang CFI`](https://lore.kernel.org/r/YeiPn8Mo1AM7X9Ud@archlinux-ax161/)

* [`Re: Patch "lib/Kconfig.debug: make TEST_KMOD depend on PAGE_SIZE_LESS_THAN_256KB" has been added to the 5.16-stable tree`](https://lore.kernel.org/r/Ye7jp3flqBwlGzrV@archlinux-ax161/)

* [`Re: [PATCH v3 1/3] string: Make stpcpy() possible to use`](https://lore.kernel.org/r/YfF46oYCaelKU5qU@dev-arch.archlinux-ax161/)

* [`Re: [PATCH 0/2] xor: enable auto-vectorization in Clang`](https://lore.kernel.org/r/YfMQeRVkNMHr81P9@dev-arch.archlinux-ax161/)

* [`Re: [PATCHv3] powerpc: mm: radix_tlb: rearrange the if-else block`](https://lore.kernel.org/r/YfQFom4xDcysN1yb@dev-arch.archlinux-ax161/)

* [`Re: [PATCH 1/2] stack: Introduce CONFIG_RANDOMIZE_KSTACK_OFFSET`](https://lore.kernel.org/r/YfQ54x8zglPT%2FYnL@dev-arch.archlinux-ax161/)

* [`Re: [PATCH v3] Kconfig.debug: Make DEBUG_INFO selectable from a choice`](https://lore.kernel.org/r/YfRY6+CaQxX7O8vF@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] Kconfig.debug: Make DEBUG_INFO always default=n`](https://lore.kernel.org/r/YfR+xXcTYvHooTc0@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] usb: dwc3: xilinx: fix uninitialized return value`](https://lore.kernel.org/r/YfWpznEA95bH1Bvg@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] fortify: Update compile-time tests for Clang 14`](https://lore.kernel.org/r/YfbtQKtpyAM1hHiC@dev-arch.archlinux-ax161/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Formatting of llvm-objdump trips up x86 insn_decoder_test`](https://github.com/ClangBuiltLinux/linux/issues/1364#issuecomment-1003721819)

* [`AOSP LLVM r437112 is missing d7e089f2d6a5cd5f283a90ab29241d20d4fc3ed1`](https://github.com/ClangBuiltLinux/linux/issues/1561)

* [`ppc32/CONFIG_ALTIVEC=y: frame-larger-than in arch/powerpc/lib/xor_vmx.c`](https://github.com/ClangBuiltLinux/linux/issues/563#issuecomment-1005175153)

* [`relocation R_ARM_LDR_PC_G2 out of range`](https://github.com/ClangBuiltLinux/linux/issues/1562)

* [`Re: [TCWG CI] Regression caused by linux: mm: simplify try_to_unuse`](https://lore.kernel.org/r/YdcvkPTWAklekKJd@archlinux-ax161/)

* [`arm64 big endian boot failures on linux-4.9.y`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/269)

* [`Re: [PATCH v7 07/14] vdpa/mlx5: Support configuring max data virtqueue`](https://lore.kernel.org/r/YdiABvwrK3WxfHqb@archlinux-ax161/)

* [`Re: [PATCH v2 1/2] kbuild: move headers_check.pl to usr/include/`](https://lore.kernel.org/r/YdiLO0VstqY66LyP@archlinux-ax161/)

* [`[SCEV] Sequential/in-order UMin expression`](https://reviews.llvm.org/D116766), [`Re: [TCWG CI] Regression caused by llvm: [SCEV] Sequential/in-order UMin expression`](https://lore.kernel.org/r/Yd56LdGd17JZe+br@archlinux-ax161/)

* [`Re: [GIT PULL] Networking for 5.17`](https://lore.kernel.org/r/Yd2p3IbHJdzNok+1@archlinux-ax161/)

* [`[Clang/Test]: ename enable_noundef_analysis to disable-noundef-analysis and turn it off by default`](https://reviews.llvm.org/D105169)

* [`issue with volatile?`](https://github.com/ClangBuiltLinux/linux/issues/1566)

* [`Re: [PATCH v1 0/9] drivers: hv: dxgkrnl: Driver overview`](https://lore.kernel.org/r/Yd9SQxTznfHGjuDD@archlinux-ax161/)

* [`Re: [PATCH] ALSA: pcm: accept the OPEN state for snd_pcm_stop()`](https://lore.kernel.org/r/YeBe0CDN3gCfNKlj@archlinux-ax161/)

* [`Re: [PATCH v9 01/12] user_events: Add minimal support for trace_event into ftrace`](https://lore.kernel.org/r/YeGk0nIH9x91k01I@archlinux-ax161/)

* [`test PPC with LLVM_IAS=1`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/265)

* [`Re: [for-next][PATCH 10/31] scripts: ftrace - move the sort-processing in ftrace_init`](https://lore.kernel.org/r/YeMwNEfNaGErFthk@archlinux-ax161/)

* [`CFI failure in blake2s_update()`](https://github.com/ClangBuiltLinux/linux/issues/1567)

* [`Re: [TCWG CI] Regression caused by llvm: [cmake] Use `GNUInstallDirs` to support custom installation dirs.`](https://lore.kernel.org/r/YeOBitysu3dSskRe@archlinux-ax161/)

* [`Thin LTO + -Os gives vmlinux.o of almost the same size as thin LTO + -O2`](https://github.com/ClangBuiltLinux/linux/issues/1559)

* [`make tinyconfig and turning on LTO (HAS_LTO_CLANG depends appears to have bug)`](https://github.com/ClangBuiltLinux/linux/issues/1568)

* [`Re: drivers/iio/adc/ti-tsc2046.c:242:62: warning: taking address of packed member 'data' of class or structure 'tsc2046_adc_atom' may result in an unaligned pointer value`](https://lore.kernel.org/r/YenXz+RznXBuJMSR@dev-arch.archlinux-ax161/)

* [`Many instances of -Wunaligned-access`](https://github.com/ClangBuiltLinux/linux/issues/1569)

* [`Re: fs/ntfs3/fsntfs.c:2248:41: warning: taking address of packed member 'de' of class or structure 'NTFS_DE_SII' may result in an unaligned pointer value`](https://lore.kernel.org/r/Ye7kyA3yJupMLSD2@archlinux-ax161/)

* [`Re: stable-rc 5.4 queue riscv tinyconfig build failed`](https://lore.kernel.org/r/YfAy8u5u43x1z%2Fjg@archlinux-ax161/)

* [`Re: [PATCH] ARM: stackprotector: prefer compiler for TLS based per-task protector`](https://lore.kernel.org/r/YfBGg+zmIlHmTIm%2F@dev-arch.archlinux-ax161/)

* [`selected processor does not support XXX in ARM mode`](https://github.com/ClangBuiltLinux/linux/issues/1570)

* [`INT DW_ATE_unsigned_56 Error emitting BTF type`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/290)

* [`Re: [PATCH bpf-next v2 1/6] tools: Help cross-building with clang`](https://lore.kernel.org/r/YfSUAPnZX%2FwP8U+p@archlinux-ax161/)

* [`Re: arch/arm/lib/xor-neon.c:30:2: warning: This code requires at least version 4.6 of GCC`](https://lore.kernel.org/r/YfWiIZ9QZuWRTYRy@dev-arch.archlinux-ax161/)

* [`Re: drivers/input/touchscreen/ads7846.c:705:24: warning: taking address of packed member 'data' of class or structure 'ads7846_buf' may result in an unaligned pointer value`](https://lore.kernel.org/r/YfWj+hnMiK8dECOp@dev-arch.archlinux-ax161/)



## Tooling improvements

* [`Spin down CFI trees for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/268)

* [`qemu: Install git`](https://github.com/ClangBuiltLinux/containers/pull/1)

* [`patches: android12-5.10: Add patch to fix Thumb2 kernel boot on newer QEMU`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/270)

* [`boot-utils: Remove '--use-cbl-qemu'`](https://github.com/ClangBuiltLinux/boot-utils/pull/51)

* [`patches: Drop mainline and next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/272)

* [`patches: Drop 5.15 and 5.10`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/273)

* [`patches: Apply hack patch to fix the build on android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/274)

* [`Revert #274`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/275)

* [`patches: Add a series to fix ARCH=arm64 with mainline and next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/276)

* [`check-patches.sh: Check for patches but no '--patch-series'`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/277)

* [`Drop clang-10 builds on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/278)

* [`boot-qemu.sh: Support booting ARMv7 kernels under KVM on AArch64 hosts`](https://github.com/ClangBuiltLinux/boot-utils/pull/52)

* [`Update to Linux 5.16 for PGO and update known good revision](https://github.com/ClangBuiltLinux/tc-build/pull/179)

* [`patches/mainline: Add a SCSI patch from -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/280)

* [`patches: Remove -next`](https://github.com/ClangBuiltLinux/continuous-integration2/commit/3b585a00361843992074575344a090e818a65022)

* [`Revert "patches/mainline: Add a SCSI patch from -next"`](https://github.com/ClangBuiltLinux/continuous-integration2/commit/bff9efed6fca2a5cbd28f0d300f718b33893fcd9)

* [`patches: Remove tip`](https://github.com/ClangBuiltLinux/continuous-integration2/commit/e46d6ae465a6851b98dcfc69a4b3639145b959ba)

* [`patches: mainline: Add a patch for drivers/regulator/max20086-regulator.c`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/281)

* [`Drop android12-5.10 patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/282)

* [`Enable arm64 big endian build on -next with AOSP LLVM`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/283)

* [`Enable x86_64 GCOV build with AOSP LLVM`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/284)

* [`Disable CONFIG_DEBUG_INFO_BTF for gki_defconfig`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/285)

* [`Add builds for android13-5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/287)

* [`Add pinctrl-thunderbay.c patches to arm64-fixes`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/288)

* [`Add blake2s CFI workaround patch for android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/289)

* [`boot-qemu.sh: Add 'arm' as an alias for 'arm32_v7'`](https://github.com/ClangBuiltLinux/boot-utils/pull/53)

* [`boot-qemu.sh: Add support for booting pmac32_defconfig`](https://github.com/ClangBuiltLinux/boot-utils/pull/54)

* [`Disable CONFIG_DEBUG_INFO_BTF for Fedora configs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/291)

* [`Fix CONFIG_DEBUG_INFO_BTF=n for Fedora configs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/292)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 3 and 4, HP desktop, ASUS laptop, and Hyper-V and VMware platforms on my workstation. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).