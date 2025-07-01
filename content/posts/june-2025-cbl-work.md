---
title: June 2025 ClangBuiltLinux Work
date: 2025-06-30T16:30:00-0700
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

  * `drm/sitronix: st7571-i2c: Select VIDEOMODE_HELPERS` ([`v1`](https://lore.kernel.org/20250610-drm-st7571-i2c-select-videomode-helpers-v1-1-d30b50ff6e64@kernel.org/))
  * `riscv: Require clang-17 or newer for kCFI` ([`v1`](https://lore.kernel.org/20250612-riscv-require-clang-17-for-kcfi-v1-1-216f7cd7d87f@kernel.org/))
  * `loongarch: Use '.ascii' instead of '.string' in __BUGVERBOSE_LOCATION` ([`v1`](https://lore.kernel.org/20250616-loongarch-fix-warn-cond-llvm-ias-v1-1-6c6d90bb4466@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `ARM: Use an absolute path to unified.h in KBUILD_AFLAGS` ([`v1`](https://lore.kernel.org/20250618-arm-expand-include-unified-h-path-v1-1-aef6eb4c44ca@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Backports for 6.1 and older due to clang -Qunused-arguments change`](https://lore.kernel.org/20250604233141.GA2374479@ax162/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `bcachefs: Fix -Wc23-extensions in bch2_check_dirents()` ([`v1`](https://lore.kernel.org/20250604-bcachefs-fix-clang-label-followed-by-declaration-v1-1-68a8bce1d981@kernel.org/))
  * `staging: rtl8723bs: Avoid memset() in aes_cipher() and aes_decipher()` ([`v1`](https://lore.kernel.org/20250609-rtl8723bs-fix-clang-arm64-wflt-v1-1-e2accba43def@kernel.org/))
  * `crypto: lib/curve25519-hacl64 - Disable KASAN with clang-17 and older` ([`v1`](https://lore.kernel.org/20250609-curve25519-hacl64-disable-kasan-clang-v1-1-08ea0ac5ccff@kernel.org/))
  * `mmc: rtsx_usb_sdmmc: Fix clang -Wimplicit-fallthrough in sd_set_power_mode()` ([`v1`](https://lore.kernel.org/20250620-mmc-rtsx-usb-sdmmc-fix-clang-implicit-fallthrough-v1-1-4031d11159c0@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 1/2] kbuild: set y instead of 1 to KBUILD_{BUILTIN,MODULES}`](https://lore.kernel.org/20250602220431.GA924363@ax162/)
* [`Re: [PATCH v2 1/4] kbuild: move W=1 check for scripts/misc-check to top-level Makefile`](https://lore.kernel.org/20250602220624.GB924363@ax162/)
* [`Re: [PATCH] drm/i915/pmu: Fix build error with GCOV and AutoFDO enabled`](https://lore.kernel.org/20250602221656.GC924363@ax162/)
* [`Add basic support for building Rust`](https://github.com/ClangBuiltLinux/tc-build/pull/299#pullrequestreview-2890150114)
* [`Re: [PATCH] riscv: vdso: Exclude .rodata from the PT_DYNAMIC segment`](https://lore.kernel.org/20250603215218.GA3631276@ax162/)
* [`Re: [PATCH] LoongArch: vDSO: correctly use asm parameters in syscall wrappers`](https://lore.kernel.org/20250603215427.GB3631276@ax162/)
* [`Re: [PATCH v3 1/3] riscv: vdso: Deduplicate CFLAGS_REMOVE_* variables`](https://lore.kernel.org/20250611184018.GA2192624@ax162/)
* [`Re: [PATCH v3 2/3] riscv: vdso: Disable LTO for the vDSO`](https://lore.kernel.org/20250611184106.GB2192624@ax162/)
* [`Re: [PATCH v3 3/3] vdso: Reject absolute relocations during build`](https://lore.kernel.org/20250611185727.GC2192624@ax162/)
* [`Re: [PATCH] kbuild: move warnings about linux/export.h from W=1 to W=2`](https://lore.kernel.org/20250612181551.GA815757@ax162/)
* [`Re: FAILED: patch "[PATCH] kbuild: userprogs: fix bitsize and target detection on clang" failed to apply to 5.4-stable tree`](https://lore.kernel.org/20250617232006.GB3356351@ax162/)
* [`Re: FAILED: patch "[PATCH] kbuild: hdrcheck: fix cross build with clang" failed to apply to 5.4-stable tree`](https://lore.kernel.org/20250617233200.GC3356351@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`[Clang] Add resource_dir_EQ flag to CC1Option group`](https://github.com/llvm/llvm-project/pull/140870#issuecomment-2931610238)
* [`Re: [BUG?] clang miscompilation of inline ASM with overlapping input/output registers`](https://lore.kernel.org/20250602193718.GA915101@ax162/)
* [`Re: [PATCH v4 15/21] drm/i915/flipq: Provide the nuts and bolts code for flip queue`](https://lore.kernel.org/20250609213505.GA1634981@ax162/)
* [`bpf-restrict-fs fails to load without DYNAMIC_FTRACE_WITH_DIRECT_CALLS on arm64`](https://lore.kernel.org/20250610232418.GA3544567@ax162/)
* [```[SelectionDAG] Make `(a & x) | (~a & y) -> (a & (x ^ y)) ^ y` available for all targets```](https://github.com/llvm/llvm-project/pull/137641#issuecomment-2960997509)
* [`Re: selftests/filesystem: clang warning null passed to a callee that requires a non-null argument [-Wnonnull]`](https://lore.kernel.org/20250611175052.GA2307190@ax162/)
* [`[clang] Reset FileID based diag state mappings`](https://github.com/llvm/llvm-project/pull/143695#issuecomment-2968423968)
* [`Re: ld.lld: error: Cannot export BSS symbol __inittext_end to startup code`](https://lore.kernel.org/20250616175026.GA1187576@ax162/)
* [`as-instr in Kbuild broken for arch/arm`](https://lore.kernel.org/20250617200406.GA3636948@ax162/)
* [`[Reland] [PowerPC] frontend get target feature from backend with cpu name`](https://github.com/llvm/llvm-project/pull/144594#issuecomment-2993736654)
* [`Re: [PATCH 5.10 000/352] 5.10.239-rc2 review`](https://lore.kernel.org/20250625235008.GA4073092@ax162/)
* [`Non constant size and offset in DWARF`](https://github.com/llvm/llvm-project/pull/141106#issuecomment-3010987379)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Updates patches (June 2, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/840)
* [`Drop d0afcfeb9e3810ec89d1ffde1a0e36621bb75dca from most upstream stable trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/841)
* [`Disable CONFIG_FORTIFY_KUNIT_TEST for allmodconfig + LTO (part 2)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/842)
* [`Drop SCSI QEDF patch from -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/843)
* [`boot-qemu.py: Fix always downloading LoongArch firmware`](https://github.com/ClangBuiltLinux/boot-utils/pull/123)
* [`Disable RISC-V full LTO build on mainline with LLVM 14`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/844)
* [`patches: Drop SCSI QEDF patch from several trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/845)
* [`patches: 6.6: Drop -Wdefault-const-init-unsafe patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/846)
* [`generator: yml: Update Alpine Linux configuration links`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/847)
* [`Update Android LLVM ARM builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/848)
* [`utils.py: Update get_cbl_name() for new Alpine configuration scheme`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/849)
* [`Update patches (June 25, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/850)
* [`utils.py: Fix get_cbl_name() for Alpine config change for real`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/851)
* [`patches: stable: Drop iwlwifi fortify workaround`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/852)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [20.1.7](https://lore.kernel.org/20250613213311.GA1445087@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
