---
title: "April 2022 ClangBuiltLinux Work"
date: 2022-04-29T16:30:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Build failures: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `arm64: Improve HAVE_DYNAMIC_FTRACE_WITH_REGS selection for clang` ([`v1`](https://lore.kernel.org/20220413181420.3522187-1-nathan@kernel.org/))

* Downstream patches: Downstream consumers of our work are not always paying attention to the changes that we land every release so it is important to give them a little help. In this case, we had to disable `CONFIG_FORTIFY_SOURCE` for clang upstream since it was broken, which impacted Android. As this is an important security feature, it is important that it gets re-enabled once it is fixed, which is what the following change does.

  * [`Revert "ANDROID: GKI: CONFIG_FORTIFY is broken in clang, so it has been disabled"`](https://android-review.googlesource.com/c/kernel/common/+/2063388)

* Stable backports and patches: These are patches It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `Backport of 4013e26670c5 and 60210a3d86dc for 4.9 to 5.10` ([`v1`](https://lore.kernel.org/Ykytcg+xI%2FMRSLue@dev-arch.thelio-3990X/))
  * `Fix two instances of -Wstrict-prototypes in drm/amd` ([`v1`](https://lore.kernel.org/20220411164308.2491139-1-nathan@kernel.org/))
  * [`btrfs warning fixes for 5.15 and 5.17`](https://lore.kernel.org/YliEOSL8z3%2Fs7DGG@dev-arch.thelio-3990X/)
  * [`Apply d799769188529abc6cbf035a10087a51f7832b6b to 5.17 and 5.15?`](https://lore.kernel.org/Yl8pNxSGUgeHZ1FT@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [everyone should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `lib/raid6: Add -Wno-declaration-after-statement to NEON_FLAGS for clang < 14.0.1` ([`v1`](https://lore.kernel.org/20220404210804.3537324-1-nathan@kernel.org/))
  * `drm/msm/gpu: Avoid -Wunused-function with !CONFIG_PM_SLEEP` ([`v2`](https://lore.kernel.org/20220411181249.2758344-1-nathan@kernel.org/))
  * `platform/x86: amd-pmc: Shuffle location of amd_pmc_get_smu_version()` ([`v1`](https://lore.kernel.org/20220418213800.21257-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 1/3] kbuild: Change CFI_CLANG to depend on __builtin_function_start`](https://lore.kernel.org/Ykt2mz3gBTAyu9pL@dev-arch.thelio-3990X/)

* [`Re: [PATCH v2] cfi: Use __builtin_function_start`](https://lore.kernel.org/YkzAtwW4KtZ22ySb@thelio-3990X/)

* [`Re: [tip: x86/build] x86/configs: Add x86 debugging Kconfig fragment plus docs`](https://lore.kernel.org/Yk3GCnyA8rhy1Syj@thelio-3990X/)

* [`Re: [PATCH] rtw89: ser: add a break statement`](https://lore.kernel.org/Yk8nAnDcnPF5rC7N@dev-arch.thelio-3990X/)

* [`Re: [PATCH] MAINTAINERS: add self as clang reviewer`](https://lore.kernel.org/Yk8nV1kBngon+o6N@dev-arch.thelio-3990X/)

* [`Re: [PATCH] drm/amd/display: fix 64 bit divide in freesync code`](https://lore.kernel.org/Yk90X7v8hIgmX5Q5@dev-arch.thelio-3990X/)

* [`Re: [PATCH] drm/amd/display: fix 64 bit divide in freesync code`](https://lore.kernel.org/YlCkKQE8wQ%2FTmi7F@dev-arch.thelio-3990X)

* [`Re: [PATCH] ipc/sem: Remove redundant assignments`](https://lore.kernel.org/YlRNmXucZwNasoFq@thelio-3990X/)

* [`Re: [PATCH] usb: typec: tipd: improve handling of failures in interrupt handlers`](https://lore.kernel.org/YlXyM7e+Bkqa3HCZ@dev-arch.thelio-3990X/)

* [`Re: [PATCH] init/Kconfig: Remove USELIB syscall by default`](https://lore.kernel.org/YlXztpgTxmn29KF3@dev-arch.thelio-3990X/)

* [`Re: [PATCH] fbcon: Fix delayed takeover locking`](https://lore.kernel.org/YlbqRxp4IY775RZj@dev-arch.thelio-3990X/)

* [`Re: [PATCHv4] drm/amdgpu: disable ASPM on Intel Alder Lake based systems`](https://lore.kernel.org/Ylbu7OGHVaqnznQb@thelio-3990X/)

* [`Re: [PATCH] spi: initialize status to success`](https://lore.kernel.org/YltL1RXdeO82s%2FbR@dev-arch.thelio-3990X/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [next] riscv: Linux next-20220404 riscv defconfig builds failed.`](https://lore.kernel.org/Yksjb9Mkq658k4YJ@dev-arch.thelio-3990X/)

* [`Re: Build perf with clang, failure with libperf`](https://lore.kernel.org/Ykto1FgmPMMCysbI@dev-arch.thelio-3990X/)

* [`arm64 defconfig kernel (4.14.275) no longer boots after FEAT_LPA implementation in TCG`](https://gitlab.com/qemu-project/qemu/-/issues/964)

* [`Re: allmodconfig builds failing to link on arm64`](https://lore.kernel.org/YlBPGFs2PMNX6est@dev-arch.thelio-3990X/)

* [`Kernel building script not working for ARM64`](https://github.com/ClangBuiltLinux/tc-build/issues/184)

* [`Re: [PATCH 1/1] um: fix error return code in winch_tramp()`](https://lore.kernel.org/YlRp9KR1mp3%2F4Txo@thelio-3990X/)

* [`Re: [PATCH v3 14/17] fbcon: Move console_lock for register/unlink/unregister`](https://lore.kernel.org/YlSrYyVlcq%2FgHV5T@dev-arch.thelio-3990X/)

* [`question: how to find out why a linking error happens?`](https://github.com/ClangBuiltLinux/tc-build/issues/185)

* [`PGO clang crashes with BOLT instrumentation`](https://github.com/llvm/llvm-project/issues/55004)

* [`BOLT fails to instrument clang on AArch64 host`](https://github.com/llvm/llvm-project/issues/55005)

* [`[ELF] Assert on invalid GOT or PLT relocations`](https://reviews.llvm.org/D123985)

* [`Re: [tty:tty-linus 23/26] drivers/tty/n_gsm.c:939:13: warning: variable 'size' is used uninitialized whenever 'if' condition is false`](https://lore.kernel.org/YmXJDmMWBKwDKSmf@dev-arch.thelio-3990X/)

* [`Re: [PATCH v2 05/11] smp: replace smp_call_function_single_async() with  smp_call_private()`](https://lore.kernel.org/YmXJ0q24kaxtG1R5@dev-arch.thelio-3990X/)

* [`Re: [PATCH] checksyscalls: ignore -Wunused-macros`](https://lore.kernel.org/YmXMiTXEvFXZ%2FswU@dev-arch.thelio-3990X/)

* [`INIT_STACK_ALL_ZERO - System freezes (kernel oops?) on boot`](https://github.com/ClangBuiltLinux/linux/issues/1626)

* [`LTO on ARM 32bit ARM system fails`](https://github.com/ClangBuiltLinux/linux/issues/1627)

* [`Re: Android 11 clang toochain: kernel 5.18 build error with 32 bit target`](https://lore.kernel.org/Ymg43Ed%2FSg8szdKq@dev-arch.thelio-3990X/)

* [`[VPlan] Replace use of needsVectorIV with VPlan user check.`](https://reviews.llvm.org/D123720)

* [`[BOLT] Minimum hardware requirements?`](https://github.com/ClangBuiltLinux/tc-build/issues/188)

* [`[RISCV] Merge addi into load/store as there is a ADD between them`](https://reviews.llvm.org/D124231)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Drop UML -next patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/346)

* [`docker: clang-android: Update to r450784b (14.0.4)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/251)

* [`Turn on ARCH=um builds again`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/348)

* [`check_logs.py: Set execute bit for UML images before running 'boot-uml.sh'`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/350)

* [`Update mainline patches (April 6, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/351)

* [`boot-qemu.sh: Use different CPU for arm64 with new QEMU + old kernel`](https://github.com/ClangBuiltLinux/boot-utils/pull/61)

* [`check_logs.py: Fix name of variable in run_boot()`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/352)

* [`Fix ARCH=um boot testing, part 2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/353)

* [`Remove mainline patches (April 14, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/354)

* [`Enable ARCH=hexagon allmodconfig on 5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/355)

* [`build-llvm.py: Dynamically link the instrumented stage against libLLVM`](https://github.com/ClangBuiltLinux/tc-build/pull/186)

* [`Disable arm64 CFI builds on -next for clang-{12,13}`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/356)

* [`docker: clang-android: Update to r450784d (14.0.6)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/260)

* [`tc-build: Initial BOLT support`](https://github.com/ClangBuiltLinux/tc-build/pull/187)

* [`github: workflows: Fix clang-version`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/359)

* [`boot-qemu.sh: Use implementation defined pointer authentication algorithm`](https://github.com/ClangBuiltLinux/boot-utils/pull/62)

* [`Build powerpc64le configs with LLVM=1 with LLVM 14`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/360)

* [`boot-utils: Upgrade for implementation defined pointer authentication`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/361)

* [`qemu: Update package list`](https://github.com/ClangBuiltLinux/containers/pull/4)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* Two new blog posts, which may help other developers over time:

  * [Package a standalone Linux kernel using the Arch Linux Build System](/posts/package-standalone-linux-kernel-with-abs/)

  * [Bisecting a Linux Kernel boot failure due to changed compiler flags](/posts/bisect-compiler-flag-problem-linux-kernel/)

* I wrote [a Python script](https://github.com/nathanchance/env/blob/d29902e324b666c4fbbd8a2f302a47e73d0c30f4/python/cbl_vmm.py) for easily driving QEMU, which is useful for testing new changes or verifying issues in a safe and easy to debug manner.


## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).
