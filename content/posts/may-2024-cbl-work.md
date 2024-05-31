---
title: May 2024 ClangBuiltLinux Work
date: 2024-05-31T16:30:00-0700
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

  * `tty: mxser: Remove __counted_by from mxser_board.ports[]` ([`v1`](https://lore.kernel.org/20240529-drop-counted-by-ports-mxser-board-v1-1-0ab217f4da6d@kernel.org/))
  * `nvmet-fc: Remove __counted_by from nvmet_fc_tgt_queue.fod[]` ([`v1`](https://lore.kernel.org/20240529-drop-counted-by-fod-nvmet-fc-tgt-queue-v1-1-286adbc25943@kernel.org/))
  * `drm/radeon: Remove __counted_by from StateArray.states[]` ([`v1`](https://lore.kernel.org/20240529-drop-counted-by-radeon-states-state-array-v1-1-5cdc1fb29be7@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `kbuild: Remove support for Clang's ThinLTO caching` ([`v1`](https://lore.kernel.org/20240501-kbuild-llvm-drop-thinlto-cache-v1-1-c117cc50a24b@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `f2fs: Add inline to f2fs_build_fault_attr() stub` ([`v1`](https://lore.kernel.org/20240513-f2fs-add-missing-inline-to-f2fs_build_fault_attr-v1-1-c3ce1c995fa2@kernel.org/))
  * `scsi: mpi3mr: Use proper format specifier in mpi3mr_sas_port_add()` ([`v1`](https://lore.kernel.org/20240514-mpi3mr-fix-wformat-v1-1-f1ad49217e5e@kernel.org/))
  * `x86/boot: Address clang -Wimplicit-fallthrough in vsprintf()` ([`v1`](https://lore.kernel.org/20240516-x86-boot-fix-clang-implicit-fallthrough-v1-1-04dc320ca07c@kernel.org/))
  * `efi/libstub: zboot.lds: Discard .discard sections` ([`v1`](https://lore.kernel.org/20240522-efi-zboot-lds-add-discard-sections-to-discard-v1-1-6b415efa0f85@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] selftests/harness: fix many "format string is empty" warnings`](https://lore.kernel.org/20240501020813.264878-1-jhubbard@nvidia.com/)
* [`Re: [PATCH] powerpc: Set _IO_BASE to POISON_POINTER_DELTA not 0 for CONFIG_PCI=n`](https://lore.kernel.org/20240501162056.GA2458112@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86/alternatives: Make FineIBT mode Kconfig selectable`](https://lore.kernel.org/20240501163314.GA2472577@dev-arch.thelio-3990X/)
* [`Re: [PATCH] maple_tree: Fix build failure with W=1 and LLVM=1`](https://lore.kernel.org/20240503160821.GB3960118@thelio-3990X/)
* [`Re: [PATCH] kunit: tool: Build compile_commands.json`](https://lore.kernel.org/20240517183208.GA3699823@thelio-3990X/)
* [`kunit/fortify: Fix memcmp() test to be amplitude agnostic`](https://lore.kernel.org/20240518184020.work.604-kees@kernel.org/)
* [`Re: [PATCH 2/7] riscv: Implement cmpxchg8/16() using Zabha`](https://lore.kernel.org/20240528193110.GA2196855@thelio-3990X/)
* [`Re: [PATCH v2] selftests/user_events: silence a clang warning: address of packed member`](https://lore.kernel.org/20240528202833.GB2680415@thelio-3990X/)
* [`Re: [PATCH v2] selftests/net: suppress clang's "variable-sized type not at the end" warning`](https://lore.kernel.org/20240528203941.GD2680415@thelio-3990X/)
* [`Re: [PATCH 1/2] x86/percpu: Fix "multiple identical address spaces specified for type" clang warning`](https://lore.kernel.org/20240530155758.GA2974773@thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v5 1/2] lib/test_bitops: Add benchmark test for fns()`](https://lore.kernel.org/20240503041701.GA3660305@thelio-3990X/)
* [`Re: [PATCH v3 0/2] Fix Kernel CI issues`](https://lore.kernel.org/20240503162733.GA4136865@thelio-3990X/)
* [`Re: [PULL 18/19] KVM: SEV: Provide support for SNP_EXTENDED_GUEST_REQUEST NAE event`](https://lore.kernel.org/20240513151920.GA3061950@thelio-3990X/)
* [`loongarch ld.lld warning from libstub after enabling CONFIG_UNWINDER_ORC`](https://github.com/ClangBuiltLinux/linux/issues/2023)
* [`Many objtool warnings for LoongArch`](https://github.com/ClangBuiltLinux/linux/issues/2024)
* [`[MCAsmParser] .rept/.irp/.irpc: remove excess tail EOL in expansion`](https://github.com/llvm/llvm-project/commit/c6e787f771d1f9d6a846b2d9b8db6adcd87e8dba#commitcomment-142149509)
* [`fortify_test_memcmp from fortify_kunit does not pass with LLVM 15 and newer`](https://github.com/ClangBuiltLinux/linux/issues/2025)
* [`-Wbounds-safety-counted-by-elt-type-unknown-size in drivers/tty/mxser.c`](https://github.com/ClangBuiltLinux/linux/issues/2026)
* [`-Wbounds-safety-counted-by-elt-type-unknown-size in drivers/nvme/target/fc.c`](https://github.com/ClangBuiltLinux/linux/issues/2027)
* [`-Wbounds-safety-counted-by-elt-type-unknown-size in drivers/gpu/drm/radeon/pptable.h`](https://github.com/ClangBuiltLinux/linux/issues/2028)
* [`Re: [jlayton:mgtime 4/5] fs/inode.c:73:15: error: '__section__' attribute only applies to functions, global variables, Objective-C methods, and Objective-C properties`](https://lore.kernel.org/20240528201035.GA2680415@thelio-3990X/)
* [`fatal error: error in backend: Unsupported expression in static initializer: addrspacecast (ptr addrspace(256) @const_pcpu_hot to ptr)`](https://github.com/llvm/llvm-project/issues/93449)
* [`Re: [PATCH v3 3/4] LoongArch: Fix entry point in image header`](https://lore.kernel.org/20240529182859.GA3994034@thelio-3990X/)
* [`Boot issue on arm64 and loongarch after LLVM commit 5c214eb0c628c874f2c9496e663be4067e64442a`](https://github.com/ClangBuiltLinux/linux/issues/2031)
* [`[DAGCombine] Transform shl X, cttz(Y) to mul (Y & -Y), X if cttz is unsupported`](https://github.com/llvm/llvm-project/pull/85066#issuecomment-2142553617)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`patches: Drop android15-6.6 (May 1, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/748)
* [`Update korg-clang-18 to 18.1.5`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/386)
* [`patches: Drop Bluetooth patch (May 2, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/749)
* [`Add a CONFIG_CFI_CLANG build for ARCH=arm on -next with LLVM >= 16`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/750)
* [`patches: Drop media patch from -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/751)
* [`Enable RISC-V allmodconfig ThinLTO builds on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/752)
* [`scripts/build-local.py: Add else with assertion to clear up new pylint warning`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/753)
* [`Update stable anchor to 6.9`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/754)
* [`boot-utils: Address new pylint possibly-used-before-assignment warnings`](https://github.com/ClangBuiltLinux/boot-utils/pull/120)
* [`Update korg-clang-18 to 18.1.6`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/387)
* [`Update default PGO kernel to 6.9.0 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/270)
* [`Fix broken builds where possible (May 22, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/757)
* [`src: Drop patches that fail to apply after 6.9 update`](https://github.com/ClangBuiltLinux/tc-build/pull/271)
* [`tc_build: llvm: Handle removal of LLVM_ENABLE_TERMINFO`](https://github.com/ClangBuiltLinux/tc-build/pull/272)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [18.1.5](https://lore.kernel.org/20240502152416.GA3178126@dev-arch.thelio-3990X/)
  * [18.1.6](https://lore.kernel.org/20240520153939.GA1166124@thelio-3990X/)

* I [worked with Miguel Ojeda](https://lore.kernel.org/CANiq72mYRkmRffFjNLWd4_Bws5nEyTYvm3xroT-hoDiWMqUOTA@mail.gmail.com/) to provide versions of the LLVM toolchains mentioned above with Rust for developers looking to get involved in the kernel through Rust for Linux.

* I ran the [May 1](https://github.com/ClangBuiltLinux/meeting-notes/commit/05396642dbae4fcb41f1e2d9012362104706fb1a), [May 15](https://github.com/ClangBuiltLinux/meeting-notes/commit/878f20dfe4da92ba61b51fe6c91a596521345cda), and [May 29](https://github.com/ClangBuiltLinux/meeting-notes/commit/389095603e2fd07df15a01474a37ac31d6ba1472) ClangBuiltLinux meetings.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
