---
title: March 2024 ClangBuiltLinux Work
date: 2024-03-29T16:30:00-0700
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

  * `ALSA: hwdep: Move put_user() call out of scoped_guard() in snd_hwdep_control_ioctl()` ([`v1`](https://lore.kernel.org/20240301-fix-snd-hwdep-guard-v1-1-6aab033f3f83@kernel.org/))
  * `` ([`v1`]())

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * `` ([`v1`]())

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `` ([`v1`]())

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `` ([`v1`]())

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `PCI: imx6: Fix clang -Wimplicit-fallthrough in imx6_pcie_probe()` ([`v1`](https://lore.kernel.org/20240301-pci-imx6-fix-clang-implicit-fallthrough-v1-1-db78c7cbb384@kernel.org/))
  * `kbuild: Move -Wenum-{compare-conditional,enum-conversion} into W=1` ([`v1`](https://lore.kernel.org/20240305-disable-extra-clang-enum-warnings-v1-1-6a93ef3d35ff@kernel.org/), [`v2`](https://lore.kernel.org/20240305-disable-extra-clang-enum-warnings-v2-1-ba529ec15f95@kernel.org/))
  * `media: mxl5xx: Move xpt structures off stack` ([`v2`](https://lore.kernel.org/20240305-dvb-mxl5xx-move-structs-off-stack-v2-1-6903844aba3c@kernel.org/))
  * `` ([`v1`]())



## LLVM patches

* [``]()



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [``]()



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Allow using Hexagon with kernel.org LLVM`](https://gitlab.com/Linaro/tuxmake/-/issues/216)
* [``]()



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`generator: workflow: Use setup-python action in check cache stage`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/720)
* [`Fix Python dependency installation in clang-nightly containers`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/721)
* [`Switch s390 builds on -next with LLVM 18 over to LLVM=1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/722)
* [`Update patches (March 5, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/723)
* [``]()



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
