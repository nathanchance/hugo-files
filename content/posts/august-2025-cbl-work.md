---
title: August 2025 ClangBuiltLinux Work
date: 2025-08-29T16:30:00-0700
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

  * `hardening: Require clang 20.1.0 for __counted_by` ([`v1`](https://lore.kernel.org/20250807-fix-counted_by-clang-19-v1-1-902c86c1d515@kernel.org/))
  * `riscv: Use 64-bit variable for output in __get_user_asm` ([`v1`](https://lore.kernel.org/20250811-riscv-wa-llvm-asm-goto-outputs-assertion-failure-v1-1-7bb8c9cbb92b@kernel.org/))
  * `dm-pcache: Fix 64-bit division for 32-bit platforms in get_kset_id()` ([`v1`](https://lore.kernel.org/20250821-dm-pcache-fix-32-bit-div-err-v1-1-cab5448f44e6@kernel.org/))
  * `drm/bridge: cdns-dsi: Select VIDEOMODE_HELPERS` ([`v1`](https://lore.kernel.org/20250821-cdns-videohelpers-v1-1-853e021908cf@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `x86: Clean ups after minimum support compiler version bump` ([`v1`](https://lore.kernel.org/20250814-x86-min-ver-cleanups-v1-0-ff7f19457523@kernel.org/))
  * `Bump minimum supported version of LLVM for building the kernel to 15.0.0` ([`v1`](https://lore.kernel.org/20250818-bump-min-llvm-ver-15-v1-0-c8b1d0f955e0@kernel.org/), [`v2`](https://lore.kernel.org/20250821-bump-min-llvm-ver-15-v2-0-635f3294e5f0@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`[PATCH 5.4 0/6] Fix build due to clang -Qunused-arguments change`](https://lore.kernel.org/20250811235151.1108688-1-nathan@kernel.org/)
  * `kbuild: userprogs: use correct linker when mixing clang and GNU ld` ([`6.6`](https://lore.kernel.org/20250821182949.1216551-1-nathan@kernel.org/), [`6.1`](https://lore.kernel.org/20250821183051.1259435-1-nathan@kernel.org/), [`5.15`](https://lore.kernel.org/20250821183058.1264091-1-nathan@kernel.org/), [`5.10`](https://lore.kernel.org/20250821183106.1268616-1-nathan@kernel.org/), [`5.4 comment`](https://lore.kernel.org/20250821183553.GA1697222@ax162/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `bcachefs: Add statement to switch in check_bch_counter_ids_unique()` ([`v1`](https://lore.kernel.org/20250812-bcachefs-fix-switch-check_bch_counter_ids_unique-v1-1-def262fd6574@kernel.org/))
  * `mm/rmap: Always inline __folio_rmap_sanity_checks()` ([`v1`](https://lore.kernel.org/20250814-rmap-fix-build_bug-conversion-v1-1-fb7b10a0b362@kernel.org/))
  * `mmc: sdhci-cadence: Fix -Wuninitialized in sdhci_cdns_tune_blkgap()` ([`v1`](https://lore.kernel.org/20250819-mmc-sdhci-cadence-fix-uninit-hrs37_mode-v1-1-94aa2d0c438a@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v3 0/8] cleanup: Introduce ACQUIRE(), a guard() for conditional locks`](https://lore.kernel.org/20250801190203.GA939298@ax162/)
* [`Re: [PATCH] kbuild: Re-enable -Wunterminated-string-initialization`](https://lore.kernel.org/20250802004316.GA3910513@ax162/)
* [`Re: [PATCH v2] kbuild: Re-enable -Wunterminated-string-initialization`](https://lore.kernel.org/20250803173235.GA716998@ax162/)
* [`Re: [PATCH 6.1.y] KVM: arm64: sys_regs: disable -Wuninitialized-const-pointer warning`](https://lore.kernel.org/20250806222557.GA1654483@ax162/)
* [`Re: [PATCH 5.15.y] KVM: arm64: sys_regs: disable -Wuninitialized-const-pointer warning`](https://lore.kernel.org/20250806222624.GB1654483@ax162/)
* [`Re: [PATCH] Makefile: mrproper deletes signing_key.x509`](https://lore.kernel.org/20250811191408.GA169691@ax162/)
* [`Re: [PATCH] ALSA: hda/ca0132: Fix compile error with CLASS() after label`](https://lore.kernel.org/20250813150131.GA2842554@ax162/)
* [`Re: [PATCH 0/6] kbuild: uapi: various fixes`](https://lore.kernel.org/20250812234458.GA52733@ax162/)
* [`Re: [PATCH v2] .gitignore: ignore compile_commands.json globally`](https://lore.kernel.org/20250812221424.GA488781@ax162/)
* [`Re: [RFC][PATCH] x86,ibt: Use UDB instead of 0xEA`](https://lore.kernel.org/20250814194029.GA2179272@ax162/)
* [`Re: [PATCH 1/2] sparc/module: Add R_SPARC_UA64 relocation handling`](https://lore.kernel.org/20250814224009.GA2217114@ax162/)
* [`Re: [PATCH 0/2] kbuild: userprogs: also inherit byte order and ABI from kernel`](https://lore.kernel.org/175529257494.1745051.12925907473487620695.b4-ty@kernel.org/)
* [`Re: [PATCH 0/2] Fix objtool warnings if LTO is enabled for LoongArch`](https://lore.kernel.org/20250814230137.GA2247447@ax162/)
* [`Re: [PATCH v2 6/6] kbuild: enable -Werror for hostprogs`](https://lore.kernel.org/20250818174534.GA1261249@ax162/)
* [`Re: [PATCH] kconfig: qconf/xconfig: show the OptionsMode radio button setting at startup`](https://lore.kernel.org/175554087932.3761802.7665709422071619681.b4-ty@kernel.org/)
* [`Re: [PATCH v6 4/9] scsi: Always define blogic_pci_tbl structure`](https://lore.kernel.org/20250825165616.GA2719297@ax162/)
* [`Re: [PATCH] scripts/misc-check: update export checks for EXPORT_SYMBOL_FOR_MODULES()`](https://lore.kernel.org/20250825170710.GC2719297@ax162/)
* [`Re: [PATCH v2 2/4] PCI/MSI: Add startup/shutdown for per device domains`](https://lore.kernel.org/20250827004719.GA2519033@ax162/)
* [`Re: [PATCH 5/5] kcfi: Rename CONFIG_CFI_CLANG to CONFIG_CFI`](https://lore.kernel.org/20250827013444.GA2859318@ax162/)
* [`Re: [PATCH 1/5] compiler_types.h: Move __nocfi out of compiler-specific header`](https://lore.kernel.org/20250827194657.GA3572128@ax162/)
* [`Re: [PATCH 3/5] x86/cfi: Add option for cfi=debug bootparam`](https://lore.kernel.org/20250827195753.GB3572128@ax162/)
* [`Re: [PATCH 2/2] kbuild: userprogs: also inherit byte order and ABI from kernel`](https://lore.kernel.org/20250827224935.GB414199@ax162/)
* [`Re: [PATCH] extract-vmlinux: Output used decompression method`](https://lore.kernel.org/175642054004.464288.16840468164440733967.b4-ty@kernel.org/)
* [`Re: [PATCH v2 0/4] Enable measuring the kernel's Source-based Code Coverage and MC/DC with Clang`](https://lore.kernel.org/20250829181007.GA468030@ax162/)
* [`Re: [PATCH v2 00/12] Bump minimum supported version of LLVM for building the kernel to 15.0.0`](https://lore.kernel.org/175650682606.3003527.17329504429724755241.b4-ty@kernel.org/)
* [`Re: [PATCH] dmaengine: ti: edma: Fix memory allocation size for queue_priority_map`](https://lore.kernel.org/20250829232132.GA1983886@ax162/)
* [`Re: [PATCH] .gitignore: ignore temporary files from 'bear'`](https://lore.kernel.org/20250829233824.GB1983886@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Reland "RegisterCoalescer: Add implicit-def of super register when coalescing SUBREG_TO_REG"`](https://github.com/llvm/llvm-project/pull/134408#issuecomment-3145468321)
* [`Re: Add AT_* constants from Linux 6.12`](https://lore.kernel.org/20250803175420.GA1386776@ax162/)
* [`Failed assertion in writeSectionData after LLVM commit faa931b717c02d57f0814caa9133219040e6a85b`](https://github.com/ClangBuiltLinux/linux/issues/2116)
* [`Re: next-20250804: clang-nightly hardening.config boot failed on arm64 rock-pi-4b`](https://lore.kernel.org/20250804175338.GA2197404@ax162/)
* [`Re: ld.lld: error: Function Import: link error: linking module flags 'Code Model': IDs have conflicting values: 'i32 3' from vmlinux.a(init.o at 861458), and 'i32 1' from vmlinux.a(net-traces.o at 1004198)`](https://lore.kernel.org/20250806221935.GA1397381@ax162/)
* [`[ELF] -r: Synthesize R_RISCV_ALIGN at input section start`](https://github.com/llvm/llvm-project/pull/151639#issuecomment-3172332391)
* [`Re: [BUG] Objtool warning from -next and mainline`](https://lore.kernel.org/20250814224356.GA2247343@ax162/)
* [`[Clang] improve -Wstring-concatenation to warn on every missing comma in initializer lists`](https://github.com/llvm/llvm-project/pull/154018#issuecomment-3199396282)
* [`[BOLT] Instrumented aarch64 clang-21 crashes with illegal instruction`](https://github.com/llvm/llvm-project/issues/153123)
* [`[BOLT] Instrumented aarch64 clang-21 hangs indefinitely`](https://github.com/llvm/llvm-project/issues/153492)
* [`Re: [GIT PULL] tracing: Fixes for v6.17`](https://lore.kernel.org/20250822192437.GA458494@ax162/)
* [`Re: [linux-next:master 3139/4234] error: <unknown>:0:0: ran out of registers during register allocation in function 'hv_call_get_vp_registers'`](https://lore.kernel.org/20250825170051.GB2719297@ax162/)
* [`Re: [akpm-mm:mm-new 216/233] arch/riscv/include/asm/pgtable.h:951:36: error: too few arguments to function call, expected 3, have 2`](https://lore.kernel.org/20250825200715.GA598466@ax162/)
* [`Re: [PATCH v2 2/4] PCI/MSI: Add startup/shutdown for per device domains`](https://lore.kernel.org/20250826220959.GA4119563@ax162/)
* [`Re: [linux-next:master 4166/4221] vmlinux.o: warning: objtool: dev_pm_opp_find_level_exact+0x47: no-cfi indirect call!`](https://lore.kernel.org/20250827223247.GA414199@ax162/)
* [`[X86][APX] Remove redundant TEST*ri instructions`](https://github.com/llvm/llvm-project/pull/155586#issuecomment-3235249433)
* [`Re: [PATCH 2/5] ASoC: Intel: avs: Cleanup duplicate members`](https://lore.kernel.org/20250829225532.GA400117@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update stable anchor to 6.16 and update patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/861)
* [`patches: android14-5.15: Use 6.1 version of KVM patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/862)
* [`build-llvm.py: Update DEFAULT_KERNEL_FOR_PGO to 6.16`](https://github.com/ClangBuiltLinux/tc-build/pull/305)
* [`Visualize and adjust builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/863)
* [`Add support for LLVM 21`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/864)
* [`Reset CONFIG_EFI_SBAT_FILE for Fedora configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/865)
* [`patches: Drop KCSAN dummy variable patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/866)
* [`scripts/check-logs.py: Treat empty string values as 'n' for config check`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/867)
* [`ci: Regenerate clang-21 GitHub Actions workflow and TuxSuite files`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/868)
* [`Drop patches released in the most recent LTS updates (August 28, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/869)
* [`Drop support for LLVM 13 and 14 from -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/870)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, and an AMD-based desktop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [21.1.0-rc3](https://lore.kernel.org/20250812234847.GA265853@ax162/)
  * [21.1.0](https://lore.kernel.org/20250827033843.GA2135688@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
