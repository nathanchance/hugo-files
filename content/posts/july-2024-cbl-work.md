---
title: July 2024 ClangBuiltLinux Work
date: 2024-07-31T16:30:00-0700
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

  * `ASoC: fsl: lpc3xxx-i2s: Include bitfield.h for FIELD_PREP` ([`v1`](https://lore.kernel.org/20240701-lpc32xx-asoc-fix-include-for-field_prep-v1-1-0c5d7f71921b@kernel.org/))
  * `kbuild: Make ld-version.sh more robust against version string changes` ([`v1`](https://lore.kernel.org/20240704-update-ld-version-for-new-lld-ver-str-v1-1-91bccc020a93@kernel.org/), [`v2`](https://lore.kernel.org/20240707-update-ld-version-for-new-lld-ver-str-v2-1-8f24421198c0@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `kbuild: Fix '-S -c' in x86 stack protector scripts` ([`v1`](https://lore.kernel.org/20240726-fix-x86-stack-protector-tests-v1-1-a30fe80e8925@kernel.org/), [`stable backport`](https://lore.kernel.org/20240730184054.254156-1-nathan@kernel.org))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `ASoC: fsl: lpc3xxx-i2s: Avoid using ret uninitialized in lpc32xx_i2s_probe()` ([`v1`](https://lore.kernel.org/20240701-lpc32xx-asoc-fix-uninitialized-ret-v1-1-985d86189739@kernel.org/))
  * `power: supply: cros_charge-control: Avoid accessing attributes out of bounds` ([`v1`](https://lore.kernel.org/20240702-cros_charge-control-fix-clang-array-bounds-warning-v1-1-ae04d995cd1d@kernel.org/))
  * `ACPI: HMAT: Mark hmat_set_default_dram_perf() as __init` ([`v1`](https://lore.kernel.org/20240710-fix-modpost-warning-default_dram_nodes-v1-1-8961453cc82d@kernel.org/), [`v2`](https://lore.kernel.org/20240719-fix-modpost-warning-default_dram_nodes-v2-1-792ff57a50b0@kernel.org/))
  * `Bluetooth: btmtk: Mark all stub functions as inline` ([`v1`](https://lore.kernel.org/20240710-btmtk-add-missing-inline-to-stubs-v1-1-ba33143ee148@kernel.org/))
  * `drm/amd/display: Reapply 2fde4fdddc1f` ([`v1`](https://lore.kernel.org/20240724-amdgpu-dml2-fix-float-enum-warning-again-v1-1-740e7946f77a@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] cpumask: Switch from inline to __always_inline`](https://lore.kernel.org/20240703195724.GA292031@thelio-3990X/)
* [`Re: [PATCH v3 2/2] rust: add flags for shadow call stack sanitizer`](https://lore.kernel.org/20240704163908.GA1394865@thelio-3990X/)
* [`Re: [PATCH v3 1/2] rust: SHADOW_CALL_STACK is incompatible with Rust`](https://lore.kernel.org/20240704164548.GB1394865@thelio-3990X/)
* [`Re: [PATCH] kbuild: add script and target to generate pacman package`](https://lore.kernel.org/20240704182115.GA300903@thelio-3990X/)
* [`Re: [RFC PATCH] selftests: introduce and use SELFTESTS_CC_IS_CLANG instead of LLVM`](https://lore.kernel.org/20240704182332.GB300903@thelio-3990X/)
* [`Re: [PATCH 1/3] kbuild: raise the minimum GNU Make requirement to 4.0`](https://lore.kernel.org/20240705165221.GA987634@thelio-3990X/)
* [`Re: [PATCH 2/3] modpost: remove self-definitions of R_ARM_* macros`](https://lore.kernel.org/20240705165851.GB987634@thelio-3990X/)
* [`Re: [PATCH 3/3] modpost: rename R_ARM_THM_CALL to R_ARM_THM_PC22`](https://lore.kernel.org/20240705170034.GC987634@thelio-3990X/)
* [`Re: [PATCH] kbuild: deb-pkg: use default string when variable is unset or null`](https://lore.kernel.org/20240705170503.GD987634@thelio-3990X/)
* [`Re: [PATCH v2] kbuild: add script and target to generate pacman package`](https://lore.kernel.org/20240708055046.GB1968570@thelio-3990X/)
* [`Re: [PATCH] kbuild: rpm-pkg: avoid the warnings with dtb's listed twice`](https://lore.kernel.org/20240711211337.GA1816765@thelio-3990X/)
* [`Re: [PATCH v6] kbuild: add script and target to generate pacman package`](https://lore.kernel.org/20240717011515.GA1230090@thelio-3990X/)
* [`Re: [PATCH v7] kbuild: add script and target to generate pacman package`](https://lore.kernel.org/20240721033209.GA545406@thelio-3990X/)
* [`Re: [PATCH] drm/xe: Fix warning on unreachable statement`](https://lore.kernel.org/20240721192534.GA3459346@thelio-3990X/)
* [`Re: [PATCH]: kbuild doc typo fix`](https://lore.kernel.org/20240721231014.GA9588@thelio-3990X/)
* [`Re: [PATCH] fuse: fs-verity: aoid out-of-range comparison`](https://lore.kernel.org/20240801003544.GA468777@thelio-3990X/)
* [`Re: [PATCH] ARM: assembler: Drop obsolete VFP accessor fallback`](https://lore.kernel.org/20240801004609.GA1614495@thelio-3990X/)
* [`Re: [PATCH] Remove *.orig pattern from .gitignore`](https://lore.kernel.org/20240801011120.GA1620143@thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`ld.lld: error: drivers/gpu/drm/amd/amdgpu/amdgpu.o:(.debug_info+0x7d117f5): unknown relocation (33554442) against symbol`](https://github.com/ClangBuiltLinux/linux/issues/2034#issuecomment-2200506199)
* [`GCC images appear to be used for clang builds`](https://gitlab.com/Linaro/tuxsuite/-/issues/204)
* [`-Wframe-larger-than in drivers/gpu/drm/amd/amdgpu/vcn_v5_0_0.c`](https://github.com/ClangBuiltLinux/linux/issues/2036)
* [`Re: [PATCH v2 17/20] mm/zsmalloc: convert get/set_first_obj_offset() to take zpdesc`](https://lore.kernel.org/20240705171554.GE987634@thelio-3990X/)
* [`Re: [PATCH v2 01/10] riscv: Implement cmpxchg32/64() using Zacas`](https://lore.kernel.org/20240705172750.GF987634@thelio-3990X/)
* [`Re: Plumbers Testing MC potential topic: specialised toolchains`](https://lore.kernel.org/20240709053031.GB2120498@thelio-3990X/)
* [`Re: [PATCH]   infiniband/hw/ocrdma: increase frame warning limit in verifier when using KASAN or KCSAN`](https://lore.kernel.org/20240709212608.GA1649561@thelio-3990X/)
* [`Re: [PATCH] mm/slub: quiet the clang warning with -Wunused-function enabled`](https://lore.kernel.org/20240710150154.GA1684801@thelio-3990X/)
* [`Excessive stack usage with array bounds sanitizer when targeting PowerPC in Linux kernel's drivers/gpu/drm/amd/amdgpu/vcn_v5_0_0.c`](https://github.com/llvm/llvm-project/issues/98367)
* [`Re: [PATCH V6 2/5] mailbox: Add support for QTI CPUCP mailbox controller`](https://lore.kernel.org/20240715031451.GA2940276@thelio-3990X/)
* [`Re: [PATCH v12 4/8] platform: cznic: turris-omnia-mcu: Add support for poweroff and wakeup`](https://lore.kernel.org/20240715032402.GA2968547@thelio-3990X/)
* [`Re: [PATCH] misc: Kconfig: add a new dependency for MARVELL_CN10K_DPI`](https://lore.kernel.org/20240716132603.GA3136577@thelio-3990X/)
* [`Crash when booting UML after e3c92e81711d14b46c3121d36bc8e152cb843923`](https://lore.kernel.org/20240716143644.GA1827132@thelio-3990X/)
* [`Re: [linux-next:master 12947/13217] drivers/net/ethernet/meta/fbnic/fbnic_txrx.c:1088:6: error: call to '__compiletime_assert_749' declared with 'error' attribute: FIELD_PREP: value too large for the field`](https://lore.kernel.org/20240717013254.GA683176@thelio-3990X/)
* [`High stack usage in Linux kernel's drivers/gpu/drm/omapdrm/dss/dispc.c`](https://github.com/llvm/llvm-project/issues/99265)
* [`Re: [soc:sophgo/dt 1/1] arch/riscv/boot/dts/sophgo/sg2042.dtsi:7:10: fatal error: 'dt-bindings/clock/sophgo,sg2042-clkgen.h' file not found`](https://lore.kernel.org/20240717143016.GA94990@thelio-3990X/)
* [`Re: ld.lld: error: undefined symbol: iosf_mbi_available`](https://lore.kernel.org/20240717202806.GA728411@thelio-3990X/)
* [`Remove support for 3DNow!, both intrinsics and builtins.`](https://github.com/llvm/llvm-project/pull/96246#issuecomment-223348648)
* [`[LD/LTO] Error when building Linux 6.7 with Clang-18 and ThinLTO`](https://github.com/ClangBuiltLinux/linux/issues/1987#issuecomment-2233505699)
* [`It seems like --targets option is broken`](https://github.com/ClangBuiltLinux/tc-build/issues/273)
* [`Crash in net/core/dev.c when assertions are enabled after commit 13cabc47f8ae`](https://github.com/ClangBuiltLinux/linux/issues/2039)
* [`Assertion failure in Hexagon constant-extender optimization from Hexagon Linux kernel`](https://github.com/llvm/llvm-project/issues/99714)
* [`[LoongArch] Remove spurious mask operations from andn->icmp on 16 and 8 bit values`](https://github.com/llvm/llvm-project/pull/99272)
* [`[Clang] Loop over FieldDecls instead of all Decls`](https://github.com/llvm/llvm-project/pull/99574#issuecomment-2241814842)
* [`"value is too large for field of 1 byte" after commit 1a47f3f3db665`](https://github.com/llvm/llvm-project/issues/100283)
* [`Re: [tip:x86/microcode 1/1] arch/x86/kernel/cpu/microcode/amd.c:714:6: warning: variable 'equiv_id' is used uninitialized whenever 'if' condition is false`](https://lore.kernel.org/20240729153008.GA685493@thelio-3990X/)
* [`Re: [PATCH v3 2/3] dma-mapping: replace zone_dma_bits by zone_dma_limit`](https://lore.kernel.org/20240730021208.GA8272@thelio-3990X/)
* [`[llvm][CodeGen] Fixed max cycle calculation with zero-cost instructions for window scheduler`](https://github.com/llvm/llvm-project/pull/99454#issuecomment-2258935571)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update patches (July 1, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/761)
* [`Disable CONFIG_DRM_WERROR for certain Fedora configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/762)
* [`patches: stable: Drop merged __counted_by patches from 6.9.8`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/763)
* [`patches: mainline: Drop radeon __counted_by patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/764)
* [`boot-qemu.py: Handle LoongArch's EFI firmware location change`](https://github.com/ClangBuiltLinux/boot-utils/pull/121)
* [`Update DEFAULT_KERNEL_FOR_PGO to 6.10.0`](https://github.com/ClangBuiltLinux/tc-build/pull/274)
* [`Disable CONFIG_DRM_OMAP with LoongArch configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/767)
* [`Update builds for 6.10 final release`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/768)
* [`tc_build: Improve source set up logging`](https://github.com/ClangBuiltLinux/tc-build/pull/275)
* [`Update korg-clang-18 to 18.1.8 and add korg-clang-19`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/397)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [19.1.0-rc1](https://lore.kernel.org/20240726211227.GA1347002@thelio-3990X/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
