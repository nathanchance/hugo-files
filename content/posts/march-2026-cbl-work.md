---
title: March 2026 ClangBuiltLinux Work
date: 2026-03-31T22:30:00+0100
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

  * `integrity: Eliminate weak definition of arch_get_secureboot()` ([`v1`](https://lore.kernel.org/20260309-integrity-drop-weak-arch-get-secureboot-v1-1-6460d5c4bb89@kernel.org/))
  * `init/Kconfig: Require a release version of clang-22 for CC_HAS_COUNTED_BY_PTR` ([`v1`](https://lore.kernel.org/20260318-counted_by_ptr-release-clang-22-v1-1-e017da246df0@kernel.org/))

* Kbuild / Kconfig fixes and improvements: These are changes that I have authored as part of maintaining Kbuild and Kconfig.

  * `Documentation: kbuild: Update the debug information notes in reproducible-builds.rst` ([`v1`](https://lore.kernel.org/20260313-kbuild-docs-repro-builds-fdebug-prefix-map-updates-v1-1-3aeeef7fa710@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but matter in some way to my other work.

  * `tracing: Adjust cmd_check_undefined to show unexpected undefined symbols` ([`v1`](https://lore.kernel.org/20260320-cmd_check_undefined-verbose-v1-1-54fc5b061f94@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `ARM: multi_v7_defconfig: Drop duplicate CONFIG_TI_PRUSS=m` ([`v1`](https://lore.kernel.org/20260305-arm-defconfig-drop-duplicate-ti-pruss-v1-1-2839e3b42a8b@kernel.org/))
  * `powerpc/vdso: Drop -DCC_USING_PATCHABLE_FUNCTION_ENTRY from 32-bit flags with clang` ([`v1`](https://lore.kernel.org/20260311-ppc-vdso-drop-cc-using-pfe-define-clang-v1-1-66c790e22650@kernel.org/))
  * `drm/amdgpu/discovery: Add braces to case statements in amdgpu_discovery_table_check()` ([`v1`](https://lore.kernel.org/20260312-amdgpu-fix-clang-c23-extensions-v1-1-59883120a451@kernel.org/))
  * `platform/x86: lenovo: wmi-gamezone: Drop gz_chain_head` ([`v1`](https://lore.kernel.org/20260313-lenovo-wmi-gamezone-remove-gz_chain_head-v1-1-ce5231f0c6fa@kernel.org/))
  * `drm/xe: Fix format specifier for printing pointer differences` ([`v1`](https://lore.kernel.org/20260316-drm-xe-fix-32-bit-wformat-ptrdiff-v1-1-0108b10b2b6b@kernel.org/))
  * `dtc: Remove unused dts_version in dtc-lexer.l` ([`v1`](https://lore.kernel.org/20260325-dtc-lexer-remove-dts_version-v1-1-0b5d64903bbb@kernel.org/))
  * `extract-cert: Wrap key_pass with '#ifdef USE_PKCS11_ENGINE'` ([`v1`](https://lore.kernel.org/20260325-certs-extract-cert-key_pass-unused-but-set-global-v1-1-ecf94326d532@kernel.org/))
  * `modpost: Declare extra_warn with unused attribute` ([`v1`](https://lore.kernel.org/20260325-modpost-extra_warn-unused-but-set-global-v1-1-2e84003b7e81@kernel.org/))
  * `perf/arm-cmn: Fix resource_size_t printk specifier in arm_cmn_init_dtc()` ([`v1`](https://lore.kernel.org/20260325-perf-arm-cmn-fix-resource_size_t-format-v1-1-e84d52ee3e81@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v2 09/13] lib/fonts: Compare font data for equality with font_data_is_equal()`](https://lore.kernel.org/20260305201227.GA2200688@ax162/)
* [`Re: [PATCH kbuild] kbuild: Allow to reduce the number of suffixes for clang thin-lto build`](https://lore.kernel.org/20260306224522.GA2746259@ax162/)
* [`Re: [PATCH] integrity: avoid using __weak functions`](https://lore.kernel.org/20260306225648.GC2746259@ax162/)
* [`Re: [PATCH] scripts: kconfig: merge_config.sh: fix unexpected operator warning`](https://lore.kernel.org/177308909202.2903875.13663071115217399607.b4-ty@kernel.org/)
* [`Re: Patch "ACPI: APEI: GHES: Disable KASAN instrumentation when compile testing with clang < 18" has been added to the 6.12-stable tree`](https://lore.kernel.org/20260309212807.GB3411535@ax162/)
* [`Re: [PATCH 0/9] kbuild: uapi: remove usage of toolchain headers`](https://lore.kernel.org/20260310010021.GA1321389@ax162/)
* [`Re: [PATCH] kbuild: reduce output spam when building out of tree`](https://lore.kernel.org/20260310010716.GA1656066@ax162/)
* [`Re: [PATCH 0/3] check-uapi: improve portability for testing headers`](https://lore.kernel.org/20260310011204.GA2407733@ax162/)
* [`Re: [PATCH kbuild v2] kbuild: Reduce the number of compiler-generated suffixes for clang thin-lto build`](https://lore.kernel.org/20260310031418.GA3988399@ax162/)
* [`Re: [PATCH v2] ACPI: CPPC: Move reference performance to capabilities`](https://lore.kernel.org/20260310212936.GA2143491@ax162/)
* [`Re: [PATCH] scripts: kconfig: merge_config.sh: create tmp file for awk`](https://lore.kernel.org/20260311074813.GA1996626@ax162/)
* [`Re: [PATCH 0/3] Fix merge_config.sh`](https://lore.kernel.org/20260311075029.GB1996626@ax162/)
* [`Re: [PATCH] Documentation/kbuild: fix empty string KBUILD_BUILD_TIMESTAMP`](https://lore.kernel.org/20260311080645.GC1996626@ax162/)
* [`Re: [PATCH RFC 0/5] kbuild: uapi: also test UAPI headers against C++ compilers`](https://lore.kernel.org/20260312081613.GB3161678@ax162/)
* [`Re: [PATCH v5 0/2] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/20260312082502.GA790897@ax162/)
* [`Re: [REGRESSION][PATCH] drm/amd/display: Fix uninitialized variable which breaks full LTO`](https://lore.kernel.org/20260312080245.GA3988095@ax162/)
* [`Re: [PATCH v2] drm/amd/display: Fix uninitialized variable use which breaks full LTO`](https://lore.kernel.org/20260312203740.GA2747807@ax162/)
* [`Re: (subset) [PATCH 1/3] KVM: arm64: fix include path for ring buffer implementation`](https://lore.kernel.org/20260312220829.GA1068615@ax162/)
* [`Re: [PATCH] tracing: Generate undef symbols allowlist for simple_ring_buffer`](https://lore.kernel.org/20260312235153.GA1147071@ax162/)
* [`Re: [PATCH v2] tracing: Generate undef symbols allowlist for simple_ring_buffer`](https://lore.kernel.org/20260313163724.GA2573924@ax162/)
* [`Re: [PATCH] scripts: headers_install.sh: Normalize __ASSEMBLY__ to __ASSEMBLER__`](https://lore.kernel.org/20260313235618.GA4171564@ax162/)
* [`Re: [PATCH 09/14] kbuild: Only run objtool if there is at least one command`](https://lore.kernel.org/20260314004522.GA3886710@ax162/)
* [`Re: [PATCH -tip v3 3/4] x86/segment: Use ASM_INPUT_RM in __loadsegment_fs()`](https://lore.kernel.org/20260314014628.GA1012212@ax162/)
* [```Re: [PATCH v2 1/3] kbuild: rust: add `CONFIG_RUSTC_CLANG_LLVM_COMPATIBLE` ```](https://lore.kernel.org/20260314002606.GA534169@ax162/)
* [`Re: [PATCH] kbuild: pacman-pkg: package unstripped vDSO libraries`](https://lore.kernel.org/20260319223000.GA1282459@ax162/)
* [`Re: [PATCH] kbuild: rust: add AutoFDO support`](https://lore.kernel.org/20260319235443.GB769346@ax162/)
* [`Re: [PATCH v3 15/16] mm: add mmap_action_map_kernel_pages[_full]()`](https://lore.kernel.org/20260320210812.GA3988975@ax162/)
* [`Re: [PATCH] bug: shut up format attribute warning for clang as well`](https://lore.kernel.org/20260321000827.GA1391538@ax162/)
* [`Re: [PATCH] sched/topology: Initialize sd_span after assignment to *sd`](https://lore.kernel.org/20260323224121.GA1420432@ax162/)
* [`Re: [PATCH] compiler: Simplify generic RELOC_HIDE()`](https://lore.kernel.org/20260323232552.GA4147808@ax162/)
* [`Re: [PATCH tip/locking/core] compiler-context-analysis: Add support for multi-argument guarded_by`](https://lore.kernel.org/20260323232831.GB4147808@ax162/)
* [`Re: [PATCH] ppc/pnv: generate dtb after machine initialization is complete`](https://lore.kernel.org/20260324192243.GA1270341@ax162/)
* [`Re: [PATCH] lib: typecheck: initialize const variable to avoid Clang warning`](https://lore.kernel.org/20260326020448.GA1294592@ax162/)
* [`Re: [PATCH v2] kbuild: modules-cpio-pkg: Respect INSTALL_MOD_PATH`](https://lore.kernel.org/20260326022533.GA2302780@ax162/)
* [`Re: [PATCH v2] ppc/pnv: generate dtb after machine initialization is complete`](https://lore.kernel.org/20260327171159.GA3586365@ax162/)
* [`Re: [PATCH] kbuild: add -fdiagnostics-show-inlining-chain for FORTIFY_SOURCE`](https://lore.kernel.org/20260327221837.GA3622500@ax162/)
* [`Re: [PATCH v3] kbuild: modules-cpio-pkg: Respect INSTALL_MOD_PATH`](https://lore.kernel.org/177484921220.1009805.17340922567466216649.b4-ty@b4/)
* [`Re: [PATCH v2] kbuild: add GCC stability warning`](https://lore.kernel.org/20260330054350.GC879042@ax162/)
* [`Re: [PATCH v7 3/3] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/20260330100119.GA1953970@ax162/)
* [`Re: [PATCH] kconfig: forbid multiple entries with the same symbol in a choice`](https://lore.kernel.org/20260330141417.GA1990358@ax162/)
* [`Re: [PATCH tip/locking/core] compiler-context-analysis: Bump required Clang version to 23`](https://lore.kernel.org/20260330141824.GC1990358@ax162/)
* [`Re: [PATCH 02/15] scripts/sbom: integrate script in make process`](https://lore.kernel.org/20260330095011.GA1458050@ax162/)
* [`Re: [PATCH 6.1] net: enetc: fix PF !of_device_is_available() teardown path`](https://lore.kernel.org/20260331153818.GA1992169@ax162/)
* [`Re: [PATCH v2] kbuild: expand inlining hints with -fdiagnostics-show-inlining-chain`](https://lore.kernel.org/20260331155531.GA2004441@ax162/)
* [`Re: [PATCH v9 3/3] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/20260331162729.GA2006419@ax162/)
* [`Re: [PATCH v2 0/4] kbuild: vdso_install: drop build ID architecture allow-list`](https://lore.kernel.org/177499159014.1741142.7219917962979782500.b4-review@b4/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: KCFLAGS vs KCPPFLAGS for -fdebug-prefix-map`](https://lore.kernel.org/20260302222914.GA3292653@ax162/)
* [`Re: [PATCH v1 4/8] ACPI: x86/rtc-cmos: Use platform device for driver binding`](https://lore.kernel.org/20260303060752.GA2749263@ax162/)
* [`Re: [patch 20/48] x86/apic: Enable TSC coupled programming mode`](https://lore.kernel.org/20260303012905.GA978396@ax162/)
* [`Re: [PATCH v2 09/13] lib/fonts: Compare font data for equality with font_data_is_equal()`](https://lore.kernel.org/20260305002347.GA4102761@ax162/)
* [`Re: [PATCH 5.15 000/410] 5.15.202-rc2 review`](https://lore.kernel.org/20260305220801.GA3148061@ax162/)
* [`Re: linux-next: manual merge of the kvm-x86 tree with the tip tree`](https://lore.kernel.org/20260305192401.GA1783111@ax162/)
* [`Re: [PATCH 0/2] kbuild: Switch from '-fms-extensions' to '-fms-anonymous-structs' when available`](https://lore.kernel.org/20260306231705.GD2746259@ax162/)
* [`Re: clang/objtool failures on linux-next`](https://lore.kernel.org/20260306232505.GF2746259@ax162/)
* [`Re: [PATCH v3 1/3] scripts: kconfig: merge_config.sh: refactor from shell/sed/grep to awk`](https://lore.kernel.org/20260309170904.GA2779008@ax162/)
* [`Re: [PATCH v2] ACPI: CPPC: Move reference performance to capabilities`](https://lore.kernel.org/20260310003026.GA2639793@ax162/)
* [`Re: Hosting first-party kernel.org container images`](https://lore.kernel.org/20260310004815.GA3293460@ax162/)
* [`[SystemZ] Add a SystemZ specific pre-RA scheduling strategy`](https://github.com/llvm/llvm-project/pull/135076#issuecomment-4043240672)
* [`Re: [PATCH v14 18/30] tracing: Check for undefined symbols in simple_ring_buffer`](https://lore.kernel.org/20260311221816.GA316631@ax162/)
* [```Re: [linux-next:master 5585/5731] error[E0277]: `*const kernel::bindings::vm_uffd_ops` cannot be shared between threads safely```](https://lore.kernel.org/20260313213638.GA147391@ax162/)
* [`Re: [PATCH v5 1/2] kbuild: move vmlinux.a build rule to scripts/Makefile.vmlinux_a`](https://lore.kernel.org/20260316175953.GA1910339@ax162/)
* [`Apply e2ffa15b9baa and fde0ab43b9a3 to 6.12`](https://lore.kernel.org/20260316233716.GA758717@ax162/)
* [`NULL pointer dereference when booting ppc64_guest_defconfig in QEMU on -next`](https://lore.kernel.org/20260319233745.GA769346@ax162/)
* [`Re: [tip: sched/core] sched/topology: Compute sd_weight considering cpuset partitions`](https://lore.kernel.org/20260320235824.GA1176840@ax162/)
* [`Re: [PATCH v2] staging: rtl8723bs: remove reduntant functions`](https://lore.kernel.org/20260323230000.GA2585066@ax162/)
* [`Re: [PATCH v2] ppc/pnv: fix dumpdtb option`](https://lore.kernel.org/20260323231612.GA2637687@ax162/)
* [`Re: [PATCH 6.1 379/481] net: enetc: allocate vf_state during PF probes`](https://lore.kernel.org/20260330073356.GA1017537@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Updates patches (March 5, 2026)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/920)
* [`build-rust.py: Introduce '--configure-set-args'`](https://github.com/ClangBuiltLinux/tc-build/pull/324)
* [`Drop hwmon macsmc patch from mainline and -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/921)
* [`Update korg-clang-22 to 22.1.1`](https://github.com/kernelci/tuxmake/pull/253)
* [`patches: stable: Drop drivers/hwmon/macsmc-hwmon.c change`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/922)
* [`generator: Drop build frequencies`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/923)
* [`Update korg-clang-22 to 22.1.2`](https://github.com/kernelci/tuxmake/pull/256)
* Switch to `ruff format` for Python in ClangBuiltLinux repositories
  * [actions-workflow](https://github.com/ClangBuiltLinux/actions-workflows/pull/11)
  * [boot-utils](https://github.com/ClangBuiltLinux/boot-utils/pull/129)
  * [continuous-integration2](https://github.com/ClangBuiltLinux/continuous-integration2/pull/924)
  * [tc-build](https://github.com/ClangBuiltLinux/tc-build/pull/325)
* [`Disable i386 builds with clang < 17 on 6.12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/925)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [22.1.1](https://lore.kernel.org/20260311071652.GA1991880@ax162/)
  * [22.1.2](https://lore.kernel.org/20260325000809.GA618985@ax162/)

* I submitted the following pull requests.

  * [`[GIT PULL] Kbuild fixes for 7.0 #2`](https://lore.kernel.org/20260307011249.GA748682@ax162/)
  * [`[GIT PULL] Kbuild fixes for 7.0 #3`](https://lore.kernel.org/20260324211035.GA2629374@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
