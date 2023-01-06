---
title: "September 2021 ClangBuiltLinux Work"
date: 2021-09-30T10:00:00-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

## Linux kernel patches

* `tools: compiler-gcc.h: Guard error attribute use with __has_attribute` ([v1](https://lore.kernel.org/r/20210913220900.142820-1-nathan@kernel.org/)): The minimum version of GCC was raised to 5.1, which helped simplify a lot of code but caused issues for older versions of clang. This helps resolve it.

* `Harden clang against unknown flag options` ([v2](https://lore.kernel.org/r/20210916184017.1881473-1-nathan@kernel.org/)): A follow up.

* `ptp: ocp: Avoid operator precedence warning in ptp_ocp_summary_show()` ([v1](https://lore.kernel.org/r/20210916194351.3860836-1-nathan@kernel.org/), [v2](https://lore.kernel.org/netdev/20210917045204.1385801-1-nathan@kernel.org/)): An interesting warning around operator precedence, which will not be an issue in practice but now that Linus has decreed that warnings are unacceptable, it is important to clean them up!

* `locking/ww-mutex: Fix uninitialized use of ret in test_aa()` ([v1](https://lore.kernel.org/r/20210922145822.3935141-1-nathan@kernel.org/)): More uninitialized variable usage biting us because GCC's warning is turned off for the kernel.

* `kasan: Always respect CONFIG_KASAN_STACK` ([v1](https://lore.kernel.org/r/20210922205525.570068-1-nathan@kernel.org/)): Clang's `asan-stack` parameter shows high stack usage so it has been disabled but RISC-V's implementation was not respecting that so this patch resolves that, meaning RISC-V's `allmodconfig` target now builds completely clean with clang. `asan-stack` has been a topic of [recent conversation](https://github.com/ClangBuiltLinux/linux/issues/39) so hopefully that can be resolved soon.

* `drm/kmb: Remove set_test_mode_src_osc_freq_target_{hi,low}_bits()` ([v1](https://lore.kernel.org/r/20210928192338.1987872-1-nathan@kernel.org/)): Intel's kernel test robot exposed a couple of instances of unused inline functions, which GCC will never warn about. Cleaning up dead code always helps with maintainability in the long run.

* `drm/amd: Guard IS_OLD_GCC assignment with CONFIG_CC_IS_GCC` ([v1](https://lore.kernel.org/r/20210930160142.2301257-1-nathan@kernel.org/)): An instance where clang's fake GCC version of 4.2.1 caused problems for us. Hopefully as time goes on, we will run into less of these as more people test with clang.

* `drm/amd: Initialize remove_mpcc in dcn201_update_mpcc()` ([v1](https://lore.kernel.org/r/20210930161641.2333583-1-nathan@kernel.org/)): Another uninitialized warning...

* `drm/amd: Return NULL instead of false in dcn201_acquire_idle_pipe_for_layer()` ([v1](https://lore.kernel.org/r/20210930162302.2344542-1-nathan@kernel.org/)): Returning 0 or false when the function returns a pointer will cause a clang warning so that you either use NULL or change the value, just to make sure that is what was intended.



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`9934a5b2ed5aa6e6bbb2e55c3cd98839722c226e breaks x86_64 ThinLTO`](https://github.com/ClangBuiltLinux/linux/issues/1440#issuecomment-911966138)

* [`Re: [PATCH] s390/unwind: use current_frame_address() to unwind current task`](https://lore.kernel.org/r/YTKuj0Bu0CJRKqU7@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] fix missing 'sys' package`](https://lore.kernel.org/r/YTfGXn4cTZ87zTP1@archlinux-ax161/)

* [`Re: [PATCH] Revert "Enable '-Werror' by default for all kernel builds"`](https://lore.kernel.org/r/YTfvI2it1G2yBiD%2F@MSI.localdomain/)

* [`Re: [PATCH] lpfc: Fix compilation errors on kernels with no CONFIG_DEBUG_FS`](https://lore.kernel.org/r/YTfwjGOHqKY55cwQ@MSI.localdomain/)

* [`Re: [PATCH v2] gen_compile_commands: fix missing 'sys' package`](https://lore.kernel.org/r/YTjt5C7xTqNLUSl%2F@archlinux-ax161/)

* [`Re: [PATCH v2] lpfc: Fix compilation errors on kernels with no CONFIG_DEBUG_FS`](https://lore.kernel.org/r/YTjxDPsaoeQydKkR@archlinux-ax161/)

* [`Re: [PATCH 0/4] Fix stack usage of DML`](https://lore.kernel.org/r/YTqCWyZ7mXDaJ5HQ@Ryzen-9-3900X.localdomain/)

* [`[PATCH 00/10] raise minimum GCC version to 5.1`](https://lore.kernel.org/r/20210910234047.1019925-1-ndesaulniers@google.com/)

* [`Re: [PATCH] hardening: Default to INIT_STACK_ALL_ZERO if CC_HAS_AUTO_VAR_INIT_ZERO`](https://lore.kernel.org/r/01f572ab-bea2-f246-2f77-2f119056db84@kernel.org/)

* [`Re: [PATCH] gen_compile_commands: add missing sys import`](https://lore.kernel.org/r/026a4e82-da46-a559-06d5-18cff798ad96@kernel.org/)

* [`Re: [PATCH] powerpc: clean up UPD_CONSTR`](https://lore.kernel.org/r/c2ebc59f-c14e-d78f-78f0-2204d09cf81c@kernel.org/)

* [`Re: [PATCH] lib/zlib_inflate/inffast: Check config in C to avoid unused function warning`](https://lore.kernel.org/r/decede05-591b-b51c-bd5f-d844b1895e54@kernel.org/)

* [`Add .config artifact checker`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/199)

* [`Re: [PATCH] [RFC/RFT]SCS:Add gcc plugin to support Shadow Call Stack`](https://lore.kernel.org/r/YUeva0jP7P2qCr+R@archlinux-ax161/)

* [`Re: [PATCH v3] lib/zlib_inflate/inffast: Check config in C to avoid unused function warning`](https://lore.kernel.org/r/YUishGbHeaDMJDj+@archlinux-ax161/)

* [`Re: [PATCH v2] x86/setup: call early_reserve_memory() earlier`](https://lore.kernel.org/r/YUlYpWhGCxpJ9diw@archlinux-ax161/)

* [`problem matcher updates`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/211#issuecomment-925404667)

* [`Re: [PATCH v2] drm/vc4: Unselect PM`](https://lore.kernel.org/r/YU3y2AUpnSKHMZOO@archlinux-ax161/)

* [`Re: [PATCH] objtool: Teach get_alt_entry() about more relocation types`](https://lore.kernel.org/r/YVX0lxyRxZuN1idm@archlinux-ax161/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: kernel/sched/core.c:5854:20: warning: unused function 'sched_core_cpu_deactivate'`](https://lore.kernel.org/r/ab74f0f3-a7ea-e525-0725-946b1c754df3@kernel.org/)

* [`Re: [vmlinux.lds.h]  d4c6399900: BUG:unable_to_handle_page_fault_for_address`](https://lore.kernel.org/r/YTJumKboBddccDUJ@Ryzen-9-3900X.localdomain/)

* [`Re: [CI-NOTIFY]: TCWG Bisect tcwg_kernel/llvm-master-aarch64-mainline-allyesconfig - Build # 16 - Successful!`](https://lore.kernel.org/r/YTK4%2F8lnQykNLj%2Fe@Ryzen-9-3900X.localdomain/)

* [`Re: ❌ FAIL: Test report for kernel 5.14.0 (mainline.kernel.org-clang, f1583cb1)`](https://lore.kernel.org/r/YTK7aNSIjU3KmjH8@Ryzen-9-3900X.localdomain/)

* [`Re: [GIT PULL v2] Kbuild updates for v5.15-rc1`](https://lore.kernel.org/r/3b461878-a4a0-2f84-e177-9daf8fe285e7@kernel.org/)

* [`Re: [linux-next:master 10023/11721] include/linux/memory_hotplug.h:312:26: warning: unused parameter 'group'`](https://lore.kernel.org/r/ece61908-f8eb-4c45-5d5f-bfc52f3b8f45@kernel.org/)

* [`Re: ❌ FAIL: Test report for kernel 5.14.0 (mainline.kernel.org-clang, 1dbe7e38)`](https://lore.kernel.org/r/YTZyMx91zV9kfDkQ@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] Enable '-Werror' by default for all kernel builds`](https://lore.kernel.org/r/YTbOs13waorzamZ6@Ryzen-9-3900X.localdomain/)

* [`Re: ipv4/tcp.c:4234:1: error: the frame size of 1152 bytes is larger than 1024 bytes [-Werror=frame-larger-than=]`](https://lore.kernel.org/r/92c20b62-c4a7-8e63-4a94-76bdf6d9481e@kernel.org/)

* [`Re: ERROR: modpost: __mulodi4 [drivers/block/nbd.ko] undefined!`](https://lore.kernel.org/r/YTjrVWaTIA5ZZumW@archlinux-ax161/)

* [`drivers/infiniband/hw/qib/qib_sysfs.c:413:1: error: static_assert expression is not an integral constant expression`](https://github.com/ClangBuiltLinux/linux/issues/1452)

* [`RISC-V KASAN does not disable asan-stack`](https://github.com/ClangBuiltLinux/linux/issues/1453)

* [`Higher stack use with KASAN on 32-bit ARM`](https://github.com/ClangBuiltLinux/linux/issues/1454)

* [`-Wframe-larger-than= in drivers/gpu/drm/amd/display/dc/calcs/dce_calcs.c`](https://github.com/ClangBuiltLinux/linux/issues/1455)

* [`Re: [mcgrof-next:20210908-firmware-builtin-v4 2/11] drivers/base/firmware_loader/builtin/main.c:36:6: error: no previous prototype for function 'firmware_is_builtin'`](https://lore.kernel.org/r/YTwLw+frJLbntgCJ@archlinux-ax161/)

* [`Re: [dborkman:pr/bpf-cgrp 2/4] include/linux/cgroup-defs.h:771:70: warning: unused parameter 'skcd'`](https://lore.kernel.org/r/YT+RqrkQAOVhbkWu@archlinux-ax161/)

* [`"Patch series format unknown" with mbox file`](https://gitlab.com/Linaro/tuxsuite/-/issues/132)

* [`Re: linux: build faulure: error: "__has_attribute" is not defined`](https://lore.kernel.org/r/3bf6f4f4-9c96-6e0c-951d-5509175dddfe@kernel.org/)

* [`QEMU 6.1.0 regression with CONFIG_THUMB2_KERNEL`](https://github.com/ClangBuiltLinux/linux/issues/1447)

* [`Re: ❌ FAIL: Test report for kernel 5.15.0-rc1 (mainline.kernel.org-clang, 80be5998)`](https://lore.kernel.org/r/YUI7UIZvrsfMO716@archlinux-ax161/)

* [`Re: clang: error: unsupported argument '-mimplicit-it=always' to option 'Wa,'`](https://lore.kernel.org/r/c3a29fd8-e743-868c-3705-460fd38b7add@kernel.org/)

* [`Re: Odd pci_iounmap() declaration rules..`](https://lore.kernel.org/r/YUeriU9EIJ5hiFjL@archlinux-ax161/)

* [`Re: x86_64: clang-10: <instantiation>:2:2: error: unknown use of instruction mnemonic without a size suffix`](https://lore.kernel.org/r/YUgDV31uzAQy5IcN@archlinux-ax161/)

* [`Re: [tip: x86/urgent] x86/setup: Call early_reserve_memory() earlier`](https://lore.kernel.org/r/YUkPsjUUtRewyOn3@archlinux-ax161/)

* [`Re: [TCWG CI] Regression caused by linux: parisc: Declare pci_iounmap() parisc version only when CONFIG_PCI enabled`](https://lore.kernel.org/r/YUn+MRw4dkxrMouZ@archlinux-ax161/)

* [`Re: [Intel-gfx] [PATCH v3 03/13] drm/dp: add LTTPR DP 2.0 DPCD addresses`](https://lore.kernel.org/r/YUpjj7IwBqMYSR7z@archlinux-ax161/)

* [`[RISCV] Optimize (add (mul x, c0), c1)`](https://reviews.llvm.org/D108607)

* [`Re: [PATCH v3 1/6] drm/vc4: select PM (openrisc)`](https://lore.kernel.org/r/YUtQnml8FO8BC7sM@archlinux-ax161/)

* [`Re: [PATCH v2] platform/x86: amd-pmc: Export Idlemask values based on the APU`](https://lore.kernel.org/r/YUz2t+bdes2I+gMK@archlinux-ax161/)

* [`Re: next/master build: 209 builds: 5 failed, 204 passed, 5 errors, 1677 warnings (next-20210923)`](https://lore.kernel.org/r/YU0Tgl9qtuItvcxB@archlinux-ax161/)

* [`Re: drivers/gpu/drm/kmb/kmb_dsi.c:812:2: warning: unused function 'set_test_mode_src_osc_freq_target_low_bits'`](https://lore.kernel.org/r/YVNGMg%2FTI+jo%2F1R1@archlinux-ax161/)

* [`Re: [v2 PATCH] crypto: api - Fix built-in testing dependency failures`](https://lore.kernel.org/r/YVNfqUVJ7w4Z3WXK@archlinux-ax161/)

* [`Re: [PATCH bpf-next 06/13] xsk: optimize for aligned case`](https://lore.kernel.org/r/YVOiCYXTL63R4Mu9@archlinux-ax161/)

* [`Re: [PATCH v2 2/7] PCI: ACPI: PM: Do not use pci_platform_pm_ops for ACPI`](https://lore.kernel.org/r/YVS5k1H8KyVAk%2Fh8@archlinux-ax161/)

* [`Re: [PATCH v7] hugetlbfs: Extend the definition of hugepages parameter to support node allocation`](https://lore.kernel.org/r/YVS9VhrAuKE2YdbF@archlinux-ax161/)



## Tooling improvements

* [`Disable -Werror for pseries_defconfig for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/190)

* [`Check matrices for number of jobs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/191)

* [`Disable -Werror for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/194)

* [`check_logs.py: Increase timeout for CONFIG_KASAN (part 2)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/195)

* [`Make mainline green`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/196)

* [`Disable CONFIG_CFI_CLANG=y x86_64 builds with clang-12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/198)

* [`A couple of small fixes`](https://github.com/ClangBuiltLinux/tc-build/pull/171)

* [`Switch to directory-based patch management`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/203)

* [`Disable boot for CONFIG_KASAN_SW_TAGS=y on clang-11 and clang-12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/204)

* [`patches: Add futex patch to android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/206)

* [`Disable some builds to make -next green`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/207)

* [`check_logs.py: Handle equals sign in configuration value`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/208)

* [`patches: Update FPU patch to upstream submission`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/210)

* [`patches: Remove FPU patch for -next and -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/212)

* [`check_logs.py: Try multiple times to fetch items with exponential backoff`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/214)

* [`Add problem matchers for depmod, dtc, and modpost`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/215)

* [`Remove lto-cfi-tip patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/216)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 3 and 4, HP desktop, ASUS laptop, and Hyper-V and VMware platforms on my workstation. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* This month, we had a few conferences: [the LLVM Distributors Conference](https://github.com/ClangBuiltLinux/llvm-distributors-conf-2021), [the ClangBuiltLinux meetup](https://github.com/ClangBuiltLinux/CBL-meetup-II-turbo), and [Linux Plumbers Conference](https://linuxplumbersconf.org/). It was super helpful for interfacing with the greater LLVM and Linux kernel community and reorient ourselves going into the next year. I gave a talk at the ClangBuiltLinux meetup around our continuous integration.


## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
