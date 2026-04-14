---
title: April 2026 ClangBuiltLinux Work
date: 2026-04-30T16:30:00-0700
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

  * `` ([`v1`]())

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * `` ([`v1`]())

* Kbuild / Kconfig fixes and improvements: These are changes that I have authored as part of maintaining Kbuild and Kconfig.

  * `` ([`v1`]())

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but matter in some way to my other work.

  * `riscv: Define __riscv_copy_{,vec_}{words,bytes}_unaligned() using SYM_TYPED_FUNC_START` ([`v1`](https://lore.kernel.org/20260406-measure_cycles-cfi-failure-v1-1-03e0234ae02f@kernel.org/))
  * `` ([`v1`]())

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `drm/amd/display: Do not add '-mhard-float' to calcs, dsc, and dcn30 FP files for clang` ([`v1`](https://lore.kernel.org/20260406-5-10-clang-amdgpu-hard-float-errors-v1-1-09c4c045f848@kernel.org/))
  * `` ([`v1`]())

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `` ([`v1`]())



## LLVM patches

* [``]()



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: (subset) [PATCH] modpost: Declare extra_warn with unused attribute`](https://lore.kernel.org/177508467966.1735993.3178343195907981949.b4-ty@b4/)
* [`Re: [PATCH v2 0/2] selftests/mm: clean up build output and verbosity`](https://lore.kernel.org/20260406193216.GA1319599@ax162/)
* [`Re: [PATCH] gcov: use atomic counter updates to fix concurrent access crashes`](https://lore.kernel.org/20260406193707.GB1319599@ax162/)
* [`Re: [PATCH v2] kbuild: rust: add AutoFDO support`](https://lore.kernel.org/20260407164526.GA2309039@ax162/)
* [`Re: [PATCH] crypto: ecc - Unbreak the build on arm with CONFIG_KASAN_STACK=y`](https://lore.kernel.org/20260408205746.GA2877926@ax162/)
* [`Re: [PATCH v2] gcov: Disable GCOV_PROFILE_ALL on 32-bit UML with Clang 20/21`](https://lore.kernel.org/20260408220334.GA3962465@ax162/)
* [`Re: [PATCH] kbuild: builddeb - avoid recompiles for non-cross-compiles`](https://lore.kernel.org/20260408221301.GB3963285@ax162/)
* [`build: use user-supplied CROSS_COMPILE in compiler check`](https://github.com/kernelci/tuxmake/pull/266#issuecomment-4210200494)
* [`clang hangs when building Linux kernel's rkvdec-vdpu383-h264.c for ARCH=hexagon`](https://github.com/llvm/llvm-project/issues/178535#issuecomment-4210183920)
* [`Re: [PATCH v3] gcov: Disable GCOV_PROFILE_ALL on 32-bit UML with Clang 20/21`](https://lore.kernel.org/20260409182620.GA2550473@ax162/)
* [`Re: [patch 01/12] clockevents: Prevent timer interrupt starvation`](https://lore.kernel.org/20260410211310.GA3924786@ax162/)
* [``](https://lore.kernel.org/)
* [``]()



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH] KVM: arm64: Advertise ID_AA64PFR2_EL1.GCIE`](https://lore.kernel.org/20260404181330.GA3987102@ax162/)
* [`Manually specified CROSS_COMPILE no longer works with null runtime`](https://github.com/kernelci/tuxmake/issues/265)
* [`Reapply "[clang][ModulesDriver] Add support for Clang modules to -fmodules-driver"`](https://github.com/llvm/llvm-project/pull/191258#issuecomment-4227294864)
* [`Re: [patch 01/12] clockevents: Prevent timer interrupt starvation`](https://lore.kernel.org/20260410205203.GA3922321@ax162/)
* [`Re: [PATCH 5.15 000/570] 5.15.203-rc1 review`](https://lore.kernel.org/20260413185112.GA501334@ax162/)
* [``](https://lore.kernel.org/)
* [``]()



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update korg-clang-22 to 22.1.3`](https://github.com/kernelci/tuxmake/pull/264)
* [`workflows: python_lint: Add ability to run ty`](https://github.com/ClangBuiltLinux/actions-workflows/pull/12)
* [`workflows: python_lint: Bump setup-uv to v8.0.0`](https://github.com/ClangBuiltLinux/actions-workflows/pull/13)
* [`boot-utils: Add type annotations and linting with ty`](https://github.com/ClangBuiltLinux/boot-utils/pull/130)
* [`A couple of m68k fixes`](https://github.com/ClangBuiltLinux/boot-utils/pull/131)
* [`tc-build: Add typing annotations and enable ty`](https://github.com/ClangBuiltLinux/tc-build/pull/326)
* [`tc-build: ci: Show verbose kernel build logs`](https://github.com/ClangBuiltLinux/tc-build/pull/327)
* [`Bump PGO kernel to 7.0 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/328)
* [`tc-build: Enable more Ruff lints`](https://github.com/ClangBuiltLinux/tc-build/pull/329)
* [`boot-utils: Enable and fix several Ruff rules`](https://github.com/ClangBuiltLinux/boot-utils/pull/132)
* [`Update stable anchor to 7.0`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/926)
* [``]()



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [22.1.3](https://lore.kernel.org/20260408001057.GA3264248@ax162/)

* I submitted the following pull requests.

  * [`[GIT PULL] Kbuild fixes for 7.0 #4`](https://lore.kernel.org/20260409231842.GA3337002@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
