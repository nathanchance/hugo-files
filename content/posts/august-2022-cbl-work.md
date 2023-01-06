---
title: "August 2022 ClangBuiltLinux Work"
date: 2022-08-31T16:15:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `scripts/Makefile.extrawarn: Do not disable clang's -Wformat-zero-length` ([`v1`](https://lore.kernel.org/20220810230133.1895778-1-nathan@kernel.org/))
  * `x86/build: Move '-mindirect-branch-cs-prefix' out of GCC-only block` ([`v1`](https://lore.kernel.org/20220817185410.1174782-1-nathan@kernel.org/))

* Stable backport requests: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply f2928e224d85e7cc139009ab17cefdfec2df5d11 to 5.15 and 5.10?`](https://lore.kernel.org/YvWduqRcPYhYZWMT@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `MIPS: tlbex: Explicitly compare _PAGE_NO_EXEC against 0` ([`v1`](https://lore.kernel.org/20220802175936.2278362-1-nathan@kernel.org/))
  * `usb: cdns3: Don't use priv_dev uninitialized in cdns3_gadget_ep_enable()` ([`v1`](https://lore.kernel.org/20220803162422.2981308-1-nathan@kernel.org/))
  * `ASoC: mchp-spdiftx: Fix clang -Wbitfield-constant-conversion` ([`v1`](https://lore.kernel.org/20220810010809.2024482-1-nathan@kernel.org/))
  * `ASoC: codes: src4xxx: Avoid clang -Wsometimes-uninitialized in src4xxx_hw_params()` ([`v1`](https://lore.kernel.org/20220822183101.1115095-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20220823151939.2493697-1-nathan@kernel.org/))
  * `mm: pagewalk: Restore err initialization in walk_hugetlb_range()` ([`v1`](https://lore.kernel.org/20220823153055.2517764-1-nathan@kernel.org/))
  * `net/mlx5e: Do not use err uninitialized in mlx5e_rep_add_meta_tunnel_rule()` ([`v1`](https://lore.kernel.org/20220825180607.2707947-1-nathan@kernel.org/))
  * `drm/msm/dsi: Remove use of device_node in dsi_host_parse_dt()` ([`v1`](https://lore.kernel.org/20220829165450.217628-1-nathan@kernel.org/))
  * `powerpc/papr_scm: Ensure rc is always initialized in papr_scm_pmu_register()` ([`v1`](https://lore.kernel.org/20220830151256.1473169-1-nathan@kernel.org/))
  * `drm/amd/display: Reduce stack usage for clang` ([`v1`](https://lore.kernel.org/20220830203409.3491379-1-nathan@kernel.org/))
  * `powerpc/math_emu/efp: Include module.h` ([`v1`](https://lore.kernel.org/20220831152014.3501664-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] Makefile.extrawarn: re-enable -Wformat for clang`](https://lore.kernel.org/YugYbvRu1xqnx6mC@dev-arch.thelio-3990X/)
* [`Re: [PATCH] soc: sof: fix clang -Wformat warnings`](https://lore.kernel.org/YumINAZ4WaM4rG7Q@dev-arch.thelio-3990X/)
* [`Re: [PATCH resend] ASoC: SOF: ipc3-topology: Fix clang -Wformat warning`](https://lore.kernel.org/Yure82N7%2F4NLEMsW@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86: assemble with -Wa,--noexecstack to avoid BFD 2.39 warning`](https://lore.kernel.org/YvFkmfHTUYGzeeQs@dev-arch.thelio-3990X/)
* [`Re: [PATCH] selftests: add missing ')' in lib.mk`](https://lore.kernel.org/YvKM%2FJ5xO8gKto+p@dev-arch.thelio-3990X/)
* [`Re: [PATCH] selftests: fix LLVM build for i386 and x86_64`](https://lore.kernel.org/YvKOV1L73Mv%2FDc6P@dev-arch.thelio-3990X/)
* [`Migrate from tuxsuite build-sets to plans`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/395)
* [`Re: [PATCH v2] powerpc/kexec: Fix build failure from uninitialised variable`](https://lore.kernel.org/YvPgNsl1RalFdPH+@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] RISC-V: Clean up the Zicbom block size probing`](https://lore.kernel.org/Yvpo96wal40ROTsX@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ntb_perf: Fix 64-bit division on 32-bit architectures`](https://lore.kernel.org/YvpqfDNIBqLbC0FA@dev-arch.thelio-3990X/)
* [`[TypePromotion] Don't promote PHI + ZExt if wider than RegisterBitWidth`](https://reviews.llvm.org/D131966)
* [`Re: [PATCH] platform/x86/amd/pmf: Fix clang unused variable warning`](https://lore.kernel.org/YwAEvbHW%2FUwwIYbt@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] asm goto: eradicate CC_HAS_ASM_GOTO`](https://lore.kernel.org/YwAFHPRVVa9X3Gue@dev-arch.thelio-3990X/)
* [`Re: [Intel-gfx] [PATCH v1] drm/i915: fix null pointer dereference`](https://lore.kernel.org/YwO5fW%2F5N16L1gz0@dev-arch.thelio-3990X/)
* [`Re: [PATCH 7/6] mm: pagewalk: add back missing variable initializations`](https://lore.kernel.org/YwY+1xD52ep54M3y@dev-arch.thelio-3990X/)
* [`RISCV: permit unaligned nop-slide padding emission`](https://reviews.llvm.org/D132482)
* [`Re: [PATCH] wifi: rtw88: fix uninitialized use of primary channel index`](https://lore.kernel.org/YwZ+RsHL+n02gHZx@dev-arch.thelio-3990X/)
* [`[Local] Allow creating callbr with duplicate successors`](https://reviews.llvm.org/D129997)
* [`Re: [PATCH 1/3] Makefile.compiler: s/KBUILD_CFLAGS/KBUILD_AFLAGS/ for as-option`](https://lore.kernel.org/YwkPNyHvxR2dM+CQ@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] powerpc: remove old code for binutils < 2.25`](https://lore.kernel.org/Yw6EoSojRZbj+STd@dev-arch.thelio-3990X/)
* [`[PATCH v2 0/5] fix debug info for asm and DEBUG_INFO_SPLIT`](https://lore.kernel.org/20220831184408.2778264-1-ndesaulniers@google.com/)



## Issue triage, reporting, and input

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: arm ixp4xx_defconfig build failure with linux-next and mainline`](https://lore.kernel.org/YuhbtB9+1rTYtT23@dev-arch.thelio-3990X/)
* [`implement -mindirect-branch-cs-prefix`](https://github.com/ClangBuiltLinux/linux/issues/1665)
* [`Add support for clang-15`](https://gitlab.com/Linaro/tuxmake/-/issues/192)
* [`Re: [GIT PULL] USB / Thunderbolt driver changes for 6.0-rc1`](https://lore.kernel.org/YuqXtcaUPflINBd6@dev-arch.thelio-3990X/)
* [`-Wframe-larger-than in drivers/gpu/drm/amd/display/dc/dml/dcn3{0,1,2}/display_mode_vba_3{0,1,2}.c`](https://github.com/ClangBuiltLinux/linux/issues/1681)
* [`Re: mainline build failure for x86_64 allmodconfig with clang`](https://lore.kernel.org/YuwvfsztWaHvquwC@dev-arch.thelio-3990X/)
* [`Re: [PATCH v5 09/14] iio: magnetometer: yas530: Introduce "chip_info" structure`](https://lore.kernel.org/YvEy9uq49ZiBHtFd@dev-arch.thelio-3990X/)
* [`Re: [PATCH kernel v2] pseries/iommu/ddw: Fix kdump to work in absence of ibm,dma-window`](https://lore.kernel.org/YvE+d7xcB77GODjc@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 00/12] PolarFire SoC reset controller & clock cleanups`](https://lore.kernel.org/YvP%2Fbjh+wXihlrdG@dev-arch.thelio-3990X/)
* [`Re: mainline build failure for arm64 allmodconfig with clang`](https://lore.kernel.org/YvUZ+9kJ%2FAvUMxzO@dev-arch.thelio-3990X/)
* [`Re: build failure of next-20220811 due to d79b32c2e4a4 ("vdpa_sim_blk: add support for discard and write-zeroes")`](https://lore.kernel.org/YvVK+ZqO75QAYYnB@dev-arch.thelio-3990X/)
* [`Fortify source failure in drivers/infiniband/core/cma.c with s390 allmodconfig`](https://github.com/ClangBuiltLinux/linux/issues/1687)
* [`__aeabi_uldivmod in drivers/mtd/mtdswap.ko after LLVM commit 164067918725d1ee8875e38213b6aee9be13383e`](https://github.com/ClangBuiltLinux/linux/issues/1688)
* [`Re: [stable:linux-5.15.y 5373/9027] arch/x86/kvm/hyperv.c:2185:5: warning: stack frame size (1036) exceeds limit (1024) in 'kvm_hv_hypercall'`](https://lore.kernel.org/Yvp87jlVWg0e376v@dev-arch.thelio-3990X/)
* [`Re: fs/ntfs/aops.c:378:12: warning: stack frame size (2216) exceeds limit (1024) in 'ntfs_read_folio'`](https://lore.kernel.org/Yvp+OnhAAQI5Zvj9@dev-arch.thelio-3990X/)
* [`Re: arch/riscv/include/asm/jump_label.h:42:3: error: invalid operand for inline asm constraint 'i'`](https://lore.kernel.org/YvqkjlUZ9DA%2FkA4H@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Regression caused by llvm: [TypePromotion] Search from ZExt + PHI`](https://lore.kernel.org/Yvro06Uid0t0Ou1M@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] ASoC: codecs: add uspport for the TI SRC4392 codec`](https://lore.kernel.org/YvvbKry5FVFbNdcI@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Regression caused by llvm: Fix -Wbitfield-constant-conversion on 1-bit signed bitfield`](https://lore.kernel.org/Yvvvn9RxoL9Dcb3r@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 net 13/17] net: Fix data-races around sysctl_fb_tunnels_only_for_init_net.`](https://lore.kernel.org/Yv5nd3cVX2ZKysC%2F@dev-arch.thelio-3990X/)
* [```[cmake] Use `CMAKE_INSTALL_LIBDIR` too```](https://reviews.llvm.org/D130586)
* [`Re: [PATCH v4 03/11] platform/x86/amd/pmf: Add support SPS PMF feature`](https://lore.kernel.org/Yv7BmjjLtA3RaKju@thelio-3990X/)
* [`unable to write nop sequence of ... bytes after LLVM commit 73a9dfcee24df959b59a46d75dcbdc0bcfb50fe6`](https://github.com/ClangBuiltLinux/linux/issues/1693)
* [`Re: ANNOUNCE: pahole v1.24 (Faster BTF encoding, 64-bit BTF enum entries)`](https://lore.kernel.org/YwZQ0UkLsoa+6VyY@dev-arch.thelio-3990X/)
* [`[DebugInfo] Extend the InstrRef LDV to support DbgValues with many Ops`](https://reviews.llvm.org/D128212)
* [`Re: [PATCH 5.19 000/362] 5.19.4-rc2 review`](https://lore.kernel.org/YwaHy9An68xJkxdu@dev-arch.thelio-3990X/)
* [`Re: [next] x86: fs/hugetlbfs/inode.c:673:16: error: variable 'm_index' is uninitialized when used here [-Werror,-Wuninitialized]`](https://lore.kernel.org/Ywee393cssPJ07Gr@dev-arch.thelio-3990X/)
* [`Re: ld.lld: error: undefined symbol: drm_gem_fb_get_obj`](https://lore.kernel.org/Ywjty2iSyer5FDS7@dev-arch.thelio-3990X/)
* [`Re: [PATCH -next 2/3] mm/zswap: delay the initializaton of zswap until the first enablement`](https://lore.kernel.org/YwksRZPIfXmlOmHR@dev-arch.thelio-3990X/)
* [`Re: build failure of next-20220830 due to 5f8cdece42ff ("drm/msm/dsi: switch to DRM_PANEL_BRIDGE")`](https://lore.kernel.org/Yw4FQm6V7d3MuMKG@dev-arch.thelio-3990X/)
* [`Re: powerpc-linux-objdump: Warning: Unrecognized form: 0x23`](https://lore.kernel.org/Yw+A+0BY26l0AC5j@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Disable CONFIG_FPE_NWFPE for ARCH=arm all{mod,yes}config for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/388)
* [`boot-utils: Rewrite in Python`](https://github.com/ClangBuiltLinux/boot-utils/pull/67)
* [`Bump llvm_tot anchor and LLVM_TOT_VERSION`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/389)
* [`README: Fix badges after #389`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/390)
* [`Update boot-utils for Python rewrite`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/391)
* [`boot-uml.py: Fix stray 'i' in decomp_rootfs()`](https://github.com/ClangBuiltLinux/boot-utils/pull/68)
* [`boot-utils: Update to fix 'boot-uml.py'`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/392)
* [`boot-utils.py: Fix searching for Linux kernel version numbers`](https://github.com/ClangBuiltLinux/boot-utils/pull/69)
* [`boot-utils: Update for another 'boot-qemu.py' fix`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/393)
* [`Fix mainline and next builds (August 8, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/394)
* [`patches: next: Drop rtc patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/396)
* [`utils.py: Adjust for new builds.json output with 'tuxsuite plan'`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/397)
* [`docker: clang-android: Update to r458507 (15.0.1)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/278)
* [`Fix -tip (August 10, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/398)
* [`patches: mainline: Add a couple in-flight -Wformat fixes`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/399)
* [`Update stable anchor to 5.19`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/400)
* [`Clean up generator.yml (August 10th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/401)
* [`patches: mainline: Drop -Wformat patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/407)
* [`Remove CONFIG_PPC_DISABLE_WERROR from powernv_defconfig`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/408)
* [`utils.py: Update binutils to 2.39`](https://github.com/ClangBuiltLinux/tc-build/pull/202)
* [`patches: mainline/tip: Drop i915 patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/409)
* [`patches: Add pending -Wbitfield-constant-conversion mchp-spdiftx.c patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/410)
* [`patches: Add mchp-spdiftx.c to -tip trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/411)
* [`Stop disabling CONFIG_WERROR for arm64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/412)
* [`Improve notes for August 10th, 2022`](https://github.com/ClangBuiltLinux/meeting-notes/pull/28)
* [`Add support for LLVM 15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/413)
* [`patches: arm64: Add a couple more patches to fix warnings`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/414)
* [`Revert "patches: arm64: Add a couple more patches to fix warnings"`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/415)
* [`patches: arm64: Update mchp-spdiftx.c patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/416)
* [`Disable CONFIG_WERROR for Hexagon`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/417)
* [`Add support for android14-5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/418)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, an Intel-based laptop, and a SolidRun Honeycomb LX2. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
