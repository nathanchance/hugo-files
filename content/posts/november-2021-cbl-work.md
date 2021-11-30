---
title: "November 2021 ClangBuiltLinux Work"
date: 2021-11-30T16:23:00-07:00
toc: false
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Build errors: These are hard errors that appear for one reason or another. The first two are from a recent change in LLVM to avoid crashing the compiler (see the commit message for more details, I learned floating point literals exist in C). The third series is a resend that fixes `ARCH=hexagon` allmodconfig, which is an important target for us because `ARCH=hexagon` requires LLVM. Lastly, the final series fixes a generic issue uncovered by the kernel test robot with an `ARCH=hexagon` randconfig.

  * `power: reset: ltc2952: Fix use of floating point literals` ([`v1`](https://lore.kernel.org/r/20211104215047.663411-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20211105152049.2522250-1-nathan@kernel.org/))
  * `usb: dwc2: hcd_queue: Fix use of floating point literal` ([`v1`](https://lore.kernel.org/r/20211104215923.719785-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20211105145802.2520658-1-nathan@kernel.org/))
  * `Fixes for ARCH=hexagon allmodconfig` ([`v2`](https://lore.kernel.org/r/20211115174250.1994179-1-nathan@kernel.org/))
  * `Fix CONFIG_TEST_KMOD with 256kB page size` ([`v1`](https://lore.kernel.org/r/20211129230141.228085-1-nathan@kernel.org/))

* Stable backports and patches: Keeping the stable kernels building is important because most people use them, rather than the development tree that we normally work against. Our CI has decent coverage of stable but we are looking to ramp that up going forward.

  * `Please apply 3d5e7a28b1ea2d603dea478e58e37ce75b9597ab to 5.15, 5.14, and 5.10` ([`v1`](https://lore.kernel.org/r/YYQjyo%2FdKfDb%2Fno3@archlinux-ax161/))
  * `scripts/lld-version.sh: Rewrite based on upstream ld-version.sh` ([`v1`](https://lore.kernel.org/r/20211115164322.560965-1-nathan@kernel.org/))
  * `Apply a52f8a59aef46b59753e583bf4b28fccb069ce64 to 5.15 through 4.19` ([`v1`](https://lore.kernel.org/r/YZKpVkYpYfYfHD50@archlinux-ax161/))

* `-Wimplicit-fallthrough`: Now that `-Wimplicit-fallthrough` is enabled by default for `clang-14` and newer [thanks to the great work of Gustavo A.R. Silva](https://git.kernel.org/linus/dee2b702bcf067d7b6b62c18bdd060ff0810a800), we will be able to catch potential issues such as the one fixed by [commit 652b44453ea9](https://git.kernel.org/linus/652b44453ea953d3157f02a7f17e18e329952649) ("habanalabs/gaudi: fix missing code in ECC handling").

  * `hwmon: (tmp401) Fix clang -Wimplicit-fallthrough in tmp401_is_visible()` ([`v1`](https://lore.kernel.org/r/20211116154438.1383290-1-nathan@kernel.org/))

* `-Wsometimes-uninitialized`/`-Wuninitialized`: Unfortunately, because the kernel is written in C, uninitialized variables continue to be an issue, especially since GCC's `-Wmaybe-uninitialized` has been [disabled by default since 5.7](https://git.kernel.org/linus/78a5255ffb6a1af189a83e493d916ba1c54d8c75). These will continue to pop up until GCC can be fixed but it does not seem like that will occur anytime soon as it has been a deficiency for a long time; until then, we will continue to fix them.

  * `NFS: Avoid using error uninitialized in nfs_lookup()` ([`v1`](https://lore.kernel.org/r/20211105155704.3293957-1-nathan@kernel.org/))
  * `fpga: stratix10-soc: Do not use ret uninitialized in s10_probe()` ([`v1`](https://lore.kernel.org/r/20211129161009.3625548-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20211130221123.3800420-1-nathan@kernel.org/))

* `-Wreturn-type`: The kernel has this as a hard error because having a mismatched return type is clearly a bug. Unfortunately, this patch ended up being wrong, and [the proper patch](https://git.kernel.org/linus/f91140e4553408cacd326624cd50fc367725e04a) was applied to fixed it.

  * `soc: ti: wkup_m3_ipc: Fix return type error in wkup_m3_rproc_boot_thread()` ([`v1`](https://lore.kernel.org/r/20211102160315.1067553-1-nathan@kernel.org/))

* Other series: Last year, [we decided](https://git.kernel.org/linus/3519c4d6e08ea695658b961829d4a603acf8e28a) that LLVM 10.0.1 would be the minimum supported version for building the kernel. However, as time has gone on, the kernel has continued to stress `clang` and exposed more deficiencies that had to be fixed in the compiler; some times, we work around these deficiencies but other times, such as this, we have to bump the minimum supported version to avoid invasively changing the kernel.

  * `Bump minimum supported version of LLVM to 11.0.0` ([`RFC`](https://lore.kernel.org/r/20211129165803.470795-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Increase number of vp counters per site with PGO`](https://github.com/ClangBuiltLinux/tc-build/pull/176)

* [`Re: [PATCH] riscv: fix building external modules`](https://lore.kernel.org/r/YYFr3F89YWfUDJxm@archlinux-ax161/)

* [`[PATCH 0/3] Fix some W=1 clang Werror at staging/media`](https://lore.kernel.org/r/cover.1636672052.git.mchehab+huawei@kernel.org/)

* [`Re: [PATCH] kconfig: Add support for -Wimplicit-fallthrough`](https://lore.kernel.org/r/YZF9MY6rRLQwdTgM@archlinux-ax161/)

* [`Re: [PATCH] omapfb: add one more "fallthrough" statement`](https://lore.kernel.org/r/YZPLpxGXVyaaKZsm@archlinux-ax161/)

* [`Re: [PATCH] soc/tegra: fuse: fix bitwice vs. logical warning`](https://lore.kernel.org/r/YZUY69vfq%2FRrVWMw@archlinux-ax161/)

* [`Re: [PATCH] clocksource/drivers/arm_arch_timer: Force inlining of erratum_set_next_event_generic()`](https://lore.kernel.org/r/YZVW+%2F4nbDtcU85h@archlinux-ax161/)

* [`Re: [PATCH] [v3] iwlwifi: pcie: fix constant-conversion warning`](https://lore.kernel.org/r/YZZ5b0FoppEBRcdL@archlinux-ax161/)

* [`Re: [PATCH] powerpc: mm: radix_tlb: rearrange the if-else block`](https://lore.kernel.org/r/YaEBTbjGyUBmISGK@archlinux-ax161/)

* [`[PATCH 00/20] Solve the remaining issues with clang and W=1 on media`](https://lore.kernel.org/r/cover.1637781097.git.mchehab+huawei@kernel.org/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Fortify error with a stable/released UTS_RELEASE`](https://github.com/ClangBuiltLinux/linux/issues/1496)

* [`Re: next/master build: 230 builds: 25 failed, 205 passed, 449 errors, 236 warnings (next-20211101)`](https://lore.kernel.org/r/YYBWK21wi%2FJ997sL@archlinux-ax161/)

* [`X86InstrInfo: Support immediates that are +1/-1 different in optimizeCompareInstr`](https://reviews.llvm.org/D110867#3106890)

* [`error: expression requires 'long double' type support, but target 'x86_64-unknown-linux-gnu' does not support it`](https://github.com/ClangBuiltLinux/linux/issues/1497)

* [`Re: [dborkman-bpf:pr/bpf-tstamp 3/3] drivers/net/ethernet/oki-semi/pch_gbe/pch_gbe_main.c:2482:44: warning: shift count >= width of type`](https://lore.kernel.org/r/YYbk2BE3ihkHlBBz@archlinux-ax161/)

* [`[Clang][LLVM][Attr] support btf_type_tag attribute`](https://reviews.llvm.org/D111199)

* [`Inconsistent build timeouts`](https://gitlab.com/Linaro/tuxsuite/-/issues/145)

* [`[Clang/Test]: Rename enable_noundef_analysis to disable-noundef-analysis and turn it off by default`](https://reviews.llvm.org/D105169#3116810)

* [`error: initializer element is not a compile-time constant`](https://github.com/ClangBuiltLinux/linux/issues/1501)

* [`error: hardware TLS register is not supported for the arm sub-architecture`](https://github.com/ClangBuiltLinux/linux/issues/1502)

* [`BUILD_BUG() failed with arm64's gki_defconfig`](https://github.com/ClangBuiltLinux/linux/issues/1503)

* [`Re: [arm-platforms:hack/m1-pmu 8/8] drivers/perf/apple_m1_cpu_pmu.c:100:32: warning: initializer overrides prior initialization of this subobject`](https://lore.kernel.org/r/YZGCrpVfY5xng4EC@archlinux-ax161/)

* [`2105a92748e83e2e3ee6be539da959706bbb3898 breaks x86_64 CFI`](https://github.com/ClangBuiltLinux/linux/issues/1508)

* [`Turn on CONFIG_WERROR for x86_64`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/245)

* [`Poor fixit suggestion for an uninitialized then dereferenced pointer`](https://bugs.llvm.org/show_bug.cgi?id=52559)

* [`Turn on CONFIG_WERROR for arm64`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/246)

* [`inlinable function call in a function with debug info must have a !dbg location`](https://github.com/ClangBuiltLinux/linux/issues/1510)

* [```Assertion `isa<X>(Val) && "cast<Ty>() argument of incompatible type!"' failed```](https://github.com/ClangBuiltLinux/linux/issues/1512)

* [`ld.lld: error: <inline asm>:25:2: macro 'extable_type_reg' is already defined`](https://github.com/ClangBuiltLinux/linux/issues/1513)

* [`polly causes compiler runtime error only on ARCH=arm`](https://github.com/ClangBuiltLinux/linux/issues/1515)

* [`Re: [PATCH v5] nl80211: reset regdom when reloading regdb`](https://lore.kernel.org/r/YaZLEEj2pUU%2FDhZD@archlinux-ax161/)

* [`Re: [PATCH 51/64] cachefiles: Implement the I/O routines`](https://lore.kernel.org/r/YaZOCk9zxApPattb@archlinux-ax161/)

* [`Re: [PATCH v5 5/5] powerpc/inst: Optimise copy_inst_from_kernel_nofault()`](https://lore.kernel.org/r/YaZqs2tPxMzhgkAW@archlinux-ax161/)



## Tooling improvements

This month was mostly spent focusing on getting the CI green via the application of patches from the mainline list as well as slowly increasing our coverage through trees (latest LTS, 5.15) and builds (hexagon allmodconfig).

* [`Update mainline builds and patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/230)

* [`Fix more errors in next and mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/231)

* [`Apply a patch to unbreak the Thumb2 build`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/232)

* [`Update patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/233)

* [`Update patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/236)

* [`parse-debian-clang.sh: Remove newlines between information print outs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/237)

* [`kernel: Update to Linux 5.15`](https://github.com/ClangBuiltLinux/tc-build/pull/177)

* [`patches: next: Remove return type error patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/238)

* [`patches: Remove android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/239)

* [`Add 5.15 trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/240)

* [`patches: mainline: Drop this_cpu_has_cap patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/241)

* [`Fix 5.15 builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/243)

* [`Make it green (11-18-2021)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/244)

* [`Work on misc-scripts`](https://github.com/ClangBuiltLinux/misc-scripts/commits/main)

* [`patches/5.10: Drop scripts/lld-version.sh patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/247)

* [`Remove CROSS_COMPILE_COMPAT and add CONFIG_COMPAT_VDSO=n build`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/248)

* [`patches/5.15: Drop applied FORTIFY_SOURCE patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/249)

* [`Add Hexagon allmodconfig for mainline and next with LLVM 13+`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/250)

* [`patches: mainline: Remove USB patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/251)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 3 and 4, HP desktop, ASUS laptop, and Hyper-V and VMware platforms on my workstation. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I wrote [a blog post](https://nathanchance.dev/posts/cvise-lto-kernel-bug/) about one of the bugs I triaged above so that I can remember how to do it in the future, as well as show others what reducing a C file looks like.

* During periods of downtime this month (i.e. not drowing in emails, triaging bugs, etc), I worked on containerizing some of [my development environment](https://github.com/nathanchance/env). This allows me to potentially move from one development machine to another with minimal overhead, which can happen in the case of upgrading or failure, as well as have a consistent workspace, since everything is described in a `Containerfile`. It makes getting access to older versions of LLVM and GCC easier as well, as I can build a container from a version of Debian or Ubuntu without actually having to run it as my host system.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).