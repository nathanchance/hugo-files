---
title: February 2025 ClangBuiltLinux Work
date: 2025-02-28T17:30:00-0700
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

  * `x86/build: Raise the minimum LLVM version to 15.0.0` ([`v1`](https://lore.kernel.org/20250220-x86-bump-min-llvm-for-stackp-v1-1-ecb3c906e790@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `ACPI: platform-profile: Fix CFI violation when accessing sysfs files` ([`v3`](https://lore.kernel.org/20250210-acpi-platform_profile-fix-cfi-violation-v3-1-ed9e9901c33a@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `arm64: Handle .ARM.attributes section in linker scripts` ([`v2`](https://lore.kernel.org/20250204-arm64-handle-arm-attributes-in-linker-script-v2-1-d684097f5554@kernel.org/), [`v3`](https://lore.kernel.org/20250206-arm64-handle-arm-attributes-in-linker-script-v3-1-d53d169913eb@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 1/1] alloc_tag: work around clang-14 issue with __builtin_object_size()`](https://lore.kernel.org/20250205195735.GA1444602@ax162/)
* [`Re: [PATCH] kbuild: remove EXTRA_*FLAGS support`](https://lore.kernel.org/20250205203817.GA2060120@ax162/)
* [`Re: [PATCH] gen_compile_commands.py: remove code for '\#' replacement`](https://lore.kernel.org/20250207213439.GA661404@ax162/)
* [`Re: [PATCH 1/2] clang-format: do not allow short enums on a single line`](https://lore.kernel.org/20250211181319.GA2995885@ax162/)
* [`Re: [PATCH 2/2] clang-format: align consecutive macros`](https://lore.kernel.org/20250211181442.GB2995885@ax162/)
* [`Re: [PATCH 1/2] kbuild: userprogs: fix bitsize and target detection on clang`](https://lore.kernel.org/20250213175539.GB2756218@ax162/)
* [`Re: [PATCH 2/2] kbuild: userprogs: use lld to link through clang`](https://lore.kernel.org/20250213175437.GA2756218@ax162/)
* [`Re: [PATCH] kbuild: move -fzero-init-padding-bits=all to the top-level Makefile`](https://lore.kernel.org/20250215203733.GA2954129@ax162/)
* [`Re: [PATCH v2] kbuild: userprogs: use correct lld when linking through clang`](https://lore.kernel.org/20250217185456.GA435791@ax162/)
* [`Boot failure with ppc64_guest_defconfig after LLVM commit 7763119c6eb0976e4836f81c9876c49a36d46d73`](https://github.com/ClangBuiltLinux/linux/issues/2072#issuecomment-2669475194)
* [`Re: [PATCH] kbuild: hdrcheck: fix cross build with clang`](https://lore.kernel.org/20250221212603.GA3561672@ax162/)
* [`Re: [PATCH] [v2] kbuild: hdrcheck: fix cross build with clang`](https://lore.kernel.org/20250224172456.GB585841@ax162/)
* [`Re: [for-next][PATCH 4/6] scripts/sorttable: Zero out weak functions in mcount_loc table`](https://lore.kernel.org/20250225180044.GA3655100@ax162/)
* [`Re: [PATCH] kbuild: deb-pkg: add debarch for ARCH=loongarch64`](https://lore.kernel.org/20250301042551.GA3565873@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: FAIL: Test report for for-kernelci (6.14.0-rc1, upstream-arm-next, 2014c95a)`](https://lore.kernel.org/20250204201019.GA3095838@ax162/)
* [`Re: [PATCH 3/5] lib/crc32: standardize on crc32c() name for Castagnoli CRC32`](https://lore.kernel.org/20250207224233.GA1261167@ax162/)
* [`[PowerPC] Deprecate uses of ISD::ADDC/ISD::ADDE/ISD::SUBC/ISD::SUBE`](https://github.com/llvm/llvm-project/pull/116984#issuecomment-2661045466)
* [`clang-19 built kernel fails several stackinit_kunit tests w. "ASSERTION FAILED at lib/stackinit_kunit.c" vs. same kernel built with gcc-14 passes tests (v6.14-rc3, x86_32)`](https://github.com/ClangBuiltLinux/linux/issues/2071#issuecomment-2667097342)
* [`Boot failure with ppc64_guest_defconfig after LLVM commit 7763119c6eb0976e4836f81c9876c49a36d46d73`](https://github.com/ClangBuiltLinux/linux/issues/2072)
* [`Re: [PATCH 4/5] btrfs: prepare extent_io.c for future larger folio support`](https://lore.kernel.org/20250225184136.GA1679809@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update patches (February 4, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/813)
* [`Move llvm_latest to 20`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/814)
* [`tc_build: llvm: Use runtimes build for LLVM 20 and newer`](https://github.com/ClangBuiltLinux/tc-build/pull/291)
* [`Update patches for acceptance of arm64 .ARM.attributes linker script update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/815)
* [`patches: mainline: Remove ca0f4fe7cf7183bfbdc67ca2de56ae1fc3a8db2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/816)
* [`patches: Drop arm64 due to 6.13-rc3 update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/817)
* [`Drop x86 builds with LLVM 13 and 14 on -next and tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/818)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [20.1.0-rc1](https://lore.kernel.org/20250204180013.GA3940674@ax162/)
  * [20.1.0-rc2](https://lore.kernel.org/20250212172405.GA3066861@ax162/)
  * [20.1.0-rc3](https://lore.kernel.org/20250227025319.GA3607450@ax162/)

* I continue to work on triaging our current list of `objtool` warnings due to the upcoming `CONFIG_OBJTOOL_WERROR`. It is slow going because each warning requires individual analysis, even if they have the same text, and it requires understanding the source code and disassembly enough to try and figure out what optimization LLVM did to introduce the warning.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
