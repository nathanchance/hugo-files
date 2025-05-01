---
title: April 2025 ClangBuiltLinux Work
date: 2025-04-30T16:30:00-0700
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

  * `kbuild: Add '-fno-builtin-wcslen'` ([`v1`](https://lore.kernel.org/20250407-fno-builtin-wcslen-v1-1-6775ce759b15@kernel.org/))
  * `x86/boot/startup: Disable LTO` ([`v1`](https://lore.kernel.org/20250414-x86-boot-startup-lto-error-v1-1-7c8bed7c131c@kernel.org/))
  * `gpio: Restrict GPIO_ICH to compile testing with HAS_IOPORT` ([`v1`](https://lore.kernel.org/20250418-gpio-ich-fix-build-without-ioport-v1-1-83fc753438ec@kernel.org/))
  * `riscv: vdso.lds.S: Do not export __vdso_getrandom when building a 32-bit vDSO` ([`v1`](https://lore.kernel.org/20250423-riscv-fix-compat_vdso-lld-v1-1-820de5cad605@kernel.org/), [`v2`](https://lore.kernel.org/20250423-riscv-fix-compat_vdso-lld-v2-1-b7bbbc244501@kernel.org/))
  * `riscv: crypto - Use SYM_FUNC_START for functions only called directly` ([`v1`](https://lore.kernel.org/20250424-riscv-crypto-fix-cfi-build-v1-1-2d7516737379@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `vsprintf: Use __diag macros to disable '-Wsuggest-attribute=format'` ([`v1`](https://lore.kernel.org/20250404-vsprintf-convert-pragmas-to-__diag-v1-0-5d6c5c55b2bd@kernel.org/))
  * `lib/Kconfig.ubsan: Remove 'default UBSAN' from UBSAN_INTEGER_WRAP` ([`v1`](https://lore.kernel.org/20250414-drop-default-ubsan-integer-wrap-v1-1-392522551d6b@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Re: FAILED: patch "[PATCH] ARM: 9443/1: Require linker to support KEEP within OVERLAY" failed to apply to 6.13-stable tree`](https://lore.kernel.org/20250408152300.GA3301081@ax162/)
  * [`Re: FAILED: patch "[PATCH] ACPI: platform-profile: Fix CFI violation when accessing" failed to apply to 6.13-stable tree`](https://lore.kernel.org/20250408184518.GA2217235@ax162/)
  * [`Backports of 84ffc79bfbf7 for 6.12 and earlier`](https://lore.kernel.org/20250417171024.GA171175@ax162/)
  * [`Please apply 8b55f8818900 to 6.12 through 6.1`](https://lore.kernel.org/20250417172213.GA1197662@ax162/)
  * `lib/Kconfig.ubsan: Remove 'default UBSAN' from UBSAN_INTEGER_WRAP` ([`6.14`](https://lore.kernel.org/20250421154059.3248712-1-nathan@kernel.org/), [`6.12`](https://lore.kernel.org/20250421153918.3248505-2-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `drm/sysfb: efidrm: Avoid clang -Wsometimes-uninitialized in efidrm_device_create()` ([`v1`](https://lore.kernel.org/20250409-efidrm-avoid-uninit-screen_info-warning-v1-1-67babb19d831@kernel.org/), [`v2`](https://lore.kernel.org/20250410-efidrm-avoid-uninit-screen_info-warning-v2-1-b79646f58c24@kernel.org/))
  * `riscv: Avoid fortify warning in syscall_get_arguments()` ([`v1`](https://lore.kernel.org/20250409-riscv-avoid-fortify-warning-syscall_get_arguments-v1-1-7853436d4755@kernel.org/))
  * `ASoC: cs48l32: Use modern PM_OPS` ([`v1`](https://lore.kernel.org/20250418-cs48l32-modern-pm_ops-v1-1-640559407619@kernel.org/))
  * `drm/panel: himax-hx8279: Always initialize goa_{even,odd}_valid in hx8279_check_goa_config()` ([`v1`](https://lore.kernel.org/20250422-panel-himax-hx8279-fix-sometimes-uninitialized-v1-1-614dba12b30d@kernel.org/), [`v2`](https://lore.kernel.org/20250423-panel-himax-hx8279-fix-sometimes-uninitialized-v2-1-fc501c6558d9@kernel.org/))
  * `btrfs: Fix use of GCC_VERSION in messages.h` ([`v1`](https://lore.kernel.org/20250428-btrfs-fix-messages-h-clang-v1-1-5ede51586a9c@kernel.org/))
  * `scsi: dc395x: Remove leftover if statement in reselect()` ([`v1`](https://lore.kernel.org/20250429-scsi-dc395x-fix-uninit-var-v1-1-25215d481020@kernel.org/))
  * `kbuild: Properly disable -Wunterminated-string-initialization for clang` ([`v1`](https://lore.kernel.org/20250430-unterminated-string-init-clang-v1-1-10eab729bf3d@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [GIT PULL] string fixes for v6.15-rc1`](https://lore.kernel.org/20250407173741.GA3847400@ax162/)
* [`Re: [PATCH 5.15 253/279] mmc: sdhci-brcmstb: Add ability to increase max clock rate for 72116b0`](https://lore.kernel.org/20250408160235.GA3698564@ax162/)
* [`Re: [PATCH] ref_tracker: use %ld format specifier for PTR_ERR() during directory creation failure`](https://lore.kernel.org/20250413013245.GA2989337@ax162/)
* [`Re: [PATCH] correct disabling of -Wshift-negative-value`](https://lore.kernel.org/20250414193836.GA107755@ax162/)
* [`Re: [PATCH] HID: simplify code in fetch_item()`](https://lore.kernel.org/20250415003326.GA4164044@ax162/)
* [`Re: [PATCH] Bluetooth: vhci: Avoid needless snprintf() calls`](https://lore.kernel.org/20250415161920.GA1692211@ax162/)
* [`Disable -fdollars-in-identifiers by default`](https://github.com/llvm/llvm-project/pull/135407#issuecomment-2810106630)
* [`Add basic support for building Rust`](https://github.com/ClangBuiltLinux/tc-build/pull/299#pullrequestreview-2765578451)
* [`Re: [PATCH] kbuild: Switch from -Wvla to -Wvla-larger-than=0`](https://lore.kernel.org/20250421161725.GA3253782@ax162/)
* [`Re: [PATCH] wifi: iwlwifi: mld: Work around Clang loop unrolling bug`](https://lore.kernel.org/20250422195903.GA3475704@ax162/)
* [`Re: [PATCH v2] wifi: iwlwifi: mld: Work around Clang loop unrolling bug`](https://lore.kernel.org/20250426000401.GB3584546@ax162/)
* [`Re: [PATCH 1/1] hardening: simplify CONFIG_CC_HAS_COUNTED_BY`](https://lore.kernel.org/20250430231306.GA3715926@ax162/)
* [`Re: [PATCH] kunit: fix longest symbol length test`](https://lore.kernel.org/20250427200916.GA1661412@ax162/)
* [`[C] Modify -Wdefault-const-init`](https://github.com/llvm/llvm-project/pull/137961#pullrequestreview-2808491619)
* [`__attribute__((nonstring)) and -Wunterminated-string-initialization`](https://github.com/llvm/llvm-project/issues/137705#issuecomment-2843602728)
* [`Re: [PATCH] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/20250501012019.GB3762678@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v10 2/2] riscv: Add runtime constant support`](https://lore.kernel.org/20250401192833.GA3645424@ax162/)
* [`Re: [GIT PULL] more printk for 6.15`](https://lore.kernel.org/20250402203422.GA655609@ax162/)
* [`Re: [PATCH 6.1 051/198] Xen/swiotlb: mark xen_swiotlb_fixup() __init`](https://lore.kernel.org/20250407181218.GA737271@ax162/)
* [`Re: [PATCH v7 2/6] syscall.h: add syscall_set_arguments()`](https://lore.kernel.org/20250408213131.GA2872426@ax162/)
* [`Re: [PATCH 6.13 444/499] x86/tdx: Fix arch_safe_halt() execution for TDX VMs`](https://lore.kernel.org/20250410180423.GA3430900@ax162/)
* [`Boot failures on arm64 after LLVM commit b326cb6792b3951881d63d5a02ea163921da18d9`](https://github.com/ClangBuiltLinux/linux/issues/2082)
* [`Build error in x86 vDSO with x32 enabled using clang and ld.lld`](https://github.com/ClangBuiltLinux/linux/issues/2083)
* [`modpost errors due to undefined __ubsan_handle_*_overflow`](https://github.com/ClangBuiltLinux/linux/issues/2084)
* [`LLVM assembler does not support '.set eva'`](https://github.com/ClangBuiltLinux/linux/issues/2086)
* [`New instances of -Wframe-larger-than in arm64 allmodconfig with clang-17 and earlier after 6f110a5e4f99`](https://github.com/ClangBuiltLinux/linux/issues/2087)
* [`Re: drivers/bluetooth/hci_vhci.o: error: objtool: vhci_coredump_hdr(): STT_FUNC at end of section`](https://lore.kernel.org/20250415003021.GA4163917@ax162/)
* [`call to '__bad_copy_from' declared with 'error' attribute:`](https://github.com/llvm/llvm-project/issues/132359#issuecomment-2807422993)
* [`Re: [PATCH v2 0/2] pidfs: ensure consistent ENOENT/ESRCH reporting`](https://lore.kernel.org/20250415223454.GA1852104@ax162/)
* [`Re: [PATCH 07/10] pinctrl: sx150x: enable building modules with COMPILE_TEST=y`](https://lore.kernel.org/20250416223706.GA3230303@ax162/)
* [`Re: [PATCH] x86/e820: discard high memory that can't be addressed by 32-bit systems`](https://lore.kernel.org/20250417162206.GA104424@ax162/)
* [`Please apply 8dcd71b45df3 to 5.4`](https://lore.kernel.org/20250422005106.GA3437285@ax162/)
* [`Re: [akpm-mm:mm-nonmm-unstable 66/67] fs/ocfs2/aops.c:1133:17: warning: variable 'i' is uninitialized when used here`](https://lore.kernel.org/20250422201241.GA3761951@ax162/)
* [`[SDag][ARM][RISCV] Allow lowering CTPOP into a libcall`](https://github.com/llvm/llvm-project/pull/101786)
* [`Adding __popcountsi2 and __popcountdi2`](https://lore.kernel.org/20250425003342.GA795313@ax162/)
* [`Re: clang and drm issue: objtool warnings from clang build`](https://lore.kernel.org/20250426200513.GA427956@ax162/)
* [`[C] Warn on uninitialized const objects`](https://github.com/llvm/llvm-project/pull/137166#issuecomment-2831550023)
* [`Re: [PATCH 6.1 000/167] 6.1.136-rc1 review`](https://lore.kernel.org/20250430233214.GC3715926@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Add wcslen() patch series to every tree that needs it`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/823)
* [`Apply 96dc364d483bc12ab6f49bd9c214b639497d6198 to android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/824)
* [`Add android16-6.12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/825)
* [`Add wcslen() patch to every tree that needs it (April 7, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/826)
* [`patches: android-mainline: Drop .ARM.attributes patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/827)
* [`Apply 3d9c49ab to mainline trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/828)
* [`patches: Drop stable and 6.12 (April 21, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/829)
* [`patches: Drop 6.6 and 6.1 (April 25, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/830)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [20.1.2](https://lore.kernel.org/20250402203516.GA281819@ax162/)
  * [20.1.3](https://lore.kernel.org/20250416152800.GA570023@ax162/)
  * [20.1.4](https://lore.kernel.org/20250430165343.GA1436080@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
