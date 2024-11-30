---
title: November 2024 ClangBuiltLinux Work
date: 2024-11-29T17:30:00-0700
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

* Build errors: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `cdx: Fix cdx_mmap_resource() after constifying attr in ->mmap()` ([`v1`](https://lore.kernel.org/20241107-sysfs-const-mmap-fix-cdx-v1-1-2ed9b7cd5f8b@kernel.org/))
  * `apparmor: Add empty statement between label and declaration in profile_transition(()` ([`v1`](https://lore.kernel.org/20241111-apparmor-fix-label-declaration-warning-v1-1-adb64ab6482b@kernel.org/))
  * `staging: gpib: Make GPIB_NI_PCI_ISA depend on HAS_IOPORT` ([`v1`](https://lore.kernel.org/20241123-gpib-tnt4882-depends-on-has_ioport-v1-1-033c58b64751@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `ACPI: platform-profile: Fix CFI violation when accessing sysfs files` ([`v2`](https://lore.kernel.org/20241118-acpi-platform_profile-fix-cfi-violation-v2-1-62ff952804de@kernel.org/))
  * `hexagon: Disable constant extender optimization for LLVM prior to 19.1.0` ([`v2`](https://lore.kernel.org/20241121-hexagon-disable-constant-expander-pass-v2-1-1a92e9afb0f4@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`[PATCH 5.15] ACPI: PRM: Clean up guid type in struct prm_handler_info`](https://lore.kernel.org/20241111143730.845068-3-nathan@kernel.org/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `irqchip/mips-gic: Fix selection of GENERIC_IRQ_EFFECTIVE_AFF_MASK` ([`v1`](https://lore.kernel.org/20241101-mips-fix-generic_irq_effective_aff_mask-select-v1-1-d94db6e0de0d@kernel.org/))
  * `Input: ads7846 - Increase xfer array size in 'struct ser_req'` ([`v1`](https://lore.kernel.org/20241111-input-ads7846-increase-xfer-array-size-v1-1-06cd92e9f20f@kernel.org/))
  * `LoongArch: KVM: Ensure ret is always initialized in kvm_eiointc_{read,write}()` ([`v1`](https://lore.kernel.org/20241112-loongarch-kvm-eiointc-fix-sometimes-uninitialized-v1-1-7d881f728d67@kernel.org/))
  * `iommu/arm-smmu-v3: Import IOMMUFD module namespace` ([`v1`](https://lore.kernel.org/20241114-arm-smmu-v3-import-iommufd-module-ns-v1-1-c551e7b972e9@kernel.org/))
  * `ubifs: Fix uninitialized use of err in ubifs_jnl_write_inode()` ([`v1`](https://lore.kernel.org/20241115-ubifs-fix-uninitialized-err-v1-1-446849982e27@kernel.org/))
  * `riscv: Always inline bitops` ([`v1`](https://lore.kernel.org/20241123-riscv-always-inline-bitops-v1-1-00e8262ab1cf@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v7 6/8] x86/module: prepare module loading for ROX allocations of text`](https://lore.kernel.org/20241105190427.GA2903209@thelio-3990X/)
* [`Re: [PATCH v5 01/16] x86/stackprotector: Work around strict Clang TLS symbol requirements`](https://lore.kernel.org/20241105193054.GA927577@thelio-3990X/)
* [`Re: [PATCH] powerpc/Makefile: Allow overriding CPP`](https://lore.kernel.org/20241107175211.GA1277020@thelio-3990X/)
* [`Re: [PATCH v7 7/7] Add Propeller configuration for kernel build`](https://lore.kernel.org/20241107204504.GA3432398@thelio-3990X/)
* [`Re: [PATCH] kbuild: Fix Propeller build option`](https://lore.kernel.org/20241108174638.GC2564051@thelio-3990X/)
* [`Re: [PATCH v2] kbuild: Fix Propeller build option`](https://lore.kernel.org/20241109025647.GA1182230@thelio-3990X/)
* [`Re: [PATCH] ARM: cfi: Fix compilation corner case`](https://lore.kernel.org/20241109043138.GA1649953@thelio-3990X/)
* [`Re: allmodconfig link failure on -next (relocation R_HEX_B22_PCREL out of range)`](https://lore.kernel.org/20241113182523.GA2701299@thelio-3990X/)
* [`Re: [PATCH 1/2] scripts/min-tool-version.sh: Raise minimum clang version to 19.1.0 for s390`](https://lore.kernel.org/20241113182109.GA3713382@thelio-3990X/)
* [`Re: [PATCH 2/2] s390/fpu: Remove inline assembly variants for old clang versions`](https://lore.kernel.org/20241113182254.GB3713382@thelio-3990X/)
* [`Re: [PATCH 4/6] block: add a rq_list type`](https://lore.kernel.org/20241114201103.GA2036469@thelio-3990X/)
* [`Re: [PATCH] bcachefs: initialize local variables in bch2_evacuate_bucket`](https://lore.kernel.org/20241118010040.GB3588455@thelio-3990X/)
* [`[llvm] Fix ObjectSizeOffsetVisitor behavior in exact mode upon negative offset`](https://github.com/llvm/llvm-project/pull/116955#issuecomment-2488644597)
* [`Re: [PATCH v2 0/4] Enable measuring the kernel's Source-based Code Coverage and MC/DC with Clang`](https://lore.kernel.org/20241123043922.GA584876@thelio-3990X/)
* [`Re: [PATCH 2/2] string: retire bcmp()`](https://lore.kernel.org/20241123190922.GA3314432@thelio-3990X/)
* [`Re: [PATCH 1/9] objtool: Generic annotation infrastructure`](https://lore.kernel.org/20241124031640.GA3646332@thelio-3990X/)
* [`Re: [PATCH RFC] kbuild: disable -Wc23-extensions from clang`](https://lore.kernel.org/20241125150700.GB2067874@thelio-3990X/)
* [`Re: [PATCH] kconfig: prefer toolchain default for debug information choice`](https://lore.kernel.org/20241125145251.GA2067874@thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [```[SelectionDAG] Add preliminary plumbing for `samesign` flag```](https://github.com/llvm/llvm-project/commit/19c8475871faee5ebb06281034872a85a2552675#commitcomment-148634626)
* [`Reapply [APInt] Enable APInt ctor assertion by default`](https://github.com/llvm/llvm-project/pull/114539#issuecomment-2453008679)
* [`[CodeGen] Use first EHLabel as a stop gate for live range shrinking`](https://github.com/llvm/llvm-project/pull/114195#issuecomment-2455173554)
* [`Re: [PATCH v7 6/8] x86/module: prepare module loading for ROX allocations of text`](https://lore.kernel.org/20241104232741.GA3843610@thelio-3990X/)
* [`Re: [PATCH v3 sched_ext/for-6.13] sched_ext: Do not enable LLC/NUMA optimizations when domains overlap`](https://lore.kernel.org/20241108181753.GA2681424@thelio-3990X/)
* [`sh_entsize is different between GNU ld and ld.lld after linking`](https://github.com/ClangBuiltLinux/linux/issues/2057)
* [`allmodconfig link failure on -next (relocation R_HEX_B22_PCREL out of range)`](https://lore.kernel.org/20241113041319.GA158543@thelio-3990X/)
* [`Re: [PATCH v4 28/33] netfs: Change the read result collector to only use one work item`](https://lore.kernel.org/20241114163931.GA1928968@thelio-3990X/)
* [`Re: [PATCH 6.11 066/184] media: dvbdev: prevent the risk of out of memory access`](https://lore.kernel.org/20241118003338.GA3311143@thelio-3990X/)
* [`Re: [GIT PULL] bcachefs`](https://lore.kernel.org/20241118005434.GA3588455@thelio-3990X/)
* [`Re: [dhowells-fs:rxrpc-iothread 54/64] net/rxrpc/input.c:902:3: error: invalid operand in inline asm: ' shr${1:z} $1`](https://lore.kernel.org/20241118142714.GA1941815@thelio-3990X/)
* [```Re: [PATCH 2/3] kbuild: enable objtool for *.mod.o and additional kernel objects```](https://lore.kernel.org/20241119022730.GA2908286@thelio-3990X/)
* [`Re: DEFINE_FLEX_test: EXPECTATION FAILED at lib/overflow_kunit.c:1200:`](https://lore.kernel.org/20241119150516.GB2196859@thelio-3990X/)
* [`[llvm] Fix behavior of llvm.objectsize in presence of negative / large offset`](https://github.com/llvm/llvm-project/pull/115504#issuecomment-2487257617)
* [`Re: pc : qnoc_probe (drivers/interconnect/qcom/icc-rpmh.c:269) : Dragonboard 410c - arm64 - boot failed`](https://lore.kernel.org/20241125154235.GC2067874@thelio-3990X/)
* [`Re: korg-clang-19-lkftconfig-hardening: TI x15 board - PC is at edma_probe (drivers/dma/ti/edma.c`](https://lore.kernel.org/20241125162354.GD2067874@thelio-3990X/)
* [`[SDAG] Don't allow implicit trunc in getConstant()`](https://github.com/llvm/llvm-project/pull/117558#issuecomment-2502138759)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`runtime: docker: Enable SPARC for clang-nightly`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/456)
* [`patches: Apply fix for -Wframe-larger-than in vdec_vp9_req_lat_if.c`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/787)
* [`Add support for sparc64`](https://github.com/ClangBuiltLinux/boot-utils/pull/122)
* [`Add initial support for testing sparc64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/788)
* [`Update stable to 6.12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/789)
* [`tc_build: llvm: Add Sparc to default targets`](https://github.com/ClangBuiltLinux/tc-build/pull/283)
* [`Update patches (November 22, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/790)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I hosted [a Linux Foundation webinar](https://www.linuxfoundation.org/webinars/using-clang-and-llvm-to-build-the-linux-kernel?hsLang=en) on building the Linux kernel with LLVM.

* I ran the November 13th ClangBuiltLinux bi-weekly meeting.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
