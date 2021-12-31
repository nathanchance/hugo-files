---
title: "December 2021 ClangBuiltLinux Work"
date: 2021-12-31T12:00:00-07:00
toc: false
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Android patches: Android is one of the biggest downstream consumers of our work. Making sure that it stays working is incredibly important so that it can reach real users.

  * [`Revert "ANDROID: disable INFINIBAND_QIB from allmodconfig build"`](https://android-review.googlesource.com/c/kernel/common/+/1907758)

  * [`ANDROID: allmodconfig: Re-enable TEST_KMOD`](https://android-review.googlesource.com/c/kernel/common/+/1907759)

  * [`Revert "ANDROID: fix build error in arch/arm64/include/asm/arch_timer.h"`](https://android-review.googlesource.com/c/kernel/common/+/1922369)

* Build errors: This is a collection of patches that either fix build errors specific to clang or generally, in certain scenarios. We are slowly getting to the point where there are few build errors that are clang specific, which is good, as breakage is more likely to be dealt with by other people so that we can focus on other issues.

  * `MIPS: Loongson64: Use three arguments for slti` ([`v1`](https://lore.kernel.org/r/20211207170129.578089-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20211208165616.1746108-1-nathan@kernel.org/))

  * `MIPS: Loongson2ef: Remove unnecessary {as,cc}-option calls` ([`v1`](https://lore.kernel.org/r/20211207175951.135400-1-nathan@kernel.org/))

  * `x86/extable: Fix extable_type_reg macro with Clang LTO` ([`v1`](https://lore.kernel.org/r/20211210234953.3420108-1-nathan@kernel.org/))

  * `Fix build errors with do_exit() to make_task_dead() transition` ([`v1`](https://lore.kernel.org/r/20211227184851.2297759-1-nathan@kernel.org/))

  * `iwlwifi: mvm: Use div_s64 instead of do_div in iwl_mvm_ftm_rtt_smoothing()` ([`v1`](https://lore.kernel.org/r/20211227191757.2354329-1-nathan@kernel.org/))

* Other fixes: These are fixes that do not fit into a specific category. The first one was noticed while I was building kernels on a powerful arm64 server and the second came up during testing another patch.

  * `x86/boot/compressed: Move CLANG_FLAGS to beginning of KBUILD_CFLAGS` ([`v1`](https://lore.kernel.org/r/20211222005245.3081136-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/r/20211222163040.1961481-1-nathan@kernel.org/))

  * `ARM: davinci: da850-evm: Avoid NULL pointer dereference` ([`v1`](https://lore.kernel.org/r/20211223222141.1253092-1-nathan@kernel.org/))

* `-Wbitwise-instead-of-logical`: A new warning in LLVM exposed a few places in the kernel where bitwise operations were being used with boolean expressions with side effects, which may be undesirable. This was one of the last ones in the tree, which is important for keeping the warning enabled.

  * `soc/tegra: fuse: Fix bitwise vs. logical OR warning` ([`v2`](https://lore.kernel.org/r/20211210165528.3232292-1-nathan@kernel.org/))

* `-Wframe-larger-than=`: The kernel cares about stack usage quite a bit so it enables `-Wframe-larger-than=` to warn about large amounts of stack usage. These warnings have been present for a while but until `-Werror` was enabled, there were often larger fires to fight, as these warnings are usually in configurations that are not going to run in the real world. Getting them cleaned up now is very important for Android so that they can enable `-Werror` for all configurations that they care about.

  * `staging: greybus: fix stack size warning with UBSAN` ([`v2`](https://lore.kernel.org/r/20211209195141.1165233-1-nathan@kernel.org/))

  * `cxl/core: Remove cxld_const_init in cxl_decoder_alloc()` ([`v1`](https://lore.kernel.org/r/20211210213627.2477370-1-nathan@kernel.org/))

* `-Wtypedef-redefinition`: Trying to typedef something twice is almost always a bug. GCC does not warn on this but clang does so we need to clean these warnings up.

  * `media: atomisp: Do not define input_system_cfg2400_t twice` ([`v1`](https://lore.kernel.org/r/20211227164243.2329724-1-nathan@kernel.org/))



## LLVM patches

* [`[clang][driver] Warn when '-mno-outline-atomics' is used with a non-AArch64 triple`](https://github.com/llvm/llvm-project/commit/be8180af5854806a343c3dd334d97ba2c4bfadfa)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] nl80211: remove reload flag from regulatory_request`](https://lore.kernel.org/r/YaeYDmN8DhADXOSg@archlinux-ax161/)

* [`Unexpected -Wuninitialized with asm goto with outputs`](https://github.com/ClangBuiltLinux/linux/issues/1521)

* [`Re: [PATCH] kcov: fix generic Kconfig dependencies if ARCH_WANTS_NO_INSTR`](https://lore.kernel.org/r/Yaet8x%2F1WYiADlPh@archlinux-ax161/)

* [```Assertion `isa<X>(Val) && "cast<Ty>() argument of incompatible type!"' failed```](https://github.com/ClangBuiltLinux/linux/issues/1512)

* [`Re: [PATCH v3] arm64: Enable KCSAN`](https://lore.kernel.org/r/Yaj6vxHk0wFQb8E3@archlinux-ax161/)

* [`Re: [PATCH RFC 0/6] Bump minimum supported version of LLVM to 11.0.0`](https://lore.kernel.org/r/Yao86FeC2ybOobLO@archlinux-ax161/)

* [`error: hardware TLS register is not supported for the arm sub-architecture`](https://github.com/ClangBuiltLinux/linux/issues/1515)

* [`Re: [PATCH v2] selftests: vDSO: parse: warning: fix assignment as a condition`](https://lore.kernel.org/r/Ya45Q9hLPIaociYW@archlinux-ax161/)

* [`Re: [PATCH 00/12] IIO: clang W=1 warning cleanup.`](https://lore.kernel.org/r/Ya5T5CMHn5hzzJy5@archlinux-ax161/)

* [`Re: Makefile: CC_IMPLICIT_FALLTHROUGH passed quoted as argument to gcc`](https://lore.kernel.org/r/Ya6WZw3s4EdGbp3a@archlinux-ax161/)

* [`Re: [PATCH v5 5/5] powerpc/inst: Optimise copy_inst_from_kernel_nofault()`](https://lore.kernel.org/r/Ya7ntJ0ehZPy6HKA@archlinux-ax161/)

* [`Re: [PATCH] powerpc: platforms: cell: pervasive: fix clang -Wimplicit-fallthrough`](https://lore.kernel.org/r/20211207110228.698956-1-anders.roxell@linaro.org/)

* [`Re: [PATCH] power: reset: ltc2952: fix float conversion error`](https://lore.kernel.org/r/Ya+wlfX7ZPb95mZ4@archlinux-ax161/)

* [`Re: [PATCH v2] MIPS: Makefile: Remove "ifdef need-compiler" for Kbuild.platforms`](https://lore.kernel.org/r/YbN+0NrHmsFKfNWP@archlinux-ax161/)

* [`Re: [PATCH] Kconfig.debug: Make DEBUG_INFO selectable from a choice`](https://lore.kernel.org/r/YbOANJLMlllo6B7e@archlinux-ax161/)

* [`[PATCH v3 0/2] MIPS: Remove some code`](https://lore.kernel.org/r/1639389477-17586-1-git-send-email-yangtiezhu@loongson.cn/)

* [`[llvm-objcopy] Fix handling of MIPS64 little endian files`](https://reviews.llvm.org/D115635)

* [`[PATCH 0/3] MIPS: Add support for LTO`](https://lore.kernel.org/r/20211213224914.1501303-1-paul@crapouillou.net/)

* [`Re: [PATCH] x86: use builtins to read eflags`](https://lore.kernel.org/r/YbpwXjGcLty1ns81@archlinux-ax161/)

* [`Re: [PATCH] MIPS: Octeon: Fix build errors using clang`](https://lore.kernel.org/r/YbtlNT7WUi3b8Dqh@archlinux-ax161/)

* [`Re: [PATCH] x86/sgx: Fix NULL pointer dereference on non-SGX systems`](https://lore.kernel.org/r/Yb0Vy2Q2ifugclva@archlinux-ax161/)

* [`Re: [patch V3 28/35] PCI/MSI: Simplify pci_irq_get_affinity()`](https://lore.kernel.org/r/Yb4w2wVvIwN7qaNy@archlinux-ax161/)

* [`Re: [PATCH] gpio: sim: fix uninitialized ret variable`](https://lore.kernel.org/r/Yb5Z1A1tX6xdrDce@archlinux-ax161/)

* [`Re: [PATCH] crypto: cleanup warning in qm_get_qos_value()`](https://lore.kernel.org/r/YcJHoqXXVFZatIla@archlinux-ax161/)

* [`Re: [PATCH v2] ARM: avoid literal references in inline assembly`](https://lore.kernel.org/r/YcN4xJfH5NaPSibU@archlinux-ax161/)

* [`Re: [PATCH linux-next] tools: compiler-gcc.h::Keep compatible with non-clang compilers.`](https://lore.kernel.org/r/Yc81vBwPo%2FaHMfXB@archlinux-ax161/)

* [`Re: [PATCH] Fix compilation errors when using special directory`](https://lore.kernel.org/r/Yc84UG6nwqyb37o2@archlinux-ax161/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`error: expression requires 'long double' type support, but target 'x86_64-unknown-linux-gnu' does not support it`](https://github.com/ClangBuiltLinux/linux/issues/1497)

* [`llvm-objcopy produces corrupted .debug_str for elf32-x86-64 (X32 ABI) emulation (Z_DATA_ERROR)`](https://github.com/ClangBuiltLinux/linux/issues/514)

* [`-Wshift-count-negative in drivers/net/ethernet/sfc/`](https://github.com/ClangBuiltLinux/linux/issues/1439)

* [`LTO + -Oz trigger seg. fault in ld.lld-13`](https://github.com/ClangBuiltLinux/linux/issues/1523)

* [`s390: ld.lld: error: unknown emulation: elf64_s390`](https://github.com/ClangBuiltLinux/linux/issues/1524)

* [`Both LLVM_IAS=1 and =0 fail with ARCH=powerpc (kernel 5.16rc3)`](https://github.com/ClangBuiltLinux/linux/issues/1525)

* [`Inconsistent build timeouts`](https://gitlab.com/Linaro/tuxsuite/-/issues/145#note_752714658)

* [```CONFIG_CPU_LOONGSON2F=y and LLVM_IAS=1 fails with arch/mips/loongson2ef/Platform:36: *** only binutils >= 2.20.2 have needed option -mfix-loongson2f-nop. Stop.```](https://github.com/ClangBuiltLinux/linux/issues/1529)

* [`llvm-objcopy: error: invalid output format: 'elf64-s390'`](https://github.com/ClangBuiltLinux/linux/issues/1530)

* [`mips: drivers/misc/xilinx_sdfec.c:787:8: error: couldn't allocate input reg for constraint 'r'`](https://github.com/ClangBuiltLinux/linux/issues/1531)

* [`error: instruction requires a CPU feature not currently enabled in arch/mips/include/asm/mipsregs.h`](https://github.com/ClangBuiltLinux/linux/issues/1532)

* [`CONFIG_KEXEC=y gives build error on mips`](https://github.com/ClangBuiltLinux/linux/issues/1528)

* [`RISC-V relocation R_RISCV_HI20 out of range with CONFIG_CMODEL_MEDLOW=y`](https://github.com/ClangBuiltLinux/linux/issues/1533)

* [`mips llvm-objcopy: error: invalid symbol index: 67108864`](https://github.com/ClangBuiltLinux/linux/issues/1534)

* [`Builds still running when client exits`](https://gitlab.com/Linaro/tuxsuite/-/issues/147)

* [`Re: [PATCH v6 21/21] cxl/core: Split decoder setup into alloc + add`](https://lore.kernel.org/r/YbOswyDRX1SEtE8C@archlinux-ax161/)

* [`Please merge c4582a689c2c74e0635309979176c7ada086f066 into release/13.x`](https://github.com/llvm/llvm-project/issues/52633)

* [`Assertion 'Symbol' failed with -fpatchable-function-entry=2 + -Oz AArch64`](https://github.com/llvm/llvm-project/issues/52635)

* [`"cannot insert node between set or sequence node and its filter children" in the Linux kernel`](https://github.com/llvm/llvm-project/issues/52637)

* [`error: couldn't allocate input reg for constraint 'r' on MIPS when building the Linux kernel`](https://github.com/llvm/llvm-project/issues/52638)

* [`Unsupported compiler and assembler flags for MIPS`](https://github.com/ClangBuiltLinux/linux/issues/1544)

* [`llvm-objcopy: "error: invalid symbol index" with mips64el-linux-gnuabi64`](https://github.com/llvm/llvm-project/issues/52647)

* [`Kernel build never starts`](https://github.com/ClangBuiltLinux/linux/issues/1545)

* [`Re: [PATCH v2] arm64/xor: use EOR3 instructions when available`](https://lore.kernel.org/r/YbgDR461O9gCEi+Q@archlinux-ax161/)

* [`ld.lld: error: section exceeds available address space (MIPS vmlinuz.bin)`](https://github.com/ClangBuiltLinux/linux/issues/1546)

* [`MIPS: vmlinuz does not boot if LD=ld.lld`](https://github.com/ClangBuiltLinux/linux/issues/1333)

* [`Re: drivers/pinctrl/bcm/pinctrl-bcm2835.c:412:14: warning: variable 'group' is used uninitialized whenever 'for' loop exits because its condition is false`](https://lore.kernel.org/r/YbjlV4HdWI8WUcAR@archlinux-ax161/)

* [`Re: ANNOUNCE: pahole v1.23 (BTF tags and alignment inference)`](https://lore.kernel.org/r/YbkTAPn3EEu6BUYR@archlinux-ax161/)

* [`Re: linux-next: Tree for Dec 14`](https://lore.kernel.org/r/Ybk4vF68Uhsd%2F8JH@archlinux-ax161/)

* [`-Wmissing-braces in drivers/firmware/efi/libstub/efi-stub-helper.c`](https://github.com/ClangBuiltLinux/linux/issues/1547)

* [`Re: [linux-stable-rc:queue/5.10 6492/9999] ERROR: modpost: "raid6_2data_recov" [fs/btrfs/btrfs.ko] undefined!`](https://lore.kernel.org/r/YbokKLRVkIDCnE8F@archlinux-ax161/)

* [`Re: [LKP] Re: [x86/mm/64] f154f29085: BUG:kernel_reboot-without-warning_in_boot_stage - clang KCOV?`](https://lore.kernel.org/r/YbzRHXEMnZjyXzWa@archlinux-ax161/)

* [`Re: [PATCH v13 2/2] x86/sgx: Add an attribute for the amount of SGX memory in a NUMA node`](https://lore.kernel.org/r/YbzhBrimHGGpddDM@archlinux-ax161/)

* [`Re: [patch V3 28/35] PCI/MSI: Simplify pci_irq_get_affinity()`](https://lore.kernel.org/r/Yb0PaCyo%2F6z3XOlf@archlinux-ax161/)

* [`Re: [Intel-gfx] [PATCH v3 4/4] drm/i915/fbc: Register per-crtc debugfs files`](https://lore.kernel.org/r/Yb6EP13FeEhmvq5c@archlinux-ax161/)

* [`error: hardware TLS register is not supported for the arm sub-architecture`](https://github.com/ClangBuiltLinux/linux/issues/1502#issuecomment-998133633)

* [`Re: [hyperv:hyperv-next 4/5] drivers/hv/vmbus_drv.c:2082:29: warning: shift count >= width of type`](https://lore.kernel.org/r/YcC1CobR%2Fn0tJhdV@archlinux-ax161/)

* [`error: out of range pc-relative fixup value`](https://github.com/ClangBuiltLinux/linux/issues/1551)

* [`'-outline-atomics' is not a recognized feature for this target`](https://github.com/ClangBuiltLinux/linux/issues/1552)

* [`Re: [PATCH 09/10] kthread: Ensure struct kthread is present for all kthreads`](https://lore.kernel.org/r/YcNsG0Lp94V13whH@archlinux-ax161/)

* [`Re: [next] arm: current.h:53:6: error: out of range pc-relative fixup value`](https://lore.kernel.org/r/YcNvUAMaRKBzUUcy@archlinux-ax161/)

* [`Issue with booting multi_v5_defconfig kernel with GCC 11`](https://lore.kernel.org/r/YcS4xVWs6bQlQSPC@archlinux-ax161/)

* [`Re: [PATCH v2 05/15] scsi: hisi_sas: Fix some issues related to asd_sas_port->phy_list`](https://lore.kernel.org/r/Ycn3FoW9eOZNFMiL@archlinux-ax161/)

* [`x86 Thin LTO + allyesconfig causes ld.lld: error: kernel image bigger than KERNEL_IMAGE_SIZE `](https://github.com/ClangBuiltLinux/linux/issues/1556)

* [`[Clang/Test]: Rename enable_noundef_analysis to disable-noundef-analysis and turn it off by default`](https://reviews.llvm.org/D105169#3213144)



## Tooling improvements

* [`patches: Drop dwc2 floating literal patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/253)

* [`docker: clang-android: Update to r433403b`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/224)

* [`readme.md: Add subscription link`](https://github.com/ClangBuiltLinux/ClangBuiltLinux.github.io/pull/32)

* [`Update architectures list`](https://github.com/ClangBuiltLinux/ClangBuiltLinux.github.io/pull/33)

* [`Make failures more obvious`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/254)

* [`Drop -next clang-10 builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/255)

* [`Change TuxSuite targets`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/256)

* [`Improve check_logs.py output`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/257)

* [`docker: clang-android: Update to r437112`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/225)

* [`Update patches (12-10-2021)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/258)

* [`Improvements around non-x86_64 native use`](https://github.com/ClangBuiltLinux/tc-build/pull/178)

* [`patches: Drop android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/260)

* [`check_logs.py: Print build log and exit when patch fails to apply`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/262)

* [`frame_larger_than.py: Support DW_TAG_enumeration_type`](https://github.com/ClangBuiltLinux/frame-larger-than/pull/6)

* [`Move to a container based boot workflow`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/266)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 3 and 4, HP desktop, ASUS laptop, and Hyper-V and VMware platforms on my workstation. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* As I brought up last month, I finalized containerizing certain aspects of [my environment scripts](https://github.com/nathanchance/env) so that no matter what machine I am working on, I can have consistent access to recent versions of compilers and other development tools, independent of the host operating system. I believe this is extremely important for reproducing issues as well as avoiding issues such as broken dependencies or upgrades because all the tools are in a container, which is a lot easier to reset and start over from than the host operating system.

* LLVM moved from [bugs.llvm.org](https://bugs.llvm.org/) to [GitHub Issues](https://github.com/llvm/llvm-project/issues), which necessitated updating any LLVM bug links in our issue tracker (similar to what I did for Linux kernel mailing list links in October). To ensure this does not happen again in the future, I documented best practices for links [in our wiki](https://github.com/ClangBuiltLinux/linux/wiki/Adding-links-to-issues).



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).
