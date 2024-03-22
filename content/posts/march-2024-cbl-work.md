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
  * `hexagon: vmlinux.lds.S: Handle attributes section` ([`v1`](https://lore.kernel.org/20240319-hexagon-handle-attributes-section-vmlinux-lds-s-v1-1-59855dab8872@kernel.org/))
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
  * `clk: qcom: Fix two driver Kconfig dependencies` ([`v1`](https://lore.kernel.org/20240318-fix-some-qcom-kconfig-deps-v1-0-ea0773e3df5a@kernel.org/))
  * `tracing: Fully silence instance of -Wstring-compare` ([`v1`](https://lore.kernel.org/20240319-tracing-fully-silence-wstring-compare-v1-0-81adb44403f5@kernel.org/))
  * `` ([`v1`]())



## LLVM patches

* [``]()



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kbuild: remove GCC's default -Wpacked-bitfield-compat flag`](https://lore.kernel.org/20240306164950.GB3659677@dev-arch.thelio-3990X/)
* [`Re: [PATCH] printk: fix _entry_ptr build warning`](https://lore.kernel.org/20240306194020.GA3711543@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ssb: use "break" on default case to prevent warning`](https://lore.kernel.org/20240313153236.GA2931742@dev-arch.thelio-3990X/)
* [`Re: [PATCH] tracing: Use strcmp() in __assign_str() WARN_ON() check`](https://lore.kernel.org/20240313165903.GA3021536@dev-arch.thelio-3990X/)
* [`Re: [PATCH 5.15 45/76] modpost: Add '.ltext' and '.ltext.*' to TEXT_SECTIONS`](https://lore.kernel.org/20240313171229.GA3064248@dev-arch.thelio-3990X/)
* [`Re: [PATCH] memtest: use {READ,WRITE}_ONCE in memory scanning`](https://lore.kernel.org/20240313172141.GB3064248@dev-arch.thelio-3990X/)
* [`Re: [PATCH-next] arm: fix clang build warning in include/asm/memory.h`](https://lore.kernel.org/all/20240315004352.GA768888@dev-arch.thelio-3990X/)
* [`Re: [PATCH V2] kbuild: rpm-pkg: add dtb files in kernel rpm`](https://lore.kernel.org/20240315190021.GA721491@dev-arch.thelio-3990X/)
* [`Re: [tip: x86/percpu] x86/percpu: Convert this_percpu_xchg_op() from asm() to C code, to generate better code`](https://lore.kernel.org/20240320173758.GA3017166@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scripts/package: buildtar: Output as vmlinuz for riscv`](https://lore.kernel.org/20240321154320.GA616931@dev-arch.thelio-3990X/)
* [`Split -Wcast-function-type into a separate group`](https://github.com/llvm/llvm-project/pull/86131#issuecomment-2012821677)
* [``]()



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Allow using Hexagon with kernel.org LLVM`](https://gitlab.com/Linaro/tuxmake/-/issues/216)
* [`[MachineFrameInfo] Refactoring with computeMaxcallFrameSize() (NFC)`](https://github.com/llvm/llvm-project/pull/78001)
* [`Turn 'counted_by' into a type attribute and parse it into 'CountAttributedType'`](https://github.com/llvm/llvm-project/pull/78000#issuecomment-2010269493)
* [``]()



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`generator: workflow: Use setup-python action in check cache stage`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/720)
* [`Fix Python dependency installation in clang-nightly containers`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/721)
* [`Switch s390 builds on -next with LLVM 18 over to LLVM=1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/722)
* [`Update patches (March 5, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/723)
* [`patches: Drop 6.1 AMDGPU patch from upstream`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/724)
* [`Fix clang-android builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/725)
* [`tc_build: tools: Update location of LLVM_VERSION_MAJOR`](https://github.com/ClangBuiltLinux/tc-build/pull/263)
* [`Switch s390 builds on mainline with LLVM 18+ over to LLVM=1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/727)
* [`Update stable anchor to 6.8`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/728)
* [`Update patches (March 15, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/730)
* [`patches: mainline: Drop powerpc '-mhard-float' patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/731)
* [`Disable MIPS builds that use GNU ld`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/734)
* [`Add patches to fix performing PGO with current LLVM main`](https://github.com/ClangBuiltLinux/tc-build/pull/264)
* [`Update korg-clang-18 to 18.1.2`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/376)
* [`patches: mainline: Drop -Wenum-enum-conversion patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/735)
* [``]()



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I ran the [March 6th](https://github.com/ClangBuiltLinux/meeting-notes/pull/56) and [March 20th](https://github.com/ClangBuiltLinux/meeting-notes/pull/57) ClangBuiltLinux meetings.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [18.1.0](https://lore.kernel.org/20240307000841.GA3806611@dev-arch.thelio-3990X/)
  * [18.1.1](https://lore.kernel.org/20240315142607.GA73955@dev-arch.thelio-3990X/)
  * [18.1.2](https://lore.kernel.org/20240320155007.GA12384@dev-arch.thelio-3990X/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
