---
title: "February 2022 ClangBuiltLinux Work"
date: 2022-02-28T00:17:30-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Miscellaneous improvements: These are series or patches that do not really fit into any of the other categories I typically use with my reports.

  * `Allow CONFIG_DEBUG_INFO_DWARF5=y + CONFIG_DEBUG_INFO_BTF=y` ([`v1`](https://lore.kernel.org/r/20220201205624.652313-1-nathan@kernel.org/))
  * `tools/resolve_btfids: Do not print any commands when building silently` ([`v1`](https://lore.kernel.org/r/20220201212503.731732-1-nathan@kernel.org/))

* `-Wenum-conversion`: This warning occurs when one enumerated type is implicitly converted to another enumerated type, which is typically a bug, although it might not actually be a problem in practice if the used enum has the same value as the intended enum (which is the case here). It is still good to get it corrected, as the enums might not stay in sync.

  * `drm/amdkfd: Use proper enum in pm_unmap_queues_v9()` ([`v1`](https://lore.kernel.org/r/20220217162142.1828990-1-nathan@kernel.org/))

* `-Wimplicit-fallthrough`: Clang's `-Wimplicit-fallthrough` (enabled in commit [dee2b702bcf0](https://git.kernel.org/linus/dee2b702bcf067d7b6b62c18bdd060ff0810a800) ("kconfig: Add support for -Wimplicit-fallthrough")) is more pedantic than GCC's version, requiring all case statements to have a `fallthrough` annotation, not just ones that do not fallthrough to `break` or `return`. Clang's version matches [the kernel's preference for switch statements](https://www.kernel.org/doc/html/latest/process/deprecated.html#implicit-switch-case-fall-through), so keeping the kernel clean of these warnings is important, in case GCC ever allows a mode that conforms to the kernel's preference.

  * `lib/maple_tree: Fix clang -Wimplicit-fallthrough in mte_set_pivot()` ([`v1`](https://lore.kernel.org/r/20220224161705.1937458-1-nathan@kernel.org/))

* `-Wunaligned-access`: This is a new warning introduced in `clang-14` to help catch unaligned accesses on ARM, which can be expensive, as they have to go through the exception handler for fixups. The warning is quite noisy, so we shut it up for default builds, but we allow it to show up for `W=1` to help prevent new instances from creeping in. This should eventually be enabled for default builds as well but cleaning up the instances will take time.

  * `Makefile.extrawarn: Move -Wunaligned-access to W=1` ([`v1`](https://lore.kernel.org/r/20220201232229.2992968-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20220202230515.2931333-1-nathan@kernel.org/), [`stable backport`](https://lore.kernel.org/r/20220214161641.1818-1-nathan@kernel.org/))

* `-Wunitialized` / `-Wsometimes-uninitialized`: As always, Clang will catch uninitialized variables because GCC's version of the warning was [moved to `W=2`](https://git.kernel.org/linus/78a5255ffb6a1af189a83e493d916ba1c54d8c75), which people rarely test with, due to the noise.

  * `io_uring: Fix use of uninitialized ret in io_eventfd_register()` ([`v1`](https://lore.kernel.org/r/20220207162410.1013466-1-nathan@kernel.org/))
  * `drm/stm: Avoid using val uninitialized in ltdc_set_ycbcr_config()` ([`v1`](https://lore.kernel.org/r/20220207165304.1046867-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20220222152045.484610-1-nathan@kernel.org/))
  * `iio: accel: adxl367: Fix handled initialization in adxl367_irq_handler()` ([`v1`](https://lore.kernel.org/r/20220224211034.625130-1-nathan@kernel.org/))

* `-Wunneeded-internal-declaration`: This is a really interesting warning that only Clang appears to have. When a variable is only used in a compile time expression (such as in `sizeof()` or `typeof()`), clang will warn about it, as the programmer might have intended to use it elsewhere. In this particular case, it is expected, but there have been [other times](https://git.kernel.org/linus/8289c4b6f2e53750de78bd38cecb6bce4d7a988c) where it is clearly a bug.

  * `mm/page_alloc: Mark pagesets as __maybe_unused` ([`v1`](https://lore.kernel.org/r/20220215184322.440969-1-nathan@kernel.org/))

* `-Wunused-function`: This is not typically a Clang specific warning but in this case, only Clang caught it because GCC does not warn on `static inline` functions in `.c` files, whereas Clang does. We make Clang match GCC for a default build but Masahiro Yamada [changed that behavior for `W=1`](https://git.kernel.org/linus/6863f5643dd717376c2fdc85a47a00f9d738a834) to help clean up or fix unused code.

  * `ftrace: Remove unused ftrace_startup_enable() stub` ([`v1`](https://lore.kernel.org/r/20220214192847.488166-1-nathan@kernel.org/))

* `-Wunused-variable`: Another warning that is not Clang specific but it impacts our builds just like GCC so it is important to clean them up.

  * `proc: Avoid unused variable warning in pagemap_pmd_range()` ([`v1`](https://lore.kernel.org/r/20220207171049.1102239-1-nathan@kernel.org/))

* `-Wvisibility`: This warning merely tells a developer that they have declared a type within a function, which cannot be used outside of the function. GCC has a similar warning, and like the past few warnings, it is important to fix all warnings that appear.

  * `thermal: netlink: Fix parameter type of thermal_genl_cpu_capability_event() stub` ([`v1`](https://lore.kernel.org/r/20220207163829.1025904-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] tools: Ignore errors from `which' when searching a GCC toolchain`](https://lore.kernel.org/r/YflZcEjaPoN8G84c@dev-arch.archlinux-ax161/)

* [`Re: [PATCH v3] seq_file: fix NULL pointer arithmetic warning`](https://lore.kernel.org/r/YflfI9uAz0bR2r4m@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] kallsyms: ignore all local labels prefixed by '.L'`](https://lore.kernel.org/r/Yflg560OtUrCNt7F@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] include: drop pointless __compiler_offsetof indirection`](https://lore.kernel.org/r/YfqqwxKCP+qJCyYg@dev-arch.archlinux-ax161/)

* [`[X86] Implement -fzero-call-used-regs option`](https://reviews.llvm.org/D110869#3294696)

* [`Re: [PATCH 1/1] Drivers: hv: vmbus: Rework use of DMA_BIT_MASK(64)`](https://lore.kernel.org/r/YgB36FwuRaF85WQq@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] drm/amd/pm: fix enabled features retrieving on Renoir and Cyan Skillfish`](https://lore.kernel.org/r/YgR4nzB294ZZEs9j@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] drm/i915: fix build issue when using clang`](https://lore.kernel.org/r/YglQW7gVNoRJ7QpQ@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] spi: Fix warning for Clang build and simplify code`](https://lore.kernel.org/r/Ygpz2M9sH+vAKqNL@dev-arch.archlinux-ax161/)

* [`Re: [PATCH v2] drm/i915: fix build issue when using clang`](https://lore.kernel.org/r/Ygv2fvIKiM9w+aSb@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] Documentation/llvm: refresh docs for LLVM=1`](https://lore.kernel.org/r/Ygw7s5XRokaPP1J5@dev-arch.archlinux-ax161/)

* [`[libc] Use '+' constraint on inline assembly`](https://reviews.llvm.org/D119978)

* [`Re: [PATCH] um: Allow builds with Clang`](https://lore.kernel.org/r/Yg2YubZxvYvx7%2Fnm@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] tools: fix unavoidable GCC call in Clang builds`](https://lore.kernel.org/r/Yg6PousYSZXimFCS@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] arm64 module: remove (NOLOAD)`](https://lore.kernel.org/r/Yg%2FKjpHOZ7HwZq9f@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] riscv module: remove (NOLOAD)`](https://lore.kernel.org/r/YhEelqnPDr1u4uTD@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] [PATCH] AARCH64: Add gcc Shadow Call Stack support`](https://lore.kernel.org/r/YhUMRoLDan7tJRiL@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] KVM: x86: Fix pointer mistmatch warning when patching RET0 static calls`](https://lore.kernel.org/r/YhZuk8eA6rsDuJkd@dev-arch.archlinux-ax161/)

* [`Re: [PATCH v21 2/2] dmaengine: tegra: Add tegra gpcdma driver`](https://lore.kernel.org/r/YhfwuCA71h8vMYUQ@dev-arch.archlinux-ax161/)

* [`Re: [PATCH] [v2] Kbuild: move to -std=gnu11`](https://lore.kernel.org/r/Yhz+rfcdp9jxgr+P@archlinux-ax161/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH 5/5] drm/stm: ltdc: add support of ycbcr pixel formats`](https://lore.kernel.org/r/Yfq3XwozrxYaFhgD@dev-arch.archlinux-ax161/)

* [`[LLVM 15] Getting invalid-symbol-name-offset while linking with LLD`](https://github.com/ClangBuiltLinux/linux/issues/1578)

* [`Skipping BTF generation for net/netfilter/nf_conntrack_h323.ko because it's a Rust module`](https://github.com/Rust-for-Linux/linux/issues/657)

* [`Re: ld.lld: error: undefined symbol: socfpga_reset_init`](https://lore.kernel.org/r/YgBtAdhl8dZVTAV+@dev-arch.archlinux-ax161/)

* [`Boot hang with certain PowerPC configurations after da0e5b885b25cf4ded0fa89b965dc6979ac02ca9`](https://github.com/ClangBuiltLinux/linux/issues/1581)

* [`-Wgnu-variable-sized-type-not-at-end in include/linux/cgroup-defs.h`](https://github.com/ClangBuiltLinux/linux/issues/1582)

* [`Re: [PATCH V3 4/7] drm/amd/pm: correct the usage for 'supported' member of smu_feature structure`](https://lore.kernel.org/r/YgK1Xetw4YVcUJ+n@dev-arch.archlinux-ax161/)

* [`Re: [PATCH 1/3] pwm: driver for qualcomm ipq6018 pwm block`](https://lore.kernel.org/r/YgK63cI177ZeF5v1@dev-arch.archlinux-ax161/)

* [`Turn on CONFIG_WERROR for arm64`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/246)

* [`Turn on CONFIG_WERROR for x86_64`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/245)

* [`Boot issue with CONFIG_ZERO_CALL_USED_REGS=y and _paravirt_ident_64()`](https://github.com/ClangBuiltLinux/linux/issues/1585)

* [`Recent infrastructure errors`](https://gitlab.com/Linaro/tuxsuite/-/issues/149)

* [`[AArch64] Adds SUBS and ADDS instructions to the MIPeepholeOpt.`](https://reviews.llvm.org/D118663)

* [`Re: arm64: clang-nightly: net/ipv4/tcp_input.c: clang: error: clang frontend command failed with exit code 139`](https://lore.kernel.org/r/Ygp2wVo8JfWh5iOk@dev-arch.archlinux-ax161/)

* [`Re: kernel/trace/ftrace.c:7157:20: error: unused function 'ftrace_startup_enable'`](https://lore.kernel.org/r/Ygp64CsyyKyRykqE@dev-arch.archlinux-ax161/)

* [`Re: [PATCH v6 7/7] kernfs: Replace per-fs rwsem with hashed ones.`](https://lore.kernel.org/r/20220214174951.131423-1-nathan@kernel.org/)

* [`-Wattribute-warning in drivers/scsi/libfc/fc_elsct.c`](https://github.com/ClangBuiltLinux/linux/issues/1590)

* [`-Wattribute-warning in drivers/misc/habanalabs/common/firmware_if.c`](https://github.com/ClangBuiltLinux/linux/issues/1591)

* [`-Wattribute-warning in drivers/net/ethernet/huawei/hinic/hinic_devlink.c`](https://github.com/ClangBuiltLinux/linux/issues/1592)

* [`[clang] Assertion tripped in X86 FP Stackifier`](https://github.com/llvm/llvm-project/issues/53391#issuecomment-1040923723)

* [`Re: BUG: Kernel NULL pointer dereference on write at 0x00000000 (rtmsg_ifinfo_build_skb)`](https://lore.kernel.org/r/Yg2h2Q2vXFkkLGTh@dev-arch.archlinux-ax161/)

* [`"ld.lld: error: section type mismatch for .plt" after LLVM commit 66f8ac8d3604d67599734c3fd272032e9448aca2`](https://github.com/ClangBuiltLinux/linux/issues/1597)

* [`[Question] Does build-llvm.py need an update for clang-14?`](https://github.com/ClangBuiltLinux/tc-build/issues/181)

* [`Re: [PATCH v7 1/2] ACPI: APEI: explicit init HEST and GHES in apci_init`](https://lore.kernel.org/r/YhPXX+CSoK++9MP6@dev-arch.archlinux-ax161/)

* [`Re: [PATCH v20 2/2] dmaengine: tegra: Add tegra gpcdma driver`](https://lore.kernel.org/r/YhUBt20I471s9Bhv@dev-arch.archlinux-ax161/)

* [`-Wpointer-type-mismatch in arch/x86/include/asm/kvm-x86-ops.h`](https://github.com/ClangBuiltLinux/linux/issues/1602)

* [`-Wdeclaration-after-statement in lib/raid6/ with proposed -std=gnu11`](https://github.com/ClangBuiltLinux/linux/issues/1603)

* [`-Wdeclaration-after-statement in arm_neon.h`](https://github.com/llvm/llvm-project/issues/54062)

* [`[DAG] Attempt to fold bswap(shl(x,c)) -> zext(bswap(trunc(shl(x,c-bw/2))))`](https://reviews.llvm.org/D120192)

* [`.plt section in modules with IBT`](https://github.com/ClangBuiltLinux/linux/issues/1606)

* [`Re: [PATCH v6 00/71] Introducing the Maple Tree`](https://lore.kernel.org/r/Yhlfkk%2FgTz6a%2FhOD@archlinux-ax161/)

* [`Full LTO builds getting killed again?`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/315)

* [`android-4.9 + clang-13 "Inconsistent kallsyms data"`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/316)



## Tooling improvements

* [`patches: Remove max20086-regulator.c patch from mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/293)

* [`patches: Remove mainline series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/294)

* [`Bump llvm_tot anchor and LLVM_TOT_VERSION`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/296)

* [`Stop doing builds of linux-4.4.y`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/297)

* [`debian/build.sh improvements`](https://github.com/ClangBuiltLinux/boot-utils/pull/55)

* [`Drop CFI patch on android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/298)

* [`Enable CONFIG_DEBUG_INFO_BTF for Android and Fedora configs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/299)

* [`docker: clang-android: Update to r445002`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/234)

* [`Run generate.sh on pull requests and pushes`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/300)

* [`Add script to generate Markdown for CI badges`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/301)

* [`Split GitHub Actions and TuxSuite by toolchain version`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/302)

* [`Add support for clang-14`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/304)

* [`Include toolchain version in workflow name`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/305)

* [`Fix CI badges`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/306)

* [`Move shell scripts to scripts/`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/307)

* [`Move and update cron logic`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/309)

* [`Update cron schedule logic`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/310)

* [`Make it green (February 18th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/311)

* [`patches: next: Drop net/netfilter/xt_socket.c patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/312)

* [`patches: next: Add a patch to fix build with KVM_WERROR`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/313)

* [`patches: mainline: Drop netfilter patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/314)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
