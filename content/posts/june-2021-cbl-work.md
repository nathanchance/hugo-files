---
title: "June 2021 ClangBuiltLinux Work"
date: 2021-07-01T13:48:16-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

## Linux kernel patches

* [`ANDROID: sched: Gate sched_stat tracepoint exports on CONFIG_SCHEDSTATS`](https://android-review.googlesource.com/c/kernel/common/+/1725551): One of those instances where a tangential change in the Android trees broke our continuous integration. Our CI tests some 32-bit ARM configurations because certain Android OEMs care about them but these configs are not tested by the actual Android team, meaning we find breakage that they do not.

* [`[PATCH net-next] net: ks8851: Make ks8851_read_selftest() return void`](https://lore.kernel.org/r/20210603165612.2088040-1-nathan@kernel.org/): `clang` implemented GCC's `-Wunused-but-set-variable` and this instance was found by Intel's kernel test robot. Rather simple fix but it is important to fix warnings as they crop up so that the build stays as clean as possible so that the real issues are easily visible.

* [`[PATCH net-next] net: ethernet: rmnet: Restructure if checks to avoid uninitialized warning`](https://lore.kernel.org/r/20210603173410.310362-1-nathan@kernel.org/): Uninitialized variable warnings are always worrisome because using an uninitialized variable results in undefined behavior and could be a security problem if the attacker were able to control or read the contents. In this particular instance, it was just that clang could not understand that the variable was always initialized because of how the code was structure so I rearranged it to make it cleaner to both humans and the compiler.

* [`[PATCH] btrfs: Remove total_data_size variable in btrfs_batch_insert_items()`](https://lore.kernel.org/r/20210603174311.1008645-1-nathan@kernel.org/): btrfs turns on some extra warnings for their code, known in the kernel as `W=1` because of the variable that is passed to `make`. This cleans up one of those instances, which will be visible in a normal build.

* [`[PATCH] scsi: elx: efct: Do not use id uninitialized in efct_lio_setup_session()`](https://lore.kernel.org/r/20210617061721.2405511-1-nathan@kernel.org/): Unlike the uninitialized warning above, this one is legitimate and it shows up in a debug print so it is important to get a proper value.

* [`[PATCH] scsi: elx: efct: Eliminate unnecessary boolean check in efct_hw_command_cancel()`](https://lore.kernel.org/r/20210617063123.21239-1-nathan@kernel.org/): A fairly uninteresting warning from `clang` but it can be a bug occasionally so it is important to clean them up.

* [`[PATCH] scripts/min-tool-version.sh: Raise minimum clang version to 13.0.0 for s390`](https://lore.kernel.org/r/20210617193139.856957-1-nathan@kernel.org/): The s390 folks had their compiler team implement support for even/odd register pair in LLVM, which means that older versions of LLVM are broken in non-obvious ways, so we codify this requirement before the build can even start. This is unfortunate because LLVM 13.0.0 is still in development at this point but using the tip of tree versions of LLVM is not all that scary because of LLVM's aggressive "revert to green" policy, meaning the tree is intended to be "release worthy" at any point.

* [`[PATCH] scsi: lpfc: Reduce scope of uuid in lpfc_queuecommand()`](https://lore.kernel.org/r/20210617233759.2355447-1-nathan@kernel.org/): A small build error that I noticed in a few of the configurations that I test. Unfortunately, this was not accepted because it was fixed by just declaring the variable outside of the `CONFIG_SCSI_LPFC_DEBUG_FS` at the top of the function.

* [`[PATCH net-next] net/mlx5: Use cpumask_available() in mlx5_eq_create_generic()`](https://lore.kernel.org/r/20210618000358.2402567-1-nathan@kernel.org/): Yet another instance of `-Wpointer-bool-conversion`, which happens in certain configurations. This was solved through a helper function in the early days of ClangBuiltLinux so we use that again here.

* [`[PATCH] KVM: PPC: Book3S HV: Workaround high stack usage with clang`](https://lore.kernel.org/r/20210621182440.990242-1-nathan@kernel.org/): This warning has been around since the beginning of the year or so, which was starting to become annoying for a few parties, including the kernel test robot. This is now fixed in LLVM 13.0.0 but this workaround still buys us a decent amount of stack space so it is probably worth keeping around for a while.

* [`[PATCH] mailbox: imx: Avoid using val uninitialized in imx_mu_isr()`](https://lore.kernel.org/r/20210621185645.2373845-1-nathan@kernel.org/): Switch statements with `default` cases that do not do anything sort of defeats the purpose of the `default` case in my opinion, as `clang` points out here, so we avoid the warning and make it obvious to the developer that this switch statement will need to be updated.

* [`[PATCH 1/2] ACPI: bgrt: Fix CFI violation`](https://lore.kernel.org/r/20210623013802.1904951-1-nathan@kernel.org/): My girlfriend had a laptop lying around that she was not using anymore so I threw an SSD in it and installed Linux on it for testing. Virtualized testing is nice because it scales really well but it is no replacement for running on real hardware to test various device specific drivers. In this case, I found an issue in an ACPI table driver with Control Flow Integrity, which was rather easy to fix, and now allows the computer to boot with CFI enabled in enforcing mode.

* [`[PATCH 4.4 to 4.19] Makefile: Move -Wno-unused-but-set-variable out of GCC only block`](https://lore.kernel.org/r/20210623172610.3281050-1-nathan@kernel.org/): A stable specific backport of an upstream patch that I sent so that there are not new warnings for downstream users such as Android, which is where I discovered this.

* [`ANDROID: Add CONFIG_LLD_VERSION`](https://android-review.googlesource.com/q/I686ce6cc5f503540d8336b914f0fe978951c469b) and [`UPSTREAM: x86, lto: Pass -stack-alignment only on LLD < 13.0.0`](https://android-review.googlesource.com/q/Icebcd5669e851e2c26e9f807b9d1aeb86e95dcea): An Android specific backport so that our continuous integration would stay building with tip of tree LLVM. The Android LLVM team picks a particular revision of LLVM then stays on that for a few months, just cherry-picking reverts and features as needed so they will not run into this for a bit but it is better to get ahead of the curve. After all, that is the entire point of this project :)

* [`[PATCH] ALSA: usb-audio: scarlett2: Fix for loop increment in scarlett2_usb_get_config`](https://lore.kernel.org/r/20210624212048.1356136-1-nathan@kernel.org/): This bug was rather fun for me as I learned a bit about C that I did not know. Casting an increment does not do anything because a cast is an rvalue and the increment operator only works on lvalues. I did not do very well in my "Principles of Programming Languages" class before switching degrees but I do remember that distinction. This patch went through quite a few revisions but it did get picked up eventually.

* [`[PATCH net-next] net: sparx5: Do not use mac_addr uninitialized in mchp_sparx5_probe()`](https://lore.kernel.org/r/20210627184543.4122478-1-nathan@kernel.org/): More and more uninitialized warnings :( unfortunately, GCC does not warn about uninitialized variables in the kernel after commit [`78a5255ffb6a`](https://git.kernel.org/linus/78a5255ffb6a1af189a83e493d916ba1c54d8c75) ("Stop the ad-hoc games with -Wno-maybe-initialized") because GCC does its variable initiailization analysis after inlining, which can be somewhat confusing at different optimization levels. `clang`'s analysis happens much easier, which makes it a little dumber at times, but it also makes it a little more reliable when it comes to stack variables with `if` and `switch` statements in my experience.



## LLVM patches

* [`[BitCode] Add noprofile to getAttrFromCode()`](https://github.com/llvm/llvm-project/commit/4ae0ab095bf97123c2d2a6aa2e82dcc25cf040f1): A latent bug in LLVM was exposed by a prior change that we needed for the kernel, resulting in broken LTO and CFI builds so I fixed it up with a simple patch and test.



## Patch review and input

For the next sections, I link directly to my response when possible but there are times where the link is to the main post and my response can be seen inline.

* [`Re: [PATCH] MAINTAINERS: Expand and relocate PGO entry`](https://lore.kernel.org/r/75a5aefd-084f-ef59-ceff-0f3856dcce71@kernel.org/)

* [`Re: [PATCH v2 1/1] pgo: Fix allocate_node() v2`](https://lore.kernel.org/r/d7e94352-0b24-1ab1-8b54-b6ffd4347963@kernel.org/)

* [`Re: [PATCH v3 1/1] pgo: Fix allocate_node() v2`](https://lore.kernel.org/r/b04bafeb-541b-b1e7-9fbc-66bd1a04916f@kernel.org/)

* [`Re: [PATCH v3 16/16] objtool,x86: Rewrite retpoline thunk calls`](https://lore.kernel.org/r/e351ac97-4038-61b5-b373-63698a787fc1@kernel.org/)

* [`Re: [PATCH RFC] x86: remove toolchain check for X32 ABI capability`](https://lore.kernel.org/r/1992c9cf-739e-d98f-85c0-bbcf7df123ea@kernel.org/)

* [`Update to Linux 5.12.9 and add missing packages on Archlinux/Manjaro`](https://github.com/ClangBuiltLinux/tc-build/pull/159)

* [`Re: [kbuild-all] Re: kernel/rcu/tree.c:2073:23: warning: stack frame size of 2704 bytes in function 'rcu_gp_kthread'`](https://lore.kernel.org/r/7f03e5bc-d8a3-74a4-273a-f8047b62ab02@kernel.org/)

* [`Re: [PATCH] platform/x86: dell-wmi-sysman/think-lmi: Make fw_attr_class global static`](https://lore.kernel.org/r/64af7560-689b-373c-607a-85e242a2b57c@kernel.org/)

* [`[PATCH v2 0/4] iio: Drop use of %hhx and %hx format strings`](https://lore.kernel.org/r/20210603180612.3635250-1-jic23@kernel.org/)

* [`Re: [PATCH v3] scsi: ufs: Fix a possible use before initialization case`](https://lore.kernel.org/r/YMD5xoeiE7+wrCEK@archlinux-ax161/)

* [`Re: [PATCH] x86/Makefile: make -stack-alignment conditional on LLD < 13.0.0`](https://lore.kernel.org/r/ea01f4cb-3e65-0b79-ae93-ba0957e076fc@kernel.org/)

* [`Re: [PATCH v2 1/1] x86/Makefile: make -stack-alignment conditional on LLD < 13.0.0`](https://lore.kernel.org/r/9b93b016-c19a-21db-2cc4-041810ae722d@kernel.org/)

* [`Re: [PATCH 1/1] Makefile: Pass -warn-stack-size only on LLD < 13.0.0`](https://lore.kernel.org/r/51c33b62-1e91-7c69-5b77-75ffe0ad6e77@kernel.org/)

* [`Re: [PATCH rdma-next v2 00/15] Reorganize sysfs file creation for struct ib_devices`](https://lore.kernel.org/r/0b6de703-1071-ca39-5657-cd00862bfbfd@kernel.org/)

* [`[PATCH v2 0/3] no_profile fn attr and Kconfig for GCOV+PGO`](https://lore.kernel.org/r/20210621231822.2848305-1-ndesaulniers@google.com/)

* [`[PowerPC] Combine 64-bit bswap(load) without LDBRX`](https://reviews.llvm.org/D104836)

* [`Re: [PATCH v2] kallsyms: strip LTO suffixes from static functions`](https://lore.kernel.org/r/a970613b-014f-be76-e342-4a51e792b56d@kernel.org/)



## Issue triage and reporting

* [`ld.lld: error: drivers/gpu/drm/amd/amdgpu/amdgpu.lto.o: SHT_SYMTAB_SHNDX has 79046 entries, but the symbol table associated has 79048`](https://github.com/ClangBuiltLinux/linux/issues/1388)

* [`Re: [PATCH] riscv: mm: init: Consolidate vars, functions`](https://lore.kernel.org/r/YLaWseLdg5JYElVx@Ryzen-9-3900X.localdomain/)

* [`Re: drivers/net/ethernet/micrel/ks8851_common.c:995:6: warning: variable 'ret' set but not used`](https://lore.kernel.org/r/b34e07af-4559-7707-b00b-5a36789e566d@kernel.org/)

* [```Assertion `(IK == IK_FpInduction || Step->getType()->isIntegerTy()) && "StepValue is not an integer"' failed```](https://github.com/ClangBuiltLinux/linux/issues/1383)

* [`x86_64 allmodconfig + CONFIG_LTO_CLANG_THIN=y compile time regression`](https://github.com/ClangBuiltLinux/linux/issues/1393)

* [`ld.lld segmentation fault for CONFIG_X86_X32 check with binutils master branch`](https://github.com/ClangBuiltLinux/linux/issues/1141)

* [`llvm-objcopy produces corrupted .debug_str for elf32-x86-64 (X32 ABI) emulation (Z_DATA_ERROR)`](https://github.com/ClangBuiltLinux/linux/issues/514)

* [`Re: [PATCH v5] platform/x86: firmware_attributes_class: Create helper file for handling firmware-attributes class registration events`]()

* [`Re: [PATCH v1 2/3] scsi: ufs: Optimize host lock on transfer requests send/compl paths`](https://lore.kernel.org/r/YL+umjDMd4Rao%2FNs@Ryzen-9-3900X/)

* [`Re: [PATCH 01/13] objtool: Rewrite hashtable sizing`](https://lore.kernel.org/r/YMJWmzXgSipOqXAf@DESKTOP-1V8MEUQ.localdomain/)

* [`Re: [linux-next:master 7012/7430] include/linux/compiler_types.h:328:38: error: call to '__compiletime_assert_183' declared with attribute error: unexpected size in kmalloc_index()`](https://lore.kernel.org/r/99de1f59-1e38-6410-86ff-0ea1f016c49f@kernel.org/)

* [`Re: [PATCH v3 16/16] objtool,x86: Rewrite retpoline thunk calls`](https://lore.kernel.org/r/e351ac97-4038-61b5-b373-63698a787fc1@kernel.org/)

* [`Re: [linux-next:master 9529/10007] mm/hugetlb.c:1591:9: warning: no previous prototype for function 'hugetlb_basepage_index'`]()

* [`Re: drivers/crypto/talitos.c:3328:12: warning: stack frame size of 1040 bytes in function 'talitos_probe'`](https://lore.kernel.org/r/a2dc22ce-30af-0cb0-130c-1078e7ef52a5@kernel.org/)

* [```Re: [next] [clang] x86_64-linux-gnu-ld: mm/mremap.o: in function `move_pgt_entry': mremap.c:(.text+0x763): undefined reference to `__compiletime_assert_342'```](https://lore.kernel.org/r/YMuOSnJsL9qkxweY@archlinux-ax161/)

* [`Re: [PATCH v9 21/31] elx: efct: Hardware IO and SGL initialization`](https://lore.kernel.org/r/YMvpc5KsGYFSAjok@archlinux-ax161/)

* [`Re: [PATCH v2 2/2] mm/zbud: don't export any zbud API`](https://lore.kernel.org/r/YMvsYm8b+yTIrqBC@archlinux-ax161/)

* [`[InstrProfiling] Make __profd_ unconditionally private for ELF`](https://reviews.llvm.org/D103717)

* [`Re: [PATCH 2/2] arm64: insn: move AARCH64_INSN_SIZE into <asm/insn.h>`](https://lore.kernel.org/r/YMv2B6HCnDReOFIr@archlinux-ax161/)

* [`Re: [PATCH v4 1/2] media: rc: new driver for USB-UIRT device`](https://lore.kernel.org/r/https://lore.kernel.org/r/63f389df-e128-6438-97b4-0b66b30e7028@kernel.org//)

* [`Re: arch/powerpc/kvm/book3s_hv_nested.c:264:6: error: stack frame size of 2304 bytes in function 'kvmhv_enter_nested_guest'`](https://lore.kernel.org/r/e6167885-30e5-d149-bcde-3e9ad9f5d381@kernel.org/)

* [`Re: [PATCH v8 4/6] KVM: PPC: Book3S HV: Nested support in H_RPT_INVALIDATE`](https://lore.kernel.org/r/YNDIitJ3Hn1%2FG8Jw@Ryzen-9-3900X.localdomain/)

* [`LLVM bb1dc876ebb8a2eef38d5183d00c2db1437f1c91 breaks arm64 builds`](https://github.com/ClangBuiltLinux/linux/issues/1403)

* [`Re: how can we test the hexagon port in mainline`](https://lore.kernel.org/r/YNQE0YJzC2xmWg+2@Ryzen-9-3900X.localdomain/)

* [`Re: linux-next: build failure after merge of the net-next tree`](https://lore.kernel.org/r/YNPt91bfjrgSt8G3@Ryzen-9-3900X.localdomain/)

* [`Hexagon backend crash in drivers/md/persistent-data/dm-bitset.c`](https://github.com/ClangBuiltLinux/linux/issues/1410)

* [`Re: [PATCH v15 06/12] swiotlb: Use is_swiotlb_force_bounce for swiotlb data bouncing`](https://lore.kernel.org/r/YNvMDFWKXSm4LRfZ@Ryzen-9-3900X.localdomain/)



## Tooling improvements

* [`Deal with most of the current fires`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/145)

* [`Enable pseries_defconfig (ppc64) again`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/146)

* [`Update list of Arch Linux packages`](https://github.com/ClangBuiltLinux/tc-build/pull/160)

* [`Upgrade Linux to 5.12.9 and add a PowerPC build fix`](https://github.com/ClangBuiltLinux/tc-build/pull/161)

* [`kernel/build.sh: Fix patch application with two PGO kernel runs`](https://github.com/ClangBuiltLinux/tc-build/pull/162)

* [`Re-enable Hexagon on -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/148)

* [`Disable s390 builds for clang 12 and earlier on -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/150)

* [`Update the start time of the -next jobs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/153)

* [`README.md: Clarify that latest version of distribution should be used whenever possible`](https://github.com/ClangBuiltLinux/tc-build/pull/164)

* [`check_logs.py: Do not use number of errors to fail the build`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/155)

* [`Use ld.bfd for powerpc64le on -next with LLVM 11`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/156)

* [`Enable CONFIG_COMPAT_VDSO for arm64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/157)

* [`Remove CROSS_COMPILE_COMPAT for upstream 5.4`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/158)

* [`Move RISC-V back to ld.bfd on 5.10`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/159)



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).