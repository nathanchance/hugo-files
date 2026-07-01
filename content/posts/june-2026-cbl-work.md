---
title: June 2026 ClangBuiltLinux Work
date: 2026-06-30T16:30:00-0700
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

  * `can: virtio: Fix comment in UAPI header` ([`v1`](https://lore.kernel.org/20260604-virtio_can-fix-uapi-comment-v1-1-199fa96ec5f0@kernel.org/))
  * `cfi: Include uaccess.h for get_kernel_nofault()` ([`v1`](https://lore.kernel.org/20260604-tracing-fix-cfi-h-build-error-v1-1-b27015390901@kernel.org/))
  * `spi: cadence-xspi: Revert COMPILE_TEST support` ([`v1`](https://lore.kernel.org/20260606-spi-cadence-xspi-revert-compile-testing-v1-1-76219ea378bd@kernel.org/))
  * `vduse: Fix error around jumping over a __cleanup() variable` ([`v1`](https://lore.kernel.org/20260610-vduse_vq_kick-fix-guard-usage-v1-1-0ce02c08006e@kernel.org/))
  * `x86/boot/compressed: Disable jump tables for clang` ([`v1`](https://lore.kernel.org/20260623-x86-boot-compressed-disable-jt-clang-v1-1-575fccd58107@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but matter in some way to my other work.

  * `btrfs: Drop WQ_PERCPU from ordered_flags in btrfs_init_workqueues()` ([`v1`](https://lore.kernel.org/20260601-btrfs-fix-wq-warning-qgroup-rescan-v1-1-aff9a1128f27@kernel.org/))
  * `driver core: Use system_percpu_wq instead of system_wq` ([`v1`](https://lore.kernel.org/20260601-driver-core-fix-system_wq-warning-v1-1-f9001a70ee25@kernel.org/))
  * `drm/amd/display: Shorten hdmi_frl_status_polling_workqueue` ([`v1`](https://lore.kernel.org/20260604-amdgpu-fix-wq_name_len-warning-v1-1-eb5415b45b27@kernel.org/), [`v2`](https://lore.kernel.org/20260618-amdgpu-fix-wq_name_len-warning-v2-1-ef0e2e6f5be7@kernel.org/))
  * `kbuild: Use ld.lld for linking host programs when LLVM is set` ([`v1`](https://lore.kernel.org/20260610-kbuild-use-lld-for-linking-hostprogs-v1-1-70396fe42ee3@kernel.org/))
  * `io_uring: Use system_dfl_wq instead of system_unbound_wq to fix warning` ([`v1`](https://lore.kernel.org/20260616-io_uring-fix-wq-warning-v1-1-cfc9d934eedb@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Backports of 175db11786bde9061db526bf1ac5107d915f5163 for 6.6 and older`](https://lore.kernel.org/20260606032024.GA3120787@ax162/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `wifi: mac80211: Fix -Wc23-extensions in hwmp_route_info_get()` ([`v1`](https://lore.kernel.org/20260604-mac80211-mesh_hwmp-fix-c23-extensions-v1-1-25a64d6ce541@kernel.org/))
  * `power: supply: max17042_battery: Use modern PM ops to clear up warning` ([`v1`](https://lore.kernel.org/20260604-max17042_battery-fix-unused-suspend_soc_alerts-v1-1-3562a68e6f36@kernel.org/))
  * `MIPS: lib: Remove '.hidden' for local symbols` ([`v1`](https://lore.kernel.org/20260608-mips-fix-binutils-visibility-warning-v1-1-3c809cfb5a9d@kernel.org/))
  * `MIPS: VDSO: Avoid including .got in dynamic segment` ([`v1`](https://lore.kernel.org/20260609-mips-vdso-fix-section-layout-v1-1-0e80ffadf7c7@kernel.org/))
  * `fortify: Disable -Wstringop-overread in tests` ([`v1`](https://lore.kernel.org/20260623-fix-test_fortify-for-clang-stringop-overread-v1-1-15ee8342a953@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [```Re: [PATCH v3] kbuild: rust: make `*.long-type-*.txt` a target for cleanup```](https://lore.kernel.org/20260601192305.GA965997@ax162/)
* [`Re: [PATCH v3 1/2] kconfig: Remove the architecture specific config for AutoFDO`](https://lore.kernel.org/20260601192458.GB965997@ax162/)
* [`Re: [PATCH v3 2/2] kconfig: Remove the architecture specific config for Propeller`](https://lore.kernel.org/20260603015354.GD1940387@ax162/)
* [`Re: [PATCH v12 2/3] kbuild: change --thin back to 'T' in $(AR)`](https://lore.kernel.org/20260603014520.GC1940387@ax162/)
* [```Re: [PATCH] kbuild: rust: rename flag to `-Zdebuginfo-for-profiling` for Rust >= 1.98```](https://lore.kernel.org/20260603015559.GA3013922@ax162/)
* [`Re: [PATCH] kbuild: try readelf first in gen_symversions`](https://lore.kernel.org/20260604013858.GB1329739@ax162/)
* [`Re: [RFC PATCH v2 2/6] kcov: add build system support for dataflow instrumentation`](https://lore.kernel.org/20260604214840.GA3915915@ax162/)
* [`Re: [PATCH kselftest] selftests: harness: Add __maybe_unused to _##fixture_name##_##test_name##_object.`](https://lore.kernel.org/20260604220553.GA3936720@ax162/)
* [`Re: [PATCH 2/3] vmsplice: make vmsplice a trivial wrapper for preadv2/pwritev2`](https://lore.kernel.org/20260605015724.GA520134@ax162/)
* [`Re: [PATCH] init/Kconfig: Update the THREAD_INFO_IN_TASK description`](https://lore.kernel.org/20260608214558.GA2340474@ax162/)
* [`Re: [PATCH] kbuild: normalize paths in quiet compile output`](https://lore.kernel.org/20260608220859.GC2340474@ax162/)
* [`Re: [PATCH] filelock: update break_lease() stub with correct arguments`](https://lore.kernel.org/20260608221014.GD2340474@ax162/)
* [`Re: [PATCH v2 00/16] Bump minimum version of LLVM for building the kernel to 17.0.1`](https://lore.kernel.org/178104773630.2704789.17858317658280322569.b4-ty@b4/)
* [`Re: [PATCH] run-clang-tools: run multiprocessing.Pool as context manager`](https://lore.kernel.org/178104854408.2704789.17704419269230550087.b4-ty@b4/)
* [`Re: [PATCH v12 0/3] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/178104893725.2707941.7165957214704818524.b4-ty@b4/)
* [`Re: [PATCH] modpost: Add __llvm_covfun and __llvm_covmap to section_white_list`](https://lore.kernel.org/178104908935.2707941.7429097671960685644.b4-ty@b4/)
* [`Re: [PATCH] Revert "i2c: designware: defer probe if child GpioInt controllers are not bound"`](https://lore.kernel.org/20260610165939.GA3440020@ax162/)
* [`Re: [PATCH 0/2] arm64: ftrace: support DIRECT_CALLS without CALL_OPS`](https://lore.kernel.org/20260610233647.GB3099686@ax162/)
* [`[PowerPC] Fix crash when promoting smaller types to 64 bit ucmp`](https://github.com/llvm/llvm-project/pull/203399#issuecomment-4699108847)
* [`Re: [PATCH] x86/cfi: Use symmetric SYM_START and SYM_END in __CFI_TYPE()`](https://lore.kernel.org/20260613162923.GB2697431@ax162/)
* [`Re: [PATCH] net: correcting section tags for .init and .exit data/functions`](https://lore.kernel.org/20260613170143.GB3152432@ax162/)
* [`Re: [PATCH v6 6/6] s390: Enable Rust support`](https://lore.kernel.org/20260615164013.GA249489@ax162/)
* [`Re: [PATCH] modpost: Ignore Clang LTO suffixes in symbol matching`](https://lore.kernel.org/20260617210410.GA3894029@ax162/)
* [`Re: [PATCH] modpost: release allocation when early return no suffix .o in read_symbols()`](https://lore.kernel.org/178167011232.2064238.5669414796099955471.b4-review@b4/)
* [`Re: [PATCH] x86/kcfi: Optimize call sequence`](https://lore.kernel.org/20260617223728.GA3913972@ax162/)
* [`Re: [PATCH] docs: kbuild: remove ISDN references in Makefile examples`](https://lore.kernel.org/178175273131.3916864.13607634190318049114.b4-review@b4/)
* [`Re: [PATCH 17/29] ksmbd: return requested create allocation size`](https://lore.kernel.org/20260622230109.GA3328@ax162/)
* [`Re: [PATCH v4 2/2] tracing: Remove trace_printk.h from kernel.h`](https://lore.kernel.org/20260625234158.GA261868@ax162/)
* [`Re: [PATCH] kbuild: Use --force-group-allocation when linking modules`](https://lore.kernel.org/178278447248.3414597.4179963162938722029.b4-review@b4/)
* [`Re: [PATCH 1/4] scripts: fix spelling mistakes`](https://lore.kernel.org/178278503295.3414597.965604791088936644.b4-review@b4/)
* [`Re: [PATCH v2] modpost: prevent leak when early return no suffix .o in read_symbols()`](https://lore.kernel.org/178278682007.3414597.2722879998608613962.b4-review@b4/)
* [`Re: [PATCH] tools/compiler: match glibc 2.42 definition of __attribute_const__`](https://lore.kernel.org/20260630184241.GA327950@ax162/)
* [`Re: [PATCH] selftests: harness: Mark test fixture objects __maybe_unused`](https://lore.kernel.org/20260701024906.GA3960633@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v8 2/2] i2c: designware: defer probe if child GpioInt controllers are not bound`](https://lore.kernel.org/20260602185339.GA404948@ax162/)
* [`Re: linux-next: manual merge of the kbuild tree with the clang-fixes tree`](https://lore.kernel.org/20260603011629.GA1940387@ax162/)
* [`Re: linux-next: manual merge of the jc_docs tree with the kbuild tree`](https://lore.kernel.org/20260603011751.GB1940387@ax162/)
* [`Re: [brauner-vfs:vfs-7.2.vmsplice 2/4] fs/read_write.c:1219:1: warning: alias and aliasee have different types 'long (unsigned long, const struct iovec *, unsigned long, unsigned int)' and 'long (typeof (__builtin_choose_expr((__builtin_types_compatible_p(typeof ((unsigned long)0), typeof (0LL...`](https://lore.kernel.org/20260603015808.GA3014010@ax162/)
* [`Re: [akpm-mm:mm-unstable 274/423] mm/userfaultfd.c:4475:1: warning: alias and aliasee have different types 'long (ulong, ulong, ulong, ulong, ulong, ulong, ulong)' (aka 'long (unsigned long, unsigned long, unsigned long, unsigned long, unsigned long, unsigned long, unsigned long)') and 'l...`](https://lore.kernel.org/20260604011921.GA1329739@ax162/)
* [`"Unexpected run-time relocations (.rela) detected!" when building x86_64 bzImage after LLVM commit fa02a6ed66b1700c996b49c96c6bc0eb014c9518`](https://github.com/ClangBuiltLinux/linux/issues/2165)
* [`Re: [tip: timers/ptp] KVM: arm64: Use ktime_get_snapshot_id() to retrieve CLOCK_BOOTTIME`](https://lore.kernel.org/20260604015506.GA1998428@ax162/)
* [`Re: [PATCH v3] spi: cadence-xspi: Support 32bit and 64bit slave dma interface`](https://lore.kernel.org/20260605020634.GA1820810@ax162/)
* [`Re: linux-next: build failure after merge of the drm-xe tree`](https://lore.kernel.org/20260605060308.GA249502@ax162/)
* [`Re: [GIT PULL] tracing: Fixes for 7.1`](https://lore.kernel.org/20260605155804.GA3472545@ax162/)
* [`[Legalizer] Add support for promoting integers for s/ucmp`](https://github.com/llvm/llvm-project/pull/198554#issuecomment-4635148286)
* [`Re: [brauner-github:vfs-7.2.misc 28/28] fs/open.c:112:29: warning: converting the result of '<<' to a boolean always evaluates to true`](https://lore.kernel.org/20260607041414.GA2161204@ax162/)
* [`Re: -next boot failures during KVM setup`](https://lore.kernel.org/20260608232753.GA2993766@ax162/)
* [`Re: [REGRESSION] next/master: (build) stack frame size (2088) exceeds limit (2048) in 'dml31_ModeSupport...`](https://lore.kernel.org/20260609003238.GA3576576@ax162/)
* [`Re: [PATCH v6 2/2] pinctrl: qcom: lpass-lpi: Switch to PM clock framework for runtime PM`](https://lore.kernel.org/20260609231238.GA1901681@ax162/)
* [`HOSTCC invocations don't use LLD with LLVM=1`](https://github.com/ClangBuiltLinux/linux/issues/2167#issuecomment-4666433680)
* [`Re: [PATCH v4 0/2] workqueue: Add warnings and check WQ flags usage`](https://lore.kernel.org/20260610172823.GA804799@ax162/)
* [`Re: [PATCH] lib/xz: replace min_t with min`](https://lore.kernel.org/20260610232323.GA1071374@ax162/)
* [`Re: [PATCH v4 2/2] pinctrl: ultrarisc: Add UltraRISC DP1000 pinctrl driver`](https://lore.kernel.org/20260613164847.GA3152104@ax162/)
* [`Re: [PATCH v7 2/2] lib: bitmap: optimize bitmap_find_next_zero_area_off()`](https://lore.kernel.org/20260630205240.GA921840@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`tc-build: Address new ty warnings`](https://github.com/ClangBuiltLinux/tc-build/pull/337)
* [`tc_build: tools: Fix LD printing when using GCC`](https://github.com/ClangBuiltLinux/tc-build/pull/338)
* [`Update korg-clang-22 to 22.1.7`](https://github.com/kernelci/tuxmake/pull/286)
* [`Drop clang-15 and clang-16 builds from mainline, next, and tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/929)
* [`build-llvm.py: Hide ty "invalid-argument-type" warning`](https://github.com/ClangBuiltLinux/tc-build/pull/339)
* [`Update stable anchor to 7.1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/930)
* [`Bump PGO kernel to 7.1 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/340)
* [`build-binutils.py: 2.46.1`](https://github.com/ClangBuiltLinux/tc-build/pull/341)
* [`Update korg-clang-22 to 22.1.8`](https://github.com/kernelci/tuxmake/pull/289)
* [`Apply fix for -Wattribute-alias to Android trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/931)
* Regular GitHub Actions updates
  * [`actions-workflows`](https://github.com/ClangBuiltLinux/actions-workflows/pull/16)
  * [`continuous-integration2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/932)
  * [`tc-build`](https://github.com/ClangBuiltLinux/tc-build/pull/342)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [22.1.7](https://lore.kernel.org/20260602215932.GA1352412@ax162/)
  * [22.1.8](https://lore.kernel.org/20260617020040.GA3268077@ax162/)

* I submitted the following pull requests.

  * [`[GIT PULL] Kbuild / Kconfig updates for 7.2`](https://lore.kernel.org/20260610235818.GA873456@ax162/)
  * [`[GIT PULL] Kbuild updates for 7.2 #2`](https://lore.kernel.org/20260624225711.GA1132314@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
