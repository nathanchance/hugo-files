---
title: September 2026 ClangBuiltLinux Work
date: 2026-09-30T08:30:00-0700
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

  * `serial: 8250_mid: Add missing module namespace import for SERIAL_8250` ([`v1`](https://lore.kernel.org/20260927-tty-8250_mid-ns-import-v1-1-84d0482e1c5d@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`[PATCH stable] arch_numa: avoid false positive fortify warning in setup_node_to_cpumask_map()`](https://lore.kernel.org/20260903184036.3827332-2-nathan@kernel.org/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `drm/amd/ras: Adjust second parameter of mp1_v13_0_eeprom_send_msg()` ([`v1`](https://lore.kernel.org/20260903-amdgpu-ras-wifpts-v1-1-7f3b4528f12c@kernel.org/))
  * `loongarch: Do not select HAVE_RUST when KASAN is enabled` ([`v1`](https://lore.kernel.org/20260903-loongarch-disable-rust-with-kasan-v1-1-1813d3c4baad@kernel.org/))
  * `samples: rpmsg: Fix mtu printk specifiers` ([`v1`](https://lore.kernel.org/20260908-samples-rpmsg-fix-mtu-print-v1-1-f995518f0a46@kernel.org/))
  * `random: vDSO: Avoid call to memset() when zeroing reserved in __cvdso_getrandom_data()` ([`v1`](https://lore.kernel.org/20260916-vdso-getrandom-avoid-memset-llvm-24-v1-1-80a92f2e225a@kernel.org/), [`v2`](https://lore.kernel.org/20260925-vdso-getrandom-avoid-memset-llvm-24-v2-1-ce640f872393@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`[Clang][Sema] Add fortify warnings for strlcat`](https://github.com/llvm/llvm-project/pull/220341#issuecomment-5531772238)
* [`Re: [PATCH] kbuild: don't delete in-flight filechk temporaries in asm-headers`](https://lore.kernel.org/20260903064544.GA1942038@ax162/)
* [`Re: [PATCH 1/2] scripts: add TOML config to container tool`](https://lore.kernel.org/20260903202700.GA3790602@ax162/)
* [`Re: [PATCH] kconfig: fix extra output from savedefconfig on out-of-range defaults`](https://lore.kernel.org/178847452255.440755.3609720780671235760.b4-review@b4/)
* [`Re: [PATCH 2/2] randstruct: report bad casts as warnings rather than notes`](https://lore.kernel.org/20260904202415.GA2787252@ax162/)
* [`Re: [PATCH v4 0/5] add kconfirm`](https://lore.kernel.org/20260904220559.GB2787252@ax162/)
* [`Re: [PATCH 4/4] kconfig: prevent out-of-bounds user input for numeric options`](https://lore.kernel.org/178856431424.3782172.2818215537125415152.b4-review@b4/)
* [`Re: [PATCH] x86/mm/pat: skip RWX verification until kernel text is set to read only`](https://lore.kernel.org/20260908233334.GA2902183@ax162/)
* [`Re: [PATCH] once_lite: Simplify condition handling and fix context analysis`](https://lore.kernel.org/20260907221334.GA1619741@ax162/)
* [`Re: [PATCH 00/23] kbuild: significantly speed up kernel builds`](https://lore.kernel.org/178901395286.3971858.10992395592923835285.b4-review@b4/)
* [`Re: [PATCH] kbuild: fix grammar in output Makefile .gitignore generation comment`](https://lore.kernel.org/20260915013752.GA1794450@ax162/)
* [`Re: [PATCH v2] checkkconfigsymbols: resolve revisions before resetting the tree`](https://lore.kernel.org/20260915233606.GA863881@ax162/)
* [`Re: [PATCH v3] tee: remove TZMEM_MODE_GENERIC`](https://lore.kernel.org/20260917234456.GA1570097@ax162/)
* [`Re: [PATCH v2] docs: kconfig: fix shell function syntax in caveats`](https://lore.kernel.org/20260915220204.GA1534234@ax162/)
* [`Re: [PATCH] kbuild: add header check facility as a manually run static analyzer`](https://lore.kernel.org/20260916231319.GA550816@ax162/)
* [`Re: [PATCH v3 01/20] kbuild: do not allocate .modinfo in vmlinux`](https://lore.kernel.org/20260918005142.GA1585590@ax162/)
* [`Re: [PATCH 0/3] kconfig: Introduce cc-option-str`](https://lore.kernel.org/20260919012508.GA3573999@ax162/)
* [`Re: [PATCH 1/4] kconfig: tests: Reset KCONFIG_WARN_CHANGED_INPUT by default`](https://lore.kernel.org/179036642082.3653489.376583884931759728.b4-reply@b4/)
* [`Re: [PATCH] fortify: Disable colored diagnostics in test_fortify.sh`](https://lore.kernel.org/20260925205244.GA1518255@ax162/)
* [`Re: [PATCH] clang-tools: Import os for broken pipe handling`](https://lore.kernel.org/179037565553.3985470.12035061965740287126.b4-reply@b4/)
* [`Re: [PATCH] clang-tools: Decode dollar escaping in compile commands`](https://lore.kernel.org/179037692488.3985470.2852393421034692930.b4-reply@b4/)
* [`Re: [PATCH] kbuild: keep .modinfo when vmlinux is linked with --gc-sections`](https://lore.kernel.org/20260928115628.GA964114@ax162/)
* [`Re: [PATCH] scripts: run-clang-tools: import os for broken pipe handling`](https://lore.kernel.org/20260929202319.GB3237201@ax162/)
* [`Re: [PATCH] sched/core: Fix context analysis errors in non-preferred CPU push`](https://lore.kernel.org/20260929202720.GC3237201@ax162/)
* [`Re: [PATCH] iio: temperature: ltc2983: avoid string comparison for leak detector`](https://lore.kernel.org/20260930150336.GC3142230@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v2 0/8] Support Clang context analysis for ext2`](https://lore.kernel.org/20260903072759.GA1750084@ax162/)
* [`Re: [tip: x86/urgent] x86/mm/pat: Fix effective RW computation in lookup_address_in_pgd_attr()`](https://lore.kernel.org/20260905044253.GA3816371@ax162/)
* [`Re: [PATCH 1/5] riscv: smp: Move enum ipi_message_type to asm/smp.h`](https://lore.kernel.org/20260908222349.GA2324870@ax162/)
* [`-Wconstant-conversion in lib/base64.c`](https://github.com/ClangBuiltLinux/linux/issues/2181)
* [`-Wconstant-conversion in drivers/net/wireless/broadcom/brcm80211/brcmsmac/phy/phy_n.c`](https://github.com/ClangBuiltLinux/linux/issues/2182)
* [`Re: lib/base64.c:58:18: warning: implicit conversion from 'int' to 's8' (aka 'signed char') changes value from 131 to -125`](https://lore.kernel.org/20260910223520.GA3375179@ax162/)
* [`RISC-V vDSO build error after LLVM commit 90cebef1411617fc3eedd359bdf00cb44b1c2439`](https://github.com/ClangBuiltLinux/linux/issues/2183)
* [`Re: linux-next: manual merge of the amdgpu tree with the drm-misc,drm-fixes tree`](https://lore.kernel.org/20260914230828.GA331288@ax162/)
* [`Re: [PATCH v2] tpm: use DEFINE_SIMPLE_DEV_PM_OPS and pm_sleep_ptr()`](https://lore.kernel.org/20260914233601.GA1539446@ax162/)
* [`Re: [PATCH v4 11/13] lib/crypto: sha2: Provide functions for zeroizing SHA2 hmac_sha* structures`](https://lore.kernel.org/20260924145806.GA1949494@ax162/)
* [`Re: [PATCH v14 09/13] sched/debug: Add migration stats due to non preferred CPUs`](https://lore.kernel.org/20260929121838.GA1814129@ax162/)
* [`CodeGen: Compute LiveIntervals before TwoAddressInstructions`](https://github.com/llvm/llvm-project/pull/225174#issuecomment-5916202325)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Bump PGO kernel to 7.2 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/350)
* [`build-llvm.py: Drop ty call-top-callable suppression`](https://github.com/ClangBuiltLinux/tc-build/pull/351)
* [`boot-qemu.py: Add '--ephemeral-initrd'`](https://github.com/ClangBuiltLinux/boot-utils/pull/136)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [23.1.1](https://lore.kernel.org/20260909034832.GA748182@ax162/)

* I continued working on [the third generation of ClangBuiltLinux continuous integration](https://github.com/nathanchance/continuous-integration3).

* I began working on optimizing my local testing framework for better utilization of my machines, allowing me to iterate as a maintainer and developer quicker.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
