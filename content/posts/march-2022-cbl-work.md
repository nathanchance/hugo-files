---
title: "March 2022 ClangBuiltLinux Work"
date: 2022-03-31T16:30:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Boot fixes: These patches fix boot failures that we have seen in testing. In this case, it is a User Mode Linux fix, which we are looking to enable in our continuous integration setup, as it is useful for quick testing.

  * `um: Fix filtering '-mno-global-merge'` ([`v1`](https://lore.kernel.org/r/20220322173547.677760-1-nathan@kernel.org/))

* Build failures: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `crypto: virtio - Select new dependencies` ([`v1`](https://lore.kernel.org/r/20220308205309.2192502-1-nathan@kernel.org/))

  * `arm64: Do not include __READ_ONCE() block in assembly files` ([`v1`](https://lore.kernel.org/r/20220309191633.2307110-1-nathan@kernel.org/))

  * `ARM: Do not use NOCROSSREFS directive with ld.lld` ([`v1`](https://lore.kernel.org/r/20220309220726.1525113-1-nathan@kernel.org/))

  * `x86/ibt: Fix CC_HAS_IBT check for clang` ([`v1`](https://lore.kernel.org/r/20220311195642.2033108-1-nathan@kernel.org/))

  * `x86: Avoid CONFIG_X86_X32_ABI=y with llvm-objcopy` ([`v1`](https://lore.kernel.org/r/20220314194842.3452-1-nathan@kernel.org/))

  * `Fix CONFIG_X86_KERNEL_IBT for clang and ld.lld < 14.0.0` ([`v1`](https://lore.kernel.org/r/20220318230747.3900772-1-nathan@kernel.org/))

* Kbuild improvements: These are patches to improve or fix issues with how we build the kernel with LLVM. The first one is particularly notable, as it was requested by a high profile kernel developer.

  * `kbuild: Make $(LLVM) more flexible` ([`v1`](https://lore.kernel.org/r/20220302234400.782002-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20220304170813.1689186-1-nathan@kernel.org/))

  * `Remove '-mno-global-merge' from KBUILD_CFLAGS` ([`v1`](https://lore.kernel.org/r/20220330234528.1426991-1-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [everyone should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `drm/selftest: plane_helper: Put test structures in static storage` ([`v1`](https://lore.kernel.org/r/20220302235909.784935-1-nathan@kernel.org/))

  * `KVM: x86: Fix clang -Wimplicit-fallthrough in do_host_cpuid()` ([`v1`](https://lore.kernel.org/r/20220322152906.112164-1-nathan@kernel.org/))

  * `netfs: Ensure ret is always initialized in netfs_begin_read()` ([`v1`](https://lore.kernel.org/r/20220303163826.1120936-1-nathan@kernel.org/))

  * `btrfs: Remove unused variable in btrfs_{start,write}_dirty_block_groups()` ([`v1`](https://lore.kernel.org/r/20220324153644.4079376-1-nathan@kernel.org/))

  * `clocksource/drivers/imx-tpm: Move tpm_read_sched_clock() under CONFIG_ARM` ([`v1`](https://lore.kernel.org/r/20220303184212.2356245-1-nathan@kernel.org/))

  * `i2c: designware: Mark dw_i2c_plat_{suspend,resume}() as __maybe_unused` ([`v1`](https://lore.kernel.org/r/20220303191713.2402461-1-nathan@kernel.org/))

  * `drm/msm/gpu: Avoid -Wunused-function with !CONFIG_PM_SLEEP` ([`v1`](https://lore.kernel.org/r/20220330180541.62250-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 1/3] [v3] Kbuild: move to -std=gnu11`](https://lore.kernel.org/r/Yh5PhDDLcVe4kSqk@dev-arch.archlinux-ax161/)

* [`Re: [PATCH v2 01/39] kbuild: Fix clang build`](https://lore.kernel.org/r/Yh+dMJsH+ZMPfqwD@thelio-3990X/)

* [`Re: [PATCH] um: clang: Strip out -mno-global-merge from USER_CFLAGS`](https://lore.kernel.org/r/YiD7R2wRxoWxtVq7@dev-arch.thelio-3990X/)

* [`Re: [PATCH bpf-next v2 5/8] compiler-clang.h: Add __diag infrastructure for clang`](https://lore.kernel.org/r/YiEFbKk12F0UPfx5@thelio-3990X/)

* [`[Mips] support "sp" named register`](https://reviews.llvm.org/D120926)

* [`Re: [PATCH v2 0/2] *** Fix reformat_objdump.awk ***`](https://lore.kernel.org/r/YiJKZ2541K0ll0Ed@dev-arch.thelio-3990X/)

* [`Re: [PATCH] virtio_ring: Initialize vring_size_in_bytes`](https://lore.kernel.org/r/YiZG0Nx9j++bJaA4@dev-arch.thelio-3990X/)

* [`Re: [PATCH] ARM: fix compilation error when BPF_SYSCALL is disabled`](https://lore.kernel.org/r/Yie+9J0hyA6k2KvD@dev-arch.thelio-3990X/)

* [`Re: [PATCH v2] kbuild: add --target to correctly cross-compile UAPI headers with Clang`](https://lore.kernel.org/r/YifLzLl4IE%2FxFMdn@dev-arch.thelio-3990X/)

* [`Re: [PATCH] arm64: Paper over ARM_SMCCC_ARCH_WORKAROUND_3 Clang issue`](https://lore.kernel.org/r/YijftpaApSJWuZ3m@dev-arch.thelio-3990X/)

* [`Re: [PATCH v2] drm/amkfd: bail out early if no get_atc_vmid_pasid_mapping_info`](https://lore.kernel.org/r/YijuEmuuPZYdBt0I@dev-arch.thelio-3990X/)

* [`Re: [PATCH] powerpc: Replace ppc64 DT_RELACOUNT usage with DT_RELASZ/24`](https://lore.kernel.org/r/Yij5p+TanPYnUM5V@dev-arch.thelio-3990X/)

* [`Re: [PATCH] MIPS: Only use current_stack_pointer on GCC`](https://lore.kernel.org/r/YikTQRql+il3HbrK@dev-arch.thelio-3990X/)

* [`Re: [PATCH 0/4] [v4] Kbuild: std=gnu11 changes`](https://lore.kernel.org/r/YilBFcKIN1Ao5Ld1@dev-arch.thelio-3990X/)

* [`Re: [PATCH] ASoC: atmel: mchp-pdmc: Fix -Wpointer-bool-conversion warning`](https://lore.kernel.org/r/Yi+8ft7yXrNN4+Yx@dev-arch.thelio-3990X/)

* [`Re: [PATCH] soc: qcom: smem: use correct format characters`](https://lore.kernel.org/r/YjTJRqlFOsXz7Ss7@dev-arch.thelio-3990X/)

* [`Re: [PATCH -next] sched/headers: ARM needs asm/paravirt_api_clock.h`](https://lore.kernel.org/r/Yjib7hDFRFEOwaWf@dev-arch.thelio-3990X/)

* [`[Clang][NeonEmitter] emit ret decl first for -Wdeclaration-after-statement`](https://reviews.llvm.org/D122189)

* [`Re: [PATCH v2] riscv module: remove (NOLOAD)`](https://lore.kernel.org/r/YjkrGAqx9nl%2FPtWh@dev-arch.thelio-3990X/)

* [`Re: [PATCH] x86/config: Make the x86 defconfigs a bit more usable`](https://lore.kernel.org/r/YjySjys3QZAWFlfo@dev-arch.thelio-3990X/)

* [`Re: [PATCH -next] PCI: hv: Remove unused function hv_set_msi_entry_from_desc()`](https://lore.kernel.org/r/Yj9zooV4TO8tZr9f@dev-arch.thelio-3990X/)

* [`Re: [PATCH] cifs: fix enum usage`](https://lore.kernel.org/r/YkHdKQ8TlhXTv1FP@dev-arch.thelio-3990X/)

* [`Re: [PATCH] x86/extable: prefer local labels in .set directives`](https://lore.kernel.org/r/YkNu6zmjxzhbjmei@dev-arch.thelio-3990X/)

* [`Re: [PATCH] Fix kernel build with LLVM=1`](https://lore.kernel.org/r/YkYma7wwps8LZJnH@dev-arch.thelio-3990X/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH bpf-next v1 6/6] selftests/bpf: Add tests for kfunc register offset checks`](https://lore.kernel.org/r/Yh%2F9LS7zc3nhTjsR@thelio-3990X/)

* [`Re: [next] mips: clang-14-defconfig build regression`](https://lore.kernel.org/r/YiD1UruDryIyZAJc@dev-arch.thelio-3990X/)

* [`Re: [PATCH 1/2] drm/amdkfd: judge get_atc_vmid_pasid_mapping_info before call`](https://lore.kernel.org/r/YieOPO1WaQ%2FVnqdD@dev-arch.thelio-3990X/)

* [`ARM: AT expected, but got NOCROSSREFS`](https://github.com/ClangBuiltLinux/linux/issues/1609)

* [`aspeed_g5_defconfig "error: invalid operand for instruction"`](https://github.com/ClangBuiltLinux/linux/issues/1610)

* [`Re: [lpieralisi-pci:pci/rcar 2/2] drivers/pci/controller/pcie-rcar-host.c:139:3: error: instruction requires: data-barriers`](https://lore.kernel.org/r/YilD68QeKpgJG1W2@dev-arch.thelio-3990X/)

* [`Re: [kvm:queue 210/210] arch/x86/kvm/cpuid.c:739:2: warning: unannotated fall-through between switch labels`](https://lore.kernel.org/r/Yio5gfTHY5qvcEyU@dev-arch.thelio-3990X/)

* [`CONFIG_THUMB2_KERNEL=y boot failure after Spectre BHB fixes`](https://lore.kernel.org/r/YipOoAaBIHjeCKOq@dev-arch.thelio-3990X/)

* [`Re: [kvm:queue 182/205] <inline asm>:40:208: error: expected relocatable expression`](https://lore.kernel.org/r/YipVB6MiWdjY3HI3@dev-arch.thelio-3990X/)

* [`Re: [PATCH v2 8/8] media: i2c: ov5670: Add .get_selection() support`](https://lore.kernel.org/r/YjDFBKhV0ALzu36l@dev-arch.thelio-3990X/)

* [`Re: [ammarfaizi2-block:google/android/kernel/common/android-4.19-q-release 5202/7636] ld.lld: error: -plugin-opt=-: ld.lld: Unknown command line argument '-stack-alignment=8'. Try: 'ld.lld --help'`](https://lore.kernel.org/r/YjIVzw4JdSY4NcLz@dev-arch.thelio-3990X/)

* [`Re: [GIT PULL] scheduler updates for v5.18`](https://lore.kernel.org/r/YjiddAnoCCz7Tbt3@dev-arch.thelio-3990X/)

* [`s390 ext4 crash on -next`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/334)

* [`[InstCombine] try to narrow shifted bswap-of-zext`](https://reviews.llvm.org/D122166)

* [`Re: [PATCH 1/1] um: fix error return code in winch_tramp()`](https://lore.kernel.org/r/Yjt31seiNv18HYrf@dev-arch.thelio-3990X/)

* [`ld.lld: error: <instantiation>:1:13: redefinition of 'found'`](https://github.com/ClangBuiltLinux/linux/issues/1612)

* [`pcie-rcar-host.c:(.text.fixup+0x4): relocation R_ARM_JUMP24 out of range`](https://github.com/ClangBuiltLinux/linux/issues/1615)

* [`Re: [PATCH 2/4] hwmon: (asus-ec-sensors) implement locking via the ACPI global lock`](https://lore.kernel.org/r/YkHZRzbi54t0pZkO@thelio-3990X/)

* [`s390 defconfig fails to build after 4afeb670710efa5cd5ed8b1d9f2d22d6ce332bcc`](https://lore.kernel.org/r/YkH%2FrRikE2CilpqU@dev-arch.thelio-3990X/)

* [`Re: [ammarfaizi2-block:google/android/kernel/common/android12-trusty-5.10 4036/5872] WARNING: modpost: vmlinux.o(.text+0x4111c4): Section mismatch in reference from the function memblock_bottom_up() to the variable .meminit.data:memblock`](https://lore.kernel.org/r/YkXSv8exRRUbT%2FoM@dev-arch.thelio-3990X/)



## Tooling improvements

* [`Fix up check_logs.py and messages/documentation after scripts move`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/317)

* [`patches: next: Replace warning hiding patch with proper fix`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/318)

* [`patches: next: Drop drm patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/319)

* [`Drop android-4.14 hack patch for ld.lld`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/320)

* [`Build most powerpc64le configs with LLVM=1 LLVM_IAS=1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/321)

* [`Update markdown badge alternative text`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/322)

* `workflows: Bump actions/checkout to v3` ([actions-workflows](https://github.com/ClangBuiltLinux/actions-workflows/pull/3), [containers](https://github.com/ClangBuiltLinux/containers/pull/3), [continuous-integration2](https://github.com/ClangBuiltLinux/continuous-integration2/pull/323), [tc-build](https://github.com/ClangBuiltLinux/tc-build/pull/182))

* [`[v2] Build most powerpc64le configs with LLVM=1 LLVM_IAS=1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/326)

* [`check_logs.py: Use proper syntax for removing problem matcher`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/329)

* [`boot-qemu.sh: Do not set '-cpu' for 32-bit x86`](https://github.com/ClangBuiltLinux/boot-utils/pull/56)

* [`Improve default '-smp' value`](https://github.com/ClangBuiltLinux/boot-utils/pull/57)

* [`boot-qemu.sh: arm64: Pass 'lpa2=off' when necessary`](https://github.com/ClangBuiltLinux/boot-utils/pull/58)

* [`Drop arm64-fixes patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/330)

* [`boot-utils: Update to latest main branch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/331)

* [`patches: Drop -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/335)

* [`Test the latest stable release`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/336)

* [`Add support for ARCH=um`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/337)

* [`Add support for User Mode Linux`](https://github.com/ClangBuiltLinux/boot-utils/pull/59)

* [`Update to Linux 5.17 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/183)

* [`Revert 4eec5c448d09646659806afdd42b080a7002620f for stable`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/338)

* [`Disable ARCH=um builds for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/339)

* [`Update AOSP LLVM builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/340)

* [`Disable ARCH=arm allyesconfig on -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/341)

* [`Update mainline patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/343)

* [`Disable ARCH=arm allyesconfig on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/344)

* [`Remove Fedora i686 configuration`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/345)

* [`[patches] Cherry pick CLS for: x86 mainline Linux kernel`](https://android-review.googlesource.com/c/toolchain/llvm_android/+/2047927)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
