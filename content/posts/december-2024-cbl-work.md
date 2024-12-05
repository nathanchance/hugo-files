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

  * `` ([`v1`]())

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * `` ([`v1`]())

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `` ([`v1`]())

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `` ([`v1`]())

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `` ([`v1`]())



## LLVM patches

* [``]()



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] mm: vmstat.h: Annotate operations between enums`](https://lore.kernel.org/20241202161811.GA2843691@thelio-3990X/)
* [`Re: [PATCH 3/3] media: mediatek: vcodec: Workaround a compiler warning`](https://lore.kernel.org/20241202162454.GA2848026@thelio-3990X/)
* [`Re: [PATCH 09/11] x86: rework CONFIG_GENERIC_CPU compiler flags`](https://lore.kernel.org/20241204170935.GB3356373@thelio-3990X/)
* [`Re: [PATCH] interconnect: qcom: icc-rpm: Set the count member before accessing the flex array`](https://lore.kernel.org/20241204173324.GA915644@thelio-3990X/)
* [``](https://lore.kernel.org/)
* [``]()



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`[SDAG] Don't allow implicit trunc in getConstant()`](https://github.com/llvm/llvm-project/pull/117558#issuecomment-2510208764)
* [`ANDROID: fix ABI-break in struct cgroup_root`](https://android-review.googlesource.com/c/kernel/common/+/3369094/comment/02ca5019_4b3679a2/)
* [`Re: [PATCH 6.12 000/826] 6.12.2-rc1 review`](https://lore.kernel.org/20241204164853.GA3356373@thelio-3990X/)
* [``](https://lore.kernel.org/)
* [``]()



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Enable sparc64 build on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/791)
* [`patches: mainline: Add patch to avoid tnt4882.ko modpost error`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/792)
* [`patches: arm64-fixes: Drop applied patches after 6.13-rc1 update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/793)
* [``]()



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [19.1.5](https://lore.kernel.org/20241204000652.GA1999416@thelio-3990X/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
