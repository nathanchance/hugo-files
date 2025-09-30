---
title: September 2025 ClangBuiltLinux Work
date: 2025-09-30T16:30:00-0700
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

  * `drm/tidss: dispc: Explicitly include bitfield.h` ([`v1`](https://lore.kernel.org/20250902-drm-tidss-fix-missing-bitfield-h-v1-1-aaad4a285f98@kernel.org/))
  * `mfd: tps6594: Explicitly include bitfield.h` ([`v1`](https://lore.kernel.org/20250904-mfd-tps6594-core-fix-bitfield-h-v1-1-5d0f00cfe58f@kernel.org/))
  * `md/md-llbitmap: Use DIV_ROUND_UP_SECTOR_T` ([`v1`](https://lore.kernel.org/20250910-llbitmap-fix-64-div-for-32-bit-v1-1-453a5c8e3e00@kernel.org/))
  * `tpm: loongson: Add bufsiz parameter to tpm_loongson_send()` ([`v1`](https://lore.kernel.org/20250917-tpm-loongson-add-bufsiz-v1-1-972a75c0aab2@kernel.org/))
  * `nfsd: Avoid strlen conflict in nfsd4_encode_components_esc()` ([`v1`](https://lore.kernel.org/20250925-nfsd-fix-trace-printk-strlen-error-v1-1-1360530e4c6b@kernel.org/), [`v2`](https://lore.kernel.org/20250928-nfsd-fix-trace-printk-strlen-error-v2-1-108def6ff41c@kernel.org/), [`v3`](https://lore.kernel.org/20250930-nfsd-fix-trace-printk-strlen-error-v3-1-536cc9822ee6@kernel.org/))
  * `Fixes for pmac32_defconfig after fb.h removal from backlight.h` ([`v1`](https://lore.kernel.org/20250925-ppc-fixes-for-backlight-fb-h-removal-v1-0-d256858d86a6@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `nilfs2: fix CFI failure when accessing /sys/fs/nilfs2/features/*` ([`v1`](https://lore.kernel.org/20250905-nilfs2-fix-features-cfi-violation-v1-1-b5d35136d813@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `[PATCH 5.4 only] powerpc: boot: Remove unnecessary zero in label in udelay()` ([`v1`](https://lore.kernel.org/20250902235234.2046667-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20250903211158.2844032-1-nathan@kernel.org/))
  * `[PATCH 6.6 ONLY] s390/cpum_cf: Fix uninitialized warning after backport of ce971233242b` ([`v1`](https://lore.kernel.org/20250922-6-6-s390-cpum_cf-fix-uninit-err-v1-1-5183aa9af417@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `compiler-clang.h: Define __SANITIZE_*__ macros only when undefined` ([`v1`](https://lore.kernel.org/20250902-clang-update-sanitize-defines-v1-1-cf3702ca3d92@kernel.org/))
  * `objtool: Ignore __pi___cfi_ prefixed symbols` ([`v1`](https://lore.kernel.org/20250908-x86-startup-fix-pi-cfi-warnings-v1-1-4b42b43fecf3@kernel.org/))
  * `drm/pixpaper: Fix return type of pixpaper_mode_valid()` ([`v1`](https://lore.kernel.org/20250908-drm-pixpaper-fix-mode_valid-return-type-v1-1-705ceaf03757@kernel.org/))
  * `drm/omap: Mark dispc_save_context() with noinline_for_stack` ([`v1`](https://lore.kernel.org/20250911-omapdrm-reduce-clang-stack-usage-pt-2-v1-1-5ab6b5d34760@kernel.org/))
  * `mei: late_bind: Fix -Wincompatible-function-pointer-types-strict` ([`v1`](https://lore.kernel.org/20250920-drm-xe-fix-wifpts-v1-1-c89b5357c7ba@kernel.org/))
  * `btrfs: Fix PAGE_SIZE format specifier in open_ctree()` ([`v1`](https://lore.kernel.org/20250925-btrfs-fix-page_size-format-specifier-v1-1-8f98d300a909@kernel.org/))
  * `Kconfig fixes for QCOM clk drivers when targeting ARCH=arm` ([`v1`](https://lore.kernel.org/20250930-clk-qcom-kconfig-fixes-arm-v1-0-15ae1ae9ec9f@kernel.org/))



## LLVM patches

* [`[Frontend] Define __SANITIZE__ macros for kernel address variants`](https://github.com/llvm/llvm-project/pull/156543)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 00/17] ASoC: Intel: avs: Adjust platform names`](https://lore.kernel.org/20250902212025.GA2905120@ax162/)
* [`Re: [PATCH 5.4 00/23] 5.4.298-rc1 review`](https://lore.kernel.org/20250903172859.GB3288670@ax162/)
* [`Re: [PATCH 2/2] kbuild: userprogs: also inherit byte order and ABI from kernel`](https://lore.kernel.org/20250903223131.GA2264021@ax162/)
* [`Re: [PATCH v2 1/9] compiler_types.h: Move __nocfi out of compiler-specific header`](https://lore.kernel.org/20250904182829.GA2110053@ax162/)
* [`Re: [PATCH v2 3/9] x86/cfi: Document the "cfi=" bootparam options`](https://lore.kernel.org/20250904183254.GB2110053@ax162/)
* [`Re: [PATCH v2 4/9] x86/cfi: Standardize on common "CFI:" prefix for CFI reports`](https://lore.kernel.org/20250904184004.GC2110053@ax162/)
* [`Re: [PATCH v2 18/22] phy: apple: Add Apple Type-C PHY`](https://lore.kernel.org/20250909222544.GA3282617@ax162/)
* [`Re: [PATCH 1/3] Compiler Attributes: Add __assume macro`](https://lore.kernel.org/20250911013243.GA292340@ax162/)
* [`Re: [PATCH v1 1/1] kexec: Remove unused code in kimage_load_cma_segment()`](https://lore.kernel.org/20250915221436.GA925462@ax162/)
* [`Re: [PATCH net-next V2] net/mlx5: Improve write-combining test reliability for ARM64 Grace CPUs`](https://lore.kernel.org/20250915221859.GB925462@ax162/)
* [`Re: [patch V2 2/6] kbuild: Disable asm goto on clang < 17`](https://lore.kernel.org/20250916184440.GA1245207@ax162/)
* [`Re: [PATCH v7 3/8] kbuild: keep .modinfo section in vmlinux.unstripped`](https://lore.kernel.org/20250917011010.GA3106929@ax162/)
* [`Re: [PATCH v2 2/4] compiler_types: Add __assume macro`](https://lore.kernel.org/20250917023016.GA3935228@ax162/)
* [`Re: [PATCH v8 0/8] Add generated modalias to modules.builtin.modinfo`](https://lore.kernel.org/20250918173529.GA3360630@ax162/)
* [`Re: [PATCH] LoongArch: Fix build error for LTO with LLVM-18`](https://lore.kernel.org/20250923230705.GA2598658@ax162/)
* [`Re: [PATCH next] modpost: Initialize builtin_modname to stop SIGSEGVs`](https://lore.kernel.org/175906151390.2640739.5095902499304192973.b4-ty@kernel.org/)
* [`Re: [PATCH] dmaengine: mmp_pdma: fix DMA mask handling`](https://lore.kernel.org/20250930221001.GA66006@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`clang-22 -Walloc-size in mm/kfence/kfence_test.c in 6.6 and 6.1`](https://lore.kernel.org/20250903000752.GA2403288@ax162/)
* [`Re: next-20250903 x86_64 clang-20 allyesconfig mmp_pdma.c:1188:14: error: shift count >= width of type [-Werror,-Wshift-count-overflow]`](https://lore.kernel.org/20250903165931.GA3288670@ax162/)
* [`Re: [PATCH -v1 1/2] x86/microcode: Add microcode= cmdline parsing`](https://lore.kernel.org/20250904232952.GA875818@ax162/)
* [`Re: linux-next: manual merge of the kbuild tree with Lnus' tree`](https://lore.kernel.org/20250908153432.GA1725137@ax162/)
* [`Re: [jirislaby:devel 28/53] drivers/tty/vt/keyboard.c:1789:7: error: cannot jump from this asm goto statement to one of its possible targets`](https://lore.kernel.org/20250909215342.GA2456480@ax162/)
* [`[InstCombine] Strip leading zero indices from GEP`](https://github.com/llvm/llvm-project/pull/155415#issuecomment-3276429149)
* [`Re: [Linaro-TCWG-CI] llvmorg-22-init-6898-gdf430c33a71f: Failure on aarch64`](https://lore.kernel.org/20250910161555.GA1958434@ax162/)
* [`Re: [drm-xe:drm-xe-next 11/13] drivers/gpu/drm/xe/xe_migrate.c:422:3: error: cannot jump from this indirect goto statement to one of its possible targets`](https://lore.kernel.org/20250910193928.GA2640818@ax162/)
* [`Re: [GIT PULL] Immutable branch between MFD, Char and Crypto due for the v6.18 merge window`](https://lore.kernel.org/20250912213256.GA3062565@ax162/)
* [`Re: [jgunthorpe:iommu_pt_vtd 8/34] ERROR: modpost: "__udivdi3" [drivers/iommu/generic_pt/fmt/iommu_amdv1.ko] undefined!`](https://lore.kernel.org/20250917022043.GB3106929@ax162/)
* [`PPC: Split 64bit target feature into 64bit and 64bit-support`](https://github.com/llvm/llvm-project/pull/157206#issuecomment-3313700961)
* [`Re: [linux-next:master 9765/10654] drivers/gpu/drm/xe/tests/xe_pci.c:214:2: error: initializer element is not a compile-time constant`](https://lore.kernel.org/20250919223823.GB44805@ax162/)
* [`Re: [PATCH v1 1/2] LoongArch: Make LTO case independent in Makefile`](https://lore.kernel.org/20250920061536.GA1460394@ax162/)
* [`Re: [PATCH V2] LoongArch: Align ACPI structures if ARCH_STRICT_ALIGN enabled`](https://lore.kernel.org/20250920234836.GA3857420@ax162/)
* [`Re: linux-next: manual merge of the drm-xe tree with the drm-fixes tree`](https://lore.kernel.org/20250922182832.GA1542561@ax162/)
* [`Re: [RFC PATCH v3 6/9] NFSv4/flexfiles: Commit path updates for striped layouts`](https://lore.kernel.org/20250922203415.GA2873812@ax162/)
* [`Re: [PATCH] rv: Fix wrong type cast in enabled_monitors_next()`](https://lore.kernel.org/20250923002004.GA2836051@ax162/)
* [`Re: [PATCH v2 2/5] KVM: Export KVM-internal symbols for sub-modules only`](https://lore.kernel.org/20250923012738.GA4102030@ax162/)
* [`Re: [PATCH v5] tpm: Prevent local DOS via tpm/tpm0/ppi/*operations`](https://lore.kernel.org/20250923200748.GA3355497@ax162/)
* [`Re: Tool directory build problem..`](https://lore.kernel.org/20250929182512.GA1394437@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`patches: tip: Drop drm/msm -Wsometimes-uninitialized patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/872)
* [`Update patches (September 4, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/873)
* [`Revert "generator: yml: Disable CONFIG_DEBUG_INFO_COMPRESSED_ZSTD in Android configs"`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/874)
* [`patches: tip: Drop mt7996 -Wuninitialized-const-pointer change`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/875)
* [`patches: Remove android15-6.6 (September 8, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/876)
* [`Disable ARM allyesconfig on 5.15 with clang-11 and clang-12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/877)
* [`patches: Drop backport of 87b07a1fbc6b5c23d3b3584ab4288bc9106d3274`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/878)
* [`build-llvm.py: Add ability to override "true" arguments`](https://github.com/ClangBuiltLinux/tc-build/pull/306)
* [`tc_build: kernel: Use virtconfig for arm64 profiling target`](https://github.com/ClangBuiltLinux/tc-build/pull/307)
* [`tc_build: Enable '--conservative-instrumentation' for BOLT on Apple Silicon`](https://github.com/ClangBuiltLinux/tc-build/pull/308)
* [`tc_build: llvm: Handle some more deprecated BOLT option values`](https://github.com/ClangBuiltLinux/tc-build/pull/309)
* [`Update stable anchor to 6.17`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/879)
* [`Handle CONFIG_CFI_CLANG → CONFIG_CFI`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/880)
* [`Disable arm64 big endian builds on -next and mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/881)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and an AMD-based device. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [21.1.1](https://lore.kernel.org/20250910235830.GA1133148@ax162/)
  * [`Re: [PATCH v3 00/35] Compiler-Based Capability- and Locking-Analysis`](https://lore.kernel.org/20250918194026.GA3379805@ax162/)
  * [`Re: [PATCH v3 00/35] Compiler-Based Capability- and Locking-Analysis`](https://lore.kernel.org/20250923194915.GA2127565@ax162/)
  * [21.1.2](https://lore.kernel.org/20250924033603.GA1736082@ax162/)

* [`[GIT PULL] Kbuild updates for 6.18`](https://lore.kernel.org/20250930041119.GA1936516@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
