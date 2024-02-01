---
title: January 2024 ClangBuiltLinux Work
date: 2024-01-31T17:30:00-0700
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

  * `power: supply: bq24190_charger: Fix "initializer element is not constant" error` ([`v1`](https://lore.kernel.org/20240103-fix-bq24190_charger-vbus_desc-non-const-v1-1-115ddf798c70@kernel.org/))
  * `lib/Kconfig.debug: Disable CONFIG_DEBUG_INFO_BTF for Hexagon` ([`v1`](https://lore.kernel.org/20240105-hexagon-disable-btf-v1-1-ddab073e7f74@kernel.org/))
  * `Fix UML build with clang-18 and newer` ([`v1`](https://lore.kernel.org/20240123-fix-uml-clang-18-v1-0-efc095519cf9@kernel.org/))
  * `powerpc: xor_vmx: Add '-mhard-float' to CFLAGS` ([`v1`](https://lore.kernel.org/20240127-ppc-xor_vmx-drop-msoft-float-v1-1-f24140e81376@kernel.org/))
  * `s390: vDSO: Drop '-fPIC' from LDFLAGS` ([`v1`](https://lore.kernel.org/20240130-s390-vdso-drop-fpic-from-ldflags-v1-1-094ad104fc55@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `platform/x86: intel-uncore-freq: Fix types in sysfs callbacks` ([`v1`](https://lore.kernel.org/20240104-intel-uncore-freq-kcfi-fix-v1-1-bf1e8939af40@kernel.org/))
  * `Update LLVM Phabricator and Bugzilla links` ([`v1`](https://lore.kernel.org/20240109-update-llvm-links-v1-0-eb09b59db071@kernel.org/))
  * `selftests/bpf: Update LLVM Phabricator links` ([`v2`](https://lore.kernel.org/20240111-bpf-update-llvm-phabricator-links-v2-1-9a7ae976bd64@kernel.org/))
  * `RISC-V: Fix CONFIG_AS_HAS_OPTION_ARCH with tip of tree LLVM` ([`v1`](https://lore.kernel.org/20240125-fix-riscv-option-arch-llvm-18-v1-0-390ac9cc3cd0@kernel.org/))
  * `Bump the minimum supported version of LLVM to 13.0.1` ([`v1`](https://lore.kernel.org/20240125-bump-min-llvm-ver-to-13-0-1-v1-0-f5ff9bda41c5@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `powerpc: Use always instead of always-y in for crtsavres.o` ([`5.4`](https://lore.kernel.org/20240126-5-4-fix-lib-powerpc-backport-v1-1-2c110ed18b1d@kernel.org/), [`4.19`](https://lore.kernel.org/20240126-4-19-fix-lib-powerpc-backport-v1-1-f0de224db66b@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `drm/amd/display: Avoid enum conversion warning` ([`v1`](https://lore.kernel.org/20240110-amdgpu-display-enum-enum-conversion-v1-1-953ae94fe15e@kernel.org/))
  * `media: mxl5xx: Move xpt structures off stack` ([`v1`](https://lore.kernel.org/20240111-dvb-mxl5xx-move-structs-off-stack-v1-1-ca4230e67c11@kernel.org/))
  * `rtc: max31335: Fix comparison in max31335_volatile_reg()` ([`v1`](https://lore.kernel.org/20240117-rtc-max3133-fix-comparison-v1-1-91e98b29d564@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] drm/xe: Fix warning on impossible condition`](https://lore.kernel.org/20240103160736.GA3837497@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scripts/min-tool-version.sh: Raise min clang version to 18.0.0 for loongarch`](https://lore.kernel.org/20240108163759.GA2899468@dev-arch.thelio-3990X/)
* [`RISC-V boot failure after LLVM commit e87f33d9ce785668223c3bcc4e06956985cccda1`](https://github.com/ClangBuiltLinux/linux/issues/1965#issuecomment-1883336664)
* [`Re: [PATCH] Compiler Attributes: counted_by: bump compiler versions`](https://lore.kernel.org/20240109153249.GA205400@dev-arch.thelio-3990X/)
* [`Re: [PATCHv2 1/2] Compiler Attributes: counted_by: bump min gcc version`](https://lore.kernel.org/20240110030126.GA3624259@dev-arch.thelio-3990X/)
* [`Re: [PATCHv2 2/2] Compiler Attributes: counted_by: fixup clang URL`](https://lore.kernel.org/20240110030258.GB3624259@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/1] Fix a wrong value passed to __find_vmap_area()`](https://lore.kernel.org/20240111155511.GA3451701@dev-arch.thelio-3990X/)
* [`Re: [PATCH] docs: kbuild/kconfig: reformat/cleanup`](https://lore.kernel.org/20240112194323.GA3703896@dev-arch.thelio-3990X/)
* [`check all builds for metadata`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/683#pullrequestreview-1830398557)
* [`Re: [RFC PATCH 4/5] x86/head64: Replace pointer fixups with PIE codegen`](https://lore.kernel.org/20240122224417.GC141255@dev-fedora.aadp/)
* [`[lld] Add target support for SystemZ (s390x)`](https://github.com/llvm/llvm-project/pull/75643#issuecomment-1908792279)
* [`Re: [PATCH v2 00/17] x86: Confine early 1:1 mapped startup code`](https://lore.kernel.org/20240125222338.GA2585843@dev-arch.thelio-3990X/)
* [`Re: [PATCH linux-next v3 00/14] Split crash out from kexec and clean up related config items`](https://lore.kernel.org/20240126045551.GA126645@dev-arch.thelio-3990X/)
* [`Re: [RFC] arm64: use different compiler inlining options for arm64 kernel builds`](https://lore.kernel.org/20240126165847.GA4116262@dev-arch.thelio-3990X/)
* [`Re: [PATCH] RISC-V: fix check for zvkb with tip-of-tree clang`](https://lore.kernel.org/20240127195200.GA781164@dev-arch.thelio-3990X/)
* [`Re: [PATCH] crypto: virtio/akcipher - Fix stack overflow on memcpy`](https://lore.kernel.org/20240131191011.GA679548@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`clang-nightly images have not been updated in three weeks`](https://gitlab.com/Linaro/tuxmake/-/issues/210)
* [`Re: [bcachefs:header_cleanup 21/51] /bin/bash: line 1: 19420 Segmentation fault...`](https://lore.kernel.org/20240103235543.GA696229@dev-arch.thelio-3990X/)
* [`New fortify warning in fs/smb/client/cifsencrypt.c after LLVM commit d77067d08a3f56dc2d0e6c95bd2852c943df743a`](https://github.com/ClangBuiltLinux/linux/issues/1966#issuecomment-1877835885)
* [`[MC][RISCV][LoongArch] Add AlignFragment size if layout is available and not need insert nops`](https://github.com/llvm/llvm-project/pull/76552#issuecomment-1878952480)
* [`[LoongArch] Allow delayed decision for ADD/SUB relocations`](https://github.com/llvm/llvm-project/pull/72960#issuecomment-1879033494)
* [`Re: [PATCH v4 2/4] arch/x86: Move internal setup_data structures into setup_data.h`](https://lore.kernel.org/20240109175814.GA5981@dev-arch.thelio-3990X/)
* [`Re: riscv: clang-nightly-allmodconfig: failed on stable-rc 6.6 and 6.1`](https://lore.kernel.org/20240109152634.GA205272@dev-arch.thelio-3990X/)
* [`Clang 18.0.0: Linux 6.1 and 6.6 giving unused-variable warnings despite used attribute`](https://github.com/ClangBuiltLinux/linux/issues/1977)
* [`Compile time fortify warning in Linux kernel after commit d77067d08a3f56d`](https://github.com/llvm/llvm-project/issues/77813)
* [`Re: drivers/net/ethernet/mellanox/mlx5/core/esw/bridge.c:1833:4: warning: 'snprintf' will always be truncated; specified size is 80, but format string expands to at least 94`](https://lore.kernel.org/20240112194729.GB3703896@dev-arch.thelio-3990X/)
* [`Re: [akpm-mm:mm-nonmm-unstable 22/23] lib/group_cpus.c:352:16: error: array initializer must be an initializer list`](https://lore.kernel.org/20240116225536.GA3108998@dev-arch.thelio-3990X/)
* [`Segmentation fault in modpost when building UML on mainline after LLVM commit 4bf8a688956a`](https://github.com/ClangBuiltLinux/linux/issues/1981)
* [`Link failure when building UML on linux-6.1.y after LLVM commit ec92d74a0ef89`](https://github.com/ClangBuiltLinux/linux/issues/1982)
* [`Re: [GIT PULL]: dmaengine updates for v6.8`](https://lore.kernel.org/20240118191148.GA3329935@dev-arch.thelio-3990X/)
* [`Fio test trigger ext4 kernel panic`](https://github.com/ClangBuiltLinux/linux/issues/1979)
* [`LoongArch clang-nightly container appears to be stale`](https://gitlab.com/Linaro/tuxsuite/-/issues/202)
* [`AMD64 segfault during LTO building linux-6.7.0 with clang-17.0.6`](https://github.com/ClangBuiltLinux/linux/issues/1984)
* [`Re: [PATCH -next v21 23/27] riscv: detect assembler support for .option arch`](https://lore.kernel.org/20240122222918.GA141255@dev-fedora.aadp/)
* [`Re: [PATCH v2 2/4] modpost: inform compilers that fatal() never returns`](https://lore.kernel.org/20240122230255.GD141255@dev-fedora.aadp/)
* [`Support C++20 Modules in clang-repl`](https://github.com/llvm/llvm-project/pull/79261#issuecomment-1909190463)
* [`Re: [PATCH 5.10 000/286] 5.10.209-rc1 review`](https://lore.kernel.org/20240126203436.GA913905@dev-arch.thelio-3990X/)
* [`Re: [PATCH] tty: serial: amba-pl011: Remove QDF2xxx workarounds`](https://lore.kernel.org/20240130170556.GA1125757@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 08/10] workqueue: Introduce struct wq_node_nr_active`](https://lore.kernel.org/20240130180010.GA2011608@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update stable to 6.7 and add separate 6.6 builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/673)
* [`android15-6.6`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/674)
* [`Update LoongArch builds for latest developments`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/680)
* [`tc_build: kernel: Add support for LoongArch`](https://github.com/ClangBuiltLinux/tc-build/pull/257)
* [`Update default PGO kernel to 6.7.0 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/258)
* [`Fix ChromeOS arm64 fragment name`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/681)
* [`Add chromeos-6.1 and chromeos-6.6`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/682)
* [`Fix ChromeOS x86_64 configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/684)
* [`Update LLVM tip of tree anchor to 19`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/685)
* [`Decompose generator.yml`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/687)
* [`Patch all outstanding failures (Jan 24, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/688)
* [`Add support for RISC-V LTO on -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/690)
* [`Add a patch to fix the PowerPC build on 5.4`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/691)
* [`Combine certain YAML files further`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/692)
* [`Restructure repository and add documentation`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/693)
* [`Update patches (January 27, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/695)
* [`patches: next: Drop ARCH=um series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/696)
* [`caching: Add hash of contents of patches folder`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/697)
* [`patches: tip: Drop smb __bdos() patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/698)
* [`build-binutils.py: 2.42`](https://github.com/ClangBuiltLinux/tc-build/pull/259)
* [`patches: arm64-fixes: Drop smb __bdos() patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/699)
* [`Drop LLVM 11/12 builds on -next and LLVM 11 builds on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/700)
* [`Attempt to rebalance builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/701)
* [`Temporarily disable LLVM tip of tree builds for RISC-V`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/702)
* [`Update {upload,download}-artifact actions to v4`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/703)
* [`Add LLVM 18 builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/704)
* [`workflows: python_lint: Update setup-python action to v5`](https://github.com/ClangBuiltLinux/actions-workflows/pull/9)
* [`boot-qemu.py: Fix GDB check in QEMURunner`](https://github.com/ClangBuiltLinux/boot-utils/pull/116)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I ran the [January 10th](https://github.com/ClangBuiltLinux/meeting-notes/pull/53) and [January 24th](https://github.com/ClangBuiltLinux/meeting-notes/pull/54) ClangBuiltLinux meetings.

* I continue to upload stable LLVM releases to kernel.org to ensure kernel developers have easy access to LLVM for reproducing and fixing issues that they introduce.

  * [LLVM 18.1.0-rc1](https://lore.kernel.org/20240130145752.GA3166831@dev-arch.thelio-3990X/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
