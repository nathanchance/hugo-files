---
title: December 2024 ClangBuiltLinux Work
date: 2024-12-31T17:30:00-0700
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

  * `blk-iocost: Avoid using clamp() on inuse in __propagate_weights()` ([`v1`](https://lore.kernel.org/20241212-blk-iocost-fix-clamp-error-v1-1-b925491bc7d3@kernel.org/))
  * `crypto: qce - revert "use __free() for a buffer that's always freed"` ([`v1`](https://lore.kernel.org/20241218-crypto-qce-sha-fix-clang-cleanup-error-v1-1-7e6c6dcca345@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `x86/kexec: Fix location of relocate_kernel with -ffunction-sections` ([`v1`](https://lore.kernel.org/20241213-kexec-fix-section-name-for-ffunction-sections-v1-1-1ae6050f6a15@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `powerpc/vdso: Drop -mstack-protector-guard flags in 32-bit files with clang` ([`6.6`](https://lore.kernel.org/20241206220926.2099603-1-nathan@kernel.org/), [`6.1`](https://lore.kernel.org/20241206221005.2313691-1-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `drm/amd/display: Disable -Wenum-float-conversion for dml2_dpmm_dcn4.c` ([`v1`](https://lore.kernel.org/20241219-amdgpu-fix-enum-float-conversion-again-again-v1-1-ef2c619724b1@kernel.org/))
  * `drm/amd/display: Increase sanitizer frame larger than limit when compile testing with clang` ([`v1`](https://lore.kernel.org/20241219-amdgpu-dml2-address-clang-frame-larger-than-allconfig-v1-1-8c53a644d486@kernel.org/))



## LLVM patches

* [`[Sema] Fix tautological bounds check warning with -fwrapv`](https://github.com/llvm/llvm-project/pull/120480)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] mm: vmstat.h: Annotate operations between enums`](https://lore.kernel.org/20241202161811.GA2843691@thelio-3990X/)
* [`Re: [PATCH 3/3] media: mediatek: vcodec: Workaround a compiler warning`](https://lore.kernel.org/20241202162454.GA2848026@thelio-3990X/)
* [`Re: [PATCH 09/11] x86: rework CONFIG_GENERIC_CPU compiler flags`](https://lore.kernel.org/20241204170935.GB3356373@thelio-3990X/)
* [`Re: [PATCH] interconnect: qcom: icc-rpm: Set the count member before accessing the flex array`](https://lore.kernel.org/20241204173324.GA915644@thelio-3990X/)
* [`Re: [PATCH] kbuild: suppress stdout from merge_config for silent builds`](https://lore.kernel.org/20241208173119.GA3365428@ax162/)
* [`Re: [PATCHv3] gcc: disable '-Wstrignop-overread' universally for gcc-13+ and FORTIFY_SOURCE`](https://lore.kernel.org/20241209193558.GA1597021@ax162/)
* [`Re: [PATCH v2] kbuild: suppress stdout from merge_config for silent builds`](https://lore.kernel.org/20241210192133.GA923495@ax162/)
* [`Re: [PATCH 2/2] Propeller: Remove the architecture specific config`](https://lore.kernel.org/20241211230553.GA3654215@ax162/)
* [`Re: [PATCH v2 0/2] objtool: Add option to fail build on vmlinux warnings`](https://lore.kernel.org/20241219221913.GA1259354@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`[SDAG] Don't allow implicit trunc in getConstant()`](https://github.com/llvm/llvm-project/pull/117558#issuecomment-2510208764)
* [`ANDROID: fix ABI-break in struct cgroup_root`](https://android-review.googlesource.com/c/kernel/common/+/3369094/comment/02ca5019_4b3679a2/)
* [`Re: [PATCH 6.12 000/826] 6.12.2-rc1 review`](https://lore.kernel.org/20241204164853.GA3356373@thelio-3990X/)
* [`Hitting AUTOIBRS WARN_ON_ONCE() in init_amd() booting 32-bit kernel under KVM`](https://lore.kernel.org/20241205220604.GA2054199@thelio-3990X/)
* [`Re: [PATCH v3] mm/hugetlb: support FOLL_FORCE|FOLL_WRITE`](https://lore.kernel.org/20241206045019.GA2215843@thelio-3990X/)
* [`Re: [PATCH 6.6 000/676] 6.6.64-rc1 review`](https://lore.kernel.org/20241208024736.GA2495976@ax162/)
* [`Re: [paulmckrcu:dev 47/49] ERROR: modpost: "rcutorture_format_gp_seqs" [kernel/rcu/rcutorture.ko] undefined!`](https://lore.kernel.org/20241210203552.GA4141507@ax162/)
* [`Kernel build: arch/x86/tools/insn_decoder_test: error: malformed line`](https://github.com/ClangBuiltLinux/linux/issues/2060)
* [`Re: [PATCH v5 13/20] x86/kexec: Mark relocate_kernel page as ROX instead of RWX`](https://lore.kernel.org/20241212014418.GA532802@ax162/)
* [`Re: [PATCH v5 07/20] x86/kexec: Invoke copy of relocate_kernel() instead of the original`](https://lore.kernel.org/20241214230818.GA677337@ax162/)
* [`Re: next-20241216: drivers/crypto/qce/sha.c:365:3: error: cannot jump from this goto statement to its label`](https://lore.kernel.org/20241216180231.GA1069997@ax162/)
* [`[Sema] Diagnose tautological bounds checks`](https://github.com/llvm/llvm-project/pull/120222#issuecomment-2551936735)
* [`Re: [PATCH v6 4/4] power: supply: core: add UAPI to discover currently used extensions`](https://lore.kernel.org/20241218195229.GA2796534@ax162/)
* [`Re: [PATCH v6 9/9] drm/amd/display: Mark dc_fixpt_from_fraction() noinline`](https://lore.kernel.org/20241220223403.GA2605890@ax162/)
* [`Re: [PATCH v3 6/6] arm64/mm: Drop configurable 48-bit physical address space limit`](https://lore.kernel.org/20241221002906.GA2558963@ax162/)
* [`Re: vmlinux.o: warning: objtool: do_user_addr_fault+0x1052: __stack_chk_fail() is missing a __noreturn annotation`](https://lore.kernel.org/20241231210559.GA534233@ax162/)
* [`Re: [PATCH v4 0/7] x86: Rid .head.text of all abs references`](https://lore.kernel.org/20250101024348.GA1828419@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Enable sparc64 build on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/791)
* [`patches: mainline: Add patch to avoid tnt4882.ko modpost error`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/792)
* [`patches: arm64-fixes: Drop applied patches after 6.13-rc1 update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/793)
* [`patches: arm64: Drop patches available in 6.13-rc2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/794)
* [`patches: mainline: Drop merged gpib patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/795)
* [`Drop 4.19`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/797)
* [`patches: mainline: Drop Hexagon constant extender patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/798)
* [`Update patch metadata (December 20, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/799)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [19.1.5](https://lore.kernel.org/20241204000652.GA1999416@thelio-3990X/)
  * [19.1.6](https://lore.kernel.org/20241217212035.GA2842889@ax162/)

* As there was some workload slowdown because of the holidays on both the kernel and LLVM side, I took some time to do some maintenance on my personal workflow and environment files to make things easier to maintain over the long term and become more efficient when developing and solving issues.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
