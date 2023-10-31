---
title: October 2023 ClangBuiltLinux Work
date: 2023-10-31T16:15:00-0700
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

  * `eventfs: Use ERR_CAST() in eventfs_create_events_dir()` ([`v1`](https://lore.kernel.org/20231018-ftrace-fix-clang-randstruct-v1-1-338cb214abfb@kernel.org/))
  * `tcp: Fix -Wc23-extensions in tcp_options_write()` ([`v1`](https://lore.kernel.org/20231031-tcp-ao-fix-label-in-compound-statement-warning-v1-1-c9731d115f17@kernel.org/))

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * [`CHROMIUM: Revert "drm/mediatek: Fix backport issue in mtk_drm_gem_prime_vmap()"`](https://chromium-review.googlesource.com/c/chromiumos/third_party/kernel/+/4995831)

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `kbuild: Disable clang's -Wformat-{overflow,truncation}-non-kprintf` ([`v1`](https://lore.kernel.org/20231002-disable-wformat-truncation-overflow-non-kprintf-v1-1-35179205c8d9@kernel.org/))
  * `kbuild: Enable -Wincompatible-function-pointer-types-strict in W=1` ([`v1`](https://lore.kernel.org/20231002-enable-wincompatible-function-pointer-types-strict-w-1-v1-1-808ab955d42d@kernel.org/))
  * `RISC-V: build: Allow LTO to be selected` ([`v3`](https://lore.kernel.org/20231003-riscv-lto-v3-1-8aca61a4ecb4@kernel.org/))
  * `drm/amd/display: Respect CONFIG_FRAME_WARN=0 in DML2` ([`v1`](https://lore.kernel.org/20231018-amdgpu-dml2-respect-frame_warn-v1-1-43ec8d153743@kernel.org/))
  * `arm64: Restrict CPU_BIG_ENDIAN to GNU as or LLVM IAS 15.x or newer` ([`v1`](https://lore.kernel.org/20231025-disable-arm64-be-ias-b4-llvm-15-v1-1-b25263ed8b23@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply a1e2c031ec39 and e32683c6f7d2 to 5.15 and earlier`](https://lore.kernel.org/20231027160144.GA232578@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `vfio/cdx: Add parentheses between bitwise AND expression and logical NOT` ([`v1`](https://lore.kernel.org/20231002-vfio-cdx-logical-not-parentheses-v1-1-a8846c7adfb6@kernel.org/))
  * `Fix a couple recent instances of -Wincompatible-function-pointer-types-strict from ->mode_get() implementations` ([`v1`](https://lore.kernel.org/20231002-net-wifpts-dpll_mode_get-v1-0-a356a16413cf@kernel.org/))
  * `um: net: Fix return type of uml_net_start_xmit()` ([`v1`](https://lore.kernel.org/20231003-um-net-wifpts-v1-1-02888c634ee7@kernel.org/))
  * `OPP: Fix -Wunsequenced in _of_add_opp_table_v1()` ([`v1`](https://lore.kernel.org/20231005-opp-of-wunsequenced-v1-1-778815980a8d@kernel.org/))
  * `scsi: ibmvfc: Use 'unsigned int' for single-bit bitfields in 'struct ibmvfc_host'` ([`v1`](https://lore.kernel.org/20231010-ibmvfc-fix-bitfields-type-v1-1-37e95b5a60e5@kernel.org/))
  * `ASoC: tegra: Fix -Wuninitialized in tegra210_amx_platform_probe()` ([`v1`](https://lore.kernel.org/20231011-asoc-tegra-fix-uninit-soc_data-v1-1-0ef0ab44cf48@kernel.org/))
  * `remoteproc: st: Fix sometimes uninitialized ret in st_rproc_probe()` ([`v1`](https://lore.kernel.org/20231012-st_remoteproc-fix-sometimes-uninit-v1-1-f64d0f2d5b37@kernel.org/))
  * `PCI: rcar-gen4: Fix type of type parameter in rcar_gen4_pcie_ep_raise_irq()` ([`v1`](https://lore.kernel.org/20231017-pcie-rcar-wifpts-v1-1-ab1f42bf9386@kernel.org/))
  * `RDMA/bnxt_re: Fix clang -Wimplicit-fallthrough in bnxt_re_handle_cq_async_error()` ([`v1`](https://lore.kernel.org/20231020-bnxt_re-implicit-fallthrough-v1-1-b5c19534a6c9@kernel.org/))
  * `ASoC: codecs: aw88399: Fix -Wuninitialized in aw_dev_set_vcalb()` ([`v1`](https://lore.kernel.org/20231027-asoc-aw88399-fix-wuninitialized-v1-1-b1044493e4cd@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] x86/boot: Move x86_cache_alignment initialization to correct spot`](https://lore.kernel.org/20231002222402.GA486933@dev-arch.thelio-3990X/)
* [`Re: [PATCH] gen_compile_commands: use raw string when replacing \#`](https://lore.kernel.org/20231002230709.GA1029006@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: rpm-build: generate kernel.spec in rpmbuild/SPECS/`](https://lore.kernel.org/20231002230903.GB1029006@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: remove stale code for 'source' symlink in packaging scripts`](https://lore.kernel.org/20231002231256.GA1269812@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: make binrpm-pkg always produce kernel-devel package`](https://lore.kernel.org/20231002231551.GB1269812@dev-arch.thelio-3990X/)
* [`Re: [PATCH rebased] kbuild: rpm-pkg: Fix build with non-default MODLIB`](https://lore.kernel.org/20231006165815.GA3359308@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/2] drm/i915: drop -Wall and related disables from cflags as redundant`](https://lore.kernel.org/20231006170231.GB3359308@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] drm/i915: enable W=1 warnings by default`](https://lore.kernel.org/20231006174550.GC3359308@dev-arch.thelio-3990X)
* [`Re: [PATCH 5/5] kbuild: unify no-compiler-targets and no-sync-config-targets`](https://lore.kernel.org/20231009164424.GB1153868@dev-arch.thelio-3990X/)
* [`Re: [tip: x86/bugs] x86/retpoline: Ensure default return thunk isn't used at runtime`](https://lore.kernel.org/20231017153222.GA707258@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] clk: ralink: mtmips: quiet unused variable warning`](https://lore.kernel.org/20231017155257.GA710773@dev-arch.thelio-3990X/)
* [`Re: [PATCH] lib/Kconfig.debug: disable FRAME_WARN for kasan and kcsan`](https://lore.kernel.org/20231019155600.GB60597@dev-arch.thelio-3990X/)
* [`Re: [PATCH 04/10] modpost: remove more symbol patterns from the section check whitelist`](https://lore.kernel.org/20231023232814.GA3514685@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: dummy-tools: pretend we understand -fpatchable-function-entry`](https://lore.kernel.org/20231031160922.GA995893@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v2 2/2] x86/sev-es: Only set x86_virt_bits to correct value`](https://lore.kernel.org/20231002200426.GA4127272@dev-arch.thelio-3990X/)
* [`Re: [PATCH 8/8] crypto: cbc - Convert from skcipher to lskciphe`](https://lore.kernel.org/20231002202522.GA4130583@dev-arch.thelio-3990X/)
* [`Re: [PATCH 09/21] block: Add checks to merging of atomic write`](https://lore.kernel.org/20231002225048.GA558147@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] vfs: shave work on failed file open`](https://lore.kernel.org/20231003164505.GA624737@dev-arch.thelio-3990X/)
* [`RISC-V linker relaxation breaks booting with LTO`](https://github.com/ClangBuiltLinux/linux/issues/1942)
* [`Re: [PATCH] mm: hugetlb: Only prep and add allocated folios for non-gigantic pages`](https://lore.kernel.org/20231012000327.GA1855399@dev-arch.thelio-3990X/)
* [`Re: [tip: x86/bugs] x86/retpoline: Ensure default return thunk isn't used at runtime`](https://lore.kernel.org/20231016211040.GA3789555@dev-arch.thelio-3990X/)
* [`Re: [PATCH 0/2] Reduce stack size for DML2`](https://lore.kernel.org/20231017172231.GA2348194@dev-arch.thelio-3990X/)
* [`[VPlan] Insert Trunc/Exts for reductions directly in VPlan.`](https://github.com/llvm/llvm-project/commit/fd311126349b8fe1684d62154a9fa5a7bbb0b713#commitcomment-130346081)
* [`arm64 big endian boot failure with LLVM 13 and 14 after -next commit 34f66c4c4d55`](https://github.com/ClangBuiltLinux/linux/issues/1948)
* [`"relocation refers to a symbol in a discarded section" on older Android branches`](https://github.com/ClangBuiltLinux/linux/issues/1949)
* [`Re: [PATCH] mm: cma: report correct node id`](https://lore.kernel.org/20231025163703.GA2440148@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Adjust distribution components style and fix building with PGO`](https://github.com/ClangBuiltLinux/tc-build/pull/245)
* [`Drop 5.15 fix up patch for drivers/interconnect/core.c`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/640)
* [`python_lint: Disable pylint R0801`](https://github.com/ClangBuiltLinux/actions-workflows/pull/8)
* [`patches: next: Remove COUNT_ARGS() implementation patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/641)
* [`Drop 5.10 drm/mediatek fixup patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/643)
* [`tc_build: llvm: Fix distribution_components with disabled projects`](https://github.com/ClangBuiltLinux/tc-build/pull/246)
* [`build-llvm.py: Add '--build-targets'`](https://github.com/ClangBuiltLinux/tc-build/pull/247)
* [`Disable arm64 big endian builds with LLVM 13 and 14`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/645)
* [`patches: Add backports for new ld.lld issue in Android`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/646)
* [`Update stable anchor to 6.6`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/647)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload stable LLVM releases to kernel.org to ensure kernel developers have easy access to LLVM for reproducing and fixing issues that they introduce.

  * [LLVM 17.0.2](https://lore.kernel.org/20231003180323.GA419007@dev-arch.thelio-3990X/)
  * [LLVM 17.0.3](https://lore.kernel.org/20231017184415.GA1667503@dev-arch.thelio-3990X/)
  * [LLVM 17.0.4](https://lore.kernel.org/20231031184454.GA1258100@dev-arch.thelio-3990X/)

* I attended the 2023 US LLVM Developers Meeting in Santa Clara, CA to interact with the greater LLVM community and talk about issues that the kernel is facing.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
