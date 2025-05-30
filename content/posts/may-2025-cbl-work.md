---
title: May 2025 ClangBuiltLinux Work
date: 2025-05-30T16:30:00-0700
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

  * `net: qede: Initialize qede_ll_ops with designated initializer` ([`v1`](https://lore.kernel.org/20250507-qede-fix-clang-randstruct-v1-1-5ccc15626fba@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `genirq: Ensure flags in lock guard is consistently initialized` ([`v1`](https://lore.kernel.org/20250513-irq-guards-fix-flags-init-v1-1-1dca3f5992d6@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Series for -Wunterminated-string-initialization in 6.14 and 6.12`](https://lore.kernel.org/20250523205408.GA863786@ax162/)
  * [`Backports of d0afcfeb9e3810ec89d1ffde1a0e36621bb75dca for 6.6 and older`](https://lore.kernel.org/20250523211710.GA873401@ax162/)
  * [`Backports of 2e43ae7dd71cd9bb0d1bce1d3306bf77523feb81 for 5.15 and older`](https://lore.kernel.org/20250527235023.GA2613123@ax162/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `kbuild: Disable -Wdefault-const-init-unsafe` ([`v1`](https://lore.kernel.org/20250501-default-const-init-clang-v1-0-3d2c6c185dbb@kernel.org/), [`v2`](https://lore.kernel.org/20250506-default-const-init-clang-v2-1-fcfb69703264@kernel.org/))
  * `mfd: Remove node variables that are unused with CONFIG_OF=n` ([`v1`](https://lore.kernel.org/20250508-mfd-fix-unused-node-variables-v1-1-df84d80cca55@kernel.org/))
  * `platform: mellanox: nvsw-sn2200: Fix .items in nvsw_sn2201_busbar_hotplug` ([`v1`](https://lore.kernel.org/20250509-nvsw-sn2200-fix-items-busbar-hotplug-v1-1-8844fff38dc8@kernel.org/))
  * `cpufreq/amd-pstate: Avoid shadowing ret in amd_pstate_ut_check_driver()` ([`v1`](https://lore.kernel.org/20250512-amd-pstate-ut-uninit-ret-v1-1-fcb4104f502e@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`[C] Allow __attribute__((nonstring)) on multidimensional arrays`](https://github.com/llvm/llvm-project/pull/138133#pullrequestreview-2810602000)
* [`Re: [PATCH 2/3] randstruct: Force full rebuild when seed changes`](https://lore.kernel.org/20250502161209.GA2850065@ax162/)
* [`Re: [PATCH] arm64/cpufeature: annotate arm64_use_ng_mappings with ro_after_init to prevent wrong idmap generation`](https://lore.kernel.org/20250502162540.GB2850065@ax162/)
* [`Re: [PATCH] extrawarn: Use cc-disable-warning for shift-negative-value`](https://lore.kernel.org/20250508163138.GA834338@ax162/)
* [`Re: [PATCH] Makefile.kcov: apply needed compiler option unconditionally in CFLAGS_KCOV`](https://lore.kernel.org/20250508164425.GD834338@ax162/)
* [`Re: Patch "x86/relocs: Handle R_X86_64_REX_GOTPCRELX relocations" has been added to the 6.14-stable tree`](https://lore.kernel.org/20250522230101.GA1911411@ax162/)
* [`Re: [PATCH] scripts: add zboot support to extract-vmlinux`](https://lore.kernel.org/20250522231009.GA2020750@ax162/)
* [`Re: [PATCH] kbuild: replace deprecated T option with --thin for $(AR)`](https://lore.kernel.org/20250527221637.GA2566504@ax162/)
* [`Re: [PATCH] ubsan: integer-overflow: depend on BROKEN to keep this out of CI`](https://lore.kernel.org/20250528213223.GA3885532@ax162/)
* [`Re: [PATCH] arm64: Disable LLD linker ASSERT()s for the time being`](https://lore.kernel.org/20250530023048.GA1491122@ax162/)
* [`RISC-V boot warnings and crash after LLVM commit bb03cdcb441fd68da9d1ebb7d5f39f73667cd39c`](https://github.com/ClangBuiltLinux/linux/issues/2091#issuecomment-2919977203)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`arm64 Linux kernel boot failure after b326cb6792b3951881d63d5a02ea163921da18d9`](https://github.com/llvm/llvm-project/issues/138019#issuecomment-2845510757)
* [`Re: error: arch/x86/include/asm/mshyperv.h:108:2: ran out of registers during register allocation in function 'hyperv_flush_guest_mapping'`](https://lore.kernel.org/20250507022649.GA1984217@ax162/)
* [`Re: [linux-next:master 3551/8679] arch/riscv/kernel/compat_signal.c:220:28: error: use of undeclared identifier 'compat__vdso_rt_sigreturn_offset'`](https://lore.kernel.org/20250508163314.GB834338@ax162/)
* [`Re: [linux-next:master 5904/8679] <instantiation>:7:11: error: expected an immediate`](https://lore.kernel.org/20250508164308.GC834338@ax162/)
* [`Re: [ardb:for-kernelci 15/18] arch/x86/boot/startup/sev-startup.c:141:15: error: '__section__' attribute only applies to functions, global variables, Objective-C methods, and Objective-C properties`](https://lore.kernel.org/20250508165710.GA2787919@ax162/)
* [`RISC-V boot warnings and crash after LLVM commit bb03cdcb441fd68da9d1ebb7d5f39f73667cd39c`](https://github.com/ClangBuiltLinux/linux/issues/2091)
* [`Assertion failed: "start of copy chain MUST be COPY" on RISC-V after using asm goto with outputs`](https://github.com/ClangBuiltLinux/linux/issues/2092)
* [`llvm-readelf "invalid PT_DYNAMIC size" for RISC-V after getrandom() vDSO implementation`](https://github.com/ClangBuiltLinux/linux/issues/2093)
* [`Failed assertion "Cannot export BSS symbol" with ld.lld`](https://github.com/ClangBuiltLinux/linux/issues/2094)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Adjust the base LLVM configuration`](https://github.com/ClangBuiltLinux/tc-build/pull/300)
* [`Initial matrix reduction`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/831)
* [`patches: Drop 5.15, 5.10, and 5.4 (May 2, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/832)
* [`Update patches (May 19, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/833)
* [`Workaround known failures (May 24, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/834)
* [`patches: android-mainline: Drop patches present in 6.15 final`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/835)
* [`Address more build failures (May 28, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/836)
* [`Update stable anchor to 6.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/837)
* [`patches: mainline: Drop d8720235d5b5cad86c1f07f65117ef2a96f8bec7`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/838)
* [`Various fixes for PGO with older LLVM versions`](https://github.com/ClangBuiltLinux/tc-build/pull/301)
* [`patches: 6.12: Drop -Wunterminated-string-initialization patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/839)
* [`Update PGO kernel to 6.15 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/302)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [20.1.5](https://lore.kernel.org/20250519175752.GA1041767@ax162/)
  * [20.1.6](https://lore.kernel.org/20250529002509.GA161456@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
