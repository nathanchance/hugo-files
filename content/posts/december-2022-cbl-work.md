---
title: December 2022 ClangBuiltLinux Work
date: 2022-12-30T11:00:00-0700
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

* Miscellaneous fixes: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `Fix lack of section mismatch warnings with LTO` ([`v2`](https://lore.kernel.org/20221207191657.2852229-1-nathan@kernel.org/), [`v3`](https://lore.kernel.org/20221213183529.766630-1-nathan@kernel.org/))
  * `security: Restrict CONFIG_ZERO_CALL_USED_REGS to gcc or clang > 15.0.6` ([`v1`](https://lore.kernel.org/20221214232602.4118147-1-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM (or in the case of the first one, with GCC as the result of a patch series for ClangBuiltLinux). I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `ARM: Reduce __thumb2__ definition to crypto files that require it` ([`v1`](https://lore.kernel.org/20221222193039.2267074-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 1/2] kbuild: move -Werror from KBUILD_CFLAGS to KBUILD_CPPFLAGS`](https://lore.kernel.org/Y5N0wwIkoHuPbcwU@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] kbuild: add -Wundef to KBUILD_CPPFLAGS for W=1 builds`](https://lore.kernel.org/Y5N1HGYyJGgm3qVH@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: add test-{ge,gt,le,lt} macros`](https://lore.kernel.org/Y5dgK2D4TP35PkKg@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: ensure Make >= 3.82 is used`](https://lore.kernel.org/Y5djBr9rVhSq8+iK@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ARM: disallow pre-ARMv5 builds with ld.lld`](https://lore.kernel.org/Y5z66Oh1mcOul0Jw@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/tests: reduce drm_mm_test stack usage`](https://lore.kernel.org/Y5z7ZJdioD0Nyqss@dev-arch.thelio-3990X/)
* [`MIPS: emit .module and .nan directives only for first function`](https://reviews.llvm.org/D140270#4005242)
* [`Re: [PATCH v1] driver core: Silence 'unused-but-set variable' warning`](https://lore.kernel.org/Y6Xl3M7Tx+c2SCa9@dev-arch.thelio-3990X/)
* [`Re: [PATCH linux-next] lib: Dhrystone: initialize ret value`](https://lore.kernel.org/Y6aVA9M%2Fwxsi%2FKKh@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 2/2] kbuild: make W=1 warn files that are tracked but ignored by git`](https://lore.kernel.org/all/Y6ptNLXJK8Z3TLJl@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: add a missing line for help message`](https://lore.kernel.org/Y6sjRvdmzGqer5oL@dev-arch.thelio-3990X/)
* [`Re: [RFC PATCH] mm/thp: check and bail out if page in deferred queue already`](https://lore.kernel.org/Y6tSUutBqeXeOrcR@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: Fix running modpost with musl libc`](https://lore.kernel.org/Y6xwKFwtTrZ4Uzqk@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: rpm-pkg: add libelf-devel as alternative for BuildRequires`](https://lore.kernel.org/Y6zevMoJOZh9Ouy3@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] kbuild: sort single-targets alphabetically again`](https://lore.kernel.org/Y63MaK+98prCZq0R@dev-arch.thelio-3990X/)
* [`Re: [PATCH 5.15 000/731] 5.15.86-rc1 review`](https://lore.kernel.org/Y63Gz7Jpms95bz15@dev-arch.thelio-3990X/)
* [`Re: [PATCH] btrfs: handle -Wmaybe-uninitialized with clang`](https://lore.kernel.org/Y68nyfHNXieotO+C@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH printk v5 03/40] printk: Prepare for SRCU console list protection`](https://lore.kernel.org/Y4jw3hSuwt3RG4DL@dev-arch.thelio-3990X/)
* [`Re: [mm] f35b5d7d67: will-it-scale.per_process_ops -95.5% regression`](https://lore.kernel.org/Y4keIyIK6OA3nOwT@dev-arch.thelio-3990X/)
* [`CFI failure in efi_call_rts() after ecff303cbcbb`](https://github.com/ClangBuiltLinux/linux/issues/1767)
* [`Re: ld.lld: error: undefined symbol: __udivdi3`](https://lore.kernel.org/Y4913gH2tvIED61P@dev-arch.thelio-3990X/)
* [`Re: [PATCH 6.0 000/157] 6.0.13-rc1 review`](https://lore.kernel.org/Y5iteH7jCU83w5qm@dev-arch.thelio-3990X/)
* [`objtool memory usage regression in -tip tree after call depth tracking`](https://github.com/ClangBuiltLinux/linux/issues/1768)
* [`pmac32_defconfig boot failure after LLVM commit 1bd0b82e508d049efdb07f4f8a342f35818df341`](https://github.com/ClangBuiltLinux/linux/issues/1770)
* [`error: unknown target CPU '405'`](https://github.com/ClangBuiltLinux/linux/issues/1771)
* [`Re: [PATCH v10 8/9] powerpc/code-patching: Use temporary mm for Radix MMU`](https://lore.kernel.org/Y5uA4G7WDaL3uG8Y@dev-arch.thelio-3990X/)
* [`MIPS: fix build from IR files, nan2008 and FpAbi`](https://reviews.llvm.org/D138179)
* [`Re: Linux-stable-rc/ queue_5.10`](https://lore.kernel.org/Y6CCkWLcNZMqN8UV@dev-arch.thelio-3990X/)
* [```Re: [TCWG CI] Failure after llvmorg-16-init-14942-g6adeec881a83: [InstCombine] `sinkNotIntoOtherHandOfLogicalOp()`: allow extra invertible uses of hand-to-invert```](https://lore.kernel.org/Y6EI9exfR%2FWAg%2FOW@dev-arch.thelio-3990X/)
* [`Re: [PATCH 3/3 v6] virtio: vdpa: new SolidNET DPU driver.`](https://lore.kernel.org/Y6HjpvDfIusAz2uS@dev-arch.thelio-3990X/)
* [```[NFC][DAGCombiner] `visitFREEZE()`: use early return```](https://reviews.llvm.org/rG114cc45a095e#1158603)
* [`Re: [PATCH 8/8] btrfs: turn on -Wmaybe-uninitialized`](https://lore.kernel.org/Y6kgR4qnb23UdAEX@dev-arch.thelio-3990X/)
* [`Re: [PATCH 3/7] udf: Handle error when adding extent to a file`](https://lore.kernel.org/Y6ki+weNcHuyH7i1@dev-arch.thelio-3990X/)
* [`Re: [peterz-queue:core/wip-u128 14/14] arch/arm/include/asm/cmpxchg.h:254:1: error: instruction requires: !armv*m`](https://lore.kernel.org/Y6s0Yongvkn1WoIK@dev-arch.thelio-3990X/)
* [`Re: [PATCH] fbdev: make offb driver tristate`](https://lore.kernel.org/Y6s96iuc3NRN5tS4@dev-arch.thelio-3990X/)
* [`Inquiry around 'b4 send' individual patch Cc's`](https://lore.kernel.org/Y6zeRaKlqNhh72OD@dev-arch.thelio-3990X/)
* [`Re: [linux-stable-rc:linux-5.10.y 9077/9999] ld.lld: error: arch/riscv/kernel/vdso/rt_sigreturn.o:(.riscv.attributes): rv32i2p1_m2p0_a2p1_c2p0: unsupported version number 2.1 for extension 'i'`](https://lore.kernel.org/Y6zpt9lSRrJzwEpm@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`boot-qemu.py: Use tuples for Linux and QEMU versions`](https://github.com/ClangBuiltLinux/boot-utils/pull/78)
* [`boot-qemu.py: Add support for booting arm64 and x86_64 guests under UEFI`](https://github.com/ClangBuiltLinux/boot-utils/pull/79)
* [`Add "Check clang version" badge to README`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/479)
* Python linting improvement
  * [`actions-workflows`](https://github.com/ClangBuiltLinux/actions-workflows/pull/4)
  * [`boot-utils`](https://github.com/ClangBuiltLinux/boot-utils/pull/80)
  * [`containers`](https://github.com/ClangBuiltLinux/containers/pull/48)
  * [`continuous-integration2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/480)
  * [`tc-build`](https://github.com/ClangBuiltLinux/tc-build/pull/210)
* [`patches: android13-5.10: Add upstream series for pahole 1.24 compatability`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/481)
* [`Add support for mounting a shared folder between the host and guest via virtiofs`](https://github.com/ClangBuiltLinux/boot-utils/pull/82)
* [`generator.yml: Disable x86_64 allyesconfig builds on -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/482)
* [`Add x86 vDSO patch for tip of tree ld.lld default change`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/483)
* [`boot-qemu.py: Simplify can_use_kvm()`](https://github.com/ClangBuiltLinux/boot-utils/pull/84)
* [`Enable x86_64 KCSAN build with LLVM ToT on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/484)
* [`Update stable anchor to 6.1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/485)
* [`boot-utils: Update to latest main`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/486)
* [`boot-qemu.py: Check gdb_bin earlier`](https://github.com/ClangBuiltLinux/boot-utils/pull/85)
* [`Adjust mainline patches (December 15th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/487)
* [`tc-build: Stop building the gold plugin`](https://github.com/ClangBuiltLinux/tc-build/pull/212)
* [`build-llvm.py: Fix using relative paths for folders`](https://github.com/ClangBuiltLinux/tc-build/pull/213)
* [`Fixes for building on Alpine Linux`](https://github.com/ClangBuiltLinux/tc-build/pull/214)
* [`Two small miscellaneous improvements`](https://github.com/ClangBuiltLinux/tc-build/pull/215)
* [`Adjust x86_64 allyesconfig builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/488)
* [`patches: Remove 45be2ad007a9c6bea patch from 5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/489)
* [`docker: clang-android: Update to r475365b (16.0.2)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/285)
* [`android14-6.1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/490)
* [`patches: Drop android14-5.15, chromeos-5.15, and android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/491)
* [`Drop mainline patches (December 21st, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/492)
* [`Add fixes to linux-next during break`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/493)
* [`patches: next: Update for next-20221226`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/494)
* [`Fix build failures after OpenSUSE config update to 6.2-rc1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/495)
* [`Rewrite all shell scripts in Python`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/496)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
