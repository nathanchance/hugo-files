---
title: October 2024 ClangBuiltLinux Work
date: 2024-10-31T16:30:00-0700
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * [`ANDROID: GKI: Put vendor_data_pad behind CONFIG_GKI_DYNAMIC_TASK_STRUCT_SIZE`](https://android-review.googlesource.com/c/kernel/common/+/3311875)

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `powerpc: Prepare for clang's per-task stack protector support` ([`v1`](https://lore.kernel.org/20241007-powerpc-fix-stackprotector-test-clang-v1-0-08c15b2694e4@kernel.org/), [`v2`](https://lore.kernel.org/20241009-powerpc-fix-stackprotector-test-clang-v2-0-12fb86b31857@kernel.org/))
  * `HID: Remove default case statement in fetch_item()` ([`v1`](https://lore.kernel.org/20241015-hid-fix-fetch_item-unreachable-v1-1-b131cd10dbd1@kernel.org/))
  * `um: Fix a couple of issues with stub_exe when building with clang` ([`v1`](https://lore.kernel.org/20241016-uml-fix-stub_exe-clang-v1-0-3d6381dc5a78@kernel.org/))
  * `kprobes: Adjustments for __counted_by addition` ([`v1`](https://lore.kernel.org/20241030-kprobes-fix-counted-by-annotation-v1-0-8f266001fad0@kernel.org/))
  * `powerpc/vdso: Drop -mstack-protector-guard flags in 32-bit files with clang` ([`v1`](https://lore.kernel.org/20241030-powerpc-vdso-drop-stackp-flags-clang-v1-1-d95e7376d29c@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `staging: gpib: Fix PCI header include guard` ([`v1`](https://lore.kernel.org/20241015-staging-gpib-fix-pci-header-guard-v1-1-dfa45fe8d63f@kernel.org/))
  * `kbuild: Move -Wenum-enum-conversion to W=2` ([`v1`](https://lore.kernel.org/20241016-disable-two-clang-enum-warnings-v1-1-ae886d7a0269@kernel.org/), [`v2`](https://lore.kernel.org/20241017-disable-two-clang-enum-warnings-v2-1-163ac04346ae@kernel.org/))
  * `media: dvbdev: Avoid using uninitialized ret in dvb_register_device()` ([`v1`](https://lore.kernel.org/20241021-dvbdev-fix-uninitialized-return-v1-1-a704945f20e5@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v1] RISC-V: disallow gcc + rust builds`](https://lore.kernel.org/20241001185525.GA29379@thelio-3990X/)
* [`Enable measuring the kernel's Source-based Code Coverage and MC/DC with Clang (v2`](https://lore.kernel.org/llvm/?q=f%3Anathan%40kernel.org+t%3Awentao+v2)
* [`Re: [PATCH] Bluetooth: Fix type of len in rfcomm_sock_{bind,getsockopt_old}()`](https://lore.kernel.org/20241002152227.GA3292493@thelio-3990X/)
* [`Re: [PATCH] mm/vmstat: Fix -Wenum-enum-conversion warning in vmstat.h`](https://lore.kernel.org/20241008005136.GA241099@thelio-3990X/)
* [`[PowerPC][ISelLowering] Support -mstack-protector-guard=tls`](https://github.com/llvm/llvm-project/pull/110928#issuecomment-2398802332)
* [`Re: [PATCH] kbuild: refactor cc-option-yn, cc-disable-warning, rust-option-yn macros`](https://lore.kernel.org/20241009182709.GA3274931@thelio-3990X/)
* [`Re: [PATCH v2] x86/stackprotector: Work around strict Clang TLS symbol requirements`](https://lore.kernel.org/20241016021045.GA1000009@thelio-3990X/)
* [`Re: [PATCH -v2] resource: Remove dependency on SPARSEMEM from GET_FREE_REGION`](https://lore.kernel.org/20241016153833.GA385255@thelio-3990X/)
* [`Re: [REGRESSION][BISECTED] erroneous buffer overflow detected in bch2_xattr_validate`](https://lore.kernel.org/20241017165522.GA370674@thelio-3990X/)
* [`Re: [PATCH] rtw89: -Wenum-compare-conditional warnings`](https://lore.kernel.org/20241018224058.GA2635543@thelio-3990X/)
* [`Re: [PATCH] media: mtk-vcodec: venc: avoid -Wenum-compare-conditional warning`](https://lore.kernel.org/20241018224136.GB2635543@thelio-3990X/)
* [`Re: [PATCH] media: mediatek: vcodec: mark vdec_vp9_slice_map_counts_eob_coef noinline`](https://lore.kernel.org/20241018224502.GC2635543@thelio-3990X/)
* [`Re: [PATCH] um: fix stub exe build with CONFIG_GCOV`](https://lore.kernel.org/20241027220913.GC2755311@thelio-3990X/)
* [`Re: [PATCH v6 0/7] Add AutoFDO and Propeller support for Clang build`](https://lore.kernel.org/20241027221702.GD2755311@thelio-3990X/)
* [```Re: [PATCH] kbuild: rust: avoid errors with old `rustc`s without LLVM patch version```](https://lore.kernel.org/20241027222505.GA2882707@thelio-3990X/)
* [`Re: [PATCH v3 1/2] sparc/build: Put usage of -fcall-used* flags behind cc-option`](https://lore.kernel.org/20241029222421.GA2632697@thelio-3990X/)
* [`Re: [PATCH v3 2/2] sparc/build: Add SPARC target flags for compiling with clang`](https://lore.kernel.org/20241029222446.GB2632697@thelio-3990X/)
* [`Re: [PATCH 2/3] tmpfs: Fix type for sysfs' casefold attribute`](https://lore.kernel.org/20241101071208.GA2962282@thelio-3990X/)
* [`Re: [PATCH 3/3] tmpfs: Initialize sysfs during tmpfs init`](https://lore.kernel.org/20241101071942.GB2962282@thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: linux-6.12-rc1/drivers/iio/imu/bmi323/bmi323_core.c:133: Array contents defined but not used ?`](https://lore.kernel.org/20240930151542.GA3556370@thelio-3990X/)
* [`Re: [PATCH] acl: Annotate struct posix_acl with __counted_by()`](https://lore.kernel.org/20241002034252.GA2770260@thelio-3990X/)
* [`Re: [PATCH v13 11/40] arm64/gcs: Provide basic EL2 setup to allow GCS usage at EL0 and EL1`](https://lore.kernel.org/20241009204903.GA3353168@thelio-3990X/)
* [`New instance of -Wframe-larger-than with sanitizers enabled after commit d2408c417cfa`](https://github.com/llvm/llvm-project/issues/111903)
* [`Re: [PATCH] HID: simplify code in fetch_item()`](https://lore.kernel.org/20241010222451.GA3571761@thelio-3990X/)
* [`Re: [PATCH v5 6/8] x86/module: perpare module loading for ROX allocations of text`](https://lore.kernel.org/20241010225411.GA922684@thelio-3990X/)
* [`Re: [PATCH v9 02/10] um: use execveat to create userspace MMs`](https://lore.kernel.org/20241016023426.GA1115452@thelio-3990X/)
* [`Re: [PATCH v6 6/8] x86/module: prepare module loading for ROX allocations of text`](https://lore.kernel.org/20241021221519.GA3567210@thelio-3990X/)
* [`Re: [PATCH 1/3] kprobes: Annotate structs with __counted_by()`](https://lore.kernel.org/20241022205557.GA3004519@thelio-3990X/)
* [`[PowerPC] Expand global named register support`](https://github.com/llvm/llvm-project/pull/112603#issuecomment-2430709704)
* [```Re: [uml:next 25/45] InstrProfilingUtil.c:(.text.lprofAtExit+0x1): undefined reference to `atexit'```](https://lore.kernel.org/20241025012108.GB740745@thelio-3990X/)
* [`Re: [weiny2:dcd-v4-2024-10-29 9/28] drivers/cxl/cxlmem.h:755:35: error: use of undeclared identifier 'regions_retunred'`](https://lore.kernel.org/20241030162522.GA2228266@thelio-3990X/)
* [`Re: [PATCH v8 8/9] tmpfs: Expose filesystem features via sysfs`](https://lore.kernel.org/20241031051822.GA2947788@thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`patches: Remove patch disabling CONFIG_BUILTIN_MODULE_RANGES`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/781)
* [`patches: mainline: Add patch to avoid timeout with Hexagon`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/782)
* [`Update DEFAULT_KERNEL_FOR_PGO to 6.11.0 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/279)
* [`tc_build: kernel: Silence new ruff warning (SIM115)`](https://github.com/ClangBuiltLinux/tc-build/pull/280)
* [`patches: Remove iio bmi323 patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/783)
* [`patches: Apply iio bmi323 change to arm64 and arm64-fixes tree`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/784)
* [`patches: arm64: Apply afa9b48f327c9ef36bfba4c643a29385a633252b`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/785)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [19.1.1](https://lore.kernel.org/20241002035002.GA2829204@thelio-3990X/)
  * [19.1.2](https://lore.kernel.org/20241015192630.GA1410876@thelio-3990X/)
  * [19.1.3](https://lore.kernel.org/20241030180500.GA2334578@thelio-3990X/)

* I ran the [October 2nd](https://github.com/ClangBuiltLinux/meeting-notes/pull/58), October 16th, and October 30th ClangBuiltLinux bi-weekly meetings.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
