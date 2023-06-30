---
title: June 2023 ClangBuiltLinux Work
date: 2023-06-30T13:30:00-0700
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

  * `drm/amdgpu: Wrap -Wunused-but-set-variable in cc-option` ([`v1`](https://lore.kernel.org/20230608-amdgpu-wrap-wunused-but-set-variable-in-cc-option-v1-1-48ca005f2247@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * [`Re: [PATCH] kbuild: add $(CLANG_CFLAGS) to KBUILD_CPPFLAGS`](https://lore.kernel.org/20230602152519.GA3007575@dev-arch.thelio-3990X/)
  * `kbuild: Add KBUILD_CPPFLAGS to as-option invocation` ([`v1`](https://lore.kernel.org/20230606-fix-as-option-after-clang_flags-move-v1-1-a7f7b23a35e3@kernel.org/))
  * `percpu: Fix self-assignment of __old in raw_cpu_generic_try_cmpxchg()` ([`v1`](https://lore.kernel.org/20230607-fix-shadowing-in-raw_cpu_generic_try_cmpxchg-v1-1-8f0a3d930d43@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `riscv: vmlinux.lds.S: Explicitly handle '.got' section` ([`v1`](https://lore.kernel.org/20230605-6-3-riscv-got-orphan-warning-llvm-17-v1-1-72c4f11e020f@kernel.org/))
  * `Update as-{instr,option} to use KBUILD_AFLAGS` ([`v1`](https://lore.kernel.org/20230612-6-1-asssembler-target-llvm-17-v1-0-75605d553401@kernel.org/))
  * `riscv: Link with '-z norelro'` ([`v1`](https://lore.kernel.org/20230620-6-3-fix-got-relro-error-lld-v1-1-f3e71ec912d1@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `zswap: Avoid uninitialized use of ret in zswap_frontswap_store()` ([`v1`](https://lore.kernel.org/20230601-zswap-cgroup-wsometimes-uninitialized-v1-1-35debdd19293@kernel.org/), [`v2`](https://lore.kernel.org/20230601-zswap-cgroup-wsometimes-uninitialized-v2-1-84912684ac35@kernel.org/))
  * `MIPS: Mark core_vpe_count() as __init` ([`v1`](https://lore.kernel.org/20230615-mips-mark-core_vpe_count-as-init-v1-1-99c124367ea8@kernel.org/))
  * `drm/amdgpu: Fix instances of -Wunused-const-variable with CONFIG_DEBUG_FS=n` ([`v1`](https://lore.kernel.org/20230615-amdgpu-wunused-const-variable-wo-debugfs-v1-0-06efd224aebb@kernel.org/))
  * `coresight: dummy: Update type of mode parameter in dummy_{sink,source}_enable()` ([`v1`](https://lore.kernel.org/20230616-coresight-dummy-fix-kcfi-warnings-v1-1-c55c64f8f0f5@kernel.org/))
  * `clk: ralink: mtmips: Fix uninitialized use of ret in mtmips_register_{fixed,factor}_clocks()` ([`v1`](https://lore.kernel.org/20230622-mips-ralink-clk-wuninitialized-v1-1-ea9041240d10@kernel.org/))
  * `leds: leds-mt6323: Adjust return/parameter types in wled get/set callbacks` ([`v1`](https://lore.kernel.org/20230622-mt6323-wled-wincompatible-function-pointer-types-strict-v1-1-6ad256f220e8@kernel.org/))



## LLVM patches

* [`[Sema] Do not emit -Wunused-variable for variables declared with cleanup attribute`](https://github.com/llvm/llvm-project/commit/877210faa447f4cc7db87812f8ed80e398fedd61)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] btrfs: remove unused variable pages_processed`](https://lore.kernel.org/20230601182957.GA3028824@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: add $(CLANG_CFLAGS) to KBUILD_CPPFLAGS`](https://lore.kernel.org/20230602152519.GA3007575@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/2] bus: fsl-mc: fsl-mc-allocator: Initialize mc_bus_dev before use`](https://lore.kernel.org/20230605153915.GB2480995@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] bus: fsl-mc: fsl-mc-allocator: Drop a write-only variable`](https://lore.kernel.org/20230605154126.GC2480995@dev-arch.thelio-3990X/)
* [`Re: [PATCH -next v21 23/27] riscv: detect assembler support for .option arch`](https://lore.kernel.org/20230605154832.GA3049210@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/amd/amdgpu: enable W=1 for amdgpu`](https://lore.kernel.org/20230609201754.GA3961359@dev-arch.thelio-3990X/)
* [`scripts/rust_is_available.sh improvements`](https://lore.kernel.org/20230616001631.463536-1-ojeda@kernel.org/)
* [`Re: [PATCH] s390/decompresser: fix misaligned symbol build error`](https://lore.kernel.org/20230622143538.GA1138962@dev-arch.thelio-3990X/)
* [`Re: [PATCH] MIPS: Loongson: Fix build error when make modules_install`](https://lore.kernel.org/20230626160720.GA2174263@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/2] LoongArch: vDSO: Use CLANG_FLAGS instead of filtering out '--target='`](https://lore.kernel.org/20230627162456.GA223742@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] LoongArch: Include KBUILD_CPPFLAGS in CHECKFLAGS invocation`](https://lore.kernel.org/20230627162512.GB223742@dev-arch.thelio-3990X/)
* [`Re: [PATCH net v1] ptp: Make max_phase_adjustment sysfs device attribute invisible when not supported`](https://lore.kernel.org/20230627233344.GA1914803@dev-arch.thelio-3990X/)
* [`Re: [PATCH V2] MIPS: Loongson: Fix build error when make modules_install`](https://lore.kernel.org/20230628152208.GA250029@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/2] media: verisilicon: fix excessive stack usage`](https://lore.kernel.org/20230628182617.GA911656@dev-arch.thelio-3990X/)
* [`Re: [PATCH] media: wl128x: fix a clang warning`](https://lore.kernel.org/20230630155427.GA2889176@dev-arch.thelio-3990X/)
* [`Re: [PATCH net-next v3 10/18] nvme/host: Use sendmsg(MSG_SPLICE_PAGES) rather then sendpage`](https://lore.kernel.org/20230630161043.GA2902645@dev-arch.thelio-3990X/)
* [`Re: [PATCH 0/6] riscv: KCFI support`](https://lore.kernel.org/20230630201354.GA3346845@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH 4/6] bus: fsl-mc: fsl-mc-allocator: Improve error reporting`](https://lore.kernel.org/20230601154101.GA2368233@dev-arch.thelio-3990X/)
* [```lib/test_fortify/ tests causing lots of `unsafe X() usage lacked 'Y' warnings```](https://github.com/ClangBuiltLinux/linux/issues/1863)
* [`RISC-V '.got' linker orphan warning after LLVM commit a178ba9fbd0a`](https://github.com/ClangBuiltLinux/linux/issues/1865)
* [`6.4-rc4 kernel build fails with DEBUG_INFO_COMPRESSED_ZSTD=y (powerpc64, BE, clang 16.0.4)`](https://github.com/ClangBuiltLinux/linux/issues/1866)
* [`android14-6.1 arm64 gki_defconfig boot failures`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/578)
* [```Re: powerpc: clang: arch/powerpc/kernel/exceptions-64s.S:2976: Error: junk at end of line: `0b01010'```](https://lore.kernel.org/20230606153150.GA128872@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/4] KVM: arm64: vgic: Fix a circular locking issue`](https://lore.kernel.org/20230606221525.GA2269598@dev-arch.thelio-3990X/)
* [`arm and arm64 "kernel BUG at mm/vmalloc.c:1638!" after commit c5c0ba953b8c969 in -next`](https://github.com/ClangBuiltLinux/linux/issues/1868)
* [`Re: [PATCH v6 02/19] hexagon: mm: Convert to GENERIC_IOREMAP`](https://lore.kernel.org/20230612160237.GA199007@dev-arch.thelio-3990X/)
* [`Re: clang: Powerpc: clang-nightly-maple_defconfig — FAIL`](https://lore.kernel.org/20230612185424.GA2891387@dev-arch.thelio-3990X/)
* [`Failed assertion  when building RISCV Linux kernel after d0189584631e5`](https://github.com/llvm/llvm-project/issues/63293)
* [`clang-15 crash in UBSAN handler`](https://github.com/ClangBuiltLinux/linux/issues/1873)
* [`Re: [PATCH v2 07/23] mips: update_mmu_cache() can replace __update_tlb()`](https://lore.kernel.org/20230614231758.GA1503611@dev-arch.thelio-3990X/)
* [`Re: [song-md:md-next 25/29] drivers/md/raid1-10.c:117:25: error: casting from randomized structure pointer type 'struct block_device *' to 'struct md_rdev *'`](https://lore.kernel.org/20230615154123.GA3665766@dev-arch.thelio-3990X/)
* [`-Wframe-larger-than in drivers/media/platform/verisilicon/rockchip_vpu981_hw_av1_dec.c`](https://github.com/ClangBuiltLinux/linux/issues/1875)
* [`LLVM commit 4bdc7f7a331f82 breaks building alternatives for ARCH=arm64`](https://github.com/ClangBuiltLinux/linux/issues/1876)
* [`Re: [PATCH v7 02/19] hexagon: mm: Convert to GENERIC_IOREMAP`](https://lore.kernel.org/20230621190834.GA842758@dev-arch.thelio-3990X/)
* [`Re: [PATCH v7 10/19] s390: mm: Convert to GENERIC_IOREMAP`](https://lore.kernel.org/20230621192133.GB842758@dev-arch.thelio-3990X/)
* [`LLVM commit a79995ca6004 breaks as-instr and as-option for ARCH=arm on older kernels`](https://github.com/ClangBuiltLinux/linux/issues/1878)
* [`s390 misaligned symbol linker errors`](https://github.com/ClangBuiltLinux/linux/issues/1747)
* [`Re: [PATCH v3 8/8] leds: leds-mt6323: Add support for WLEDs and MT6332`](https://lore.kernel.org/20230621213124.GA2689001@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 0/4] riscv: enable HAVE_LD_DEAD_CODE_DATA_ELIMINATION`](https://lore.kernel.org/20230622215327.GA1135447@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 05/11] NFS: add superblock sysfs entries`](https://lore.kernel.org/20230626211223.GA3771155@dev-arch.thelio-3990X/)
* [`test building LoongArch`](https://github.com/ClangBuiltLinux/linux/issues/1787)
* [`Re: [PATCH v3 5/9] ptp: Add .getmaxphase callback to ptp_clock_info`](https://lore.kernel.org/20230627162146.GA114473@dev-arch.thelio-3990X/)
* [`Re: [EXT] Re: [linux-next:master 5324/12162] drivers/cpufreq/cpufreq-dt-platdev.c:18:34: warning: unused variable 'allowlist'`](https://lore.kernel.org/20230628153923.GA254367@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`workflows: docker: Switch to GITHUB_OUTPUT`](https://github.com/ClangBuiltLinux/containers/pull/49)
* [`Update for moving rootfs images to GitHub releases in boot-utils`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/577)
* [`Update Arch Linux x86_64 configuration link`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/580)
* [`Drop android-mainline ext4 patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/583)
* [`check_logs.py: Remove unnecessary conversion to fix updated RUF010`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/584)
* [`boot-qemu.py: Handle dtb location shuffle in linux-next`](https://github.com/ClangBuiltLinux/boot-utils/pull/107)
* [`Update RISC-V gki_defconfig build`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/586)
* [`patches: mainline: Add patch for RANDSTRUCT raid1-10.c error`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/587)
* [`Initial LoongArch support`](https://github.com/ClangBuiltLinux/boot-utils/pull/109)
* [`patches: mainline: Add patch for drivers/scsi/aacraid`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/588)
* [`Update -next patches (June 28, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/589)
* [`build-local.py improvements`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/590)
* [`Apply series to fix arm64 big endian on stable`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/591)
* [`Update stable to linux-6.4.y`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/592)
* [`Move powerpc64 to LLVM=1 on mainline/-next with LLVM 14.x+`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/593)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I built and uploaded LLVM [16.0.5](https://github.com/llvm/llvm-project/releases/llvmorg-16.0.5) [to kernel.org](https://lore.kernel.org/20230602180459.GA574022@dev-arch.thelio-3990X/).



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
