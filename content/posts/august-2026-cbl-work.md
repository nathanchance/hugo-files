---
title: August 2026 ClangBuiltLinux Work
date: 2026-08-31T16:30:00-0700
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

  * `thermal/drivers/qcom-spmi-mbg-tm: Add module namespace import for IIO_CONSUMER` ([`v1`](https://lore.kernel.org/20260812-qcom-spmi-mbg-tm-ns-modpost-error-v1-1-d849390d2714@kernel.org/))
  * `net: macb: Move macb_{alloc,free}_tieoff() out of CONFIG_OF block` ([`v1`](https://lore.kernel.org/20260818-macb-fix-no-of-build-v1-1-f2a009616384@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but matter in some way to my other work.

  * `ARM: Fix get_cycles() after delay_read_timer() conversion` ([`v1`](https://lore.kernel.org/20260819-fix-arm-get_cycles-v1-1-208bf07ac540@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Backports of c1f3e770eec26d6f96dd6d2ea30555ba7c09a244 for 6.6 and 6.1`](https://lore.kernel.org/20260814025208.GA1929807@ax162/)
  * [`[PATCH 6.12 0/2] Backport of 6ee149f61bcce39692f0335a01e99355d4cec8da`](https://lore.kernel.org/20260814052439.430858-1-nathan@kernel.org/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `swim3: Add missing MODULE_DESCRIPTION` ([`v1`](https://lore.kernel.org/20260811-swim3-module-description-v1-1-28398c5a0e32@kernel.org/))
  * `scsi: qla2xxx: Fix size_t format specifier in qla29xx_process_rd_image()` ([`v1`](https://lore.kernel.org/20260811-scsi-qla2xxxx-qla_init-wformat-v1-1-50760021914f@kernel.org/))
  * `arch_numa: Avoid false positive fortify warning in setup_node_to_cpumask_map()` ([`v1`](https://lore.kernel.org/20260811-arch_numa-avoid-fortify-warning-v1-1-59ce3e689f3a@kernel.org/), [`v2`](https://lore.kernel.org/20260813-arch_numa-avoid-fortify-warning-v2-1-093ad97a78df@kernel.org/))
  * `scsi: ibmvfc: Fix use of uninitialized rport in ibmvfc_do_work()` ([`v1`](https://lore.kernel.org/20260817-ibmvscsi-rport-wuninitialized-v1-1-0fdfb27a5f01@kernel.org/))
  * `scripts/sorttable: Mark long_size as __maybe_unused` ([`v1`](https://lore.kernel.org/20260831-sorttable-long_size-unused-but-set-global-v1-1-8a96b88697e5@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] riscv/runtime-const: Disable linker relaxation for RUNTIME_MAGIC`](https://lore.kernel.org/20260803161203.GA953175@ax162/)
* [`Re: [PATCH] Documentation: warn users not to use select on choice options in Kconfig`](https://lore.kernel.org/20260803174736.GA1067866@ax162/)
* [`Re: [PATCH 1/1] scripts: kstack_erase: use relative stackleak plugin path`](https://lore.kernel.org/20260803181217.GB1067866@ax162/)
* [`Re: [PATCH 0/2] alpha: enable building with clang`](https://lore.kernel.org/20260803195135.GA1083357@ax162/)
* [`Re: [PATCH] MAINTAINERS: add Julian Braha as Kconfig reviewer`](https://lore.kernel.org/20260804183042.GA300797@ax162/)
* [`Re: [PATCH] arm: mediatek: fix secondary CPU boot on Thumb-2 kernels with Clang`](https://lore.kernel.org/20260807185259.GA941196@ax162/)
* [`Re: [PATCH v3] tee: remove TZMEM_MODE_GENERIC`](https://lore.kernel.org/20260807190513.GA2638974@ax162/)
* [`Re: [PATCH 0/2] modpost: error logging cleanups`](https://lore.kernel.org/178613777751.32781.14792177228607560956.b4-review@b4/)
* [`Re: [PATCH] kbuild: let the environment set HOSTPKG_CONFIG`](https://lore.kernel.org/178614091702.32781.13796520858880316981.b4-review@b4/)
* [`Re: [PATCH] Bluetooth: hci_sync: add conditional locking annotations`](https://lore.kernel.org/20260811204327.GA1477601@ax162/)
* [`Re: [PATCH v2] soc: qcom: ubwc: Fix link error when QCOM_SMEM=n`](https://lore.kernel.org/20260811223622.GA934543@ax162/)
* [`Re: [PATCH v4 0/3] soc: qcom: ubwc: Fix link error`](https://lore.kernel.org/20260813232350.GA312295@ax162/)
* [`Re: [PATCH 2/2] kbuild: rust: keep Rust objects out of Clang LTO with inline helpers`](https://lore.kernel.org/20260817181212.GA1249844@ax162/)
* [`Re: [PATCH v6 0/4] soc: qcom: ubwc: Fix link error when QCOM_SMEM=n`](https://lore.kernel.org/20260817181743.GB1249844@ax162/)
* [`Re: [PATCH v2] kstack_erase: suppress -grecord-gcc-switches for external module builds`](https://lore.kernel.org/20260817185146.GC1249844@ax162/)
* [`Re: [PATCH] scripts: Make the code use consistent syntax`](https://lore.kernel.org/178707966024.2113250.6051873542503340409.b4-review@b4/)
* [`Re: [PATCH] kconfig: error out for recursive range`](https://lore.kernel.org/178707985863.2113250.4007818694470388435.b4-review@b4/)
* [`Re: [PATCH 1/1] kbuild: record real-prereqs in .cmd files`](https://lore.kernel.org/178708267641.2113250.8208366109143651745.b4-review@b4/)
* [`Re: [PATCH v3] ACPI: scan: Avoid registering platform devices with resource overlaps`](https://lore.kernel.org/20260819194039.GA3686901@ax162/)
* [`Re: [PATCH] kbuild: ubsan: skip UBSAN for external modules by default`](https://lore.kernel.org/20260821190701.GA3030879@ax162/)
* [`Re: [PATCH 12/27] kbuild: Defer running objtool to link time for all CFG features`](https://lore.kernel.org/20260828175708.GA3403925@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`RISC-V kCFI boot hang after LLVM commit 434e4e15f6a3`](https://github.com/ClangBuiltLinux/linux/issues/2170#issuecomment-5173130333)
* [`Re: [PATCH V17 0/7] Rust Support for powerpc`](https://lore.kernel.org/20260804202217.GA1109939@ax162/)
* [`Re: [PATCH 6.12 289/337] mm/slab: prevent unbounded recursion in free path with new kmalloc type`](https://lore.kernel.org/20260807180245.GA4067747@ax162/)
* [`Re: [REGRESSION] mainline/master: (build) in arch/arm/kernel/entry-common.o (/tmp/kci/linux/scripts/Makefile...`](https://lore.kernel.org/20260818165116.GA1335107@ax162/)
* [`Re: [PATCH v3] ACPI: scan: Avoid registering platform devices with resource overlaps`](https://lore.kernel.org/20260819003752.GA3063251@ax162/)
* [`Re: [GIT pull] timers/cleanups for v7.3-rc1`](https://lore.kernel.org/20260819184803.GA3333711@ax162/)
* [`Re: [PATCH v2] fbdev: platinumfb: add error checking for ioremap calls`](https://lore.kernel.org/20260819235918.GA2021182@ax162/)
* [`Add builtin/intrinsic to get current instruction pointer?`](https://github.com/llvm/llvm-project/issues/138272#issuecomment-540476747)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Revert accidental LLVM_IAS=1 enablement for sparc64 with clang-22`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/938)
* [`build-llvm.py: Deduplicate '--targets'`](https://github.com/ClangBuiltLinux/tc-build/pull/348)
* [`build-binutils.py: Add support for sparc64`](https://github.com/ClangBuiltLinux/tc-build/pull/349)
* [`Update clang-nightly to 24 and add clang-23`](https://github.com/kernelci/tuxmake/pull/303)
* [`Update stable anchor to 7.2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/940)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [23.1.0-rc3](https://lore.kernel.org/20260812162742.GA101585@ax162/)
  * [23.1.0](https://lore.kernel.org/20260827190431.GA1817346@ax162/)

* I developed [a solid, working prototype for the third generation of ClangBuiltLinux continuous integration](https://github.com/nathanchance/continuous-integration3) to permit moving to infrastructure that we have full authority over.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
