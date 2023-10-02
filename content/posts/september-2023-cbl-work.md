---
title: September 2023 ClangBuiltLinux Work
date: 2023-09-29T16:30:00-0700
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

  * `LoongArch: Drop unused parse_r and parse_v macros` ([`v1`](https://lore.kernel.org/20230905-loongarch-fix-clang-lto-parse_r-v1-1-d991ee630222@kernel.org/))
  * `drm/debugfs: Fix drm_debugfs_remove_files() stub` ([`v1`](https://lore.kernel.org/20230913-fix-drm_debugfs_remove_files-stub-v1-1-6b952ac559f3@kernel.org/))
  * `drm/mediatek: Fix backport issue in mtk_drm_gem_prime_vmap()` ([`v1`](https://lore.kernel.org/20230922-5-10-fix-drm-mediatek-backport-v1-1-912d76cd4a96@kernel.org/))

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * [`ANDROID: libsubcmd: Hoist iterator variable declarations in parse_options_subcommand()`](https://android-review.googlesource.com/q/I41f17964f3d0822b026f6ae8f06a4d49bc7f15a9)
  * [`ANDROID: tools/resolve_btfids: Pass CFLAGS to libsubcmd build via EXTRA_CFLAGS`](https://android-review.googlesource.com/q/I91c1c9a8fb8f83118a6b8ec4da6cc33a773f2124)
  * [`android14-6.1 resolve_btfids backports`](https://android-review.googlesource.com/q/owner:nathan@kernel.org+branch:android14-6.1+(path:tools/lib/subcmd/Makefile+OR+path:tools/bpf/resolve_btfids/Makefile))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `of: kexec: Mark ima_{free,stable}_kexec_buffer() as __init` ([`v1`](https://lore.kernel.org/20230905-5-15-of-kexec-modpost-warning-v1-1-4138b2e96b4e@kernel.org/))
  * [`Apply 13e07691a16f and co. to linux-6.1.y`](https://lore.kernel.org/20230908161526.GA3344687@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `bcachefs compiler warning fixes for 32-bit` ([`v1`](https://lore.kernel.org/20230912-bcachefs-warning-fixes-v1-0-a1cc83a38836@kernel.org/))
  * `drm/amd/display: Fix -Wuninitialized in dm_helpers_dp_mst_send_payload_allocation()` ([`v1`](https://lore.kernel.org/20230913-fix-wuninitialized-dm_helpers_dp_mst_send_payload_allocation-v1-1-2d1b0a3ef16c@kernel.org/))



## LLVM patches

* [`[Clang][LoongArch] Generate _mcount instead of mcount`](https://github.com/llvm/llvm-project/pull/65657)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] overlayfs: set ctime when setting mtime and atime`](https://lore.kernel.org/20230913195731.GA2922283@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] bpf: Fix BTF_ID symbol generation collision`](https://lore.kernel.org/20230915171814.GA1721473@dev-arch.thelio-3990X/)
* [`Re: [PATCH  bpf  v3 1/2] bpf: Fix BTF_ID symbol generation collision`](https://lore.kernel.org/20230915183008.GA17653@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`[ clear linux 39890 ] ld.lld: error: undefined symbol: __isoc23_strtol`](https://github.com/ClangBuiltLinux/linux/issues/1929)
* [`Re: [weiny2:v6.5-pks-core 17/34] include/linux/stddef.h:10:7: error: Expected an identifier after {`](https://lore.kernel.org/20230905182950.GA2148713@dev-arch.thelio-3990X/)
* [`Re: [GIT PULL] bcachefs`](https://lore.kernel.org/20230906222847.GA230622@dev-arch.thelio-3990X/)
* [`Re: [PATCH] fs: have setattr_copy handle multigrain timestamps appropriately`](https://lore.kernel.org/20230912231014.GA795188@dev-arch.thelio-3990X/)
* [`Re: [PATCH v7 1/3 RESEND] block:sed-opal: SED Opal keystore`](https://lore.kernel.org/20230913165612.GA2213586@dev-arch.thelio-3990X/)
* [`Re: [PATCH v7 3/3 RESEND] powerpc/pseries: PLPKS SED Opal keystore support`](https://lore.kernel.org/20230913185951.GA3643621@dev-arch.thelio-3990X/)
* [`Re: [PATCH bpf 3/4] bpf: Ensure unit_size is matched with slab cache object size`](https://lore.kernel.org/20230914181407.GA1000274@dev-arch.thelio-3990X/)
* [`Potential byte buffer overflows at snprintf() usages`](https://bugzilla.kernel.org/show_bug.cgi?id=217885#c9)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Drop patches (Sep 5, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/623)
* [`Update checkout action version from 3 to 4 and two jobs' Ubuntu version from 20.04 to latest`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/624)
* [`workflows: Update to actions/checkout@v4 (actions-workflow)`](https://github.com/ClangBuiltLinux/actions-workflows/pull/7)
* [`workflows: Update to actions/checkout@v4 (containers)`](https://github.com/ClangBuiltLinux/containers/pull/51)
* [`Update default PGO kernel to 6.5.0 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/241)
* [`workflows: Update to actions/checkout@v4 (tc-build)`](https://github.com/ClangBuiltLinux/tc-build/pull/242)
* [`patches: arm64-fixes: Update for 6.5-rc3 fast forward`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/625)
* [`Add LoongArch clang-17 builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/626)
* [`Fix LoongArch LTO builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/627)
* [`Add android15-6.1 and adding missing AOSP LLVM builds for android14-6.1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/628)
* [`Drop test_scanf.c patch from -tip stack`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/629)
* [`docker: clang-android: Update to r498229b (17.0.4)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/342)
* [`boot-qemu.py: Allow memory value to be customized`](https://github.com/ClangBuiltLinux/boot-utils/pull/114)
* [`Drop test_scanf change from all stable trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/631)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
