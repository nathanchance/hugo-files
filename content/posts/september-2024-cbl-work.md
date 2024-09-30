---
title: September 2024 ClangBuiltLinux Work
date: 2024-09-30T15:15:00+0100
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

  * `can: rockchip_canfd: Use div_s64() in rkcanfd_timestamp_init()` ([`v1`](https://lore.kernel.org/20240909-rockchip-canfd-clang-div-libcall-v1-1-c6037ea0bb2b@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `spi: Revert "spi: Insert the missing pci_dev_put()before return"` ([`v1`](https://lore.kernel.org/20240902-spi-revert-8a0ec8c2d736-v1-1-928b829fed2b@kernel.org/))
  * `KVM: x86: Avoid clang -Wimplicit-fallthrough in kvm_vm_ioctl_check_extension()` ([`v1`](https://lore.kernel.org/20240905-kvm-x86-avoid-clang-implicit-fallthrough-v1-1-f2e785f1aa45@kernel.org/))
  * `can: rockchip_canfd: fix return type of rkcanfd_start_xmit()` ([`v1`](https://lore.kernel.org/20240906-rockchip-canfd-wifpts-v1-1-b1398da865b7@kernel.org/))
  * `iio: bmi323: Fix array reference in bmi323_core_runtime_suspend()` ([`v1`](https://lore.kernel.org/20240909-iio-bmi323-fix-array-ref-v1-1-51c220f22229@kernel.org/))
  * `iio: bmi323: Drop CONFIG_PM guards around runtime functions` ([`v1`](https://lore.kernel.org/20240910-iio-bmi323-remove-config_pm-guards-v1-1-0552249207af@kernel.org/))
  * `x86/resctrl: Annotate get_mem_config() functions as __init` ([`v2`](https://lore.kernel.org/20240913-x86-restctrl-get_mem_config_intel-init-v2-1-bf4b645f0246@kernel.org/), [`v3`](https://lore.kernel.org/20240917-x86-restctrl-get_mem_config_intel-init-v3-1-10d521256284@kernel.org/))
  * `LoongArch: KVM: Ensure ret is always initialized in kvm_eiointc_{read,write}()` ([`v1`](https://lore.kernel.org/20240916-loongarch-kvm-eiointc-fix-sometimes-uninitialized-v1-1-85142dcb2274@kernel.org/))
  * `RDMA/nldev: Add missing break in rdma_nl_notify_err_msg()` ([`v1`](https://lore.kernel.org/20240916-rdma-fix-clang-fallthrough-nl_notify_err_msg-v1-1-89de6a7423f1@kernel.org/))
  * `hardening: Adjust dependencies in selection of MODVERSIONS` ([`v1`](https://lore.kernel.org/20240928-fix-randstruct-modversions-kconfig-warning-v1-1-27d3edc8571e@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] drm: enable warnings on unused static inlines`](https://lore.kernel.org/20240904223030.GA1944054@thelio-3990X/)
* [`Re: [PATCH 0/7] ASoC: mt8365: Fix -Werror builds`](https://lore.kernel.org/20240909154839.GA59917@thelio-3990X/)
* [`Re: [PATCH 0/8] drm: fix and enable warnings on unused static inlines`](https://lore.kernel.org/20240911174654.GA2209716@thelio-3990X/)
* [`Re: [PATCH 1/3] btf: remove redundant CONFIG_BPF test in scripts/link-vmlinux.sh`](https://lore.kernel.org/20240911210241.GA2305132@thelio-3990X/)
* [`Re: [PATCH 2/3] btf: move pahole check in scripts/link-vmlinux.sh to lib/Kconfig.debug`](https://lore.kernel.org/20240911210848.GA2659844@thelio-3990X/)
* [`Re: [PATCH 3/3] btf: require pahole 1.21+ for DEBUG_INFO_BTF with default DWARF version`](https://lore.kernel.org/20240911211017.GB2659844@thelio-3990X/)
* [`[HEXAGON] AddrModeOpt support for HVX and optimize adds`](https://github.com/llvm/llvm-project/pull/106368#issuecomment-2350704547)
* [`Re: [PATCH] sysctl: avoid spurious permanent empty tables`](https://lore.kernel.org/20240913234049.GA1539142@thelio-3990X/)
* [`Re: [PATCH] kbuild: rpm-pkg: include vmlinux debug symbols`](https://lore.kernel.org/20240917132848.GA2357574@thelio-3990X/)
* [`Re: [PATCH] driver core: fix async device shutdown hang`](https://lore.kernel.org/20240919141658.GA3737785@thelio-3990X/)
* [`Re: [PATCH] arm64: Force position-independent veneers`](https://lore.kernel.org/20240927134905.GA430964@thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v11 4/4] firmware: ti_sci: add CPU latency constraint management`](https://lore.kernel.org/20240903005422.GA4638@thelio-3990X/)
* [`Re: [PATCH 1/6] KVM: nVMX: Get to-be-acknowledge IRQ for nested VM-Exit at injection site`](https://lore.kernel.org/20240904210830.GA1229985@thelio-3990X/)
* [`Re: [PATCH v8 3/4] driver core: shut down devices asynchronously`](https://lore.kernel.org/20240905221340.GA2732347@thelio-3990X/)
* [`Re: (subset) [PATCH v7 00/16] Add audio support for the MediaTek Genio 350-evk board`](https://lore.kernel.org/20240906180348.GA1239602@thelio-3990X/)
* [`[llvm-19] objtool warnings for drivers/gpu/drm/radeon/radeon.o`](https://github.com/ClangBuiltLinux/linux/issues/2050)
* [`[LLVM-19] modpost warnings`](https://github.com/ClangBuiltLinux/linux/issues/2051)
* [`[LLVM-19] signed-integer-overflow in <build-dir>/include/linux/atomic/atomic-arch-fallback.h`](https://github.com/ClangBuiltLinux/linux/issues/2052)
* [`Re: [RESEND PATCH v2] params: Annotate struct module_param_attrs with __counted_by()`](https://lore.kernel.org/20240913164630.GA4091534@thelio-3990X/)
* [`Re: [PATCH v3] drm/xe: Add a xe_bo subtest for shrinking / swapping`](https://lore.kernel.org/20240913195649.GA61514@thelio-3990X/)
* [`Re: [PATCH 1/5] drm/i915/gem: fix bitwise and logical AND mixup`](https://lore.kernel.org/20240918150542.GA4049109@thelio-3990X/)
* [`Re: arch/powerpc/include/asm/switch_to.h:53:2: error: call to '__compiletime_assert_256' declared with 'error' attribute: BUILD_BUG failed`](https://lore.kernel.org/20240923183914.GA671157@thelio-3990X/)
* [`Re: ld.lld: error: unknown argument '--ppc476-workaround'`](https://lore.kernel.org/20240927122553.GA908795@thelio-3990X/)
* [`Re: [djiang:cxl/fwctl 13/25] drivers/cxl/cxlmem.h:798:44: error: 'counted_by' argument must be a simple declaration reference`](https://lore.kernel.org/20240927134116.GA3695247@thelio-3990X/)
* [`Re: [PATCH 5.10 247/352] mptcp: fix duplicate data handling`](https://lore.kernel.org/20240928175524.GA1713144@thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Revert "patches: Add fix for UBSAN error in android-mainline"`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/774)
* [`Drop OpenSUSE's PowerPC configuration from 5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/775)
* [`Drop android15-6.1`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/776)
* [`Add patch for new warning in mainline (Sep 27, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/777)
* [`Force disable CONFIG_BUILTIN_MODULE_RANGES for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/778)
* [`patches: tip: Add iio bmi323 patch from mainline folder`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/779)
* [`Update stable anchor to 6.11`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/780)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [19.1.0-rc4](https://lore.kernel.org/20240903195128.GA2607877@thelio-3990X/)
  * [19.1.0](https://lore.kernel.org/20240917191036.GA236618@thelio-3990X/)
  * [Rust 1.81.0](https://lore.kernel.org/20240930141206.GA506883@thelio-3990X/)

* I attended Linux Plumbers Conference 2024 in person in Vienna, Austria.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
