---
title: July 2025 ClangBuiltLinux Work
date: 2025-07-31T16:30:00-0700
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

  * `cxl: Include range.h in cxl.h` ([`v1`](https://lore.kernel.org/20250701-cxl-fix-struct-range-error-v1-1-1f199bddc7c9@kernel.org/))
  * `riscv: Only allow LTO with CMODEL_MEDANY` ([`v1`](https://lore.kernel.org/20250710-riscv-restrict-lto-to-medany-v1-1-b1dac9871ecf@kernel.org/))
  * `iio: adc: ad_sigma_delta: Select IIO_BUFFER_DMAENGINE and SPI_OFFLOAD` ([`v1`](https://lore.kernel.org/20250714-iio-ad_sigma_delta-fix-kconfig-selects-v1-1-32e0d6da0423@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `ARM: Fix allowing linker DCE with binutils < 2.36` ([`v1`](https://lore.kernel.org/20250707-arm-fix-dce-older-binutils-v1-1-3b5e59dc3b26@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`[PATCH 5.10 and 5.4] staging: rtl8723bs: Avoid memset() in aes_cipher() and aes_decipher()`](https://lore.kernel.org/20250701152324.3571007-1-nathan@kernel.org/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `panic: Add __maybe_unused to sys_info_avail` ([`v1`](https://lore.kernel.org/20250708-fix-clang-sys_info_avail-warning-v1-1-60d239eacd64@kernel.org/))
  * `firmware: arm_scmi: Use modern PM ops` ([`v1`](https://lore.kernel.org/20250708-arm-scmi-modern-pm-ops-v1-1-14bc9c352888@kernel.org/))
  * `platform/x86/intel/pmt/discovery: Fix size_t specifiers for 32-bit` ([`v1`](https://lore.kernel.org/20250708-discovery-pmt-fix-32-bit-formats-v1-1-296a5fc9c3d4@kernel.org/))
  * `mm/ksm: Fix -Wsometimes-uninitialized from clang-21 in advisor_mode_show()` ([`v1`](https://lore.kernel.org/20250715-ksm-fix-clang-21-uninit-warning-v1-1-f443feb4bfc4@kernel.org/))
  * `usb: atm: cxacru: Merge cxacru_upload_firmware() into cxacru_heavy_init()` ([`v1`](https://lore.kernel.org/20250715-usb-cxacru-fix-clang-21-uninit-warning-v1-1-de6c652c3079@kernel.org/), [`v2`](https://lore.kernel.org/20250722-usb-cxacru-fix-clang-21-uninit-warning-v2-1-6708a18decd2@kernel.org/))
  * `media: s5p-mfc: Always pass NULL to s5p_mfc_cmd_host2risc_v6()` ([`v1`](https://lore.kernel.org/20250715-media-s5p-mfc-fix-uninit-const-pointer-v1-1-4d52b58cafe9@kernel.org/))
  * `wifi: mt76: mt7996: Initialize hdr before passing to skb_put_data()` ([`v1`](https://lore.kernel.org/20250715-mt7996-fix-uninit-const-pointer-v1-1-b5d8d11d7b78@kernel.org/))
  * `memstick: core: Zero initialize id_reg in h_memstick_read_dev_id()` ([`v1`](https://lore.kernel.org/20250715-memstick-fix-uninit-const-pointer-v1-1-f6753829c27a@kernel.org/))
  * `phonet/pep: Move call to pn_skb_get_dst_sockaddr() earlier in pep_sock_accept()` ([`v1`](https://lore.kernel.org/20250715-net-phonet-fix-uninit-const-pointer-v1-1-8efd1bd188b3@kernel.org/))
  * `drm/msm/dpu: Initialize crtc_state to NULL in dpu_plane_virtual_atomic_check()` ([`v1`](https://lore.kernel.org/20250715-drm-msm-fix-const-uninit-warning-v1-1-d6a366fd9a32@kernel.org/))
  * `drm/amdgpu: Initialize data to NULL in imu_v12_0_program_rlc_ram()` ([`v1`](https://lore.kernel.org/20250715-drm-amdgpu-fix-const-uninit-warning-v1-1-9683661f3197@kernel.org/))
  * `wifi: brcmsmac: Remove const from tbl_ptr parameter in wlc_lcnphy_common_read_table()` ([`v1`](https://lore.kernel.org/20250715-brcmsmac-fix-uninit-const-pointer-v1-1-16e6a51a8ef4@kernel.org/))
  * `riscv: uaccess: Fix -Wuninitialized and -Wshadow in __put_user_nocheck` ([`v1`](https://lore.kernel.org/20250715-riscv-uaccess-fix-self-init-val-v1-1-82b8e911f120@kernel.org/))
  * `tracing/probes: Avoid using params uninitialized in parse_btf_arg()` ([`v1`](https://lore.kernel.org/20250715-trace_probe-fix-const-uninit-warning-v1-1-98960f91dd04@kernel.org/))
  * `ASoC: SDCA: Fix uninitialized use of name in sdca_irq_populate()` ([`v1`](https://lore.kernel.org/20250715-sdca_interrupts-fix-const-uninit-warning-v1-1-cc031c913499@kernel.org/))



## LLVM patches

* [`[clang][test] Split out staticanalyzer portion of Modules/specializations-lazy-load-parentmap-crash.cpp`](https://github.com/llvm/llvm-project/pull/151259)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kunit/fortify: Add back "volatile" for sizeof() constants`](https://lore.kernel.org/20250701123521.GA2835771@ax162/)
* [`Re: [PATCH 2/3] printk: kunit: support offstack cpumask`](https://lore.kernel.org/20250702202835.GA593751@ax162/)
* [```Re: [PATCH v1 1/1] panic: Fix compilation error (`make W=1`)```](https://lore.kernel.org/20250711014947.GA863150@ax162/)
* [`[MachinePipeliner] Fix incorrect dependency direction`](https://github.com/llvm/llvm-project/pull/149436#issuecomment-3086498489)
* [`Re: [PATCH v3] kheaders: make it possible to override TAR`](https://lore.kernel.org/20250719201002.GA3285766@ax162/)
* [`Re: [PATCH] kbuild: userprogs: use correct linker when mixing clang and GNU ld`](https://lore.kernel.org/20250724231025.GA3620641@ax162/)
* [`Re: [PATCH 6.1.y] KVM: arm64: silence -Wuninitialized-const-pointer warning`](https://lore.kernel.org/20250725162654.GA684490@ax162/)
* [`Revisiting c0a454b9044f`](https://lore.kernel.org/20250714195205.GA3723043@ax162/)
* [`Re: [PATCH] sched/task_stack: Add missing const qualifier to end_of_stack()`](https://lore.kernel.org/20250727155047.GA1183915@ax162/)
* [`Re: [PATCH] kstack_erase: Add -mgeneral-regs-only to silence Clang warnings`](https://lore.kernel.org/20250727155108.GB1183915@ax162/)
* [`Re: [PATCH] kstack_erase: Disable kstack_erase for all of arm compressed boot code`](https://lore.kernel.org/20250727155129.GC1183915@ax162/)
* [`Re: [PATCH v2] compiler_types: Provide __no_kstack_erase to disable coverage only on Clang`](https://lore.kernel.org/20250729234604.GA921387@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [tip:x86/urgent 1/1] arch/x86/coco/sev/core.c:2170:30: warning: variable 'dummy' set but not used`](https://lore.kernel.org/20250701150644.GA3563357@ax162/)
* [`Re: [tip:master 19/19] include/linux/irq-entry-common.h:201:2: error: unexpected token`](https://lore.kernel.org/20250702180136.GA3452438@ax162/)
* [`Re: [PATCH v4] btrfs: replace deprecated strcpy with strscpy`](https://lore.kernel.org/20250702182712.GA3453770@ax162/)
* [`Re: [bug report] printk: ringbuffer: Add KUnit test`](https://lore.kernel.org/20250702202203.GA3347226@ax162/)
* [`Re: [linux-next:master 3022/4739] drivers/gpu/drm/msm/msm.o: warning: objtool: msm_dp_ctrl_on_stream(): unexpected end of section .text.msm_dp_ctrl_on_stream`](https://lore.kernel.org/20250702213716.GA2103156@ax162/)
* [`Re: [linux-next:master 7731/9053] arch/arm64/kvm/vgic/vgic-mmio.c:1094:3: warning: variable 'len' is used uninitialized whenever 'if' condition is false`](https://lore.kernel.org/20250714171559.GA1364710@ax162/)
* [`Revisiting c0a454b9044f`](https://lore.kernel.org/20250714195205.GA3723043@ax162/)
* [`[Clang] Diagnose forming references to nullptr`](https://github.com/llvm/llvm-project/pull/143667#issuecomment-3084709817)
* [`[MachinePipeliner] Add validation for missed loop-carried memory deps`](https://github.com/llvm/llvm-project/pull/145878#issuecomment-3084401504)
* [`[Clang] Make the SizeType, SignedSizeType and PtrdiffType be named sugar types instead of built-in types`](https://github.com/llvm/llvm-project/pull/143653#issuecomment-3086499400)
* [`-Wformat-invalid-specifier after 88fec3526e84 in -next`](https://lore.kernel.org/20250721231041.GA1015606@ax162/)
* [`Crash on QEMU shutdown with x86_64_defconfig+ThinLTO+CFI after LLVM commit 9878ef3abd2a`](https://lore.kernel.org/20250724000219.GA2976491@ax162/)
* [`Re: [PATCH v4 0/4] stackleak: Support Clang stack depth tracking`](https://lore.kernel.org/20250726004313.GA3650901@ax162/)
* [`[SCEV] Try to re-use pointer LCSSA phis when expanding SCEVs.`](https://github.com/llvm/llvm-project/pull/147824#issuecomment-3124670997)
* [`objtool warnings "sibling call from callable instruction with modified stack frame" with CONFIG_LTO_CLANG_THIN`](https://lore.kernel.org/20250731175655.GA1455142@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update patches (July 1, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/853)
* [`patches: mainline: Add patch to help debug powerpc kallsyms failure`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/855)
* [`github: Fix internal problem matcher for color ASCII escape codes`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/857)
* [`Initial set of changes for LLVM main 21 -> 22 update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/858)
* [`patches: Drop android14-6.1 and android15-6.6 (July 27, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/860)
* [`build-binutils.py: 2.45`](https://github.com/ClangBuiltLinux/tc-build/pull/304)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [20.1.8](https://lore.kernel.org/20250710165801.GA4073042@ax162/)
  * [21.1.0-rc1](https://lore.kernel.org/20250719234008.GA220041@ax162/)
  * [21.1.0-rc2](https://lore.kernel.org/20250730045057.GA3686463@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
