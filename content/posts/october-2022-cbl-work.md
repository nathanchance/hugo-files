---
title: "October 2022 ClangBuiltLinux Work"
date: 2022-10-31T16:30:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Build errors: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `arm64: alternatives: Use vdso/bits.h instead of linux/bits.h` ([`v1`](https://lore.kernel.org/20221003193759.1141709-1-nathan@kernel.org/))
  * `drm/amd/display: Fix build breakage with CONFIG_DEBUG_FS=n` ([`v1`](https://lore.kernel.org/20221014152102.1755050-1-nathan@kernel.org/))
  * `lib/Kconfig.debug: Add check for non-constant .{s,u}leb128 support to DWARF5` ([`v2`](https://lore.kernel.org/20221014204210.383380-1-nathan@kernel.org/))
  * `pinctrl: qcom: Include bitfield.h in pinctrl-lpass-lpi.c` ([`v1`](https://lore.kernel.org/20221027191625.1738204-1-nathan@kernel.org/))
  * `bpf: Mark bpf_arch_init_dispatcher_early() as __init_or_module` ([`v1`](https://lore.kernel.org/20221031173819.2344270-1-nathan@kernel.org/))

* Miscellaneous fixes: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `drm/i915: Fix CFI violations in gt_sysfs` ([`v2`](https://lore.kernel.org/20221013205909.1282545-1-nathan@kernel.org/))
  * `drm/amdkfd: Fix type of reset_type parameter in hqd_destroy() callback` ([`v1`](https://lore.kernel.org/20221017162837.3698-1-nathan@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Backport of 607e57c6c62c009 for 5.10 and 5.15`](https://lore.kernel.org/Y0heKubSc1P6rbNB@dev-arch.thelio-3990X/)
  * [`Apply 0a6de78cff60 to 5.15+`](https://lore.kernel.org/Y02WW7iIeWPFTV8L@dev-arch.thelio-3990X/)
  * [`Re: FAILED: patch "[PATCH] x86/Kconfig: Drop check for -mabi=ms for CONFIG_EFI_STUB" failed to apply to 5.15-stable tree`](https://lore.kernel.org/Y1lhm3mNdI0PFbLe@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `fs/ntfs3: Don't use uni1 uninitialized in ntfs_d_compare()` ([`v1`](https://lore.kernel.org/20221004144145.1345772-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20221004232359.285685-1-nathan@kernel.org/))
  * `of: Define of_match_ptr() with PTR_IF() to avoid unused variable warnings` ([`v1`](https://lore.kernel.org/20221013195153.2767632-1-nathan@kernel.org/))
  * `mm/memremap: Mark folio_span_valid() as __maybe_unused` ([`v1`](https://lore.kernel.org/20221018152645.3195108-1-nathan@kernel.org/))
  * `coresight: cti: Remove unused variables in cti_{dis,en}able_hw()` ([`v1`](https://lore.kernel.org/20221024151201.2215380-1-nathan@kernel.org/))
  * `drm/amdgpu: Fix uninitialized warning in mmhub_v2_0_get_clockgating()` ([`v1`](https://lore.kernel.org/20221024151953.2238616-1-nathan@kernel.org/))
  * `mm/khugepaged: Initialize index and nr in collapse_file()` ([`v1`](https://lore.kernel.org/all/20221025173407.3423241-1-nathan@kernel.org/))
  * `hwmon: (smpro-hwmon) Add missing break in smpro_is_visible()` ([`v1`](https://lore.kernel.org/20221027195238.1789586-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20221027231611.3824800-1-nathan@kernel.org/))



## LLVM patches

* [`Revert "[AArch64]SME2 Outer Product and Accumulate instructions"`](https://github.com/llvm/llvm-project/commit/23c50432fb25775fe0eb958bc8c2a099b0a0c286)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] lib/Kconfig.debug: Add check for non-constant .{s,u}leb128 support to DWARF5`](https://lore.kernel.org/YzsJi7sT54dJtvKw@dev-arch.thelio-3990X/)
* [`[Driver] Prevent Mips specific code from claiming -mabi argument on other targets.`](https://reviews.llvm.org/D134671#3830971)
* [`Re: [PATCH] hardening: Remove Clang's enable flag for -ftrivial-auto-var-init=zero`](https://lore.kernel.org/YzsQr%2FDqrNzJILkr@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/3] Kconfig.debug: simplify the dependency of DEBUG_INFO_DWARF4/5`](https://lore.kernel.org/YzsSD2d7YPPW0rz%2F@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/3] Kconfig.debug: add toolchain checks for DEBUG_INFO_DWARF_TOOLCHAIN_DEFAULT`](https://lore.kernel.org/YzsXa0GCGT6A0szV@dev-arch.thelio-3990X/)
* [`Re: [PATCH 3/3] Kconfig.debug: split debug-level and DWARF-version into separate choices`](https://lore.kernel.org/YzsZzjjJFcPILOji@dev-arch.thelio-3990X/)
* [`Re: [Intel-gfx] [PATCH] drm/i915: Fix CFI violations in gt_sysfs`](https://lore.kernel.org/Yzsf+B8eVGfKD1dJ@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 33/39] x86/cpufeatures: Limit shadow stack to Intel CPUs`](https://lore.kernel.org/YzxViiyfMRKrmoMY@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ARM: NWFPE: avoid compiler-generated __aeabi_uldivmod`](https://lore.kernel.org/Y0hFCzVck%2FzBFwiX@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: add -fno-discard-value-names to cmd_cc_ll_c`](https://lore.kernel.org/Y0htZDJoTuQegVQR@dev-arch.thelio-3990X/)
* [`Re: [PATCH] sched: Introduce struct balance_callback to avoid CFI mismatches`](https://lore.kernel.org/Y0huQNtO8bA2j98Y@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] Remove Intel compiler support`](https://lore.kernel.org/Y0hu2%2FRZWcWAumLy@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/2] riscv: fix detection of toolchain Zicbom support`](https://lore.kernel.org/Y0hzl75d11uWC+f3@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] riscv: fix detection of toolchain Zihintpause support`](https://lore.kernel.org/Y0h1WK0Tmk0UXjmd@dev-arch.thelio-3990X/)
* [`[PATCH v4 0/4] pass -march= only to compiler`](https://lore.kernel.org/20221014201354.3190007-1-ndesaulniers@google.com/)
* [`Re: [PATCH v2] fs/select: mark do_select noinline_for_stack`](https://lore.kernel.org/Y0nlJ6whtJuZddjr@dev-arch.thelio-3990X/)
* [`Re: upgrade the orphan section warning to a hard link error`](https://lore.kernel.org/Y02eZ6A%2Fvlj8+B%2Fc@dev-arch.thelio-3990X/)
* [`Re: [PATCH] string: Convert strscpy() self-test to KUnit`](https://lore.kernel.org/Y072ZMk%2FhNkfwqMv@dev-arch.thelio-3990X/)
* [`Re: [PATCH] fortify: Short-circuit known-safe calls to strscpy()`](https://lore.kernel.org/Y075PIwTnnYF3Ak7@dev-arch.thelio-3990X/)
* [`Re: [RESEND PATCH v5] x86, mem: move memmove to out of line assembler`](https://lore.kernel.org/Y08NJohEeoYX2aIf@thelio-3990X/)
* [`Re: [PATCH v10 00/11] landlock: truncate support`](https://lore.kernel.org/Y08pn5GcTvd5sgyE@dev-arch.thelio-3990X/)
* [`Re: [PATCH][next] wifi: rtl8xxxu: Fix reads of uninitialized variables hw_ctrl_s1, sw_ctrl_s1`](https://lore.kernel.org/Y1FlEABysKCjobzu@thelio-3990X/)
* [`Re: [PATCH] Makefile.debug: support for -gz=zstd`](https://lore.kernel.org/Y1GV9sHyODVmBbFW@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4] overflow: Introduce overflows_type() and castable_to_type()`](https://lore.kernel.org/Y1LDctjps1M8MuK8@dev-arch.thelio-3990X/)
* [```[PATCH 1/5] compiler-gcc: be consistent with underscores use for `no_sanitize````](https://lore.kernel.org/all/20221021115956.9947-1-ojeda@kernel.org/)
* [`Re: [PATCH 1/1] kbuild: upgrade the orphan section warning to an error if CONFIG_WERROR is set`](https://lore.kernel.org/Y1bLk47I4pyEmJVi@dev-arch.thelio-3990X/)
* [`Re: backports of 32ef9e5054ec ("Makefile.debug: re-enable debug info for .S files")`](https://lore.kernel.org/Y1gUoS73T4nycQwr@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] kbuild: fix SIGPIPE error message for AR=gcc-ar and AR=llvm-ar`](https://lore.kernel.org/Y1r3KAyhFbwJ1W1d@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 1/1] drm/amd/display: add DCN support for ARM64`](https://lore.kernel.org/Y1vwk3J3HPGugBJO@dev-arch.thelio-3990X/)
* [`Re: [PATCH] s390: always build relocatable kernel`](https://lore.kernel.org/Y1%2Fv3TwIT1yEFm+o@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scripts/min-tool-version.sh: raise minimum clang version to 15.0.0 for s390`](https://lore.kernel.org/Y1%2Fworpj0COQvC5V@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/imx: imx-tve: Fix return type of imx_tve_connector_mode_valid`](https://lore.kernel.org/Y2BBjhdk2ZIe9RGp@dev-arch.thelio-3990X/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`[qemu-system-s390x] 7.1.0-6 fails to starts due to missing firmware`](https://bugs.archlinux.org/task/76092)
* [```Re: [linux-next:master 7287/11993] s390x-linux-ld: topology.c:undefined reference to `__tsan_memcpy'```](https://lore.kernel.org/YzsMHqG9LvMZXTz8@dev-arch.thelio-3990X/)
* [`Re: [PATCH v8 4/9] landlock: Support file truncation`](https://lore.kernel.org/YzyQASSaeVqRlTsO@dev-arch.thelio-3990X/)
* [`"fixup value out of range" with arm64 allmodconfig + ThinLTO with kCFI`](https://github.com/ClangBuiltLinux/linux/issues/1730)
* [`Re: [PATCH v9 00/11] landlock: truncate support`](https://lore.kernel.org/Y0g+TEgGGhZDm7MX@dev-arch.thelio-3990X/)
* [`Re: sound/soc/codecs/src4xxx-i2c.c:28:34: warning: unused variable 'src4xxx_of_match'`](https://lore.kernel.org/Y0hspolUEMPePK9y@dev-arch.thelio-3990X/)
* [`Re: [PATCH stable 5.15 0/2] kbuild: Fix compilation for latest pahole release`](https://lore.kernel.org/Y02Yv%2FubuCtVhtZk@dev-arch.thelio-3990X/)
* [`Re: [mm] f35b5d7d67: will-it-scale.per_process_ops -95.5% regression`](https://lore.kernel.org/Y1DNQaoPWxE+rGce@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 48/59] x86/retbleed: Add SKL return thunk`](https://lore.kernel.org/Y1HVZKW4o0KRsMtq@dev-arch.thelio-3990X/)
* [`s390 misaligned symbol linker errors`](https://github.com/ClangBuiltLinux/linux/issues/1747)
* [`Unknown results and empty builds.json`](https://gitlab.com/Linaro/tuxsuite/-/issues/179)
* [`Re: [PATCH 03/16] ASoC: SOF: ops: add readb/writeb helpers`](https://lore.kernel.org/Y1rTFrohLqaiZAy%2F@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 28/28] mfd: intel-lpss: Remove #ifdef guards for PM related functions`](https://lore.kernel.org/Y1%2FjvTbEpONmQzW6@dev-arch.thelio-3990X/)
* [`Re: [PATCH 5.4 086/255] once: add DO_ONCE_SLOW() for sleepable contexts`](https://lore.kernel.org/Y2ATiXtpwPxfsOUD@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`qemu: Temporarily switch to testing repo for updated QEMU packages`](https://github.com/ClangBuiltLinux/containers/pull/44)
* [`Adjust CFI builds for kCFI in mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/451)
* [`boot-qemu.py: Fix get_linux_ver_code() when CONFIG_LOCALVERSION_AUTO is not set`](https://github.com/ClangBuiltLinux/boot-utils/pull/74)
* [`build-llvm.py: Adjust BOLT + PGO warning for upstream fix`](https://github.com/ClangBuiltLinux/tc-build/pull/205)
* [`Fix application of NWFPE patches on Android and stable trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/458)
* [`check_logs.py: Increase attempts to verify build status to 9`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/459)
* [`Update to Linux 6.0 and the known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/206)
* [`Allow user to control binutils source folder location and installation`](https://github.com/ClangBuiltLinux/tc-build/pull/207)
* [`CONFIG_KASAN_KUNIT_TEST requires tracing now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/460)
* [`patches: Apply patch for bpf recordmcount issue`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/461)
* [`patches: Apply 2120635108b35ecad9c59c8b44f6cbdf4f98214e to arm64 trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/462)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, an Intel-based laptop, and a SolidRun Honeycomb LX2. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
