---
title: "June 2022 ClangBuiltLinux Work"
date: 2022-06-30T16:25:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Android changes: Android is one of the largest downstream consumers of our work. Our continuous integeration tests the Android trees with newer versions of LLVM to help catch any issues that will impact Android once they upgrade their version of LLVM, which can lag behind at times.

  * [`ANDROID: Incremental fs: Use ERR_CAST in handle_mapped_file()`](https://android-review.googlesource.com/c/kernel/common/+/2134978)

* Build errors: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `ntb_perf: Fix 64-bit division on 32-bit architectures` ([`v1`](https://lore.kernel.org/20220622174326.1832697-1-nathan@kernel.org/))
  * `arm64: vdso32: Small fixes for ld.lld 11 and CONFIG_DEBUG_INFO` ([`v1`](https://lore.kernel.org/20220630153121.1317045-1-nathan@kernel.org/))

* Miscellaneous fixes: These are fixes that don't fit into a particular category but are important to ClangBuiltLinux. In this case, I noticed that Chimera Linux was carrying a patch downstream to enable the stackprotector on x86_64 when cross compiling, which typically means the `--target` flag is missing somewhere, which I fixed with this patch.

  * `x86/Kconfig: Fix CONFIG_CC_HAS_SANE_STACKPROTECTOR when cross compiling with clang` ([`v1`](https://lore.kernel.org/20220617180845.2788442-1-nathan@kernel.org/))

* Stable patches and backport requests: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply 1e70212e031528918066a631c9fdccda93a1ffaa to 5.18`](https://lore.kernel.org/Yrnvm6mwtaiKu6Rj@dev-arch.thelio-3990X)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `mmc: sdhci-brcmstb: Initialize base_clk to NULL in sdhci_brcmstb_probe()` ([`v1`](https://lore.kernel.org/20220608152757.82529-1-nathan@kernel.org/))
  * `soc: mediatek: SVS: Use DEFINE_SIMPLE_DEV_PM_OPS for svs_pm_ops` ([`v1`](https://lore.kernel.org/20220622175649.1856337-1-nathan@kernel.org/))
  * `drm/amd/display: Fix indentation in dcn32_get_vco_frequency_from_reg()` ([`v1`](https://lore.kernel.org/20220623153001.3136739-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] netfilter: conntrack: fix this -Wformat clang warning:`](https://lore.kernel.org/Yp9uRz40J24vWLSb@dev-arch.thelio-3990X/)
* [`Re: [PATCH] rtw88: 8821c: fix access const table of channel parameters`](https://lore.kernel.org/YqAQTJqOTB2+242p@dev-arch.thelio-3990X/)
* [`Re: [PATCH] s390: disable -Warray-bounds`](https://lore.kernel.org/YqIJ1iimwIlFDa4n@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3] include/uapi/linux/swab.h: move explicit cast outside ternary`](https://lore.kernel.org/YqJYrImC3Bk40H1H@dev-arch.thelio-3990X/)
* [`Re: [PATCH] KVM: SVM: Hide SEV migration lockdep goo behind CONFIG_DEBUG_LOCK_ALLOC`](https://lore.kernel.org/Yqexcdad6oghl8sv@dev-arch.thelio-3990X/)
* [`Re: [PATCH] hinic: Replace memcpy() with direct assignment`](https://lore.kernel.org/YqtmIIAOH7uRNAZ5@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Documentation/llvm: Update Supported Arch table`](https://lore.kernel.org/Yqy0EkraT0O52Na7@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Fix use of uninitialized variable in rts5261_init_from_hw, when efuse_valid == 1`](https://lore.kernel.org/YqzKrdI0JBORlptt@dev-arch.thelio-3990X/)
* [`Re: [PATCH 16/31] drm/amd/display: refactor function transmitter_to_phy_id`](https://lore.kernel.org/YqzbVxByDw1xSdXb@dev-arch.thelio-3990X/)
* [`Re: [PATCH] [RFC] Kbuild: change CONFIG_FRAME_WARN for 32-bit`](https://lore.kernel.org/Yqzzuk+Wv5q1zIKm@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scripts/Makefile.clang: set --target for host based on make -v`](https://lore.kernel.org/Yq0MV2Z%2FhqSuSYbt@dev-arch.thelio-3990X/)
* [`Re: [PATCH for-next 03/10] io_uring: fix io_poll_remove_all clang warnings`](https://lore.kernel.org/YrHmaOd1md4qqlHx@dev-arch.thelio-3990X/)
* [`stage 4 & stage 5`](https://github.com/ClangBuiltLinux/containers/pull/37)
* [`Re: [PATCH] lib/test_printf.c: fix clang -Wformat warnings`](https://lore.kernel.org/Yr3JN%2FmMn4K+7WnR@dev-arch.thelio-3990X/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH 09/15] swiotlb: make the swiotlb_init interface more useful`](https://lore.kernel.org/YpehC7BwBlnuxplF@dev-arch.thelio-3990X/)
* [`Re: [PATCH v11 1/4] trace: Add trace any kernel object`](https://lore.kernel.org/YpoqulzEvwNDHFMH@dev-arch.thelio-3990X/)
* [`Considering uninitialized function parameters UB`](https://github.com/ClangBuiltLinux/linux/issues/1648)
* [`LLD segfaults while linking drivers/scsi/megaraid/megaraid_mm.ko`](https://github.com/ClangBuiltLinux/linux/issues/1649)
* [`Re: [PATCH v3 4/4] rtw88: Fix Sparse warning for rtw8821c_hw_spec`](https://lore.kernel.org/Yp+hfo5Uual8ZUkR@dev-arch.thelio-3990X/)
* [`INIT_STACK_ALL_ZERO - Framework Laptop system freezes (kernel oops?) on boot`](https://github.com/ClangBuiltLinux/linux/issues/1626)
* [`error: write on a pipe with no reader`](https://github.com/ClangBuiltLinux/linux/issues/1651)
* [`"Unable to apply kernel patch" when git quiltimport works locally`](https://gitlab.com/Linaro/tuxsuite/-/issues/169)
* [`-Wframe-larger-than in sound/soc/intel/avs/path.c`](https://github.com/ClangBuiltLinux/linux/issues/1642)
* [`Contextual conflict between kspp and rcu trees`](https://lore.kernel.org/Yqo5SequJuC2qX6S@dev-arch.thelio-3990X/)
* [`Re: linux-next: Tree for Jun 15 (drivers/gpu/drm/amd/amdgpu/../display/amdgpu_dm/amdgpu_dm.c)`](https://lore.kernel.org/YqpACmvbwiEcUfta@dev-arch.thelio-3990X/)
* [`-Wunused-but-set-variable building perf`](https://github.com/ClangBuiltLinux/linux/issues/1654)
* [`Re: [PATCH for-next v3 16/16] io_uring: mutex locked poll hashing`](https://lore.kernel.org/YqyfK34NJakiLiVe@dev-arch.thelio-3990X/)
* [`Re: [GIT PULL] Char/Misc driver fixes 5.19-rc3`](https://lore.kernel.org/Yqywy+Md2AfGDu8v@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: pass jobserver to cmd_ld_vmlinux.o`](https://lore.kernel.org/YqzXKaaWRNP9C4b0@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Regression caused by llvm: [GlobalOpt] Perform store->dominated load forwarding for stored once globals`](https://lore.kernel.org/YrHl20+cA20MwpAP@dev-arch.thelio-3990X/)
* [`Re: mainline build failure due to 281d0c962752 ("fortify: Add Clang support")`](https://lore.kernel.org/YrMu5bdhkPzkxv%2FX@dev-arch.thelio-3990X/)
* [`Triggered assertion from "mm: Add an assertion that PG_private and folio->private are in sync"`](https://lore.kernel.org/YrYzhB7aDNIBz%2FuV@dev-arch.thelio-3990X/)
* [`Re: [PATCH v1 1/2] arm64: vdso32: add ARM.exidx* sections`](https://lore.kernel.org/YryxouOYOJLM+DDK@dev-arch.thelio-3990X/)
* [`Re: Regression for duplicate (?) console parameters on next-20220630`](https://lore.kernel.org/Yr37D4P2Dmnbkb+M@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`patches: Add RISC-V inline asm fixes to mainline and -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/373)
* [`Switch to self-hosted GitHub Actions for llvm-project`](https://github.com/ClangBuiltLinux/containers/pull/25)
* [`Update to Linux 5.18 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/191)
* [`boot-uml.sh: Remove exec hack`](https://github.com/ClangBuiltLinux/boot-utils/pull/64)
* [`build-llvm.py: Add LLVM_DEFAULT_TARGET_TRIPLE when possible`](https://github.com/ClangBuiltLinux/tc-build/pull/194)
* [`patches: mainline: Drop a RISC-V patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/374)
* [`Improve shell script style`](https://github.com/ClangBuiltLinux/tc-build/pull/195)
* [`ci.sh: Use release/14.x for testing`](https://github.com/ClangBuiltLinux/tc-build/pull/196)
* [`kernel/build.sh: Support all*config for Hexagon, RISC-V, and s390`](https://github.com/ClangBuiltLinux/tc-build/pull/197)
* [`ci/test-clang.sh: Skip toolchain archive creation when run locally`](https://github.com/ClangBuiltLinux/containers/pull/27)
* [`Improve podman compatibility`](https://github.com/ClangBuiltLinux/containers/pull/28)
* [`Add support for arm64 to llvm-project`](https://github.com/ClangBuiltLinux/containers/pull/29)
* [`Drop objtool patch on stable and 5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/375)
* [`Fix tags and update Makefile workflow to match`](https://github.com/ClangBuiltLinux/containers/pull/32)
* [`github: build-test-llvm-project: Add build-args`](https://github.com/ClangBuiltLinux/containers/pull/33)
* [`Re-enable ChromeOS jobs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/376)
* [`boot-utils: Update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/377)
* [`patches: mainline: Add a fix for ARCH=arm allmodconfig`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/378)
* [`patches: mainline: Drop watchdog patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/379)
* [`Drop -next patches (June 21st, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/380)
* [`Disable ChromeOS jobs again`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/381)
* [`Drop mainline patches (June 24th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/382)
* [`kernel/build.sh: Print kernel version and small fix for header`](https://github.com/ClangBuiltLinux/tc-build/pull/199)
* [`Enable ChromeOS jobs (round 3)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/383)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
