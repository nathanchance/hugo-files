---
title: December 2025 ClangBuiltLinux Work
date: 2025-12-30T17:30:00-0700
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

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply ccc35ff2fd2911986b716a87fe65e03fac2312c9 to 5.15, 6.1, and 6.6`](https://lore.kernel.org/20251204000025.GA468348@ax162/)
  * [`Apply fc7bf4c0d65a342b29fe38c332db3fe900b481b9 to 5.15`](https://lore.kernel.org/20251204001352.GB468348@ax162/)
  * [`Please apply 5b1e38c0792cc7a44997328de37d393f81b2501a to 5.15`](https://lore.kernel.org/20251204002517.GC468348@ax162/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `virt: Fix Kconfig warning when selecting TSM without VIRT_DRIVERS` ([`v1`](https://lore.kernel.org/20251203-fix-pci-tsm-select-tsm-warning-v1-1-c3959c1cb110@kernel.org/))
  * `ALSA: hda: cix-ipbloq: Use modern PM ops` ([`v1`](https://lore.kernel.org/20251211-hda-cix-ipbloq-modern-pm-ops-v1-1-c7a5580af021@kernel.org/))
  * `drm/amd/display: Apply e4479aecf658 to dml` ([`v1`](https://lore.kernel.org/20251213-dml-bump-frame-warn-clang-sanitizers-v1-1-0e91608db9eb@kernel.org/))
  * `drm/amd/display: Address -Wframe-larger-than with clang-22` ([`v1`](https://lore.kernel.org/20251213-dml-dcn30-avoid-clang-frame-larger-than-v1-0-dd3d74b76a17@kernel.org/))
  * `ASoC: rt1320: Change return type of rt1320_t0_load() to void` ([`v1`](https://lore.kernel.org/20251219-rt1320-sdw-avoid-uninit-ret-v1-1-faa3e250ebc4@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kbuild: Add top-level target for building gen_init_cpio`](https://lore.kernel.org/20251201233654.GA2018462@ax162/)
* [`Re: [PATCH 1/2] fortify: Rename temporary file to match ignore pattern`](https://lore.kernel.org/20251201234105.GB2018462@ax162/)
* [`Re: [PATCH 2/2] fortify: Cleanup temp file also on non-successful exit`](https://lore.kernel.org/20251201234305.GC2018462@ax162/)
* [`Re: [PATCH v2] kbuild: fix compilation of dtb specified on command-line without make rule`](https://lore.kernel.org/20251202200917.GA1959956@ax162/)
* [`Re: [PATCH v3] mcb: Add missing modpost build support`](https://lore.kernel.org/20251202203421.GB1959956@ax162/)
* [`Re: [PATCH 4/4] build: rust: provide an option to inline C helpers into Rust`](https://lore.kernel.org/20251203212558.GB3060476@ax162/)
* [`Re: [PATCH] ASoC: codecs: nau8325: Silence uninitialized variables warnings`](https://lore.kernel.org/20251203214958.GC3060476@ax162/)
* [`Re: [PATCH v5 0/2] kbuild: distributed build support for Clang ThinLTO`](https://lore.kernel.org/20251204174954.GA1177092@ax162/)
* [`Re: [PATCH 6.1.y RESEND] KVM: arm64: silence -Wuninitialized-const-pointer warning`](https://lore.kernel.org/20251205002245.GA3463270@ax162/)
* [`Re: [PATCH v2] Support conditional deps using "depends on X if Y"`](https://lore.kernel.org/20251205015352.GA2060615@ax162/)
* [`Re: [PATCH iwlwifi-fixes] wifi: iwlwifi: Implement settime64 as stub for MVM/MLD PTP`](https://lore.kernel.org/20251205222615.GA3738427@ax162/)
* [`Re: Patch "dma-mapping: Allow use of DMA_BIT_MASK(64) in global scope" has been added to the 6.17-stable tree`](https://lore.kernel.org/20251208032256.GA1356249@ax162/)
* [`Re: [PATCH 6.18 regression fix] dma-mapping: Fix DMA_BIT_MASK() macro being broken`](https://lore.kernel.org/20251208032545.GB1356249@ax162/)
* [`Re: [PATCH] kbuild: fix modules.builtin.modinfo being executable`](https://lore.kernel.org/20251210002843.GA597239@ax162/)
* [`Re: [PATCH] modpost: drop '*_probe' from section check whitelist`](https://lore.kernel.org/176589079524.3551554.8965194250869606940.b4-ty@kernel.org/)
* [`Re: [PATCH] Revert "scripts/clang-tools: Handle included .c files in gen_compile_commands"`](https://lore.kernel.org/20251218001336.GA2451437@ax162/)
* [`Re: [PATCH v3] kconfig: Support conditional deps using "depends on X if Y"`](https://lore.kernel.org/20251219195657.GA1404453@ax162/)
* [`Re: [PATCH 1/1] kbuild: Only enable -Wtautological-constant-out-of-range-compare for W=2`](https://lore.kernel.org/20251219201231.GB1404453@ax162/)
* [`Re: [PATCH v3 0/1] kconfig: move XPM icons to separate files`](https://lore.kernel.org/176617677285.1406915.9246716063376321712.b4-ty@kernel.org/)
* [`Re: [PATCH v1 1/2] scripts: add tool to run containerized builds`](https://lore.kernel.org/20251219194748.GA1404325@ax162/)
* [`Re: [PATCH] kbuild: prefer ${NM} in check-function-names.sh`](https://lore.kernel.org/20251219214252.GD1407372@ax162/)
* [`Re: [PATCH v2 1/2] scripts: add tool to run containerized builds`](https://lore.kernel.org/20251219212731.GC1407372@ax162/)
* [`Re: [PATCH] kbuild: Add top-level target for building gen_init_cpio`](https://lore.kernel.org/176618153620.1413064.2360637802920518770.b4-ty@kernel.org/)
* [`Re: [PATCH v2] wifi: mt76: Fix strscpy buffer overflow in mt76_connac2_load_patch`](https://lore.kernel.org/20251223215431.GA3327658@ax162/)
* [`Re: [PATCH] riscv: boot: Always make Image from vmlinux, not vmlinux.unstripped`](https://lore.kernel.org/20251230200336.GA4062669@ax162/)
* [`Re: [PATCH] kbuild: uapi: Drop check_config()`](https://lore.kernel.org/176712531976.4072056.9156155764129336995.b4-ty@kernel.org/)
* [`Re: [PATCH 0/5] kbuild: uapi: improvements to header testing`](https://lore.kernel.org/20251230203156.GD4062669@ax162/)
* [`Re: [PATCH 1/3] scripts: kconfig: merge_config.sh: refactor from shell/sed/grep to awk`](https://lore.kernel.org/20251230205549.GE4062669@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [linux-next:master 11929/12398] sound/soc/codecs/nau8325.c:430:13: warning: variable 'n2_max' is uninitialized when used here`](https://lore.kernel.org/20251201191052.GA2727778@ax162/)
* [`Re: [PATCH v6 11/30] objtool: Trace instruction state changes during function validation`](https://lore.kernel.org/20251201202329.GA3225984@ax162/)
* [`Re: [PATCH net-next v1] r8169: add DASH support for RTL8127AP`](https://lore.kernel.org/20251201224238.GA604467@ax162/)
* [`objtool: drivers/media/pci/solo6x10/solo6x10.o: tw28_set_ctrl_val() falls through to next function __cfi_tw28_get_ctrl_val()`](https://github.com/ClangBuiltLinux/linux/issues/2144#issuecomment-3608834563)
* [`Re: [PATCH v2 1/8] drivers/virt: Drop VIRT_DRIVERS build dependency`](https://lore.kernel.org/20251202234435.GA1692217@ax162/)
* [`[Hexagon] Passes for widening vector operations and shuffle opt`](https://github.com/llvm/llvm-project/pull/169559#issuecomment-3634867412)
* [`[clang] Limit lifetimes of temporaries to the full expression`](https://github.com/llvm/llvm-project/pull/170517#issuecomment-3634886648)
* [`Re: [Linaro-TCWG-CI] v6.18-rc6-688-gcd41d3420ef6: Failure on arm`](https://lore.kernel.org/20251210072559.GB1147766@ax162/)
* [`[llvm][clang] Enable IO sandbox for assert builds`](https://github.com/llvm/llvm-project/pull/171935#issuecomment-3668579694)
* [`Re: [bug report] clk: visconti: Add support common clock driver and reset driver`](https://lore.kernel.org/20251210073611.GC1147766@ax162/)
* [`Re: [PATCH v3 1/2] drm/xe/xe_survivability: Redesign survivability mode`](https://lore.kernel.org/20251210075757.GA1206705@ax162/)
* [`Re: [PATCH v7 0/4] Add support for clean shutdown with MSHV`](https://lore.kernel.org/20251210081433.GA1238677@ax162/)
* [`Re: [linux-next:master 9676/10599] ld.lld: error: undefined symbol: rust_build_error`](https://lore.kernel.org/20251213015006.GB3115176@ax162/)
* [```Re: error: implicit declaration of function ‘rq_modified_clear’ (was [PATCH 5/5] sched: Rework sched_class::wakeup_preempt() and rq_modified_*())```](https://lore.kernel.org/20251215115121.GA505816@ax162/)
* [`Re: [GIT PULL] kbuild changes for v6.19`](https://lore.kernel.org/20251217083202.GA2118121@ax162/)
* [`Re: ARMv7 Linux + Rust doesn't boot when compiling with only LLVM=1`](https://lore.kernel.org/20251219211147.GA1407372@ax162/)
* [`Re: Do we still care about compilers without __seg_fs and __seg_gs support??`](https://lore.kernel.org/20251220002424.GA3998744@ax162/)
* [`Re: [bpf-next:master 8/9] ld.lld: error: .tmp_vmlinux1.btf.o is incompatible with elf32lriscv`](https://lore.kernel.org/20251228215613.GA2167770@ax162/)
* [`Re: [RFC PATCH v1] module: Fix kernel panic when a symbol st_shndx is out of bounds`](https://lore.kernel.org/20251229212938.GA2701672@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update stable anchor to 6.18`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/890)
* [`Drop support for 5.4`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/891)
* [`Drop drivers/dma/mmp_pdma.c patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/892)
* [`Drop downstream hack for warning in drivers/staging/rtl8712/rtl8712_cmd.c`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/893)
* [`generator: Use GitHub mirror for Arch Linux config`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/894)
* [`Disable i386 builds with clang < 17 on 6.18 and newer`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/896)
* [`Update patches (December 22, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/897)
* [`Bump PGO kernel to 6.18 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/313)
* [`tc_build: llvm: Add support for experimental targets`](https://github.com/ClangBuiltLinux/tc-build/pull/314)
* [`Stop building Fedora's powerpc64le configuration on 5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/898)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and an AMD-based device. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [21.1.7](https://lore.kernel.org/20251203024451.GA1766218@ax162/)
  * [21.1.8](https://lore.kernel.org/20251218051614.GA2453313@ax162/)

* I attended Linux Plumbers 2025 in Tokyo, having good conversations with folks around Kbuild and ClangBuiltLinux.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
