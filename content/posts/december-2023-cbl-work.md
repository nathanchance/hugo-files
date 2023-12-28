---
title: December 2023 ClangBuiltLinux Work
date: 2023-12-28T11:30:00-0700
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `RISC-V: Disable DWARF5 with known broken LLVM versions` ([`v2`](https://lore.kernel.org/20231205-riscv-restrict-dwarf5-llvm-v2-0-aedf00a382ac@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply b8ec60e1186cdcfce41e7db4c827cb107e459002 to linux-6.6.y`](https://lore.kernel.org/20231213000338.GA866722@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `x86/tools: objdump_reformat.awk: Skip bad instructions from llvm-objdump` ([`v1`](https://lore.kernel.org/20231205-objdump_reformat-awk-handle-llvm-objdump-bad_expr-v1-1-b4a74f39396f@kernel.org/))
  * `A few fixes for transparent bridge support` ([`v1`](https://lore.kernel.org/20231205-drm_aux_bridge-fixes-v1-0-d242a0ae9df4@kernel.org/))
  * `Enable -Wincompatible-function-pointer-types-strict under W=1` ([`v2`](https://lore.kernel.org/20231206-enable-wincompatible-function-pointer-types-strict-w-1-v2-0-91311b4c37b0@kernel.org/))
  * `kasan: Mark unpoison_slab_object() as static` ([`v1`](https://lore.kernel.org/20231221-mark-unpoison_slab_object-as-static-v1-1-bf24f0982edc@kernel.org/))
  * `dmaengine: xilinx: xdma: Fix two clang warnings` ([`v1`](https://lore.kernel.org/20231222-dma-xilinx-xdma-clang-fixes-v1-0-84a18ff184d2@kernel.org/))
  * `platform/x86/intel/pmc: Fix recent instances of -Wmissing-prototypes` ([`v1`](https://lore.kernel.org/20231222-intel-pmc-missing-prototypes-v1-0-3f0d47377d4c@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [RFC] drm: enable W=1 warnings by default across the subsystem`](https://lore.kernel.org/20231201175939.GA231094@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86/callthunks: Correct calculation of dest address in is_callthunk()`](https://lore.kernel.org/20231201214405.GA1533860@dev-arch.thelio-3990X/)
* [`Re: [PATCH] gen_compile_commands.py: fix path resolve with symlinks in it`](https://lore.kernel.org/20231204165920.GA16980@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] gen_compile_commands.py: fix path resolve with symlinks in it`](https://lore.kernel.org/20231205165648.GA391810@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] rpm-pkg: simplify installkernel %post`](https://lore.kernel.org/20231212191918.GA2914380@dev-arch.thelio-3990X/)
* [`Re: [PATCH] power: reset: at91: mark at91_wakeup_status non-__init`](https://lore.kernel.org/20231212215002.GA3300655@dev-arch.thelio-3990X/)
* [```Re: [PATCH] docs: rust: remove `CC=clang` mentions```](https://lore.kernel.org/20231218165552.GA601326@dev-arch.thelio-3990X/)
* [`Re: [PATCH iwl-next] i40e: Avoid unnecessary use of comma operator`](https://lore.kernel.org/20231218190055.GB2863043@dev-arch.thelio-3990X/)
* [`[Clang] Generate the GEP instead of adding AST nodes`](https://github.com/llvm/llvm-project/pull/73730#issuecomment-1861411905)
* [`Revert counted_by attribute feature`](https://github.com/llvm/llvm-project/pull/75857#issuecomment-1861700246)
* [`Re: [PATCH v4] rpm-pkg: simplify installkernel %post`](https://lore.kernel.org/20231219201719.1967948-1-jtornosm@redhat.com/)
* [`Re: [PATCH] scripts/decode_stacktrace.sh: Support LLVM addr2line`](https://lore.kernel.org/20231226163802.GA952423@dev-arch.thelio-3990X/)
* [`[LoongArch] Support R_LARCH_{ADD,SUB}_ULEB128 for .uleb128 and force relocs when sym is not in section`](https://github.com/llvm/llvm-project/pull/76433#issuecomment-1871382875)
* [`CI Caching`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/664)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH -tip v2 2/3] x86/callthunks: Handle %rip-relative relocations in call thunk template`](https://lore.kernel.org/20231201035457.GA321497@dev-arch.thelio-3990X/)
* [`Re: Patch "arm64: Restrict CPU_BIG_ENDIAN to GNU as or LLVM IAS 15.x or newer" has been added to the 4.19-stable tree`](https://lore.kernel.org/20231204162228.GA1007084@dev-arch.thelio-3990X/)
* [`Re: clang-nightly: vdso/compat_gettimeofday.h:152:15: error: instruction variant requires ARMv6 or later`](https://lore.kernel.org/20231204181304.GA2043538@dev-arch.thelio-3990X/)
* [`Error when building Linux with latest LLVM (and CLang)`](https://github.com/llvm/llvm-project/issues/72026)
* [`-Wframe-larger-than in fs/namespace.c `](https://github.com/ClangBuiltLinux/linux/issues/1964)
* [`Re: [PATCH 5.4 63/94] btrfs: add dmesg output for first mount and last unmount of a filesystem`](https://lore.kernel.org/20231209172836.GA2154579@dev-arch.thelio-3990X/)
* [`RISC-V boot failure after LLVM commit e87f33d9ce785668223c3bcc4e06956985cccda1`](https://github.com/ClangBuiltLinux/linux/issues/1965)
* [`Re: [drm-xe:drm-xe-next 989/1016] drivers/gpu/drm/xe/xe_wait_user_fence.c:46:2: warning: variable 'passed' is used uninitialized whenever switch default is taken`](https://lore.kernel.org/20231218185722.GA2863043@dev-arch.thelio-3990X/)
* [`Re: [PATCH net-next 20/24] net: intel: Use nested-BH locking for XDP redirect.`](https://lore.kernel.org/20231219000116.GA3956476@dev-arch.thelio-3990X/)
* [`Re: [PATCH 15/50] kernel/numa.c: Move logging out of numa.h`](https://lore.kernel.org/20231219163644.GA345795@dev-arch.thelio-3990X/)
* [`[LoongArch] Allow delayed decision for ADD/SUB relocations`](https://github.com/llvm/llvm-project/pull/72960#issuecomment-1866844679)
* [`Re: [PATCH v5 37/40] netfs: Optimise away reads above the point at which there can be no data`](https://lore.kernel.org/20231221230153.GA1607352@dev-arch.thelio-3990X/)
* [`Re: [PATCH v5 15/40] netfs: Add support for DIO buffering`](https://lore.kernel.org/20231226165442.GA1202197@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).


* [`Fix workflows with caching`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/669)
* [`Install requirements.txt for check cache stage`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/670)
* [`Revert #661`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/672)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
