---
title: "July 2022 ClangBuiltLinux Work"
date: 2022-07-29T16:00:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Build errors: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `clk: qcom: gpucc-sm8350: Fix "initializer element is not constant" error` ([`v1`](https://lore.kernel.org/20220711163021.152578-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20220711174004.3047516-1-nathan@kernel.org/))
  * `bpf, arm64: Mark dummy_tramp as global` ([`v1`](https://lore.kernel.org/20220713173503.3889486-1-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `x86/speculation: Use DECLARE_PER_CPU for x86_spec_ctrl_current` ([`v1`](https://lore.kernel.org/20220713152222.1697913-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20220713152436.2294819-1-nathan@kernel.org/))
  * `ASoC: Intel: avs: Mark avs_path_module_type_create() as noinline` ([`v1`](https://lore.kernel.org/20220720185228.3182663-1-nathan@kernel.org/))
  * `btrfs: Fix unused variable in load_free_space_cache()` ([`v1`](https://lore.kernel.org/20220722163854.1189931-1-nathan@kernel.org/))
  * `ASoC: amd: acp: Fix initialization of ext_intr_stat1 in i2s_irq_handler()` ([`v1`](https://lore.kernel.org/20220725180539.1315066-1-nathan@kernel.org/))
  * `ion: Make user_ion_handle_put_nolock() a void function` ([`v1`](https://lore.kernel.org/20220727164617.980209-1-nathan@kernel.org/)) 

* Other fixes: These are fixes that don't fit into a particular category but are important to ClangBuiltLinux. In this particular case, there was a Control Flow Integrity (CFI) violation in the simpledrm driver, which can cause kernels compiled with CFI to panic when booting.

  * `drm/simpledrm: Fix return type of simpledrm_simple_display_pipe_mode_valid()` ([`v1`](https://lore.kernel.org/20220725233629.223223-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kernfs: Avoid re-adding kernfs_node into kernfs_notify_list.`](https://lore.kernel.org/Yr8OSxotW2VEUyKQ@dev-arch.thelio-3990X/)
* [`Re: [PATCH 5.15 02/28] clocksource/drivers/ixp4xx: remove __init from ixp4xx_timer_setup()`](https://lore.kernel.org/Yr8TSxroBaL3oRDV@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86/Kconfig: Allow X86_X32_ABI with llvm-objcopy in some cases`](https://lore.kernel.org/Yr9lIs6H03uq+UuV@dev-arch.thelio-3990X/)
* [`Re: [PATCH v1 net-next] af_unix: Put a named socket in the global hash table.`](https://lore.kernel.org/Yr9pPJGoCDYOB3Tm@dev-arch.thelio-3990X/)
* [`Re: [PATCH] MAINTAINERS: Add a general "kernel hardening" section`](https://lore.kernel.org/YsRYORcovwCGvztR@dev-arch.thelio-3990X/)
* [`Re: [PATCH] lib: overflow: Do not define 64-bit tests on 32-bit`](https://lore.kernel.org/YsRajaMsQ5oFFL0l@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86/Kconfig: Fix CONFIG_CC_HAS_SANE_STACKPROTECTOR when cross compiling with clang`](https://lore.kernel.org/YscVEIBtyoX2jrBV@dev-arch.thelio-3990X/)
* [`Re: [PATCH] net: rxrpc: fix clang -Wformat warning`](https://lore.kernel.org/Ysce1Xur72MYp0%2Fd@dev-arch.thelio-3990X/)
* [`Re: [PATCH] eeprom: idt_89hpesx: fix clang -Wformat warnings`](https://lore.kernel.org/YscgBZuT5%2FGVLdIs@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] net: rxrpc: fix clang -Wformat warning`](https://lore.kernel.org/Ysckw9ok670tojo1@dev-arch.thelio-3990X/)
* [`[BasicBlockUtils] Allow critical edge splitting with callbr terminators`](https://reviews.llvm.org/D129256)
* [`Re: [PATCH] riscv: Pass -mno-relax only on lld < 15.0.0`](https://lore.kernel.org/YsxfiKC%2FZBr7U7qI@dev-arch.thelio-3990X/)
* [`[IR] Don't use blockaddresses as callbr arguments`](https://reviews.llvm.org/D129288)
* [`Re: [PATCH] netfilter: xt_TPROXY: remove pr_debug invocations`](https://lore.kernel.org/Ys3DwnYiF9eDwr2T@dev-arch.thelio-3990X/)
* [`Re: [linux-stable-rc:linux-5.10.y 7109/7120] arch/x86/kernel/cpu/bugs.c:57:21: warning: section attribute is specified on redeclared variable`]()
* [`[clang][CodeGen] add fn_ret_thunk_extern to synthetic fns`](https://reviews.llvm.org/D129709)
* [`Re: [PATCH] ubsan: disable UBSAN_DIV_ZERO for clang`](https://lore.kernel.org/YtCJqK2axavXWny8@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3] include/uapi/linux/swab.h: move explicit cast outside ternary`](https://lore.kernel.org/YtWq1vX+8VylSa4G@dev-arch.thelio-3990X/)
* [`[IR] Don't treat callbr as indirect terminator`](https://reviews.llvm.org/D129849#3656711)
* [`[Local] Allow creating callbr with duplicate successors`](https://reviews.llvm.org/D129997)
* [`Re: [PATCH] Makefile.extrawarn: re-enable -Wformat for clang`](https://lore.kernel.org/YtlsY2A2ZWK97Y8O@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ASoC: Intel: avs: Mark avs_path_module_type_create() as noinline`](https://lore.kernel.org/YtlzY9aYdbS4Y3+l@dev-arch.thelio-3990X/)
* [`Re: [PATCH] can: pch_can: fix uninitialized use of errc`](https://lore.kernel.org/Ytl3I9MY9tkuqTKZ@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drivers: iommu: fix clang -wformat warning`](https://lore.kernel.org/Ytm%2Fqj2ozeSY0a3Y@dev-arch.thelio-3990X/)
* [`Re: [PATCH] soc: sof: fix clang -Wformat warnings`](https://lore.kernel.org/YtnDltqEVeJQQkbW@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drivers: lkdtm: fix clang -Wformat warning`](https://lore.kernel.org/YtnGfFuYhQrbed76@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] drivers: lkdtm: fix clang -Wformat warning`](https://lore.kernel.org/YtnNlZYZCEVUiuaE@dev-arch.thelio-3990X/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [char-misc:char-misc-linus 3/3] drivers/misc/cardreader/rtsx_usb.c:639:6: warning: variable 'ret' is used uninitialized whenever 'if' condition is true`](https://lore.kernel.org/Yr8VIT2vjvGYrbmR@dev-arch.thelio-3990X/)
* [`[SimplifyCFG] Thread branches on same condition in more cases (PR54980)`](https://reviews.llvm.org/D124159)
* [`Re: Generating LLVM IR`](https://lore.kernel.org/YsWnr0C2hFNSAqch@dev-arch.thelio-3990X/)
* [`Re: [PATCH 02/40] drm/amd/display: Add SubVP required code`](https://lore.kernel.org/YsXNPayfiUGS67i0@dev-arch.thelio-3990X/)
* [`[LSR] Fix bug for optimizing unused IVs to final values`](https://reviews.llvm.org/D125990)
* [`Re: [PATCH v5 2/4] module: panic: Taint the kernel when selftest modules load`](https://lore.kernel.org/Ysd9FG1fOSnzKv8d@dev-arch.thelio-3990X/)
* [`LLVM ERROR: Broken module found, compilation aborted! when building nouveau`](https://github.com/ClangBuiltLinux/linux/issues/1659)
* [`"error: undefined symbol: dummy_tramp" with CONFIG_CFI_CLANG`](https://github.com/ClangBuiltLinux/linux/issues/1661)
* [`objtool "'naked' return found in RETHUNK build" with clang + CONFIG_K{A,C}SAN=y`](https://lore.kernel.org/Ys7pLq+tQk5xEa%2FB@dev-arch.thelio-3990X/)
* [`objtool: foo() falls through to the next function bar()`](https://github.com/ClangBuiltLinux/linux/issues/1657)
* [`Re: Linux v5.19-rc6: Building and testing x86/retbleed`](https://lore.kernel.org/YtA8ZH5sZWL35%2FPr@dev-arch.thelio-3990X/)
* [`Re: linux-next: Tree for Jul 14`](https://lore.kernel.org/YtA+127QgRifnRBZ@dev-arch.thelio-3990X/)
* [`Re: [linux-next:master 7409/10678] drivers/clk/qcom/gpucc-sm8350.c:111:2: error: initializer element is not a compile-time constant`](https://lore.kernel.org/YtBKWDIaU9Z59CEm@dev-arch.thelio-3990X/)
* [`Re: help with clang build for mips`](https://lore.kernel.org/YtBMNLXEQ4WVJezu@dev-arch.thelio-3990X/)
* [`clang-15 crash trying to build linux kernel for Target: powerpc64-unknown-linux`](https://github.com/llvm/llvm-project/issues/56530)
* [`__aeabi_uldivmod in float64_rem() after LLVM commit 3bc09c7da50a7bb2df51b1829f837f017da5ee34`](https://github.com/ClangBuiltLinux/linux/issues/1666)
* [`Re: <instantiation>:3:3: error: invalid instruction mnemonic 'annotate_unret_safe'`](https://lore.kernel.org/YtHJm0Uiv4c7ihTf@dev-arch.thelio-3990X/)
* [`Re: [git pull] drm fixes for 5.19-rc7`](https://lore.kernel.org/YtHXe4PcWXfihF9Q@dev-arch.thelio-3990X/)
* [`Re: x86_64 build failure of next-20220715 and next-20220718 with clang`](https://lore.kernel.org/YtV3Y4Ha4tPk3kU4@dev-arch.thelio-3990X/)
* [`Re: [PATCH net-next 18/29] can: pch_can: do not report txerr and rxerr during bus-off`](https://lore.kernel.org/YtlwSpoeT+nhmhVn@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Regression caused by llvm: [IR] Don't use blockaddresses as callbr arguments`](https://lore.kernel.org/YtnYBVt2OH%2FxqB+S@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 15/25] KVM: VMX: Extend VMX controls macro shenanigans`](https://lore.kernel.org/YtsQ5SkCJXQIuKGS@dev-arch.thelio-3990X/)
* [`./kernel/smp.c an unaligned pointer access [-Walign-mismatch]`](https://github.com/ClangBuiltLinux/linux/issues/1673)
* [`Re: [TCWG CI] Regression caused by linux: lib/nodemask: inline next_node_in() and node_random()`](https://lore.kernel.org/Yt63uwyLKqQl2Sak@Ryzen-9-3900X./)
* [`Re: [linux-stable-rc:linux-4.9.y 967/2229] drivers/staging/android/ion/ion-ioctl.c:71:6: warning: variable 'ret' is used uninitialized whenever 'if' condition is false`](https://lore.kernel.org/Yt65G3x2Hmoj4q/b@Ryzen-9-3900X./)
* [`cc-option failures after LLVM commit 8f0c901c1a172313a32bc06a1fcface76cd1220f`](https://github.com/ClangBuiltLinux/linux/issues/1674)
* [`Re: sound/soc/codecs/wm_adsp.c:1490:32: warning: taking address of packed member 'name' of class or structure 'wm_adsp_host_buf_coeff_v1' may result in an unaligned pointer value`](https://lore.kernel.org/Yt8aZ3OZ7lJBgf1V@dev-arch.thelio-3990X/)
* [`Re: drivers/iio/adc/ti-tsc2046.c:242:62: warning: taking address of packed member 'data' of class or structure 'tsc2046_adc_atom' may result in an unaligned pointer value`](https://lore.kernel.org/YuAFeyKirrneGPSB@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 6/7] HID: uclogic: Add support for UGEE v2 mouse frames`](https://lore.kernel.org/YuAMzSBcfsyGMjNy@dev-arch.thelio-3990X/)
* [`Re: [PATCH v12 08/69] mm: start tracking VMAs with maple tree`](https://lore.kernel.org/YuCGoB3Ackadj5up@dev-arch.thelio-3990X/)
* [`Use sccache to speed up builds`](https://github.com/ClangBuiltLinux/tc-build/issues/200)
* [`ppc44x_defconfig __umoddi3 with '-mcpu=440'`](https://github.com/ClangBuiltLinux/linux/issues/1679)
* [`Re: arm ixp4xx_defconfig build failure with linux-next and mainline`](https://lore.kernel.org/YuLqQrKtDnTuJdZK@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Stop disabling CONFIG_WERROR on x86_64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/385)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
