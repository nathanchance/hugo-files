---
title: "May 2022 ClangBuiltLinux Work"
date: 2022-05-31T16:30:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Build errors: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `riscv: Move alternative length validation into subsection` ([`v1`](https://lore.kernel.org/20220516214520.3252074-1-nathan@kernel.org/))
  * `riscv: Fix ALT_THEAD_PMA's asm parameters` ([`v1`](https://lore.kernel.org/20220518184529.454008-1-nathan@kernel.org/))
  * `ath6kl: Use cc-disable-warning to disable -Wdangling-pointer` ([`v1`](https://lore.kernel.org/20220524145655.869822-1-nathan@kernel.org/))

* Feature work: These patches focus on getting things that don't currently work or happen but should. In this case, this series enables linking the PowerPC vDSO with ld.lld, which is necessary to avoid cryptic build failures when using Link Time Optimization (LTO), which is currently a work in progress.

  * `Link the PowerPC vDSO with ld.lld` ([`v1`](https://lore.kernel.org/20220509204635.2539549-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20220511185001.3269404-1-nathan@kernel.org/))

* Miscellaneous fixes: These are fixes that don't fit into a particular category but are important to ClangBuiltLinux. In this case, this patch fixes a Control Flow Integrity (CFI) violation in the i915 driver, which I found while testing kCFI, a new CFI scheme specifically for the Linux kernel being developed in Clang/LLVM.

  * `drm/i915: Fix CFI violation with show_dynamic_id()` ([`v1`](https://lore.kernel.org/20220512211704.3158759-1-nathan@kernel.org/))

* Stable backports: These are patches It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Warning fixes for clang + x86_64 allmodconfig on 5.10 and 5.4`](https://lore.kernel.org/YnqlnFWCrEz11T5Y@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `i2c: at91: Initialize dma_buf in at91_twi_xfer()` ([`v1`](https://lore.kernel.org/20220505152738.1440249-1-nathan@kernel.org/))
  * `nvme: Ensure ret is always initialized in nvme_ns_head_chr_uring_cmd()` ([`v1`](https://lore.kernel.org/20220506150357.2443040-1-nathan@kernel.org/))
  * `misc: rtsx: Fix clang -Wsometimes-uninitialized in rts5261_init_from_hw()` ([`v1`](https://lore.kernel.org/20220523150521.2947108-1-nathan@kernel.org/))
  * `mailbox: qcom-ipcc: Fix -Wunused-function with CONFIG_PM_SLEEP=n` ([`v1`](https://lore.kernel.org/20220523224702.2002652-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v1 00/11] PCI/PM: Rework powering up PCI devices`](https://lore.kernel.org/YnQpAT59+dnfkfjO@thelio-3990X/)
* [`Re: uninitialized variables bugs`](https://lore.kernel.org/YnWYHzQC4Y55sOsT@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 00/15] kbuild: yet another series of cleanups (modpost, LTO, MODULE_REL_CRCS)`](https://lore.kernel.org/YnWlCH2tfr5YMb1%2F@dev-arch.thelio-3990X/)
* [`Re: [RESEND PATCH v1] x86/build: add -fno-builtin flag to prevent shadowing`](https://lore.kernel.org/YnhXgzhghfi17vMX@dev-arch.thelio-3990X/)
* [`Re: [PATCH kernel] powerpc/llvm/lto: Allow LLVM LTO builds`](https://lore.kernel.org/YnlYemdzBDCobK%2Fd@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 00/14] kbuild: yet another series of cleanups (modpost, LTO, MODULE_REL_CRCS, export.h)`](https://lore.kernel.org/YnmTVlo5JRqOGUPh@dev-arch.thelio-3990X/)
* [`Re: [GIT PULL] virtio: last minute fixup`](https://lore.kernel.org/YnrxTMVRtDnGA%2FEK@dev-arch.thelio-3990X/)
* [`Initial statically linked clang image`](https://github.com/ClangBuiltLinux/containers/pull/5)
* [`Re: [PATCH 4/8] s390/entry: workaround llvm's IAS limitations`](https://lore.kernel.org/YnvynSZfF%2F8I8vmT@dev-arch.thelio-3990X/)
* [`Re: [PATCH v5 00/12] kbuild: yet another series of cleanups (modpost, LTO, MODULE_REL_CRCS, export.h)`](https://lore.kernel.org/YnwWsYdL2khCikSY@dev-arch.thelio-3990X/)
* [`Re: [PATCH 0/8] s390: allow to build with llvm's integrated assembler`](https://lore.kernel.org/Ynwh%2FUk3IyiyRzO3@dev-arch.thelio-3990X/)
* [`Re: [PATCH 4.19] MIPS: fix allmodconfig build with latest mkimage`](https://lore.kernel.org/YoAZLPxTYsqEypGP@dev-arch.thelio-3990X/)
* [`Re: [PATCH] misc: rtsx: Set setting_reg2 before use.`](https://lore.kernel.org/YoJ0I%2FXPoj1B%2F+mm@dev-arch.thelio-3990X/)
* [`Re: [RFC PATCH v2 00/21] KCFI support`](https://lore.kernel.org/YoPuMhc03hUJxmPs@dev-arch.thelio-3990X/)
* [`Re: [PATCH] can: mcp251xfd: silence clang's -Wunaligned-access warning`](https://lore.kernel.org/YoUZLHIbxPu15%2FlN@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/1] um: fix error return code in winch_tramp()`](https://lore.kernel.org/YofQgDo38fAnPZEy@dev-arch.thelio-3990X/)
* [`-mharden-sls=all for x86`](https://github.com/ClangBuiltLinux/linux/issues/1633)
* [`add github action workflow`](https://github.com/ClangBuiltLinux/containers/pull/9)
* [`Epoch2`](https://github.com/ClangBuiltLinux/containers/pull/12)
* [`llvm-project: add epoch3`](https://github.com/ClangBuiltLinux/containers/pull/19)
* [`Re: [PATCH] drm/ssd130x: Only define a SPI device ID table when built as a module`](https://lore.kernel.org/YpYv8islaEySOEtg@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ARM: entry: add .ltorg directive to keep literals in range`](https://lore.kernel.org/YpY0PbvRvuA7OU%2F%2F@dev-arch.thelio-3990X/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: arch/x86/entry/entry: RFC on recent kernels building error with llvm 11.0.2 internal assembler`](https://lore.kernel.org/Ym2qdHAB6BMMlegB@thelio-3990X/)
* [`Re: [PATCH] interconnect: Restore sync state by ignoring ipa-virt in provider count`](https://lore.kernel.org/Ym%2F2QJeGHDoZSw8o@dev-arch.thelio-3990X/)
* [`Re: [willy-pagecache:for-next 60/69] fs/ntfs/aops.c:378:12: warning: stack frame size (2216) exceeds limit (1024) in 'ntfs_read_folio'`](https://lore.kernel.org/YnAcLApzs6frds2n@dev-arch.thelio-3990X/)
* [`[BOLT] Minimum hardware requirements?`](https://github.com/ClangBuiltLinux/tc-build/issues/188)
* [`Re: [PATCH 00/21] Folio patches for 5.19`](https://lore.kernel.org/YnFGuPWgT7tT7iAV@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Regression caused by llvm: [GVN] Encode GEPs in offset representation`](https://lore.kernel.org/YnFLeL93iHk+3vBd@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 4/9] PCI/PM: Rework changing power states of PCI devices`](https://lore.kernel.org/YnFtjzGYwe28tVAA@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3] random: use first 128 bits of input as fast init`](https://lore.kernel.org/YnL6ouh5xxZIJqeN@dev-arch.thelio-3990X/)
* [`Re: ld.lld: error: inline assembly requires more registers than available at line 523`](https://lore.kernel.org/YnK8f7cqMoHxSi0C@dev-arch.thelio-3990X/)
* [`INIT_STACK_ALL_ZERO - Framework Laptop system freezes (kernel oops?) on boot`](https://github.com/ClangBuiltLinux/linux/issues/1626)
* [`Re: [PATCH v2] i2c: at91: use dma safe buffers`](https://lore.kernel.org/YnPkfrI4Udl9lMR8@dev-arch.thelio-3990X/)
* [`Re: linux-next: build failure after merge of the amdgpu tree`](https://lore.kernel.org/YnQToaOno0MZzJ5r@dev-arch.thelio-3990X/)
* [`Pedantically warn about // comments in gnu89 mode`](https://reviews.llvm.org/rGf6dff93641b2259623e686eb13a1884b8b9f4a00)
* [`Re: [PATCH bpf-next 8/9] libbpf: automatically fix up BPF_MAP_TYPE_RINGBUF size, if necessary`](https://lore.kernel.org/YnqGBmOHIZhrZBFJ@dev-arch.thelio-3990X/)
* [`[ELF] Align the end of PT_GNU_RELRO to max-page-size instead of common-page-size`](https://reviews.llvm.org/D125410)
* [`__aeabi_uldivmod in drivers/mtd/parsers/scpart.o`](https://github.com/ClangBuiltLinux/linux/issues/1635)
* [`__mulodi4 in lib/overflow_kunit.o`](https://github.com/ClangBuiltLinux/linux/issues/1636)
* [`Re: [linux-next:master 9995/11651] fs/buffer.c:2254:5: warning: stack frame size (2144) exceeds limit (1024) in 'block_read_full_folio'`](https://lore.kernel.org/YoAlvnyjEbYV4T1L@dev-arch.thelio-3990X/)
* [`Suggest typoed directives in preprocessor conditionals`](https://reviews.llvm.org/D124726)
* [`"invalid input constraint '0' in asm" in arch/riscv/include/asm/errata_list.h`](https://github.com/ClangBuiltLinux/linux/issues/1641)
* [`objtool "no non-local symbols" error with tip of tree LLVM`](https://lore.kernel.org/YoK4U9RgQ9N+HhXJ@dev-arch.thelio-3990X/)
* [`-Wframe-larger-than in sound/soc/intel/avs/path.c`](https://github.com/ClangBuiltLinux/linux/issues/1642)
* [`Empty download_url key`](https://gitlab.com/Linaro/tuxsuite/-/issues/164)
* [`Re: [linux-next:master 12308/12886] arch/x86/kvm/hyperv.c:1983:22: warning: shift count >= width of type`](https://lore.kernel.org/Yoe4WQCV9903aQRP@dev-arch.thelio-3990X/)
* [`[PowerPC] Implement XL compat __fnabs and __fnabss builtins.`](https://reviews.llvm.org/D125506)
* [`Re: [PATCH v1] driver core: Extend deferred probe timeout on driver registration`](https://lore.kernel.org/YogkhvFGVcjNQ21Z@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Regression caused by llvm: [InstCombine] fold icmp with sub and bool`](https://lore.kernel.org/YoukyUoa2LfUZ9gw@dev-arch.thelio-3990X/)
* [`Re: [PATCH v5 5/7] null_blk: allow non power of 2 zoned devices`](https://lore.kernel.org/Yoz4PsQjWzVxz+YO@dev-arch.thelio-3990X/)
* [`Re: drivers/dma/imx-dma.c:1048:20: warning: cast to smaller integer type 'enum imx_dma_type' from 'const void *'`](https://lore.kernel.org/Yo7MFbwDyT+1lzcr@dev-arch.thelio-3990X/)
* [`CFI failure target: simpledrm_simple_display_pipe_mode_valid`](https://github.com/ClangBuiltLinux/linux/issues/1647)
* [`Build timeouts due to infrastructure error retries?`](https://gitlab.com/Linaro/tuxsuite/-/issues/166)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`build-llvm.py: Add a note about LLVM commit 7d7771f34d14 for BOLT`](https://github.com/ClangBuiltLinux/tc-build/pull/189)
* [`docker: clang-android: Update to r450784e (14.0.7)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/265)
* [`A couple of fixes for making tests pass on Arch Linux`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/270)
* [`ci: Wire up Arch Linux package building`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/271)
* [`Add support for ChromeOS`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/362)
* [`boot-qemu.sh: Fix aarch64 KVM after cb0698a`](https://github.com/ClangBuiltLinux/boot-utils/pull/63)
* [`Fix ChromeOS runs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/364)
* [`build-llvm.py: Introduce "slim" PGO`](https://github.com/ClangBuiltLinux/tc-build/pull/190)
* [`Disable ChromeOS jobs temporarily`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/366)
* [`Enable the integrated assembler for s390 on -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/367)
* [`Add objtool patch for stable and 5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/368)
* [`Upgrade stable anchor to 5.18`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/369)
* [`Disable arm64 CFI builds on mainline for clang-{12,13}`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/370)
* [`Test statically linked clang outside of Alpine Linux`](https://github.com/ClangBuiltLinux/containers/pull/13)
* [`patches: 5.15: Fix objtool patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/371)
* [`github: workflows: Update Docker actions`](https://github.com/ClangBuiltLinux/containers/pull/15)
* [`Sink 'docker login' step to right before image push and other small cleanups`](https://github.com/ClangBuiltLinux/containers/pull/20)
* [`github: Move to a composite action for llvm-project builds`](https://github.com/ClangBuiltLinux/containers/pull/22)
* [`github: workflows: Rename llvm-project workflows`](https://github.com/ClangBuiltLinux/containers/pull/23)
* [`Update checkout, download-artifact, and upload-artifact versions`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/372)
* [`Upload toolchain tarball when build is successful`](https://github.com/ClangBuiltLinux/containers/pull/24)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I wrote [a blog post](https://nathanchance.dev/posts/github-actions-fedora-libvirt/) as part of my work to get our [llvm-project container builds](https://github.com/ClangBuiltLinux/containers/tree/main/llvm-project) self-hosted on GitHub Actions.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
