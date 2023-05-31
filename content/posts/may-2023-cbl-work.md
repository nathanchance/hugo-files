---
title: May 2023 ClangBuiltLinux Work
date: 2023-05-31T16:30:00-0700
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

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `clk: sp7021: Adjust width of _m in HWM_FIELD_PREP()` ([`v1`](https://lore.kernel.org/20230501-sp7021-field_prep-warning-v1-1-5b36d71feefe@kernel.org/))
  * `drm/i915: Fix clang -Wimplicit-fallthrough in intel_async_flip_check_hw()` ([`v1`](https://lore.kernel.org/20230524-intel_async_flip_check_hw-implicit-fallthrough-v1-1-83de89e376a1@kernel.org/))
  * `drm/amdgpu: Fix return types of certain NBIOv7.9 callbacks` ([`v1`](https://lore.kernel.org/20230524-nbio_v7_9-wincompatible-function-pointer-types-strict-v1-1-da17e01907cf@kernel.org/))
  * `x86/csum: Fix clang -Wuninitialized in csum_partial()` ([`v1`](https://lore.kernel.org/20230526-csum_partial-wuninitialized-v1-1-ebc0108dcec1@kernel.org/))
  * `drm/i915/gt: Fix recent kCFI violations` ([`v1`](https://lore.kernel.org/20230530-i915-gt-cache_level-wincompatible-function-pointer-types-strict-v1-0-54501d598229@kernel.org/))
  * `drm/i915/pxp: Fix size_t format specifier in gsccs_send_message()` ([`v1`](https://lore.kernel.org/20230530-i915-pxp-size_t-wformat-v1-1-9631081e2e5b@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] Compiler Attributes: Add __counted_by macro`](https://lore.kernel.org/20230504211827.GA1666363@dev-arch.thelio-3990X/)
* [`Re: [PATCH v1 1/1] scripts/tags.sh: Fix gtags generation for O= kernel builds`](https://lore.kernel.org/20230504213246.GB1666363@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/4] powerpc/64: Force ELFv2 when building with LLVM linker`](https://lore.kernel.org/20230505213940.GA1337526@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] Compiler Attributes: Add __counted_by macro`](https://lore.kernel.org/20230523174913.GB1388474@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/amdgpu: Mark mmhub_v1_8_mmea_err_status_reg as __maybe_unused`](https://lore.kernel.org/20230525152247.GA187374@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 3/4] perf/core: Remove pmu linear searching code`](https://lore.kernel.org/20230525155530.GA546949@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] drm/amd/display: enable more strict compile checks`](https://lore.kernel.org/20230525162607.GA550162@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/amdkfd: remove unused function get_reserved_sdma_queues_bitmap`](https://lore.kernel.org/20230525201439.GA2741545@dev-arch.thelio-3990X/)
* [`[Lex] Only warn on defining or undefining language-defined builtins`](https://reviews.llvm.org/D151741)
* [`Re: [PATCH] s390/purgatory: Do not use fortified string functions`](https://lore.kernel.org/20230531143342.GA2250333@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: add $(CLANG_CFLAGS) to KBUILD_CPPFLAGS`](https://lore.kernel.org/20230531213319.GA2201875@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [GIT PULL]: Generic phy updates for v6.4`](https://lore.kernel.org/20230504150624.GA1000378@dev-arch.thelio-3990X/)
* [`[InstCombine] Improve bswap optimization`](https://reviews.llvm.org/D149699#4325562)
* [`Re: [stable:linux-5.15.y 36/9999] arch/s390/kernel/vtime.c:132:2: error: expected absolute expression`](https://lore.kernel.org/20230523174525.GA1388474@dev-arch.thelio-3990X/)
* [`Re: [PATCH v5 2/3] drm/panel: Add Samsung S6D7AA0 panel controller driver`](https://lore.kernel.org/20230523180212.GA1401867@dev-arch.thelio-3990X/)
* [`clang-13 arm allmodconfig compiler crash: llvm::DataLayout::getStructLayout(llvm::StructType*) const`](https://github.com/ClangBuiltLinux/linux/issues/1850)
* [`-Warray-bounds in drivers/scsi/aacraid/commsup.c`](https://github.com/ClangBuiltLinux/linux/issues/1851)
* [`Re: [PATCH v4 3/4] perf/core: Remove pmu linear searching code`](https://lore.kernel.org/20230524214133.GA2359762@dev-arch.thelio-3990X/)
* [`Re: [stable:linux-4.19.y 984/6017] ld.lld: error: drivers/usb/dwc2/platform.o: contains a compressed section, but zlib is not available`](https://lore.kernel.org/20230526152458.GA395537@dev-arch.thelio-3990X/)
* [`failed assertion in clang-14: @switch_mm_irqs_off`](https://github.com/ClangBuiltLinux/linux/issues/1853)
* [`New instances of -Wbuiltin-macro-redefined after LLVM commit 0123deb3a6f0`](https://github.com/ClangBuiltLinux/linux/issues/1855)
* [`-Wincompatible-library-redeclaration`](https://github.com/ClangBuiltLinux/linux/issues/1856)
* [`__write_overflow_field in drivers/crypto/marvell/cesa/cipher.c`](https://github.com/ClangBuiltLinux/linux/issues/1858)
* [`arm64 big endian broken in vDSO after LLVM commit d81ce04587c00`](https://github.com/ClangBuiltLinux/linux/issues/1859)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Add PowerPC allmodconfig build for mainline with LLVM 15+`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/562)
* [`boot-qemu.py: Add ppc64 big endian ELFv2 file strings to file_rosetta`](https://github.com/ClangBuiltLinux/boot-utils/pull/101)
* [`Update default PGO kernel to 6.3.0 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/235)
* [`utils: Expand relative paths in get_full_kernel_path()`](https://github.com/ClangBuiltLinux/boot-utils/pull/104)
* [`Move images to GitHub releases`](https://github.com/ClangBuiltLinux/boot-utils/pull/105)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I built and uploaded LLVM [16.0.4](https://lore.kernel.org/20230523231250.GA894339@dev-arch.thelio-3990X/) to kernel.org.


## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
