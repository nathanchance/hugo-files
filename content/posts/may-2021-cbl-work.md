---
title: "May 2021 ClangBuiltLinux Work"
date: 2021-06-01T08:29:00-07:00
toc: false
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

This month felt longer than others but I feel like that was because I was a lot more spread out in the work that I was doing. Let's dive in, shall we?

## Linux kernel patches / backports

* [`[PATCH] fbmem: Correct position of '__maybe_unused' in proc_fb_seq_ops`](https://lore.kernel.org/r/20210505182808.3855516-1-nathan@kernel.org/): A common mistake people make is splitting "struct <name>" with an attribute, in this case "__maybe_unused", which clang warns about while GCC does not. This causes a warning in all of our builds. Unfortunately, this patch did not actually make it into mainline because [Linus fixed it himself](https://git.kernel.org/linus/6dae40aed484ef2f1a3934dcdcd17b7055173e56).

* [`[PATCH] vmlinux.lds.h: Handle decrypted data section with !SMP`](https://lore.kernel.org/r/20210506001410.1026691-1-nathan@kernel.org/): Kees Cook enabled orphan section warnings in 5.10, which means the linker will warn when a section is not handled by a linker script. Intel's 0day bot turned up a randconfig that triggers a warning that will probably never be seen otherwise so I sent a patch to get that cleared up.

* [`Backport of 1139aeb1c521 for all supported stable branches`](https://lore.kernel.org/r/YJmneuxxFWIrqyWN@archlinux-ax161/): This was a series of backports to the stable kernels for a warning about intentional misalignment, which impacted ChromeOS and will impact Android when they upgrade their version of clang.

* [`UPSTREAM: crypto: arm/curve25519 - Move '.fpu' after '.arch'`](https://android-review.googlesource.com/c/kernel/common/+/1702245): Our continuous integration tests Android trees since they are one of the biggest downstream consumers of our work (go figure, Google started this...) and this upstream patch is needed to build `ARCH=arm allmodconfig`.

* [`[PATCH] Revert "ASoC: q6dsp: q6afe: remove unneeded dead-store initialization"`](https://lore.kernel.org/r/20210511190306.2418917-1-nathan@kernel.org/): Two patches that fixed the same Clang static analyzer warning were merged, which actually ended up introducing use of an uninitialized variable, which Intel's 0day build robot thankfully pointed out to us.

* [`Revert "UPSTREAM: usb: gadget: f_uac2: validate input parameters"`](https://android-review.googlesource.com/c/kernel/common/+/1706245): A mismerge in the Android trees broke our continuous integration so I sent this in to get it working again.

* [`[PATCH] x86: Fix location of '-plugin-opt=' flags`](https://lore.kernel.org/r/20210518190106.60935-1-nathan@kernel.org/): Someone reported a panic in the AMDGPU driver, which I helped track down to the fact that the stack alignment was not being passed along to the linker for LTO due to a Makefile variable clobber. This patch fixes it up and ensures we do not have something like this happen again. This will eventually be added to LLVM's IR so that this flag is not necessary.

* [`[PATCH 0/3] hexagon: Fix build error with CONFIG_STACKDEPOT and select CONFIG_ARCH_WANT_LD_ORPHAN_WARN`](https://lore.kernel.org/r/20210521011239.1332345-1-nathan@kernel.org/): The Hexagon architecture is special because it is the only architecture in the kernel that cannot be built with GCC; it can only be built with LLVM. Qualcomm got it working upstream in v5.13-rc1 then within a couple of weeks, it was broken again because of some unnoticed bitrot. This patch series fixes it up and helps make sure it should not happen again.

* [`[PATCH] powerpc/barrier: Avoid collision with clang's __lwsync macro`](https://lore.kernel.org/r/20210528182752.1852002-1-nathan@kernel.org/): An upstream LLVM change broke all of our PowerPC builds back to 4.14 due to a builtin macro defintion coliding with an already existing defintion in `arch/powerpc`. This patch fixes it up by undefining the macro because the kernel does not want the builtin version.

* [`[PATCH] MAINTAINERS: Add Clang CFI section`](https://lore.kernel.org/r/20210531205405.67993-1-nathan@kernel.org/): This patch solidifies the path for CFI patches to go upstream.



## Patch review and input

This month had quite a lot of review and patch testing, maybe more than the previous month. Code review is so critical for the success of these projects so it is important to do it quickly and well due to the velocity of both LLVM and Linux. There When possible, I link directly to my response but sometimes, the link is to the main post and my response can be seen inline.

* [`[AArch64] Adds a pre-indexed paired Load/Store optimization for LDR-STR.`](https://reviews.llvm.org/D99272#2739081)

* [`Re: CFI violation in drivers/infiniband/core/sysfs.c`](https://lore.kernel.org/r/YJL7UoSr42JfMCq1@archlinux-ax161/)

* [`Re: [PATCH v3] arm64: vdso: remove commas between macro name and arguments`](https://lore.kernel.org/r/YJMJAiwMPqlWmr8Y@archlinux-ax161/)

* [`Re: [PATCH 4.19 ONLY v4] arm64: vdso: remove commas between macro name and arguments`](https://lore.kernel.org/r/fd08dce2-71c0-3414-d661-d065480c04ff@kernel.org/)

* [`Re: [PATCH v2 dwarves] btf: Remove ftrace filter`](https://lore.kernel.org/r/YJSr7S7dRty3K8Wd@archlinux-ax161/)

* [`Re: [PATCH kernel v2] powerpc/makefile: Do not redefine $(CPP) for preprocessor`](https://lore.kernel.org/r/3579aa0d-0470-9a6b-e35b-48f997a5b48b@kernel.org/)

* [`Re: [PATCH kernel v3] powerpc/makefile: Do not redefine $(CPP) for preprocessor`](https://lore.kernel.org/r/dedc7262-2956-37b2-ebfd-ae8eb9b56716@kernel.org/)

* [`Re: [PATCH] HID: i2c-hid: fix format string mismatch`](https://lore.kernel.org/r/84b20dcb-b063-b2b0-b2dc-1a1befbc334c@kernel.org/)

* [`Re: [PATCH] iio: si1133: fix format string warnings`](https://lore.kernel.org/r/7afc367b-8103-9d48-1bfe-d505d86553b9@kernel.org/)

* [`Re: [PATCH] mm/shuffle: fix section mismatch warning`](https://lore.kernel.org/r/1904893e-1e7f-b1a4-454c-6999f8ac670a@kernel.org/)

* [`Re: [PATCH] kcsan: fix debugfs initcall return type`](https://lore.kernel.org/r/0ad11966-b286-395e-e9ca-e278de6ef872@kernel.org/)

* [`Re: [PATCH] riscv: Use -mno-relax when using lld linker`](https://lore.kernel.org/r/bd8ae62f-1ead-2d82-5fdb-87308accc023@kernel.org/)

* [`Re: [PATCH] [v2] platform/surface: aggregator: avoid clang -Wconstant-conversion warning`](https://lore.kernel.org/r/057b5568-c4b8-820c-3dd7-2200f61a4d58@kernel.org/)

* [`Re: [PATCH] drm/msm/dsi: fix 32-bit clang warning`](https://lore.kernel.org/r/58a35b85-eb0e-bc02-29be-0cae46bd75b8@kernel.org/)

* [`Re: [PATCH] arm64: Define only {pud/pmd}_{set/clear}_huge when usefull`](https://lore.kernel.org/r/YJ9HAg+o8nd2xmix@archlinux-ax161/)

* [`Re: [PATCH v2 3/4] memory: tegra124-emc: Fix compilation warnings on 64bit platforms`](https://lore.kernel.org/r/831d3af5-1e4a-f3c0-f69d-3fff145fde08@kernel.org/)

* [`[RESEND PATCH 0/4] iio: Drop use of %hhx and %hx format strings`](https://lore.kernel.org/r/20210517125554.1463156-1-jic23@kernel.org/)

* [`Re: [PATCH v2] powerpc/powernv/pci: fix header guard`](https://lore.kernel.org/r/4aa06438-7bea-a5a6-ff64-0cf8c28e2511@kernel.org/)

* [`Re: [PATCH 00/13] Reorganize sysfs file creation for struct ib_devices`](https://lore.kernel.org/r/34754eda-f135-8da8-c46f-3ef45a08ea11@kernel.org/)

* [`Re: [PATCH v3] kcov: add __no_sanitize_coverage to fix noinstr for all architectures`](https://lore.kernel.org/r/be3971b1-cf26-36c7-0f9c-d79c656ec855@kernel.org/)

* [`Re: [PATCH 1/6] pgo: modules Expose module sections for clang PGO instumentation.`](https://lore.kernel.org/r/YLUl/eQ1EzMFSb9F@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH 2/6] pgo: modules Add definitions in pgo/pgo.h for modules`](https://lore.kernel.org/r/YLUtUV3ZSIqPZbX+@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH 3/6] pgo: modules Add module profile data export machinery.`](https://lore.kernel.org/r/YLU0VAoWG7qa7u24@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH 5/6] pgo: modules Fixup memory leak.`](https://lore.kernel.org/r/YLUo6HrhWmeg2g1w@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH 6/6] pgo: Fixup code style issues.`](https://lore.kernel.org/r/YLUoiJr5+7WX+roL@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] pgo: rename the raw profile file to vmlinux.profraw`](https://lore.kernel.org/r/e22afde4-e312-4589-cf2e-3c35219d7249@kernel.org/)

* [`Re: [PATCH v9] pgo: add clang's Profile Guided Optimization infrastructure`](https://lore.kernel.org/r/YLVRTilQ5k5n+Vmz@archlinux-ax161/)



## Issue triage and reporting

As always, a little bit all over the place, LLVM and kernel issues, revisiting old problems, and triaging/reporting new ones :)

* [`FAIL: Test report for kernel 5.12.0 (mainline.kernel.org-clang, 10a3efd0)`](https://groups.google.com/g/clang-built-linux/c/r1j32US2Ag4/m/imaeSitHAAAJ)

* [`[InlineCost] Enable the cost benefit analysis on FDO`](https://reviews.llvm.org/D98213)

* [`arm64 defconfig + full LTO failure`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/128)

* [`panic in rcu_do_batch for full lto`](https://github.com/ClangBuiltLinux/linux/issues/1368)

* [`s390 all{mod,yes}config __ashlti3 and __lshrti3 after LLVM commit 1886aad9d03b`](https://github.com/ClangBuiltLinux/linux/issues/1370)

* [`llvm-NN-linker-tools Debian package`](https://github.com/ClangBuiltLinux/linux/issues/1357)

* [`Re: [BUG mips llvm] MIPS: malformed R_MIPS_{HI16,LO16} with LLVM`](https://lore.kernel.org/r/YJTwglbUOb67r733@archlinux-ax161/)

* [`Re: [PATCH v1 2/2] memory: tegra: Enable compile testing for all drivers`](https://lore.kernel.org/r/YJrARxhVD7QM%2FGPv@archlinux-ax161/)

* [`Re: [broonie-misc:asoc-5.14 5/23] sound/soc/qcom/qdsp6/q6afe.c:1213:18: warning: variable 'port_id' is uninitialized when used here`](https://lists.01.org/hyperkitty/list/kbuild-all@lists.01.org/message/B72WDKUO6KVMS4FTTJGGSERYZVWWMUMA/)

* [`Re: arch/powerpc/kernel/optprobes.c:34:1: error: unused function 'is_kprobe_ppc_optinsn_slot'`](https://lore.kernel.org/r/208186f3-aab0-d94e-bcf4-8347983cc1a6@kernel.org/)

* [`Support CROSS_COMPILE_COMPAT`](https://gitlab.com/Linaro/tuxsuite/-/issues/108)

* [`Re: -Wconstant-conversion in drivers/net/ethernet/marvell/mvpp2/mvpp2_main.c`](https://lore.kernel.org/r/YJx48BfKpWMZCbnz@archlinux-ax161/)

* [`missing mflr/mtlr (link register save/restore) when using inline asm w/ "lr" clobber`](https://bugs.llvm.org/show_bug.cgi?id=50147)

* [`Re: [next] x86_64 defconfig failed with clang-10`](https://lore.kernel.org/r/e6ee5c21-a460-b4f7-9d0a-e2711ec16185@kernel.org/)

* [`Re: [PATCH v3] mm, slub: change run-time assertion in kmalloc_index() to compile-time`](https://lore.kernel.org/r/YKC9CeAfw3aBmHTU@archlinux-ax161/)

* [`Re: [PATCH v5 3/9] mm/mremap: Use pmd/pud_poplulate to update page table entries`](https://lore.kernel.org/r/YKQdxpHVYB9H0M0j@Ryzen-9-3900X.localdomain/)

* [`Re: [staging:staging-next 201/268] drivers/staging/rtl8723bs/core/rtw_security.c:89:6: warning: stack frame size of 1120 bytes in function 'rtw_wep_encrypt'`](https://lore.kernel.org/r/YKgHy7ZNNxv%2FKMl8@archlinux-ax161/)

* [```Assertion `(IK == IK_FpInduction || Step->getType()->isIntegerTy()) && "StepValue is not an integer"' failed```](https://github.com/ClangBuiltLinux/linux/issues/1383)

* [`Stack alignment plugin option boot failure with CONFIG_CFI_CLANG`](https://github.com/ClangBuiltLinux/linux/issues/1384)

* [`51d334a845a082338735b0fdfc620a4b15fa26fe breaks x86_64 ThinLTO + dynamic ftrace`](https://github.com/ClangBuiltLinux/linux/issues/1385)

* [`Re: [PATCH v2] fb_defio: Remove custom address_space_operations`](https://lore.kernel.org/r/YLPjwUUmHDRjyPpR@Ryzen-9-3900X.localdomain/)

* [`[InstCombine] Fix miscompile on GEP+load to icmp fold (PR45210)`](https://reviews.llvm.org/D99481)



## Tooling improvements

Way more continuous integration work this time around, namely due to the additional testing with Android's LLVM and regressions or changes upstream.

* [`Disable boot for ppc32 on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/127)

* [`kernel/build.sh: Update name of source flag and variable`](https://github.com/ClangBuiltLinux/tc-build/pull/147)

* [`kernel: Update to Linux 5.12`](https://github.com/ClangBuiltLinux/tc-build/pull/149)

* [`Add problem matcher`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/130)

* [`Enable LLVM_IAS=1 for Android arm32 allmodconfig with LLVM 13`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/131)

* [`Add problem matcher`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/130)

* [`Fix problem matcher`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/134)

* [`Build arm64 with CFI on mainline and next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/132)

* [`Enable LLVM_IAS=1 for Android arm32 allmodconfig with LLVM 13`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/131)

* [`Wire up builds with AOSP LLVM`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/135)

* [`Update artifact upload name`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/136)

* [`utils.py: Fix crash with LLVM_VERSION=android`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/137)

* [`boot-utils: Add support for m68k`](https://github.com/ClangBuiltLinux/boot-utils/pull/45)

* [`Enable LLVM_IAS=1 for 32-bit ARM on mainline and next with LLVM 13`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/139)

* [`Fixup the Hexagon jobs and optimize around BOOT=0`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/140)

* [`build-llvm.py: Update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/152)

* [`build-llvm.py: Fix handling of '--projects'`](https://github.com/ClangBuiltLinux/tc-build/pull/153)

* [`kernel/build.sh: Build more ARCH=arm configs`](https://github.com/ClangBuiltLinux/tc-build/pull/154)

* [`generator.yml: Disable arm64 big endian build with AOSP LLVM`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/142)

* [`Notating "failed but unrelated to ClangBuiltLinux" builds`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/143)

* [`Enable boot for ppc32 on -next and mainline with LLVM 13`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/144)



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).