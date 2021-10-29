---
title: "October 2021 ClangBuiltLinux Work"
date: 2021-10-29T14:10:00-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

This was a bit of a shorter month for me, as I took some vacation at the beginning of the month to recouperate and meet half of my girlfriend's extended family and friends for the first time. Thankfully, the rest of the ClangBuiltLinux team was able to keep everything churning along in my absence and I was able to be super productive the rest of the month once I returned.

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* `-Wbitwise-instead-of-logical`: A new warning in LLVM exposed a few places in the kernel where bitwise operations were being used with boolean expressions with side effects, which may be undesirable (ChromeOS had [a massive bug](https://www.xda-developers.com/cant-login-chromebook-google-readying-fix/) due to this behavior...). Cleaning these up in the kernel was done with this patch and the following six here. Luckily, there were no bugs but getting the kernel free of this warning will allow new instances to pop up clearly.

  * `Input: touchscreen - Avoid bitwise vs logical OR warning` ([`v1`](https://lore.kernel.org/r/20211014205757.3474635-1-nathan@kernel.org/))
  * `drm/i915: Avoid bitwise vs logical OR warning in snb_wm_latency_quirk()` ([`v1`](https://lore.kernel.org/r/20211014211916.3550122-1-nathan@kernel.org/))
  * `staging: wlan-ng: Avoid bitwise vs logical OR warning in hfa384x_usb_throttlefn()` ([`v1`](https://lore.kernel.org/r/20211014215703.3705371-1-nathan@kernel.org/))
  * `platform/x86: thinkpad_acpi: Fix bitwise vs. logical warning` ([`v1`](https://lore.kernel.org/r/20211018182537.2316800-1-nathan@kernel.org/))
  * `nfp: bpf: Fix bitwise vs. logical OR warning` ([`v1`](https://lore.kernel.org/r/20211018193101.2340261-1-nathan@kernel.org/))
  * `lib: zstd: Add cast to silence clang's -Wbitwise-instead-of-logical` ([`v1`](https://lore.kernel.org/r/20211021202353.2356400-1-nathan@kernel.org/))
  * `soc/tegra: fuse: Fix bitwise vs. logical OR warning` ([`v1`](https://lore.kernel.org/r/20211021214500.2388146-1-nathan@kernel.org/))

* `-Wenum-conversion`: A simple implicit conversion between two different enumerated types, which can be a bug if the two values are not mapped in a one to one manner. Thankfully, the one instance of this warning that cropped up this month was not a true bug because the two enums had the same value but it was simple enough to clean up to keep the kernel warning free.

  * `regulator: lp872x: Remove lp872x_dvs_state` ([`v1`](https://lore.kernel.org/r/20211019004335.193492-1-nathan@kernel.org/))

* `-Wimplicit-fallthrough`: I would argue one of the biggest deficiencies of C as a language is implicit fallthroughs in switch statements. There has been [a lot of work done](https://github.com/KSPP/linux/issues/115) by Gustavo A.R. Silva to clean these up for both GCC and clang because they disagree in [one specific case](https://github.com/ClangBuiltLinux/linux/issues/636). These patches help keep the warning free of new instances in linux-next so that this warning can be turned on for clang in 5.16.

  * `ice: Fix clang -Wimplicit-fallthrough in ice_pull_qvec_from_rc()` ([`v1`](https://lore.kernel.org/r/20211019014203.1926130-1-nathan@kernel.org/))
  * `net: ax88796c: Fix clang -Wimplicit-fallthrough in ax88796c_set_mac()` ([`v1`](https://lore.kernel.org/r/20211025211238.178768-1-nathan@kernel.org/))
  * `ASoC: qdsp6: audioreach: Fix clang -Wimplicit-fallthrough` ([`v1`](https://lore.kernel.org/r/20211027190823.4057382-1-nathan@kernel.org/)):

* `-Winfinite-recursion`: This is a super interesting warning and the first time I have ever seen an instance of this warning in the kernel since I started building with `clang` a little over three years ago. In the kernel, it is common to have a function name then the same function name with two underscores before it (e.g. `foo()` and `__foo()`) to signify that `foo()` holds a lock while `__foo()` does not so that more code can be reused. In this particular instance, `__foo()` was created but the newly created `foo()` was not updated properly.

  * `iio: frequency: adrf6780: Fix adrf6780_spi_{read,write}()` ([`v1`](https://lore.kernel.org/r/20211022195656.1513147-1-nathan@kernel.org/))

* `-Wpointer-bool-conversion`: This warning points out that arrays in structures cannot be `NULL` when it is not the first member in said structure so the check does not do anything. The author may have intended to have a different check (such as the first element of the array being 0) or the check can just be removed. In this case, it was the latter.

  * `net: ax88796c: Remove pointless check in ax88796c_open()` ([`v1`](https://lore.kernel.org/r/20211025211238.178768-2-nathan@kernel.org/))

* `-Wuninitialized`/`-Wsometimes-uninitialized`: Unfortunately, because the kernel is written in C, uninitialized variables continue to be an issue, especially since GCC's `-Wmaybe-uninitialized` has been [disabled by default since 5.7](https://git.kernel.org/linus/78a5255ffb6a1af189a83e493d916ba1c54d8c75). These will continue to pop up until GCC can be fixed but it does not seem like that will occur anytime soon as it has been a deficiency for a long time; until then, we will continue to fix them.

  * `mm/memory_failure: Initialize extra_pins in me_pagecache_clean()` ([`v1`](https://lore.kernel.org/r/20211021180336.2328086-1-nathan@kernel.org/))
  * `drm/msm/dpu: Remove commit and its uses in dpu_crtc_set_crc_source()` ([`v1`](https://lore.kernel.org/r/20211026142435.3606413-1-nathan@kernel.org/))
  * `net/mlx5: Add esw assignment back in mlx5e_tc_sample_unoffload()` ([`v1`](https://lore.kernel.org/r/20211027153122.3224673-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kasan: Always respect CONFIG_KASAN_STACK`](https://lore.kernel.org/r/YV0NPnUbElw7cTRH@archlinux-ax161/)

* [`Re: [PATCH] compiler_types: mark __compiletime_assert failure as __noreturn`](https://lore.kernel.org/r/YWhoQ2lKeOx2U2e2@archlinux-ax161/)

* [`Re: [PATCH] KVM: x86: avoid warning with -Wbitwise-instead-of-logical`](https://lore.kernel.org/r/YWmuPOB6%2FrXWqXBH@archlinux-ax161/)

* [`Re: [PATCH 3/3] arm64: vdso32: require CROSS_COMPILE_COMPAT for gcc+bfd`](https://lore.kernel.org/r/YW38KOA8ct12F7IG@archlinux-ax161/)

* [`[PATCH v2 0/4] compat vdso cleanups`](https://lore.kernel.org/r/20211019223646.1146945-1-ndesaulniers@google.com/)

* [`[PATCH 0/2] gcc-plugins: Explicitly document purpose and deprecation schedule`](https://lore.kernel.org/r/20211020173554.38122-1-keescook@chromium.org/)

* [`Re: [PATCH] compiler-gcc.h: Define __SANITIZE_ADDRESS__ under hwaddress sanitizer`](https://lore.kernel.org/r/YXCRPsNl2Vlgd7ct@archlinux-ax161/)

* [`Re: [PATCH] dma-buf: fix uninitialized variable usage in selftests`](https://lore.kernel.org/r/YXc0KlqXvm5W61ee@archlinux-ax161/)

* [`Re: [PATCH] dma-buf: st: fix error handling in test_get_fences()`](https://lore.kernel.org/r/YXgK0HAe1+dEPvL1@archlinux-ax161/)

* [`[X86] Implement -fzero-call-used-regs option`](https://reviews.llvm.org/D110869)

* [`[ARM] Use hardware TLS register in Thumb2 mode when -mtp=cp15 is passed`](https://reviews.llvm.org/D112600)

* [`Re: [PATCH] ARM: Thumb2: avoid __builtin_thread_pointer() on Clang`](https://lore.kernel.org/r/YXrQnQNRGaE7anWy@archlinux-ax161/)

* [`Re: [PATCH] kbuild: Support clang-$ver builds`](https://lore.kernel.org/r/YXrhZoOgv5dtFMTs@archlinux-ax161/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [Intel-gfx] [vfio:next 33/38] drivers/gpu/drm/i915/i915_pci.c:975:2: warning: missing field 'override_only' initializer`](https://lore.kernel.org/r/YVdJS5RUjvokrSnn@archlinux-ax161/)

* [`Re: [v2 PATCH] crypto: api - Fix built-in testing dependency failures`](https://lore.kernel.org/r/YVdNFzs8HUQwHa54@archlinux-ax161/)

* [`Re: How to check my LLVM toolchain is optimized for PGO and/or ThinLTO?`](https://lore.kernel.org/r/YV0REIpkY6TkuGA+@archlinux-ax161/)

* [`Re: [tip:sched/core 14/47] kernel/sched/fair.c:893:22: error: variable 'p' set but not used`](https://lore.kernel.org/r/YWew3ItdPC5QrL%2Fw@archlinux-ax161/)

* [`Re: [BUG] [5.15] Compilation error in arch/x86/kvm/mmu/spte.h with clang-14`](https://lore.kernel.org/r/YWht7v%2F1RuAiHIvC@archlinux-ax161/)

* [`LTO crash related to fortify`](https://github.com/ClangBuiltLinux/linux/issues/1477)

* [`Re: [PATCH 4/4] block: cleanup the flush plug helpers`](https://lore.kernel.org/r/YXMaZoQJiR5WFZTw@archlinux-ax161/)

* [`Re: i386: tinyconfig: perf_event.h:838:21: error: invalid output size for constraint '=q'`](https://lore.kernel.org/r/YXbCFdz4R7cikpnE@archlinux-ax161/)

* [`ModuleNotFoundError: No module named 'attr'`](https://gitlab.com/Linaro/tuxsuite/-/issues/141)

* [`Apply 091bb549f7722723b284f63ac665e2aedcf9dec9 to 4.19`](https://lore.kernel.org/r/YXl98bQRrJvVKdwC@archlinux-ax161/)



## Tooling improvements

* [`patches: Drop RDMA patch from mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/218)

* [`Update patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/219)

* [`actions-workflows: Initial shfmt action and workflow`](https://github.com/ClangBuiltLinux/actions-workflows/pull/1)

* [`Fetch metadata.json and use it to display clang checkout date and hash`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/224)

* [`patches: Remove -next folder`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/225)

* [`github: Add shellcheck and yapf workflows`](https://github.com/ClangBuiltLinux/actions-workflows/pull/2)

* [`github: Switch to shared lint workflows`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/227)

* [`ci.sh: Build release/13.x`](https://github.com/ClangBuiltLinux/tc-build/pull/173)

* [`Refresh patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/228)

* [`build-llvm.py: Add '--llvm-folder'`](https://github.com/ClangBuiltLinux/tc-build/pull/174)

* [`patches: android-mainline: Remove ARM futex patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/229)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 3 and 4, HP desktop, ASUS laptop, and Hyper-V and VMware platforms on my workstation. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* While researching an old issue, I noticed that some of the links were dead. As a result, I spent some time going through [our issue tracker](https://github.com/ClangBuiltLinux/linux/issues) to replace all links to other archive sites (such as [lkml.org](https://lkml.org/), [spinics.net](https://www.spinics.net/), and [marc.info](https://marc.info/)) with canonical links to [lore.kernel.org](https://lore.kernel.org/), as those links are [expected to be stable and not disappear](https://www.kernel.org/lore.html); even if they did, the message ID is in the URL so it can be looked up across the internet, rather than some arbitrary URL. There is nothing worse than looking into an old issue and having a dead link (I always think of [this xkcd](https://xkcd.com/979/)). There is a lot of mailing list discussions that come up later so being able to access that is of the utmost importance.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).