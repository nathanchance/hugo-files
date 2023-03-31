---
title: March 2023 ClangBuiltLinux Work
date: 2023-03-31T16:30:00-0700
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

  * `clk: Avoid invalid function names in CLK_OF_DECLARE()` ([`v1`](https://lore.kernel.org/20230308-clk_of_declare-fix-v1-1-317b741e2532@kernel.org/))
  * `riscv: Handle zicsr/zifencei issues between clang and binutils` ([`v1`](https://lore.kernel.org/20230313-riscv-zicsr-zifencei-fiasco-v1-1-dd1b7840a551@kernel.org/))
  * `wifi: iwlwifi: Avoid disabling GCC specific flag with clang` ([`v1`](https://lore.kernel.org/20230315-iwlwifi-fix-pragma-v1-1-ad23f92c4739@kernel.org/))
  * `wifi: iwlwifi: mvm: Use 64-bit division helper in iwl_mvm_get_crosstimestamp_fw()` ([`v1`](https://lore.kernel.org/20230329-iwlwifi-ptp-avoid-64-bit-div-v1-1-ad8db8d66bc2@kernel.org/), [`v2`](https://lore.kernel.org/20230329-iwlwifi-ptp-avoid-64-bit-div-v2-1-22b988eb009b@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `drm/i915: Fix CFI violations in gt_sysfs` ([`v1`](https://lore.kernel.org/20230116033551.191731-1-nathan@kernel.org/))
  * `Backport of e89c2e815e76 to linux-5.10.y` ([`v1`](https://lore.kernel.org/20230328-riscv-zifencei-zicsr-5-10-v1-0-bccb3e16dc46@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `bpf: Increase size of BTF_ID_LIST without CONFIG_DEBUG_INFO_BTF again` ([`v1`](https://lore.kernel.org/20230307-bpf-kfuncs-warray-bounds-v1-1-00ad3191f3a6@kernel.org/))
  * `net: pasemi: Fix return type of pasemi_mac_start_tx()` ([`v1`](https://lore.kernel.org/20230319-pasemi-incompatible-pointer-types-strict-v1-1-1b9459d8aef0@kernel.org/))
  * `net: ethernet: ti: Fix format specifier in netcp_create_interface()` ([`v1`](https://lore.kernel.org/20230329-net-ethernet-ti-wformat-v1-1-83d0f799b553@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 1/2] powerpc/64: Move CPU -mtune options into Kconfig`](https://lore.kernel.org/20230302163055.GA3010526@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] powerpc/64: Use -mtune=pwr10/9/8 for clang`](https://lore.kernel.org/20230302164324.GB3010526@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/vmwgfx: Fix src/dst_pitch confusion`](https://lore.kernel.org/20230314211615.GA3351584@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kfence, kcsan: avoid passing -g for tests`](https://lore.kernel.org/20230316160523.GA90073@dev-arch.thelio-3990X/)
* [`Re: [PATCH -next v15 19/19] riscv: Enable Vector code to be built`](https://lore.kernel.org/20230317154658.GA1122384@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ALSA: ymfpci: remove unused snd_ymfpci_readb function`](https://lore.kernel.org/20230319233444.GA12415@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/rockchip: vop2: fix uninitialized variable possible_crtcs`](https://lore.kernel.org/20230320170057.GA592480@dev-arch.thelio-3990X/)
* [`Re: [PATCH -next v16 19/20] riscv: detect assembler support for .option arch`](https://lore.kernel.org/20230324145934.GB428955@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3] purgatory: fix disabling debug info`](https://lore.kernel.org/20230330222928.GA644044@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`crash with RISC-V scalable vectorization and kernel address sanitizer`](https://github.com/llvm/llvm-project/issues/61096)
* [`Re: [PATCH -next v14 19/19] riscv: Enable Vector code to be built`](https://lore.kernel.org/Y%2F+SoGfspjGwlH7g@dev-arch.thelio-3990X/)
* [`Re: [PATCH] power: supply: qcom_battmgr: remove bogus do_div()`](https://lore.kernel.org/Y%2F+WghSbz3l6uipn@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] clk: Mark a fwnode as initialized when using CLK_OF_DECLARE() macro`](https://lore.kernel.org/20230308160140.GA2347814@dev-arch.thelio-3990X/)
* [`Re: [PATCH] staging: axis-fifo: initialize timeouts in probe only`](https://lore.kernel.org/20230314144207.GA4106922@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 1/6] drm/rockchip: vop2: initialize possible_crtcs properly`](https://lore.kernel.org/20230314160821.GA13416@dev-arch.thelio-3990X/)
* [`Re: [PATCH net-next v6 1/1] net: dsa: hellcreek: Get rid of custom led_init_default_state_get()`](https://lore.kernel.org/20230315195154.GA1636193@dev-arch.thelio-3990X/)
* [`ppc44x_defconfig does not boot with '-mcpu=440'`](https://github.com/ClangBuiltLinux/linux/issues/1814)
* [`stack protector boot failure in android-4.14-stable`](https://github.com/ClangBuiltLinux/linux/issues/1815)
* [`"Error: non-constant .uleb128 is not supported" in mm/kfence/kfence_test.c`](https://github.com/ClangBuiltLinux/linux/issues/1816)
* [`.init.data has both ordered and unordered sections on RISC-V`](https://github.com/ClangBuiltLinux/linux/issues/1817)
* [`undefined __efistub_ symbols with CONFIG_EFI_ZBOOT=y`](https://github.com/ClangBuiltLinux/linux/issues/1819)
* [`Re: [PATCH 1/3] kvm: vmx: Add IA32_FLUSH_CMD guest support`](https://lore.kernel.org/20230317190432.GA863767@dev-arch.thelio-3990X/)
* [`Re: next-20230321: arm64: Unable to handle kernel paging request at virtual address`](https://lore.kernel.org/20230321155626.GA3765079@dev-arch.thelio-3990X/)
* [`Re: Linux 6.3-rc3`](https://lore.kernel.org/20230320180501.GA598084@dev-arch.thelio-3990X/)
* [`Re: [PATCH v8 2/2] drm: add kms driver for loongson display controller`](https://lore.kernel.org/20230328170636.GA1986005@dev-arch.thelio-3990X/)
* [```fs/select.o: llvm::ValueHandleBase::RemoveFromUseList(): Assertion `*PrevPtr == this && "List invariant broken"'```](https://github.com/ClangBuiltLinux/linux/issues/1821)
* [`kern_levels.h:32:2: error: invalid preprocessing directive`](https://github.com/ClangBuiltLinux/linux/issues/1822)
* [`insn.h:399:1 <Spelling=./arch/arm64/include/asm/insn.h:299:66>: current parser token`](https://github.com/ClangBuiltLinux/linux/issues/1823)
* [`bpf_iter.c 1. ./include/linux/kernel.h:216:8: current parser token '__printf'`](https://github.com/ClangBuiltLinux/linux/issues/1824)
* [`BTF generation does not work with LLVM/lld-16`](https://github.com/ClangBuiltLinux/linux/issues/1825)
* [`[IVDescriptors] Add pointer InductionDescriptors with non-constant strides`](https://reviews.llvm.org/rG498aa534f472d28db893aa9a8627d0b46e17f312)
* [`Re: [PATCH 03/18] wifi: iwlwifi: mvm: report hardware timestamps in RX/TX status`](https://lore.kernel.org/20230331175121.GA3127046@dev-arch.thelio-3990X/)
* [`Re: [PATCH v10 11/15] drm/atomic-helper: Set fence deadline for vblank`](https://lore.kernel.org/20230331204412.GA396777@dev-arch.thelio-3990X/)
* [`Re: [PATCH] net: netcp: MAX_SKB_FRAGS is now 'int'`](https://lore.kernel.org/20230331214444.GA1426512@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`patches: android14-6.1: Drop alternatives series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/527)
* [`patches: mainline: Drop memintristics series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/528)
* [`Add 6.1 builds and update stable anchor to 6.2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/529)
* [`Update patches (March 8, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/530)
* [`Copy stable patches to 6.1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/531)
* [`Drop stable patches (March 13, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/532)
* [`boot-qemu.py: Rewrite using classes`](https://github.com/ClangBuiltLinux/boot-utils/pull/91)
* [`Add support for mounting a shared folder between the host and guest via virtiofs (v2)`](https://github.com/ClangBuiltLinux/boot-utils/pull/93)
* [`docker: clang-android: Update to r487747 (17.0.0)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/297)
* [`boot-qemu.py: Do not add '-no-reboot' unconditionally`](https://github.com/ClangBuiltLinux/boot-utils/pull/95)
* [`Drop mainline patches (March 20, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/534)
* [`Drop android14-6.1 patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/535)
* [`Add powerpc allmodconfig build on linux-next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/536)
* [`build-llvm.py: Switch to dict() over dictionary comprehension`](https://github.com/ClangBuiltLinux/tc-build/pull/229)
* [`Update default PGO kernel to 6.2.8 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/230)
* [`Drop majority of Android patches (March 27, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/537)
* [`Drop android12-5.4 patches (March 28, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/538)
* [`Turn x86_64 allyesconfig builds back on`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/541)
* [`check_logs.py: Print logs if build result is not a pass`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/542)
* [`Disable x86_64 allyesconfig builds on stable`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/543)
* [`Update patches (March 30, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/546)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I put prebuilt LLVM toolchains on kernel.org for other developers ([download link](https://kernel.org/pub/tools/llvm/), [announcement](https://lore.kernel.org/20230328190906.GB375033@dev-arch.thelio-3990X/)).



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
