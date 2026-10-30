---
title: October 2026 ClangBuiltLinux Work
date: 2026-10-30T16:30:00-0700
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

  * `kbuild: AutoFDO: Update split machine function argument for upstream rename` ([`v1`](https://lore.kernel.org/20261014-update-autofdo-split-machine-function-option-v1-1-5c71b3005e89@kernel.org/))

* Kbuild / Kconfig fixes and improvements: These are changes that I have authored as part of maintaining Kbuild and Kconfig.

  * `Initial ergonomic improvements to scripts/Kconfig.toolchain` ([`v1`](https://lore.kernel.org/20261007-scripts-kconfig-toolchain-improvements-v1-0-1e524178da11@kernel.org/))
  * `kbuild: Fallout from moving '-mfentry' check into Kconfig stage` ([`v1`](https://lore.kernel.org/20261020-kconfig-fix-mfentry-check-v1-0-428258bc0fc0@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `random: vDSO: avoid call to memset() when zeroing reserved parameter` ([6.12](https://lore.kernel.org/20261003144155.1375966-2-nathan@kernel.org/), [6.18](https://lore.kernel.org/20261003144031.1375806-2-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `wifi: brcmsmac: Adjust txpwrindex value in nphy_ipa_rxcal_gaintbl_2GHz` ([`v1`](https://lore.kernel.org/20261005-brcmsmac-clang-24-constant-conversion-v1-1-a30c5692d582@kernel.org/))
  * `lib/base64: Silence clang-24 -Wconstant-conversion with diag pragmas` ([`v1`](https://lore.kernel.org/20261008-base64-silence-clang-24-constant-conversion-v1-1-0858b60b23c8@kernel.org/))
  * `ext2: Make sbi an proper alias for EXT2_SB(sb) in ext2_fill_super()` ([`v1`](https://lore.kernel.org/20261023-ext2-fix-context-analysis-clang-24-v1-1-2b35c33cd0fa@kernel.org/), [`v2`](https://lore.kernel.org/20261023-ext2-fix-context-analysis-clang-24-v2-1-a33961453899@kernel.org/))



## LLVM patches

* [`[clang] Introduce '-fint64-is-long-long'`](https://github.com/llvm/llvm-project/pull/234780)



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v4] scripts/config: honor $KBUILD_OUTPUT by default`](https://lore.kernel.org/179097210878.309835.10523307262544578337.b4-review@b4/)
* [`Re: [PATCH v4 1/3] scripts: add TOML config to container tool`](https://lore.kernel.org/179097236327.311770.8165714745450922135.b4-ty@b4/)
* [`Re: [PATCH] kbuild: add header check facility as a manually run static analyzer`](https://lore.kernel.org/179097250601.311770.12867462377142523689.b4-ty@b4/)
* [`Re: [PATCH] .gitignore: drop Module.markers`](https://lore.kernel.org/all/179113110514.1398860.18288939011236251563.b4-ty@b4/)
* [`Re: [PATCH] .gitignore: ignore pacman package signatures`](https://lore.kernel.org/179113111599.1398860.5391372805949286723.b4-ty@b4/)
* [`Re: [PATCH] arm_mpam: make the mon_sel guard conditional only`](https://lore.kernel.org/20261003211421.GA1389115@ax162/)
* [`[Clang][Sema] Suppress -Wconstant-conversion in unselected conditional operands`](https://github.com/llvm/llvm-project/pull/226775#issuecomment-5993295508)
* [`Re: [PATCH] kbuild: Makefile.lib: fix offsets generation with debug info`](https://lore.kernel.org/179121283718.1159753.6553387125743720689.b4-review@b4/)
* [`Re: [PATCH v5] scripts/config: honor $KBUILD_OUTPUT by default`](https://lore.kernel.org/179121581235.2275809.12565811683284940522.b4-review@b4/)
* [`Re: [PATCH] kbuild: install-extmod-build: install kernel/kallsyms_internal.h`](https://lore.kernel.org/20261006081116.GA830598@ax162/)
* [`Re: [PATCH] sparc64: Define p4d_page() instead of stubbing it to NULL`](https://lore.kernel.org/20261006083103.GA3041473@ax162/)
* [`Re: [PATCH v1 0/3] <linux/mod_devicetable.h> deprecation continues`](https://lore.kernel.org/179121379654.1159753.4410832336442519712.b4-review@b4/)
* [`Re: [PATCH v1 2/3] efi/libstub: Build the x86 stub from KBUILD_CFLAGS`](https://lore.kernel.org/20261007092013.GB1504630@ax162/)
* [`Re: [PATCH] gen_init_cpio: stop parsing on lines with a missing argument list`](https://lore.kernel.org/179136520162.1527194.9223276311672277059.b4-ty@b4/)
* [`Re: [PATCH] checkkconfigsymbols.py: Parse Git status output line by line`](https://lore.kernel.org/179137578902.1530818.14446580399575550007.b4-ty@b4/)
* [`Re: kconfig: cleanup stale comment about 'keys' array`](https://lore.kernel.org/179157780046.772474.17247617840025551450.b4-ty@b4/)
* [`Re: [PATCH v2] kconfig: fix infinite loop in conf_choice() when no choice value is visible`](https://lore.kernel.org/179157785332.772825.5892929930472929089.b4-ty@b4/)
* [`Re: [PATCH v2] kconfig: use the value argument in sym_get_near_range_bound()`](https://lore.kernel.org/179157791779.773304.13500902049943640827.b4-ty@b4/)
* [`Re: [PATCH v2] clang-tools: Import os for broken pipe handling`](https://lore.kernel.org/179157847654.778396.2514393103937606279.b4-ty@b4/)
* [`Re: [PATCH] landlock: Move the domain layer counter out of the anonymous union`](https://lore.kernel.org/20261009204554.GA778522@ax162/)
* [`Re: [PATCH v2 0/2] kbuild: Give jobserver-exec recipes the jobserver`](https://lore.kernel.org/179167827085.3510825.11265184087970988253.b4-ty@b4/)
* [`Re: [PATCH 1/2] Compiler Attributes: Add __require_explicit_initialization macro`](https://lore.kernel.org/20261014234445.GA182939@ax162/)
* [`Re: [PATCH RFC] kbuild: enable pipefail for commands where possible`](https://lore.kernel.org/20261014214043.GA3380464@ax162/)
* [`Re: [PATCH] gen_compile_commands: honor the build directory for modules`](https://lore.kernel.org/20261016012148.GD3329673@ax162/)
* [`Re: [PATCH 6.12 696/869] kcfi: Rename CONFIG_CFI_CLANG to CONFIG_CFI`](https://lore.kernel.org/20261017194548.GA318740@ax162/)
* [`Re: [PATCH] scsi: fnic: Annotate fnic_nvme functions with __must_hold()`](https://lore.kernel.org/20261018212232.GA4068315@ax162/)
* [`Revert "[clang][ThreadSafety] Do not read a C field's address as a pointer to member (#231604) (PR #233614)`](https://github.com/llvm/llvm-project/pull/233614#issuecomment-6279482896)
* [`Re: [PATCH] kbuild: improve sanity check for ARCH variable on x86`](https://lore.kernel.org/20261020003625.GA787785@ax162/)
* [`Re: [PATCH] kbuild: pass -fno-pic when probing for -mfentry`](https://lore.kernel.org/20261020215403.GA2506074@ax162/)
* [`Re: [PATCH 1/2] kbuild: Move testing of '-mfentry' back to Makefile from Kconfig`](https://lore.kernel.org/20261021182554.GA2594500@ax162/)
* [`[clang][ThreadSafety] Resolve fields named from nested struct/union members`](https://github.com/llvm/llvm-project/pull/233965#issuecomment-6329076715)
* [`Re: [PATCH bpf-next] bpf, x86: Use emit_rsb_call() to call bpf_unwind_target()`](https://lore.kernel.org/20261021234220.GA3640178@ax162/)
* [`Re: [PATCH] kbuild: prevent building single targets twice`](https://lore.kernel.org/20261021215524.GA627677@ax162/)
* [`Re: [PATCH] x86/build/64: Prevent native builds from generating SSE/AVX code`](https://lore.kernel.org/20261025194411.GA56787@ax162/)
* [`Re: [PATCH] x86/kcfi: Don't emit KCFI_REFERENCE() in modules`](https://lore.kernel.org/20261026214325.GA1330439@ax162/)
* [`Re: [PATCH v2 1/9] Compiler Attributes: Add __must_init macro`](https://lore.kernel.org/20261028190233.GA669046@ax162/)
* [`Re: [RFC PATCH 16/20] coverage: add MC/DC measurement`](https://lore.kernel.org/20261028191432.GA669102@ax162/)
* [`Re: [PATCH 0/4] Get rid of all NEON intrinsics stdint.h type hacks`](https://lore.kernel.org/20261029214105.GA44297@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: linux-next: manual merge of the random tree with the kbuild tree`](https://lore.kernel.org/20261001192731.GA713751@ax162/)
* [`Many instances of "no BTF func for kfunc ..." and "unresolved symbol ..." from resolve_btfids after pahole 1.32 upgrade`](https://lore.kernel.org/20261001224346.GA576434@ax162/)
* [`Re: [PATCH v2] random: vDSO: Avoid call to memset() when zeroing reserved in __cvdso_getrandom_data()`](https://lore.kernel.org/20261002094932.GA3435055@ax162/)
* [`Re: linux-next: manual merge of the bpf-next tree with the kbuild tree`](https://lore.kernel.org/20261002230632.GA314599@ax162/)
* [```Re: Use of `gawk -M' in kernel build scripts```](https://lore.kernel.org/20261005085332.GA2094974@ax162/)
* [`Re: linux-next: manual merge of the kbuild tree with the rust tree`](https://lore.kernel.org/20261005110503.GA1026747@ax162/)
* [`Re: linux-next: manual merge of the kbuild tree with the mm-nonmm-unstable tree`](https://lore.kernel.org/20261005131417.GA1097851@ax162/)
* [`False positive -Wconstant-conversion after signed char array enablement`](https://github.com/llvm/llvm-project/issues/223923#issuecomment-6058562241)
* [`[Clang][AArch64] Diagnose invalid FPM scale arguments`](https://github.com/llvm/llvm-project/pull/227722#issuecomment-6065215071)
* [`Re: [PATCH v2 10/14] hv: vmbus: pin buffer pages across UIO mmap to close the reclaim race`](https://lore.kernel.org/20261008175136.GA3580170@ax162/)
* [`Build breakage from renamed '-split-machine-functions' plugin option`](https://lore.kernel.org/20261009214501.GA905361@ax162/)
* [`Re: linux-next: manual merge of the efi tree with the kbuild tree`](https://lore.kernel.org/20261009215215.GA981232@ax162/)
* [`Re: linux-next: manual merge of the kapi tree with the kbuild tree`](https://lore.kernel.org/20261010134653.GA1853402@ax162/)
* [`Re: <inline asm>:1:275: error: invalid instruction mnemonic 'brk'`](https://lore.kernel.org/20261015212525.GA322951@ax162/)
* [`Re: [PATCH v4 26/54] scsi: fnic: Enable lock context analysis`](https://lore.kernel.org/20261016023120.GA2863622@ax162/)
* [`Re: [PATCH v2] kbuild: Makefile.lib: fix offsets generation with debug info`](https://lore.kernel.org/20261017214129.GA404142@ax162/)
* [`-Wthread-safety-precise in crypto/scompress.c after LLVM commit 217e229a1f32785fccdd3b3f51429f0d309dea45`](https://github.com/ClangBuiltLinux/linux/issues/2187)
* [`Missing -Wc23-extensions after 4c9fd71e18f211bfe7d43ef841b429a8ef94a140`](https://github.com/llvm/llvm-project/issues/233294)
* [`Re: linux-next: manual merge of the kbuild-current tree with the origin tree`](https://lore.kernel.org/20261021185857.GB2608581@ax162/)
* [`Re: linux-next: manual merge of the rust tree with the kbuild-fixes tree`](https://lore.kernel.org/20261021190017.GA2611266@ax162/)
* [`Re: [PATCH v4 bpf-next 2/6] bpf, x86: Support unwind_goto and unwind_exit`](https://lore.kernel.org/20261021201456.GA3197963@ax162/)
* [`Re: error[E0080]: evaluation panicked: assertion failed: size_of::<isize>() == size_of::<isize_atomic_repr>()`](https://lore.kernel.org/20261026215551.GD1330439@ax162/)
* [`Re: vmlinux.o: warning: objtool: mtk_timer_probe+0x23: no-cfi indirect call!`](https://lore.kernel.org/20261028202400.GA835963@ax162/)
* [`Re: [PATCH 36/98] blk-mq: quiesce a queue that joins a quiesced tag set`](https://lore.kernel.org/20261030223702.GA1673281@ax162/)
* [`resolve_btfids segmentation fault with CONFIG_DEBUG_INFO_BTF_INLINE`](https://lore.kernel.org/20261031040305.GA2789538@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update stable anchor to 7.3`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/941)
* [`Bump PGO kernel to 7.3 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/353)
* [`tc_build: llvm: Add libclang to distribution components for kernel and rust profiles`](https://github.com/ClangBuiltLinux/tc-build/pull/354)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I submitted the following pull requests to Linus.

  * [Kbuild / Kconfig updates for 7.4](https://lore.kernel.org/20261018035817.GA2365306@ax162/)
  * [Kbuild fixes for 7.4 #1](https://lore.kernel.org/20261030192517.GA49624@ax162/)


* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [23.1.2 / 23.1.3](https://lore.kernel.org/20261014232030.GA3382959@ax162/)
  * [23.1.4](https://lore.kernel.org/20261021001327.GA2730064@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
