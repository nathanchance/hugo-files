---
title: August 2023 ClangBuiltLinux Work
date: 2023-08-31T16:30:00-0700
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

  * `watchdog: xilinx_wwdt: Use div_u64() in xilinx_wwdt_start()` ([`v1`](https://lore.kernel.org/20230815-watchdog-xilinx-div_u64-v1-1-20b0b5a65c2e@kernel.org/))
  * `lib/Kconfig.debug: Restrict DEBUG_INFO_SPLIT for RISC-V` ([`v1`](https://lore.kernel.org/20230816-riscv-debug_info_split-v1-1-d1019d6ccc11@kernel.org/))
  * `MIPS: VDSO: Conditionally export __vdso_gettimeofday()` ([`v1`](https://lore.kernel.org/20230816-mips-vdso-cond-export-__vdso_gettimeofday-v1-1-fe725254c782@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `mISDN: Update parameter type of dsp_cmx_send()` ([`v1`](https://lore.kernel.org/20230802-fix-dsp_cmx_send-cfi-failure-v1-1-2f2e79b0178d@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `lib: test_scanf: Add explicit type cast to result initialization in test_number_prefix()` ([`v1`](https://lore.kernel.org/20230803-test_scanf-wconstant-conversion-v1-1-74da994dedbc@kernel.org/), [`v2`](https://lore.kernel.org/20230807-test_scanf-wconstant-conversion-v2-1-839ca39083e1@kernel.org/))
  * `ASoC: SOF: Intel: Initialize chip in hda_sdw_check_wakeen_irq()` ([`v1`](https://lore.kernel.org/20230809-intel-hda-missing-chip-init-v1-1-61557ca6fa8a@kernel.org/))
  * `rtc: stm32: Use NOIRQ_SYSTEM_SLEEP_PM_OPS()` ([`v1`](https://lore.kernel.org/20230815-rtc-stm32-unused-pm-funcs-v1-1-82eb8e02d903@kernel.org/))
  * `wifi: rtw89: Fix clang -Wimplicit-fallthrough in rtw89_query_sar()` ([`v1`](https://lore.kernel.org/20230822-rtw89-tas-clang-implicit-fallthrough-v1-1-5cb73f0fa976@kernel.org/))
  * `x86/hyperv: Add missing 'inline' to hv_snp_boot_ap() stub` ([`v1`](https://lore.kernel.org/20230822-hv_snp_boot_ap-missing-inline-v1-1-e712dcb2da0f@kernel.org/))
  * `ASoC: cs42l43: Initialize ret in default case in cs42l43_pll_ev()` ([`v1`](https://lore.kernel.org/20230823-cs42l43_pll_ev-init-ret-v1-1-5836f1ad5dad@kernel.org/))
  * `clk: qcom: Fix SM_GPUCC_8450 dependencies` ([`v1`](https://lore.kernel.org/20230829-fix-sm_gpucc_8550-deps-v1-1-d751f6cd35b2@kernel.org/))
  * `scsi: qla2xxx: Fix unused variable warning in qla2xxx_process_purls_pkt()` ([`v1`](https://lore.kernel.org/20230829-qla_nvme-fix-unused-fcport-v1-1-51c7560ecaee@kernel.org/))



## LLVM patches

* [`[Sema] Do not emit -Wmissing-variable-declarations for register variables`](https://github.com/llvm/llvm-project/commit/a22d385f9656c95f5ce4155ea705aab6f8ef6d82)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] LoongArch: Error out if clang version is less than 17.0.0`](https://lore.kernel.org/20230801134034.GA3831650@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 1/2] kbuild: deb-pkg: use Debian compliant shebang for debian/rules`](https://lore.kernel.org/20230801165202.GA2472327@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 2/2] kbuild: deb-pkg: split debian/rules`](https://lore.kernel.org/20230801165219.GB2472327@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scsi: ufs: Fix the build for gcc 9 and before`](https://lore.kernel.org/20230801215240.GA534984@dev-arch.thelio-3990X/)
* [`Re: [PATCH] word-at-a-time: use the same return type for has_zero regardless of endianness`](https://lore.kernel.org/20230802161553.GA2108867@dev-arch.thelio-3990X/)
* [`Re: [PATCH] clk: ralink: mtmips: quiet unused variable warning`](https://lore.kernel.org/20230802213206.GA758420@dev-arch.thelio-3990X/)
* [`Apply virtconfig to defconfig arm64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/613)
* [`Re: [PATCH] cpufreq: amd-pstate: fix global sysfs attribute type`](https://lore.kernel.org/20230807160635.GA3061@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Makefile.extrawarn: enable -Wmissing-variable-declarations for W=1`](https://lore.kernel.org/20230807165526.GA2744968@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ASoC: codecs: aw88261: avoid uninitialized variable warning`](https://lore.kernel.org/20230808150034.GA637683@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] Makefile.extrawarn: enable -Wmissing-variable-declarations for W=1`](https://lore.kernel.org/20230808161707.GA2171444@dev-arch.thelio-3990X/)
* [`Re: [PATCH] riscv: mm: fix 2 instances of -Wmissing-variable-declarations`](https://lore.kernel.org/20230808162412.GA2172017@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/9] Kbuild: only pass -fno-inline-functions-called-once for gcc`](https://lore.kernel.org/20230811141421.GA3948268@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/9] Kbuild: consolidate warning flags in scripts/Makefile.extrawarn`](https://lore.kernel.org/20230811141943.GB3948268@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86/retpoline: Don't clobber RFLAGS during srso_safe_ret()`](https://lore.kernel.org/20230811172811.GA3551@dev-arch.thelio-3990X/)
* [`Re: [PATCH] asm-generic: partially revert "Unify uapi bitsperlong.h for arm64, riscv and loongarch"`](https://lore.kernel.org/20230814172943.GB911700@dev-arch.thelio-3990X/)
* [`Re: [PATCH 0/5] riscv: SCS support`](https://lore.kernel.org/20230814175928.GA1028706@dev-arch.thelio-3990X/)
* [`Re: [PATCH] backlight: lp855x: Drop ret variable in brightness change function`](https://lore.kernel.org/20230815190910.GA2908446@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 05/11] x86/cpu: Clean up SRSO return thunk mess`](https://lore.kernel.org/20230815212931.GA3863294@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 0/6] riscv: SCS support`](https://lore.kernel.org/20230816164231.GA1653714@dev-arch.thelio-3990X/)
* [`Re: [PATCH] tty: gdm724x: use min_t() for size_t varable and a constant`](https://lore.kernel.org/20230816171338.GA2138570@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Revert "powerpc/xmon: Relax frame size for clang"`](https://lore.kernel.org/20230817184321.GA2428970@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Compiler Attributes: counted_by: Adjust name and identifier expansion`](https://lore.kernel.org/20230817201304.GA2714089@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: get lib-y objects back to static library`](https://lore.kernel.org/20230823202040.GA2236267@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Documentation/llvm: refresh docs`](https://lore.kernel.org/20230824184910.GA2015748@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Revert "Revert "powerpc/xmon: Relax frame size for clang""`](https://lore.kernel.org/20230828174631.GA1255864@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`CONFIG_FORTIFY_SOURCE may not be working for strcpy() and strlcpy() on LoongArch with clang`](https://github.com/ClangBuiltLinux/linux/issues/1897)
* [`Re: [stable:linux-4.19.y 5172/9999] lib/string.c:312:7: error: redefinition of 'stpcpy'`](https://lore.kernel.org/20230802163841.GA2149436@dev-arch.thelio-3990X/)
* [`Re: [PATCH v6 21/38] powerpc: Implement the new page table range API`](https://lore.kernel.org/20230803233814.GA2515372@dev-arch.thelio-3990X/)
* [`Re: [stable:linux-4.14.y 4459/9999] lib/mpi/mpih-div.c:142:20: error: invalid use of a cast in a inline asm context requiring an lvalue: remove the cast or build with -fheinous-gnu-extensions`](https://lore.kernel.org/20230804153749.GA2148888@dev-arch.thelio-3990X/)
* [`Re: stable-rc 5.15: clang-17: davinci_all_defconfig failed - arch/arm/include/asm/tlbflush.h:420:85: error: use of logical '&&' with constant operand [-Werror,-Wconstant-logical-operand]`](https://lore.kernel.org/20230808151353.GA798905@dev-arch.thelio-3990X/)
* [`Re: [linux-next:master 8173/8441] warning: unsafe strcpy() usage lacked '__write_overflow' warning in lib/test_fortify/write_overflow-strcpy.c`](https://lore.kernel.org/20230809183317.GA3355565@dev-arch.thelio-3990X/)
* [`ld.lld fails with "at least one side of the expression must be absolute" after SRSO mitigation`](https://github.com/ClangBuiltLinux/linux/issues/1907)
* [`SRSO mitigations cause unable to move location counter backward for: .text for LTO`](https://github.com/ClangBuiltLinux/linux/issues/1909)
* [`"missing return thunk" warning when booting on Intel machine`](https://github.com/ClangBuiltLinux/linux/issues/1911)
* [`Hang when booting guest kernels compiled with clang after SRSO mitigations`](https://lore.kernel.org/20230810013334.GA5354@dev-arch.thelio-3990X/)
* [`Re: [aegl:resctrl2_v65rc4 2/2] fs/resctrl2/arch/x86/l3_pseudolock.c:569:1: error: expected ';' after top level declarator`](https://lore.kernel.org/20230810171848.GA628575@dev-arch.thelio-3990X/)
* [`Re: [PATCH 6.1 000/127] 6.1.45-rc1 review`](https://lore.kernel.org/20230811041339.GA193223@dev-arch.thelio-3990X/)
* [`Re: Cannot find symbol for section 69: .text.arch_max_swapfile_size.`](https://lore.kernel.org/20230814154730.GA3072@dev-arch.thelio-3990X/)
* [`Re: [PATCH] serial: mxs-uart: fix Wvoid-pointer-to-enum-cast warning`](https://lore.kernel.org/20230814160457.GA2836@dev-arch.thelio-3990X/)
* [`issue with split debug info on riscv for clang-14 and older`](https://github.com/ClangBuiltLinux/linux/issues/1914)
* [`Cherry-pick fixes for clang/test/Driver/x86-no-gather-no-scatter.cpp`](https://github.com/llvm/llvm-project/issues/64903)
* [`Several new instances of -Wfortify-source after LLVM commit 0c9c9dd9a24f9`](https://github.com/ClangBuiltLinux/linux/issues/1923)
* [`Apply 9451c79bc39e610882bdd12370f01af5004a3c4f to linux-5.4.y`](https://lore.kernel.org/20230830153342.GA888898@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update llvm_latest to 17 and add llvm_16`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/612)
* [`tc_build: llvm: Switch to Path.glob()`](https://github.com/ClangBuiltLinux/tc-build/pull/239)
* [`patches: Add fix for new -Wconstant-conversion in clang-18`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/614)
* [`Drop Fedora's armv7hl configuration`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/616)
* [`boot-qemu.py: Workaround PLW1509 warning from Ruff`](https://github.com/ClangBuiltLinux/boot-utils/pull/112)
* [`utils.py: Do not query GitHub's API at all with '--gh-json-file'`](https://github.com/ClangBuiltLinux/boot-utils/pull/113)
* [`tc_build: utils: Fix new Ruff warning (PIE808)`](https://github.com/ClangBuiltLinux/tc-build/pull/240)
* [`Stop enabling CONFIG_PPC_DISABLE_WERROR=y for clang-14+`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/620)
* [`Update stable anchor to 6.5`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/621)
* [`Add support for LoongArch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/622)
* [`qemu: Install qemu-system-loongarch64`](https://github.com/ClangBuiltLinux/containers/pull/50)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
