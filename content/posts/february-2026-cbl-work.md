---
title: February 2026 ClangBuiltLinux Work
date: 2026-02-27T17:30:00-0700
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

  * `init/Kconfig: Adjust fixed clang version for __builtin_counted_by_ref` ([`v1`](https://lore.kernel.org/20260223-fix-clang-version-builtin-counted-by-ref-v1-1-3ea478a24f0a@kernel.org/))

* Kbuild / Kconfig fixes and improvements: These are changes that I have authored as part of maintaining Kbuild and Kconfig.

  * `kbuild: rpm-pkg: Address -debuginfo build regression with RPM < 4.20.0` ([`v1`](https://lore.kernel.org/20260210-kbuild-fix-debuginfo-rpm-v1-0-0730b92b14bc@kernel.org/))
  * `kbuild: rpm-pkg: Fix manual debuginfo generation when using .src.rpm` ([`v1`](https://lore.kernel.org/20260213-fix-debuginfo-srcrpm-pkg-v1-1-45cd0c0501b9@kernel.org/))
  * `kbuild: rpm-pkg: Disable automatic requires for manual debuginfo package` ([`v1`](https://lore.kernel.org/20260216-improve-manual-debuginfo-template-v1-1-e584b3f8d3be@kernel.org/))
  * `kbuild: Split .modinfo out from ELF_DETAILS` ([`v1`](https://lore.kernel.org/20260225-separate-modinfo-from-elf-details-v1-1-387ced6baf4b@kernel.org/))
  * `kbuild: Leave objtool binary around with 'make clean'` ([`v1`](https://lore.kernel.org/20260227-avoid-objtool-binary-removal-clean-v1-1-122f3e55eae9@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but matter in some way to my other work.

  * `media: rockchip: Disable VIDEO_ROCKCHIP_VDEC when compile testing for Hexagon` ([`v1`](https://lore.kernel.org/20260213-media-disable-rockchip-vdec-hexagon-v1-1-3f903398cc83@kernel.org/))
  * `kbuild: Switch from '-fms-extensions' to '-fms-anonymous-structs' when available` ([`v1`](https://lore.kernel.org/20260223-fms-anonymous-structs-v1-0-8ee406d3c36c@kernel.org/))
  * `lib/Kconfig.debug: Require a release version of LLVM 22 for context analysis` ([`v1`](https://lore.kernel.org/20260224-bump-clang-ver-context-analysis-v1-1-16cc7a90a040@kernel.org/))
  * `genksyms: Fix parsing a declarator with a preceding attribute` ([`v1`](https://lore.kernel.org/20260225-genksyms-fix-attribute-declarator-v1-1-1b21478663fb@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] MIPS: tools: relocs: Ship a definition of R_MIPS_PC32`](https://lore.kernel.org/20260202230729.GA2319189@ax162/)
* [`Re: [RFC PATCH] kbuild: Make --build-id linker flag configurable`](https://lore.kernel.org/20260202231508.GB2319189@ax162/)
* [`Re: [PATCH] MIPS: Work around LLVM bug when gp is used as global register variable`](https://lore.kernel.org/20260202232309.GA1070900@ax162/)
* [`Re: [PATCH v2 00/14] Add SPDX SBOM generation tool`](https://lore.kernel.org/20260203004032.GA52989@ax162/)
* [`Re: [PATCH v2] rust: Makefile: bound rustdoc workaround to affected versions`](https://lore.kernel.org/20260203005627.GB52989@ax162/)
* [`Re: [PATCH next] fuse: Fix 'min: signedness error' in fuse_wr_pages()`](https://lore.kernel.org/20260203010558.GA1648836@ax162/)
* [`Re: kernel/futex/core.c:604:23: error: cannot jump from this asm goto statement to one of its possible targets`](https://lore.kernel.org/20260203035603.GC52989@ax162/)
* [`Re: [PATCH next 02/14] kbuild: Add W=c for additional compile time checks`](https://lore.kernel.org/20260203044726.GA3049363@ax162/)
* [`Re: [PATCH v9 0/6] scripts/make_fit: Support ramdisks and faster operations`](https://lore.kernel.org/20260203045123.GB3049363@ax162/)
* [`Re: [PATCH] streamline_config.pl: remove superfluous exclamation mark`](https://lore.kernel.org/177009452526.3190166.2162163285999095717.b4-ty@kernel.org/)
* [`Re: [PATCH V2] modpost: Amend ppc64 save/restfpr symnames for -Os build`](https://lore.kernel.org/20260203064800.GA701088@ax162/)
* [`Re: [PATCH 1/2] hwmon: (macsmc) Fix regressions in Apple Silicon SMC hwmon driver`](https://lore.kernel.org/20260203080619.GA1329615@ax162/)
* [`Re: [PATCH] MIPS: tools: relocs: Ship a definition of R_MIPS_PC32`](https://lore.kernel.org/20260202230729.GA2319189@ax162/)
* [`Re: [PATCH] powerpc/uaccess: Fix inline assembly for clang build on PPC32`](https://lore.kernel.org/20260203205501.GC3573384@ax162/)
* [`Re: [PATCH v4] rust: Makefile: bound rustdoc workaround to affected versions`](https://lore.kernel.org/20260203221224.GA2703490@ax162/)
* [`Re: (subset) [RFC PATCH 1/3] MAINTAINERS: Add scripts/install.sh into Kbuild entry`](https://lore.kernel.org/177016418511.1146354.1905685795770381118.b4-ty@kernel.org/)
* [`Re: [PATCH] kbuild: install-extmod-build: do not exclude scripts/dtc/libfdt/`](https://lore.kernel.org/20260204021603.GA2646832@ax162/)
* [`Re: [PATCH] kbuild: Do not run kernel-doc when building external modules`](https://lore.kernel.org/20260204073903.GA1632007@ax162/)
* [`Re: [PATCH] kbuild: dummy-tools: Add python3`](https://lore.kernel.org/20260204074557.GB1632007@ax162/)
* [`Re: [PATCH] rust: kconfig: Don't require RUST_IS_AVAILABLE for rustc-option`](https://lore.kernel.org/20260204074909.GC1632007@ax162/)
* [`Re: [PATCH v9 0/6] scripts/make_fit: Support ramdisks and faster operations`](https://lore.kernel.org/177031941047.2027657.11296594038346797555.b4-ty@kernel.org/)
* [`Re: [PATCH] rust: build: remap path to avoid absolute path`](https://lore.kernel.org/20260207220543.GA1670883@ax162/)
* [`Re: [PATCH] kbuild: remove dependency of run-command on config`](https://lore.kernel.org/177050232853.438183.2849242193101976758.b4-ty@kernel.org/)
* [`Re: [RFC PATCH v3 6/6] powerpc: Enable build-time feature fixup processing by default`](https://lore.kernel.org/20260210212007.GA1148627@ax162/)
* [`Re: [PATCH] kbuild: Add objtool to top-level clean target`](https://lore.kernel.org/20260210221734.GB1148627@ax162/)
* [`Re: [PATCH] kbuild: rpm-pkg: Fix generating debuginfo manually.`](https://lore.kernel.org/20260212162643.GB802926@ax162/)
* [`[Hexagon] Fix compile-time blowup with partially-reserved physregs`](https://github.com/llvm/llvm-project/pull/181728#issuecomment-3912038659)
* [`Re: [PATCH 2/2] KVM: VMX: Use ASM_INPUT_RM in __vmcs_writel`](https://lore.kernel.org/20260225190213.GA2755431@ax162/)
* [`Re: [PATCH RESEND v2] tools build: Use -fzero-init-padding-bits=all`](https://lore.kernel.org/20260224171956.GA639152@ax162/)
* [`Re: [PATCH] kbuild: only clean objtool on mrproper`](https://lore.kernel.org/20260225200417.GE2755225@ax162/)
* [`Re: [PATCH] kbuild: host: use single executable for rustc -C linker`](https://lore.kernel.org/20260225214523.GA4062959@ax162/)
* [`Re: [PATCH] kconfig: fix potential NULL pointer dereference in conf_askvalue`](https://lore.kernel.org/20260225194404.GD2755225@ax162/)
* [`Re: [PATCH] kbuild: install-extmod-build: Package resolve_btfids if necessary`](https://lore.kernel.org/20260226203758.GC3196155@ax162/)
* [`Re: [PATCH 1/2] Documentation/llvm: drop note about LLVM=0`](https://lore.kernel.org/20260226214349.GA1534917@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH 5.15 195/206] mm/kfence: randomize the freelist on initialization`](https://lore.kernel.org/20260204184810.GA2715873@ax162/)
* [`Re: [PATCH v2] HID: pidff: Fix condition effect bit clearing`](https://lore.kernel.org/20260205004106.GA2386589@ax162/)
* [`Re: [PATCH V2 3/3] powerpc: Enable Rust for ppc64le`](https://lore.kernel.org/20260205200222.GA1298159@ax162/)
* [`Re: Regression with patch "kbuild: rpm-pkg: Generate debuginfo package manually"`](https://lore.kernel.org/20260206002412.GB2975087@ax162/)
* [`Re: [GIT PULL 12/12 for v7.0] vfs misc`](https://lore.kernel.org/20260207051114.GA2289557@ax162/)
* [`Re: Clang/LLVM 'main' regression testing and -Wthread-safety`](https://lore.kernel.org/20260211194020.GA3482274@ax162/)
* [`Re: Clang/LLVM 'main' regression testing and -Wthread-safety`](https://lore.kernel.org/20260211194020.GA3482274@ax162/)
* [`Re: make binrpm-pkg broken due to commit 62089b804895`](https://lore.kernel.org/20260212160817.GA802926@ax162/)
* [`Re: [PATCH 0/2] kbuild: rpm-pkg: Address -debuginfo build regression with RPM < 4.20.0`](https://lore.kernel.org/20260213191138.GA2131983@ax162/)
* [`Re: [arm-integrator:b4/aarch64-clear-pages 2/2] make[7]: *** No rule to make target 'arch/arm64/kvm/hyp/nvhe/../../../lib/clear_page.nvhe.o', needed by 'arch/arm64/kvm/hyp/nvhe/kvm_nvhe.tmp.o'.`](https://lore.kernel.org/20260214192543.GA2633863@ax162/)
* [`Re: [GIT PULL] fbdev fixes and updates for v7.0-rc1`](https://lore.kernel.org/20260217214523.GA3380010@ax162/)
* [`Re: extlinux can't boot kernel after commit "kbuild: keep .modinfo section in vmlinux.unstripped"`](https://lore.kernel.org/20260224204632.GA3510750@ax162/)
* [`Re: Failure to build with LLVM for the Wii`](https://lore.kernel.org/20260225213500.GI2755225@ax162/)
* [`Re: [PATCH v17 0/3] Improve proc RSS accuracy`](https://lore.kernel.org/20260227011201.GA1577380@ax162/)
* [`Re: [PATCH v2 06/10] drm/xe/tests: Add KUnit tests for new VRAM fair provisioning`](https://lore.kernel.org/20260227011639.GA1683727@ax162/)
* [`Re: [PATCH 6.19 215/781] ALSA: pcm: Relax __free() variable declarations`](https://lore.kernel.org/20260227231746.GA1772942@ax162/)
* [`[CMake] Propagate dependencies to OBJECT libraries in add_llvm_library`](https://github.com/llvm/llvm-project/pull/183541#issuecomment-3976553258)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update patches (February 3, 2026)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/905)
* [`Add support for LLVM 22`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/906)
* [`runtime: docker: Update base version for clang-android to debian13`](https://github.com/kernelci/tuxmake/pull/242)
* [`Add a hardening.config plus full LTO build for arm64 and x86_64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/907)
* [`tc-build: Introduce '--multicall'`](https://github.com/ClangBuiltLinux/tc-build/pull/317)
* [`tc_build: builder: Show output of failed commands with capture_output=True`](https://github.com/ClangBuiltLinux/tc-build/pull/318)
* [`Bump PGO kernel to 6.19 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/319)
* [`Bump stable anchor to 6.19`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/908)
* [`patches: mainline: Drop GHES patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/909)
* [`Update korg-clang-22 to 22.1.0-rc3`](https://github.com/kernelci/tuxmake/pull/246)
* [`build-binutils.py: 2.46`](https://github.com/ClangBuiltLinux/tc-build/pull/320)
* [`Drop clang-11 builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/910)
* [`Updates for latest apt.llvm.org changes`](https://github.com/kernelci/tuxmake/pull/247)
* [`Update Arch Linux configuration path`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/911)
* [`Disable CONFIG_VIDEO_ROCKCHIP_VDEC for ARCH=loongarch allmodconfig`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/912)
* [`Adjust build schedule for new -next maintainer`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/913)
* [`patches: mainline: Drop vt_do_diacrit() patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/914)
* Action version updates
  * [`actions-workflows`](https://github.com/ClangBuiltLinux/actions-workflows/pull/10)
  * [`continuous-integration2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/915)
  * [`tc-build`](https://github.com/ClangBuiltLinux/tc-build/pull/321)
* [`Convert to running scripts with uv by default`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/916)
* [`Update korg-clang-22 to 22.1.0`](https://github.com/kernelci/tuxmake/pull/252)
* [`Two updates to tc_build/tools.py`](https://github.com/ClangBuiltLinux/tc-build/pull/322)
* [`Updates to parse-debian-clang.py and its usage`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/917)
* [`patches: next: Drop CONFIG_WARN_CONTEXT_ANALYSIS version update patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/918)
* [`Various matrix build updates`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/919)
* [`Add basic support for building Rust`](https://github.com/ClangBuiltLinux/tc-build/pull/299#issuecomment-3969952048)
* [`boot-qemu.py: Add '-M' / '--modules'`](https://github.com/ClangBuiltLinux/boot-utils/pull/128)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I submitted the following pull requests:

  * [`[GIT PULL] Kbuild / Kconfig updates for 7.0`](https://lore.kernel.org/20260209223615.GA3506796@ax162/)
  * [`[GIT PULL] Kbuild fixes for 7.0 #1`](https://lore.kernel.org/20260218210423.GA3232039@ax162/)

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [22.1.0-rc3](https://lore.kernel.org/20260210202014.GA1098715@ax162/)
  * [22.1.0](https://lore.kernel.org/20260225014834.GA1284670@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
