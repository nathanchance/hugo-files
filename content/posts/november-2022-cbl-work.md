---
title: "November 2022 ClangBuiltLinux Work"
date: 2022-11-30T16:30:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Build errors: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `RISC-V: vdso: Do not add missing symbols to version section in linker script` ([`v1`](https://lore.kernel.org/20221108171324.3377226-1-nathan@kernel.org/))
  * `x86/vdso: Conditionally export __vdso_sgx_enter_enclave` ([`v1`](https://lore.kernel.org/20221109000306.1407357-1-nathan@kernel.org/))

* CFI fixes: Kernel Control Flow Integrity (kCFI) was recently merged into mainline so it is important to keep the kernel free of new CFI failures. A new warning in clang will flag this so these patches fix all the warnings so that we turn it on and have all the automation warn people when they introduce a new instance of the warning.

  * `drm/amdgpu: Fix type of second parameter in trans_msg() callback` ([`v1`](https://lore.kernel.org/20221102152540.2389891-1-nathan@kernel.org/))
  * `drm/amdgpu: Fix type of second parameter in odn_edit_dpm_table() callback` ([`v1`](https://lore.kernel.org/20221102152540.2389891-2-nathan@kernel.org/))
  * `drm/fsl-dcu: Fix return type of fsl_dcu_drm_connector_mode_valid()` ([`v1`](https://lore.kernel.org/20221102154215.78059-1-nathan@kernel.org/))
  * `drm/mediatek: Fix return type of mtk_hdmi_bridge_mode_valid()` ([`v1`](https://lore.kernel.org/20221102154712.540548-1-nathan@kernel.org/))
  * `drm/meson: Fix return type of meson_encoder_cvbs_mode_valid()` ([`v1`](https://lore.kernel.org/20221102155242.1927166-1-nathan@kernel.org/))
  * `drm/sti: Fix return type of sti_{dvo,hda,hdmi}_connector_mode_valid()` ([`v1`](https://lore.kernel.org/20221102155623.3042869-1-nathan@kernel.org/))
  * `HSI: ssi_protocol: Fix return type of ssip_pn_xmit()` ([`v1`](https://lore.kernel.org/20221102160233.4042756-1-nathan@kernel.org/))
  * `hamradio: baycom_epp: Fix return type of baycom_send_packet()` ([`v1`](https://lore.kernel.org/20221102160610.1186145-1-nathan@kernel.org/))
  * `net: ethernet: ti: Fix return type of netcp_ndo_start_xmit()` ([`v1`](https://lore.kernel.org/20221102160933.1601260-1-nathan@kernel.org/))
  * `scsi: elx: libefc: Fix second parameter type in state callbacks` ([`v1`](https://lore.kernel.org/20221102161906.2781508-1-nathan@kernel.org/))
  * `s390/ctcm: Fix return type of ctc{mp,}m_tx()` ([`v1`](https://lore.kernel.org/20221102163252.49175-1-nathan@kernel.org/), [`v2`](20221103170130.1727408-1-nathan@kernel.org))
  * `s390/netiucv: Fix return type of netiucv_tx()` ([`v1`](https://lore.kernel.org/20221102163252.49175-2-nathan@kernel.org/), [`v2`](20221103170130.1727408-2-nathan@kernel.org))
  * `s390/lcs: Fix return type of lcs_start_xmit()` ([`v1`](https://lore.kernel.org/20221102163252.49175-3-nathan@kernel.org/), [`v2`](20221103170130.1727408-3-nathan@kernel.org))
  * `counter: Adjust final parameter type in function and signal callbacks` ([`v1`](https://lore.kernel.org/20221102172217.2860740-1-nathan@kernel.org/))
  * `counter: stm32-timer-cnt: Adjust final parameter type of stm32_count_direction_read()` ([`v1`](https://lore.kernel.org/20221102172217.2860740-2-nathan@kernel.org/))
  * `counter: ti-ecap-capture: Adjust final parameter type of ecap_cnt_pol_{read,write}()` ([`v1`](https://lore.kernel.org/20221102172217.2860740-3-nathan@kernel.org/))
  * `counter: 104-quad-8: Adjust final parameter type of certain callback functions` ([`v1`](https://lore.kernel.org/20221102172217.2860740-4-nathan@kernel.org/))
  * `net: ethernet: renesas: Fix return type of rswitch_start_xmit()` ([`v1`](https://lore.kernel.org/20221102160933.1601260-1-nathan@kernel.org/))
  * `drm: xlnx: Fix return type of zynqmp_dp_bridge_mode_valid` ([`v2`](https://lore.kernel.org/20221109001424.1422495-1-nathan@kernel.org/))
  * `net: mana: Fix return type of mana_start_xmit()` ([`v3`](https://lore.kernel.org/20221109002629.1446680-1-nathan@kernel.org/))

* Miscellaneous fixes: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `vmlinux.lds.h: Fix placement of '.data..decrypted' section` ([`v1`](https://lore.kernel.org/20221108174934.3384275-1-nathan@kernel.org/))
  * `Fix lack of section mismatch warnings with LTO` ([`v1`](https://lore.kernel.org/20221129190123.872394-1-nathan@kernel.org/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `bpf: Add explicit cast to 'void *' for __BPF_DISPATCHER_UPDATE()` ([`v1`](https://lore.kernel.org/20221107170711.42409-1-nathan@kernel.org/))
  * `cpufreq: ACPI: Remove unused variables 'acpi_cpufreq_online' and 'ret'` ([`v1`](https://lore.kernel.org/20221108170103.3375832-1-nathan@kernel.org/))
  * `ARM: Drop '-mthumb' from AFLAGS_ISA` ([`v1`](https://lore.kernel.org/20221114225719.1657174-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20221118003057.3223394-1-nathan@kernel.org/))
  * `media: rzg2l-cru: Remove unnecessary shadowing of ret in rzg2l_csi2_s_stream()` ([`v1`](https://lore.kernel.org/20221128061622.1470489-1-nathan@kernel.org/))
  * `mm: Fix '.data.once' orphan section warning` ([`v1`](https://lore.kernel.org/20221128225345.9383-1-nathan@kernel.org/))



## LLVM patches

* [`Revert "[AArch64] Improve codegen for shifted mask op"`](https://github.com/llvm/llvm-project/commit/74bace2dfe57d9cf569addf94af4e01a990d2374)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v4 1/1] drm/amd/display: add DCN support for ARM64`](https://lore.kernel.org/Y2FJ5wXpEpKf9v%2FP@dev-arch.thelio-3990X/)
* [`[Clang][Sema] Add -Wincompatible-function-pointer-types-strict`](https://reviews.llvm.org/D136790)
* [`Re: [PATCH] MIPS: fix duplicate definitions for exported symbols`](https://lore.kernel.org/Y2LEvg5PEdbAtQ3e@dev-arch.thelio-3990X/)
* [`Re: KVM vs AMD: Re: [PATCH v3 48/59] x86/retbleed: Add SKL return thunk`](https://lore.kernel.org/Y2UwqXd3NFYJrjWG@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 0/8] KVM: SVM: fixes for vmentry code`](https://lore.kernel.org/Y2qxfcHC6OgBdfl8@dev-arch.thelio-3990X/)
* [`Re: [PATCH] locking: fix kernel/locking/ inline asm error`](https://lore.kernel.org/Y2rayPgDfL2NYcjQ@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: revive parallel execution for .tmp_initcalls.lds rule`](https://lore.kernel.org/Y3La2mwCgD8r%2F5PI@dev-arch.thelio-3990X/)
* [`Re: [linux-nvme:nvme-6.2 26/41] drivers/nvme/host/core.c:5122:6: error: assigning to 'int' from incompatible type 'void'`](https://lore.kernel.org/Y3PLZxNwqioynAtw@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 1/2] drm/amdgpu: Temporarily disable broken Clang builds due to blown stack-frame`](https://lore.kernel.org/Y4RMphf6BUGLA5B6@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 2/2] Kconfig.debug: Provide a little extra FRAME_WARN leeway when KASAN is enabled`](https://lore.kernel.org/Y4RMyAoa0+sJS9F3@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ASoC: qcom: lpass-sc7180: Add maybe_unused tag for system PM ops`](https://lore.kernel.org/Y4YpELN4%2F0cesonb@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`-Wframe-larger-than instances in drivers/gpu/drm/amd/display/dc/dml with ARCH=arm64`](https://github.com/ClangBuiltLinux/linux/issues/1752)
* [`CFI: Function type mismatches reported by -Wincompatible-function-pointer-types-strict`](https://github.com/ClangBuiltLinux/linux/issues/1750)
* [`CFI: Implement -Wincompatible-function-pointer-types-strict`](https://github.com/ClangBuiltLinux/linux/issues/1745)
* [`[AArch64] Improve codegen for shifted mask op`](https://reviews.llvm.org/D136014)
* [`Re: [PATCH 11/13] x86_64: Remove pointless set_64bit() usage`](https://lore.kernel.org/Y2QR%2FBRHjjYUNszh@dev-arch.thelio-3990X/)
* [`[LLD] Enable --no-undefined-version by default.`](https://reviews.llvm.org/D135402)
* [`ld.lld switch to '--no-undefined-version' breaks RISC-V and x86 vDSO builds`](https://github.com/ClangBuiltLinux/linux/issues/1756)
* [`Re: [PATCH v4 2/4] ARM: use .arch directives instead of assembler command line flags`](https://lore.kernel.org/Y2qgTyFcPdnNfkpj@dev-arch.thelio-3990X/)
* [`Re: [PATCHv5 04/13] zram: Introduce recompress sysfs knob`](https://lore.kernel.org/Y2z4DbuYgDJ%2Fv8u+@dev-arch.thelio-3990X/)
* [`Re: x86: clang: acpi-cpufreq.c:970:24: error: variable 'ret' is uninitialized when used here [-Werror,-Wuninitialized]`](https://lore.kernel.org/Y2zysjBdAERPdUOQ@dev-arch.thelio-3990X/)
* [`-Wshift-overflow in drivers/pwm/pwm-tegra.c`](https://github.com/ClangBuiltLinux/linux/issues/1759)
* [`Re: linux-next: manual merge of the drm-misc tree with the origin tree`](https://lore.kernel.org/Y3ZvffZiR+SgtY6h@dev-arch.thelio-3990X/)
* [`Re: [PATCH] arm64/mm: Drop redundant BUG_ON(!pgtable_alloc)`](https://lore.kernel.org/Y3pS5fdZ3MdLZ00t@dev-arch.thelio-3990X/)
* [`Re: [PATCH bpf-next v10 16/24] bpf: Introduce single ownership BPF linked list API`](https://lore.kernel.org/Y3vE0T3BRxKZYgnb@dev-arch.thelio-3990X/)
* [`Re: [PATCH v8 3/6] staging: media: Add support for the Allwinner A31 ISP`](https://lore.kernel.org/Y4RVzSM4FQ%2FtYQAV@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Drop patches for arm64-fixes tree`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/463)
* [`Increase 'tuxsuite plan' step timeout to eight hours`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/464)
* [`Update stable anchor to 6.0`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/465)
* [`Re-enable OpenSUSE's RISC-V configuration`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/466)
* [`patches/next: Update bpf patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/467)
* [`Disable ppc44x_defconfig builds on stable`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/468)
* [`Drop arm64 patches (November 7th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/469)
* [`Remove -next patches (November 9th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/470)
* [`Disable s390 builds with LLVM 14 on mainline and -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/471)
* [`patches: mainline: Drop BPF patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/473)
* [`Add more checks for potential TuxSuite problems`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/474)
* [`boot-utils: Python updates`](https://github.com/ClangBuiltLinux/boot-utils/pull/76)
* [`continuous-integration2: Python updates`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/475)
* [`tc-build: Python updates`](https://github.com/ClangBuiltLinux/tc-build/pull/208)
* [`boot-utils: Update to latest main`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/476)
* [`tc-build: Eliminate os.path usage and more f-strings`](https://github.com/ClangBuiltLinux/tc-build/pull/209)
* [`Adjust location of raised timeout in GitHub Actions workflow files`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/477)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, an Intel-based laptop, and a SolidRun Honeycomb LX2. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I ran two ClangBuiltLinux meetings this month, which involves reporting what happened in the past two weeks to people who might not follow the project that closes. I review emails and issues to sumarize everything clearly.

* I spent some time during the quieter parts of the month improving my personal automation for tasks such as writing these reports and setting up test/virtual machines and spot instance servers to be more robust and easier to maintain, modify, and debug. It makes me more confident in making sure the steps complete without any problems.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
