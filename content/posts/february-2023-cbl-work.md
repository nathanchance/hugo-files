---
title: February 2023 ClangBuiltLinux Work
date: 2023-02-28T17:30:00-0700
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

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * [`ANDROID: GKI: Fix copying of protected_exports`](https://android-review.googlesource.com/q/I235cef7c848a7cf9df9d7d5343af33d95b501a15)

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `Allow CONFIG_PPC64_BIG_ENDIAN_ELF_ABI_V2 with ld.lld 15+` ([`v1`](https://lore.kernel.org/20230118-ppc64-elfv2-llvm-v1-0-b9e2ec9da11d@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `f2fs: Fix type of single bit bitfield in f2fs_io_info` ([`v1`](https://lore.kernel.org/20230201-f2fs-fix-single-length-bitfields-v1-1-e386f7916b94@kernel.org/))
  * `ASoC: mchp-spdifrx: Fix uninitialized use of mr in mchp_spdifrx_hw_params()` ([`v1`](https://lore.kernel.org/20230202-mchp-spdifrx-fix-uninit-mr-v1-1-629a045d7a2f@kernel.org/))
  * `dm flakey: Fix clang -Wparentheses-equality in flakey_map()` ([`v1`](https://lore.kernel.org/20230202-dm-parenthesis-warning-v1-1-ebdee213eeb9@kernel.org/))
  * `drm: omapdrm: Do not use helper unininitialized in omap_fbdev_init()` ([`v1`](https://lore.kernel.org/20230224-omapdrm-wsometimes-uninitialized-v1-1-3fec8906ee3a@kernel.org/))
  * `net/sched: cls_api: Move call to tcf_exts_miss_cookie_base_destroy()` ([`v1`](https://lore.kernel.org/20230224-cls_api-wunused-function-v1-1-12c77986dc2d@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `static call fixes for clang's conditional tail calls (6.2 and 6.1)` ([`v1`](https://lore.kernel.org/Y%2FUf9WUi%2FrANmOk8@dev-arch.thelio-3990X/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] Documentation/llvm: add Chimera Linux, Google and Meta datacenters`](https://lore.kernel.org/Y9rWwmRkYnUiroFY@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] tools/resolve_btfids: Tidy host CFLAGS forcing`](https://lore.kernel.org/Y9r2Paqbn6PP+EsM@dev-arch.thelio-3990X/)
* [`Re: [PATCH bpf-next] tools/resolve_btfids: Compile resolve_btfids as host program`](https://lore.kernel.org/Y9vxFLA6Xj%2FzPjQu@dev-arch.thelio-3990X/)
* [`Re: [PATCH] arm64: kprobes: Drop ID map text from kprobes blacklist`](https://lore.kernel.org/Y9%2F3ZPxfpcZ+JFnf@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86/vdso: Fake 32bit VDSO build on 64bit compile for vgetcpu.`](https://lore.kernel.org/Y+KCHOkrumS1I%2FTh@dev-arch.thelio-3990X/)
* [`Re: [PATCH] randstruct: disable Clang 15 support`](https://lore.kernel.org/Y+Oyi1eZqKtqbL40@dev-arch.thelio-3990X/)
* [`Re: [PATCH 00/10] objtool: Honey, I shrunk the instruction`](https://lore.kernel.org/Y+PrYVKwXIyutFEl@dev-arch.thelio-3990X/)
* [`Re: [PATCH] RDMA/cma: Distinguish between sockaddr_in and sockaddr_in6 by size`](https://lore.kernel.org/Y+RXfXDDKxKHjLbh@dev-arch.thelio-3990X/)
* [`Re: [PATCH] rust: allow to use INIT_STACK_ALL_ZERO`](https://lore.kernel.org/Y+aBT8AD9RXde24X@dev-arch.thelio-3990X/)
* [`Re: [PATCH] EDAC/amd64: remove unneeded call to reserve_mc_sibling_devs()`](https://lore.kernel.org/Y+qdVHidnrrKvxiD@dev-arch.thelio-3990X/)
* [`[llvm][SelectionDAGBuilder] use getRegistersForValue to generate legal copies`](https://reviews.llvm.org/D143961)
* [`Re: [PATCH] powerpc/vmlinux.lds: Add .text.asan/tsan sections`](https://lore.kernel.org/Y%2FZMEqjpT6plg0nR@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: rpm-pkg: remove unneeded KERNELRELEASE from modules/headers_install`](https://lore.kernel.org/Y%2FZMsX2gtnUZXlal@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: modinst: Enable multithread xz compression`](https://lore.kernel.org/Y%2FfJhOME2OAPWILK@dev-arch.thelio-3990X/)
* [`RISC-V: avoid build issues for clang/llvm-17 with binutils 2.35`](https://lore.kernel.org/20230223220546.52879-1-conor@kernel.org/)
* [`Re: [PATCH] hw/mips/gt64xxx_pci: Don't endian-swap GT_PCI0_CFGADDR`](https://lore.kernel.org/Y%2FjqkCQjm6nKlMCs@dev-arch.thelio-3990X/)
* [`[PowerPC] Recognize long CPU name for -mtune in Clang`](https://reviews.llvm.org/D144967)
* [`Re: [PATCH v2] MIPS: Workaround clang inline compat branch issue`](https://lore.kernel.org/Y%2F5UNdcoOvJjG+fH@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`[BOLT] Missing VCSRevision.h dependency`](https://github.com/llvm/llvm-project/issues/60460)
* [`Alpine/Musl segfault when building BOLT in stage 3`](https://github.com/ClangBuiltLinux/tc-build/issues/227)
* [`Fortify source failure in drivers/infiniband/core/cma.c with s390 allmodconfig`](https://github.com/ClangBuiltLinux/linux/issues/1687)
* [`Re: [PATCH v7 2/6] arm64: kernel: move identity map out of .text mapping`](https://lore.kernel.org/Y91NmZUEZWUCNXWz@dev-arch.thelio-3990X/)
* [`Re: [PATCH 6.1 00/28] 6.1.10-rc1 review`](https://lore.kernel.org/Y+AIsz4%2F7Ms28aWK@dev-arch.thelio-3990X/)
* [`Re: [linux-next:master 8959/10357] ld.lld: error: no machine record defined`](https://lore.kernel.org/Y+ARoumDpY4+1aj%2F@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Failure after v6.1.8-105-gf5e58b546cbe: drm/panfrost: fix GENERIC_ATOMIC64 dependency`](https://lore.kernel.org/Y+EuRS3YDD8XbHgo@dev-arch.thelio-3990X/)
* [`Re: x86/include/asm/arch_hweight.h:49:15: error: invalid input size for constraint 'D'`](https://lore.kernel.org/Y+J+UQ1vAKr6RHuH@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 6/6] Driver: VMBus: Add device tree support`](https://lore.kernel.org/Y+O0FtUkLyvJLSrR@dev-arch.thelio-3990X/)
* [`Re: [PATCH qemu v3] x86: don't let decompressed kernel image clobber setup_data`](https://lore.kernel.org/Y+Pf0q6LmQKN+FHo@dev-arch.thelio-3990X/)
* [`static calls on x86_64 are broken with clang conditional tail calls`](https://github.com/ClangBuiltLinux/linux/issues/1774)
* [`x86_64 kernels fail to boot after D140931`](https://github.com/ClangBuiltLinux/linux/issues/1800)
* [`Re: [PATCH -next v2 1/3] driver core: add error handling for devtmpfs_create_node()`](https://lore.kernel.org/Y+rSXg14z1Myd8Px@dev-arch.thelio-3990X/)
* [`Clang THINLTO BTRFS balance dmesg regression with RAID`](https://github.com/ClangBuiltLinux/linux/issues/1804)
* [`'power10' is not a recognized processor for this target (ignoring processor)`](https://github.com/ClangBuiltLinux/linux/issues/1799)
* [`Re: stable-rc: 5.10: riscv: defconfig: clang-nightly: build failed - Invalid or unknown z ISA extension: 'zifencei'`](https://lore.kernel.org/Y%2FVGu9IOJEKi6VwS@dev-arch.thelio-3990X/)
* [`Invalid or unknown z ISA extension: 'zifencei'`](https://github.com/ClangBuiltLinux/linux/issues/1808)
* [`Re: [PATCH] gpu: host1x: fix uninitialized variable use`](https://lore.kernel.org/Y%2FeULFO4jbivQ679@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Disable s390 builds with LLVM < 13 on 5.10`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/510)
* [`Replace Exception with more specific RuntimeError`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/511)
* [`boot-utils: Fix new pylint warnings`](https://github.com/ClangBuiltLinux/boot-utils/pull/89)
* [`Enable KCSAN on stable`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/512)
* [`scripts/markdown-badges.py: Stop relying on parse_version()`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/513)
* [`Add patches for current issues (February 8th, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/515)
* [`Add support for LLVM 16`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/516)
* [`Add script to locally build TuxSuite configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/517)
* [`Update patches (February 23, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/518)
* `ruff` support
    * [`actions-workflows`](https://github.com/ClangBuiltLinux/workflows/pull/6)
    * [`boot-utils`](https://github.com/ClangBuiltLinux/boot-utils/pull/90)
    * [`continuous-integration2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/519)
    * [`tc-build`](https://github.com/ClangBuiltLinux/tc-build/pull/226)
* [`boot-qemu.py: Rewrite using classes`](https://github.com/ClangBuiltLinux/boot-utils/pull/91)
* [`patches: mainline: Add fix for host1x -Wuninitialized warning `](https://github.com/ClangBuiltLinux/continuous-integration2/pull/520)
* [`patches: stable: Drop alternatives series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/521)
* [`Initial stab at front end cache checks`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/522)
* [`Follow up fixes for ruff`](https://github.com/ClangBuiltLinux/boot-utils/pull/92)
* [`Add script to print the number of builds done per week per workflow`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/523)
* [`patches: Drop next series (February 28, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/525)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I ran the February 22nd, 2023 ClangBuiltLinux bi-weekly meeting/sync and [took notes](https://github.com/ClangBuiltLinux/meeting-notes/pull/38).



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
