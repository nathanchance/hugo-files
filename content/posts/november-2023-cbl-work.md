---
title: November 2023 ClangBuiltLinux Work
date: 2023-11-30T17:30:00-0700
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

  * `LoongArch: Mark __percpu functions as always inline` ([`v1`](https://lore.kernel.org/20231101-loongarch-always-inline-percpu-ops-v1-1-b8f2e9a71729@kernel.org/), [`v2`](https://lore.kernel.org/20231102-loongarch-always-inline-percpu-ops-v2-1-31c51959a5c0@kernel.org/))
  * `arm64: vdso32: Define BUILD_VDSO32_64 to correct prototypes` ([`v1`](https://lore.kernel.org/20231128-arm64-vdso32-missing-prototypes-error-v1-1-0fdd403cea07@kernel.org/))
  * `buffer: Add cast in grow_buffers() to avoid a multiplication libcall` ([`v1`](https://lore.kernel.org/20231128-avoid-muloti4-grow_buffers-v1-1-bc3d0f0ec483@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `x86: Support llvm-objdump in instruction decoder selftest` ([`v3`](https://lore.kernel.org/20231129-objdump-reformat-llvm-v3-0-0d855e79314d@kernel.org/))
  * `RISC-V: Disable DWARF5 with known broken LLVM versions` ([`v1`](https://lore.kernel.org/20231129-riscv-restrict-dwarf5-llvm-v1-0-ec0d368fb538@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `PCI: keystone: Drop __init from ks_pcie_add_pcie_{ep,port}()` ([`5.4`](https://lore.kernel.org/20231128-5-4-fix-pci-keystone-modpost-warning-v1-1-a999b944ac81@kernel.org/), [`5.10`](https://lore.kernel.org/20231128-5-10-fix-pci-keystone-modpost-warning-v1-1-43240045c516@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `drm/amd/display: Increase frame warning limit with KASAN or KCSAN in dml2` ([`v1`](https://lore.kernel.org/20231102-amdgpu-dml2-increase-frame-size-warning-for-clang-v1-1-6eb157352931@kernel.org/), [`v2`](https://lore.kernel.org/20231102-amdgpu-dml2-increase-frame-size-warning-for-clang-v2-1-b088a718131a@kernel.org/))
  * `tcp: Fix -Wc23-extensions in tcp_options_write()` ([`v1`](https://lore.kernel.org/20231031-tcp-ao-fix-label-in-compound-statement-warning-v1-1-c9731d115f17@kernel.org/), [`v2`](https://lore.kernel.org/20231106-tcp-ao-fix-label-in-compound-statement-warning-v2-1-91eff6e1648c@kernel.org/), [`v3`](https://lore.kernel.org/20231106-tcp-ao-fix-label-in-compound-statement-warning-v3-1-b54a64602a85@kernel.org/))
  * `mmc: sdhci-of-dwcmshc: Use logical OR instead of bitwise OR in dwcmshc_probe()` ([`v1`](https://lore.kernel.org/20231116-sdhci-of-dwcmshc-fix-wbitwise-instead-of-logical-v1-1-7e1a7f4ccaab@kernel.org/))
  * `power: reset: at91: Drop '__init' from at91_wakeup_status()` ([`v1`](https://lore.kernel.org/20231120-fix-at91-modpost-warnings-v1-1-813671933863@kernel.org/))
  * `hexagon: Fix up instances of -Wmissing-prototypes` ([`v1`](https://lore.kernel.org/20231130-hexagon-missing-prototypes-v1-0-5c34714afe9e@kernel.org/))
  * `s390: A couple of fixes for -Wmissing-prototypes` ([`v1`](https://lore.kernel.org/20231130-s390-missing-prototypes-v1-0-799d3cf07fb7@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] rpm-pkg: simplify installkernel %post`](https://lore.kernel.org/20231108000749.GA3723879@dev-arch.thelio-3990X/)
* [`Re: [PATCH] linux/export: clean up the IA-64 KSYM_FUNC macro`](https://lore.kernel.org/20231114200508.GA3378955@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/3] kunit: Add a macro to wrap a deferred action function`](https://lore.kernel.org/20231115152312.GA51310@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 1/1] ARM: kprobes: Explicitly reserve r7 for local variables`](https://lore.kernel.org/20231116172418.GA174808@dev-arch.thelio-3990X/)
* [`Re: [PATCH] LoongArch: Record pc instead of offset in la-abs relocation`](https://lore.kernel.org/20231120230817.GA2116806@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: support W=c and W=e shorthands for Kconfig`](https://lore.kernel.org/20231130001524.GA2513828@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/3] modpost: move __attribute__((format(printf, 2, 3))) to modpost.h`](https://lore.kernel.org/20231130001700.GB2513828@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/3] modpost: remove unreachable code after fatal()`](https://lore.kernel.org/20231130002943.GC2513828@dev-arch.thelio-3990X/)
* [`Re: [PATCH 3/3] modpost: move exit(1) for fatal() to modpost.h`](https://lore.kernel.org/20231130003207.GD2513828@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH 6/6] bcachefs: rebalance_work`](https://lore.kernel.org/20231101170255.GA1158461@dev-arch.thelio-3990X/)
* [`Re: [Linaro-TCWG-CI] v6.6-rc1-17-g1c6fdbd8f246: Failure on arm`](https://lore.kernel.org/20231101172208.GA1368360@dev-arch.thelio-3990X/)
* [`Re: [pci:controller/xilinx-xdma] BUILD REGRESSION 8d786149d78c7784144c7179e25134b6530b714b`](https://lore.kernel.org/20231101173432.GC1368360@dev-arch.thelio-3990X/)
* [`LoongArch build failure after LLVM commit 1a2e77cf9e11dbf56b5720c607313a566eebb16e`](https://github.com/ClangBuiltLinux/linux/issues/1955)
* [`Reland [SimplifyCFG] Delete the unnecessary range check for small mask operation`](https://github.com/llvm/llvm-project/pull/70542#issuecomment-1793501670)
* [`Re: [PATCH v5] kconfig: avoid an infinite loop in oldconfig/syncconfig`](https://lore.kernel.org/20231107210004.GA2065849@dev-arch.thelio-3990X/)
* [`Re: [PATCH 02/22] fb: atmel_lcdfb: Stop using platform_driver_probe()`](https://lore.kernel.org/20231108184805.GA1579138@dev-arch.thelio-3990X/)
* [`[IR] Mark lshr and ashr constant expressions as undesirable`](https://github.com/llvm/llvm-project/commit/82f68a992b9f89036042d57a5f6345cb2925b2c1#commitcomment-132406739)
* [`Error when building Linux with latest LLVM (and CLang) (LLVM issue #72026)`](https://github.com/llvm/llvm-project/issues/72026#issuecomment-1806866323)
* [`[CodeGen] Revamp counted_by calculations`](https://github.com/llvm/llvm-project/pull/70606#issuecomment-1809250417)
* [`Re: [PATCH v4 1/5] firmware: xilinx: Update firmware call interface to support additional args`](https://lore.kernel.org/20231114200056.GA3377479@dev-arch.thelio-3990X/)
* [`RISC-V configs with CONFIG_DEBUG_INFO_DWARF5=y broken after LLVM commit 1df5ea29b4369`](https://github.com/ClangBuiltLinux/linux/issues/1961)
* [`Re: [PATCH v7 net-next 4/5] net-device: reorganize net_device fast path variables`](https://lore.kernel.org/20231120194112.GA276812@dev-arch.thelio-3990X/)
* [`Assertion failure and infinite loop after bc09ec696209b3aea74d49767b15c2f34e363933`](https://github.com/llvm/llvm-project/issues/73168)
* [`[ConstraintElim] Treat ConstantPointerNull as constant offset 0.`](https://github.com/llvm/llvm-project/commit/23628137ea9e7a2942d6a691ea74a7697564e65b#commitcomment-133296604)
* [`Re: linux-next: manual merge of the vfs-brauner tree with the btrfs tree`](https://lore.kernel.org/20231128213344.GA3423530@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Add fix for -Wc23-extensions in net/ipv4/tcp_output.c`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/648)
* [`Update mainline and stable patches (November 2, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/649)
* [`Update default PGO kernel to 6.6.0 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/248)
* [`Copy mainline patches to tip (Nov 6, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/652)
* [`utils.py: Handle virtconfig`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/655)
* [`patches: mainline: Drop AMDGPU -Wframe-larger-than bump patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/656)
* [`A couple of ci.sh updates`](https://github.com/ClangBuiltLinux/tc-build/pull/250)
* [`tc_build: llvm: Move use of build_targets out of ninja_cmd`](https://github.com/ClangBuiltLinux/tc-build/pull/251)
* [`tc_build: llvm: Update BOLT flags`](https://github.com/ClangBuiltLinux/tc-build/pull/252)
* [`Update patches (November 10, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/658)
* [`Drop current -tip patches (November 13, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/660)
* [`build-llvm.py: Fix PGO using LLVM as training data without '--full-toolchain'`](https://github.com/ClangBuiltLinux/tc-build/pull/254)
* [`Delay LLVM tip of tree builds until November 28th`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/661)
* [`Update patches (November 28, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/662)
* [`build-binutils.py: Fix error with x86-64-v{2,3,4} as value to '-m'`](https://github.com/ClangBuiltLinux/tc-build/pull/256)
* [`patches: Drop SRSO patches from android13-5.10`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/663)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload stable LLVM releases to kernel.org to ensure kernel developers have easy access to LLVM for reproducing and fixing issues that they introduce.

  * [LLVM 17.0.5](https://lore.kernel.org/20231114174443.GA2423170@dev-arch.thelio-3990X/)
  * [LLVM 17.0.6](https://lore.kernel.org/20231128194910.GA3062541@dev-arch.thelio-3990X/)

* I attended Linux Plumbers Conference in Richmond, Virginia to keep in touch and interact with the greater kernel community.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
