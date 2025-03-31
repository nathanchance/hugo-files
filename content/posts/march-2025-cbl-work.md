---
title: March 2025 ClangBuiltLinux Work
date: 2025-03-31T16:30:00-0700
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

  * `media: platform: synopsys: hdmirx: Fix 64-bit division for 32-bit targets` ([`v1`](https://lore.kernel.org/20250306-synopsys-hdmirx-fix-64-div-v1-1-dd5ff38bba5e@kernel.org/))
  * `x86/kbuild/64: Restrict clang versions that can use '-march=native'` ([`v1`](https://lore.kernel.org/20250324-x86-march-native-clang-19-and-newer-v1-1-3a05ed32a89e@kernel.org/))
  * `Add wcslen()` ([`v1`](https://lore.kernel.org/20250325-string-add-wcslen-for-llvm-opt-v1-0-b8f1e2c17888@kernel.org/), [`v2`](https://lore.kernel.org/20250326-string-add-wcslen-for-llvm-opt-v2-0-d864ab2cbfe4@kernel.org/), [`v3`](https://lore.kernel.org/20250328-string-add-wcslen-for-llvm-opt-v3-0-a180b4c0c1c4@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `ARM: Fix ARM_VECTORS with CONFIG_LD_DEAD_CODE_DATA_ELIMINATION` ([`v1`](https://lore.kernel.org/20250311-arm-fix-vectors-with-linker-dce-v1-0-ec4c382e3bfd@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `drm/appletbdrm: Fix format specifier for size_t variables` ([`v1`](https://lore.kernel.org/20250304-appletbdrm-fix-size_t-specifier-v1-1-94fe1d2c91f8@kernel.org/))
  * `crypto: tegra - Fix format specifier in tegra_sha_prep_cmd()` ([`v1`](https://lore.kernel.org/20250313-crypto-tegra-fix-format-specifier-for-size_t-v1-1-0b957726c9e5@kernel.org/))
  * `x86/tools: Drop unlikely definition from insn_decoder_test` ([`v1`](https://lore.kernel.org/20250318-x86-decoder-test-fix-unlikely-redef-v1-1-74c84a7bf05b@kernel.org/))
  * `i3c: master: svc: Fix implicit fallthrough in svc_i3c_master_ibi_work()` ([`v1`](https://lore.kernel.org/20250319-i3c-fix-clang-fallthrough-v1-1-d8e02be1ef5c@kernel.org/))



## LLVM patches

* [`[ELF] Allow KEEP within OVERLAY`](https://github.com/llvm/llvm-project/pull/130661)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kbuild: clang: Support building UM with SUBARCH=i386`](https://lore.kernel.org/20250304102536.GB2529736@ax162/)
* [`Re: [PATCH] hardening: Enable i386 FORTIFY_SOURCE on Clang 16+`](https://lore.kernel.org/20250304101445.GA2529736@ax162/)
* [`Re: [PATCH] kunit/stackinit: Use fill byte different from Clang i386 pattern`](https://lore.kernel.org/20250305151251.GA1538566@ax162/)
* [`Re: [PATCH v2 1/2] x86/build: Remove -ffreestanding on i386 with GCC`](https://lore.kernel.org/20250308091746.GA707537@ax162/)
* [`Re: [PATCH v2 2/2] hardening: Enable i386 FORTIFY_SOURCE on Clang 16+`](https://lore.kernel.org/20250308091911.GB707537@ax162/)
* [`Re: [PATCH] kbuild: deb-pkg: remove "version" variable in mkdebian`](https://lore.kernel.org/20250311191900.GB1958594@ax162/)
* [`Re: [PATCH] kbuild: deb-pkg: fix versioning for -rc releases`](https://lore.kernel.org/20250311192036.GC1958594@ax162/)
* [`Re: [PATCH] kbuild: pacman-pkg: hardcode module installation path`](https://lore.kernel.org/20250317170752.GA830988@ax162/)
* [`Re: [PATCH v2] kbuild: make all file references relative to source root`](https://lore.kernel.org/20250317171738.GA950672@ax162/)
* [`Re: [tip: x86/core] x86/ibt: Implement FineIBT-BHI mitigation`](https://lore.kernel.org/20250319000427.GA2617458@ax162/)
* [`Re: [PATCH] platform: cznic: fix function parameter names`](https://lore.kernel.org/20250325155906.GA1886499@ax162/)
* [`Re: [GIT PULL] asm-generic changes for 6.15`](https://lore.kernel.org/20250326221848.GA14731@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`objtool warning in drivers/bluetooth/hci_vhci.o around vhci_coredump_hdr()`](https://github.com/ClangBuiltLinux/linux/issues/2073)
* [`Re: [PATCH 6.13 070/157] objtool: Remove annotate_{,un}reachable()`](https://lore.kernel.org/20250307212625.GA2700602@ax162/)
* [`Re: [PATCH] hardening: Enable i386 FORTIFY_SOURCE on Clang 16+`](https://lore.kernel.org/20250307214734.GA2871848@ax162/)
* [`Re: [PATCH V2 2/2] ASoC: codecs: Add aw88166 amplifier driver`](https://lore.kernel.org/20250309204211.GA960576@ax162/)
* [`__read_overflow not eliminated in lib/fortify_kunit.c after 47f4af43e7c0cf702d6a6321542f0c0d9c4216e3`](https://github.com/ClangBuiltLinux/linux/issues/2075)
* [`Re: [thomas-weissschuh:b4/module-hashes 9/9] llvm-objcopy: error: '.tmp_module_hashes.o': The file was not recognized as a valid object file`](https://lore.kernel.org/20250311194940.GA1966726@ax162/)
* [`Re: [PATCH v7 1/1] watchdog: aspeed: Update bootstatus handling`](https://lore.kernel.org/20250312202132.GA1072616@ax162/)
* [`Re: [PATCH V8 2/6] perf: attach/detach PMU specific data`](https://lore.kernel.org/20250313150902.GA2418865@ax162/)
* [`Re: [PATCH v2 0/6] btrfs: prepare for larger folios support`](https://lore.kernel.org/20250313152124.GA2420634@ax162/)
* [`Re: [PATCH v2 05/14] ASoC: amd: acp: Refactor acp machine select`](https://lore.kernel.org/20250313155255.GA2477963@ax162/)
* [`Re: [PATCH] kunit/fortify: Replace "volatile" with OPTIMIZER_HIDE_VAR()`](https://lore.kernel.org/20250317174840.GA1451320@ax162/)
* [`Re: [PATCH 5/6] mips: drop GENERIC_IOMAP wrapper`](https://lore.kernel.org/20250318203906.GA4089579@ax162/)
* [`__read_overflow in drivers/net/wireless/intel/iwlwifi/mld/d3.c with UBSAN_BOUNDS`](https://github.com/ClangBuiltLinux/linux/issues/2076)
* [`__write_overflow_field in arch/x86/kernel/apic/io_apic.c with UBSAN_INTEGER_WRAP`](https://github.com/ClangBuiltLinux/linux/issues/2077)
* [`Re: [PATCH v4 net-next 2/2] udp_tunnel: use static call for GRO hooks when possible`](https://lore.kernel.org/20250321041612.GA2679131@ax162/)
* [`Re: [PATCH v2 11/13] arch, mm: streamline HIGHMEM freeing`](https://lore.kernel.org/20250323190647.GA1009914@ax162/)
* [`Re: [PATCH v4 3/3] iommu: Drop sw_msi from iommu_domain`](https://lore.kernel.org/20250324162558.GA198799@ax162/)
* [`Re: [PATCH] rwonce: handle KCSAN like KASAN in read_word_at_a_time()`](https://lore.kernel.org/20250326203926.GA10484@ax162/)
* [`Compilation failure with flag -mlvi-cfi`](https://github.com/ClangBuiltLinux/linux/issues/2079#issuecomment-2758605341)
* [`Re: [tip:objtool/urgent 9/23] vmlinux.o: warning: objtool: cdns_mrvl_xspi_setup_clock: unexpected end of section .text.cdns_mrvl_xspi_setup_clock`](https://lore.kernel.org/20250328232449.GA2955081@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Adjust Alpine Linux configuration URLs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/820)
* [`Update stable anchor to 6.14`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/821)
* [`patches: Drop android15-6.6 (March 27, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/822)
* [`Update PGO kernel to 6.14 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/298)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [20.1.0](https://lore.kernel.org/20250305202652.GA2791050@ax162/)
  * [20.1.1](https://lore.kernel.org/20250319154242.GA3599324@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
