---
title: May 2026 ClangBuiltLinux Work
date: 2026-05-29T16:30:00-0700
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

  * `RDMA/core: Remove _ib_copy_validate_udata_in() and _ib_respond_udata() stubs` ([`v1`](https://lore.kernel.org/20260519-rdma-core-fix-ib_udata-redef-errors-v1-1-671bf2697fa5@kernel.org/))
  * `ASoC: Address es9356 build failures without CONFIG_SND_SOC_SDCA` ([`v1`](https://lore.kernel.org/20260526-es9356-dep-fixes-v1-0-39ac16f43d54@kernel.org/))
  * `audit: Update audit_alloc_mark() and audit_dupe_exe() CONFIG_AUDITSYSCALL=n stubs` ([`v1`](https://lore.kernel.org/20260527-audit-update-macro-stubs-v1-1-8cda8dbdae0a@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but matter in some way to my other work.

  * `ARM: Do not select HAVE_RUST when KASAN is enabled` ([`v1`](https://lore.kernel.org/20260511-arm-avoid-rust-with-kasan-v1-1-24d55f4a900b@kernel.org/))
  * `Bump minimum version of LLVM for building the kernel to 17.0.1` ([`v2`](https://lore.kernel.org/20260517-bump-minimum-supported-llvm-version-to-17-v2-0-b3b8cda46bdd@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `usb: typec: intel_pmc_mux: Zero initialize num_ports in pmc_usb_probe()` ([`v1`](https://lore.kernel.org/20260506-typec-intel_pmc_mux-fix-uninit-num_ports-v1-1-929b128a32e9@kernel.org/))
  * `Disable -Wattribute-alias for clang-23 and newer` ([`v1`](https://lore.kernel.org/20260515-syscall-disable-attribute-alias-for-clang-v1-1-9a9d95d41df6@kernel.org/))
  * `HID: core: Fix size_t specifier in hid_report_raw_event()` ([`v1`](https://lore.kernel.org/20260517-hid-core-fix-size_t-specifier-v1-1-bfdd959ec383@kernel.org/))
  * `drm/msm: Restore second parameter name in purge() and evict()` ([`v1`](https://lore.kernel.org/20260518-drm-msm-fix-c23-extensions-v1-1-0833559418c7@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 01/14] kbuild: Bump minimum version of LLVM for building the kernel to 17.0.1`](https://lore.kernel.org/20260506062128.GA322298@ax162/)
* [`Re: [PATCH v3 1/2] slab: support for compiler-assisted type-based slab cache partitioning`](https://lore.kernel.org/20260507093843.GA1826581@ax162/)
* [`Re: [PATCH v5 00/15] add SPDX SBOM generation script`](https://lore.kernel.org/20260507094215.GB1826581@ax162/)
* [`Re: [PATCH] ASoC: tegra: tegra210-mixer: Use div_u64() for 64-bit division`](https://lore.kernel.org/20260508123320.GD208829@ax162/)
* [`build-llvm.py: Fix DEFAULT_KERNEL_FOR_PGO version parsing`](https://github.com/ClangBuiltLinux/tc-build/pull/334#issuecomment-4412711106)
* [`build-llvm.py: Fix DEFAULT_KERNEL_FOR_PGO version parsing`](https://github.com/ClangBuiltLinux/tc-build/pull/335#pullrequestreview-4257905724)
* [`Re: [PATCH v8] kbuild: host: use single executable for rustc -C linker`](https://lore.kernel.org/20260511065915.GA325559@ax162/)
* [`Re: [PATCH] ARM: OMAP2+: Make OMAP4 finish_suspend callback CFI-safe`](https://lore.kernel.org/20260512073442.GA570003@ax162/)
* [`Re: [PATCH v4 02/12] block/bdev: Annotate the blk_holder_ops callback functions`](https://lore.kernel.org/20260513133614.GA703152@ax162/)
* [`Re: [PATCH] kbuild: pacman-pkg: package unstripped vDSO libraries`](https://lore.kernel.org/177876285334.4076323.14187523243144807265.b4-ty@b4/)
* [`Re: [PATCH] sparc: Disable compat support with LLD`](https://lore.kernel.org/20260514130026.GC1781775@ax162/)
* [`Re: [PATCH] sparc: Avoid unsupported LLD branch relocations`](https://lore.kernel.org/20260514125820.GB1781775@ax162/)
* [`Re: [PATCH] kbuild: pacman-pkg: make "rc" releases adhere to pacman versioning scheme`](https://lore.kernel.org/177876476891.305249.12721845256238248028.b4-review@b4/)
* [`Re: [PATCH] kconfig: add kconfig-sym-check static checker`](https://lore.kernel.org/177876553250.305249.17848321995033732158.b4-review@b4/)
* [`Re: [PATCH] kbuild: deb-pkg: propagate hook script failures in builddeb`](https://lore.kernel.org/20260515195339.GA553537@ax162/)
* [`Re: [PATCH v2] kbuild: pacman-pkg: make "rc" releases adhere to pacman versioning scheme`](https://lore.kernel.org/20260516153317.GA311940@ax162/)
* [`Re: [RFC PATCH v3 1/3] scripts: add kconfirm`](https://lore.kernel.org/20260517092829.GB3773662@ax162/)
* [`Re: [PATCH] HID: core: Fix size_t specifier in hid_report_raw_event()`](https://lore.kernel.org/20260518190837.GA2318678@ax162/)
* [`Re: [PATCH] memory: omap-gpmc: Silence clang kerneldoc warnings`](https://lore.kernel.org/20260519161402.GA3527449@ax162/)
* [`Re: [linux-next:master 5548/6445] kernel/dma/contiguous.c:139:13: warning: variable 'numa_cma_configured' set but not used`](https://lore.kernel.org/20260520222742.GA1607511@ax162/)
* [`Re: [PATCH] tracing: Create output file from cmd_check_undefined`](https://lore.kernel.org/20260520224219.GC1607511@ax162/)
* [`Re: [PATCH] kconfig: Fix repeated include selftest expectation`](https://lore.kernel.org/20260520224308.GD1607511@ax162/)
* [`Re: [PATCH v9 3/3] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/20260523011721.GB520407@ax162/)
* [`Re: [PATCH v2] ARM: OMAP2+: Add CFI type for omap4_finish_suspend`](https://lore.kernel.org/20260523011741.GC520407@ax162/)
* [`[RISCV] Add partial support for -fzero-call-used-regs`](https://github.com/llvm/llvm-project/pull/194883#issuecomment-4549775365)
* [`Re: [PATCH v3 1/2] x86/kvm/vmx: Move IRQ/NMI dispatch from KVM into x86 core`](https://lore.kernel.org/20260526193510.GA2851089@ax162/)
* [`Re: [PATCH] firmware: arm_ffa: Treat missing FF-A feature on a platform as a probe miss`](https://lore.kernel.org/20260526193543.GB2851089@ax162/)
* [`Re: [PATCH] err.h: use __always_inline on all error pointer helpers`](https://lore.kernel.org/20260526193733.GC2851089@ax162/)
* [`Re: [PATCH v2 1/7] scripts: modpost: detect and report truncated buf_printf() output`](https://lore.kernel.org/20260527171823.GA1893026@ax162/)
* [`Re: [PATCH v2] kconfig: add optional warnings for changed input values`](https://lore.kernel.org/177992189956.893622.4881935369487236661.b4-review@b4/)
* [`Re: [PATCH] kbuild: rpm-pkg: append %{?dist} macro to Release tag`](https://lore.kernel.org/177992899911.893622.3247043690183493347.b4-review@b4/)
* [`Re: [PATCH v10 3/3] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/177992962862.1361033.11249653355160017674.b4-review@b4/)
* [`Re: [PATCH] .gitignore: ignore rustc long type txt files`](https://lore.kernel.org/20260528203622.GA3100532@ax162/)
* [`Re: [PATCH v11 3/3] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/178000602351.678078.3534988919326810792.b4-review@b4/)
* [`Re: [PATCH v3 1/2] kconfig: Remove the architecture specific config for AutoFDO`](https://lore.kernel.org/178010203121.3743687.8945076194906841190.b4-review@b4/)
* [`Re: [PATCH v3 2/2] kconfig: Remove the architecture specific config for Propeller`](https://lore.kernel.org/178010203122.3743687.12531843798236779880.b4-review@b4/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`ld.lld hangs when building Linux kernel with full LTO after recent flatten change`](https://github.com/llvm/llvm-project/issues/195236)
* [`-Wframe-larger-than in drivers/gpu/drm/amd/amdgpu/../display/dc/dml/dcn31{,4}/display_mode_vba_31{,4}.c`](https://github.com/ClangBuiltLinux/linux/issues/2161)
* [`Re: [tip: locking/core] locking/rtmutex: Annotate API and implementation`](https://lore.kernel.org/20260507084357.GA961911@ax162/)
* [`MIPS: non-default visibility warnings from tip of tree binutils around __memcpy / __memset`](https://lore.kernel.org/20260509122517.GA1108596@ax162/)
* [`objtool: bad .discard.annotate_insn`](https://github.com/ClangBuiltLinux/linux/issues/2162)
* [`build-rust.py failure in CI: "fs::read_to_string(toml_file_name) failed with No such file or directory"`](https://github.com/ClangBuiltLinux/tc-build/issues/336)
* [`error: kernel-address sanitizer is not supported for this target when targeting 32-bit ARM`](https://github.com/Rust-for-Linux/linux/issues/1234)
* [`Re: [PATCH 6.18 000/188] 6.18.32-rc1 review`](https://lore.kernel.org/20260517043707.GC1534263@ax162/)
* [`Re: linux-next: build failure in final build`](https://lore.kernel.org/20260518192408.GA2744985@ax162/)
* [`Re: linux-next: manual merge of the kbuild tree with the origin tree`](https://lore.kernel.org/20260518191337.GB2318678@ax162/)
* [`Re: [GIT PULL for v7.1] vfs fixes`](https://lore.kernel.org/20260518194622.GA2914683@ax162/)
* [`Re: [PATCH v15 3/6] ASoC: es9356-sdca: Add ES9356 SDCA driver`](https://lore.kernel.org/20260518224657.GA536765@ax162/)
* [`Re: [PATCH v3 1/2] x86/kvm/vmx: Move IRQ/NMI dispatch from KVM into x86 core`](https://lore.kernel.org/20260520230621.GA706311@ax162/)
* [`Re: [PATCH 2/4] firmware: arm_ffa: Register core as a platform driver`](https://lore.kernel.org/20260523001148.GA1319283@ax162/)
* [`Associate documentation comments with macro definitions`](https://github.com/llvm/llvm-project/pull/198452#issuecomment-4523534712)
* [`Re: [linux-next:master 5302/7547] fs/open.c:152:1: error: unknown warning group '-Wattribute-alias', ignored`](https://lore.kernel.org/20260523005204.GA520407@ax162/)
* [`Re: [PATCH v2] ARM: OMAP2+: Add CFI type for omap4_finish_suspend`](https://lore.kernel.org/20260525165527.GA18457@ax162/)
* [`[AArch64] Fix definition of system register move instructions`](https://github.com/llvm/llvm-project/pull/185709#issuecomment-4581596395)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update korg-clang-22 to 22.1.5`](https://github.com/kernelci/tuxmake/pull/281)
* [`Enable the integrated assembler for sparc64 with clang-23+`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/928)
* [`Update korg-clang-22 to 22.1.6`](https://github.com/kernelci/tuxmake/pull/282)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [22.1.5](https://lore.kernel.org/20260506064051.GA324058@ax162/)
  * [22.1.6](https://lore.kernel.org/20260523032119.GA317916@ax162/)

* I submitted the following pull requests.

  * [`[GIT PULL] Clang build fix for 7.1 #2`](https://lore.kernel.org/20260529210055.GA2415550@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
