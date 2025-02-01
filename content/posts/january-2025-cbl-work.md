---
title: January 2025 ClangBuiltLinux Work
date: 2025-01-31T17:30:00-0700
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

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * GCC 15 fixes: While it may seem like these are not important for a Clang project, the problem resolved by the changes could impact Clang should the developers decide to change the default C standard dialect to C23 like GCC did.

    * `A couple of build fixes for x86 when using GCC 15` ([`v1`](https://lore.kernel.org/20250121-x86-use-std-consistently-gcc-15-v1-0-8ab0acf645cb@kernel.org/))
    * `s390: Add '-std=gnu11' to decompressor and purgatory CFLAGS` ([`v1`](https://lore.kernel.org/20250122-s390-fix-std-for-gcc-15-v1-1-8b00cadee083@kernel.org/))


* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `spi: amd: Fix -Wuninitialized in amd_spi_exec_mem_op()` ([`v1`](https://lore.kernel.org/20250111-spi-amd-fix-uninitialized-ret-v1-1-c66ab9f6a23d@kernel.org/))
  * `x86/asm: Always inline serialize()` ([`v1`](https://lore.kernel.org/20250116-always-inline-serialize-for-noinstr-v1-1-b7a37b2af04b@kernel.org/))
  * `tracing: Fix -Wundef around 'struct event_mod_load'` ([`v1`](https://lore.kernel.org/20250120-trace_events-fix-wundef-v1-1-61259cbbaa75@kernel.org/))
  * `f2fs: Fix format specifier in sanity_check_inode()` ([`v1`](https://lore.kernel.org/20250120-f2fs-fix-wformat-min_inline_xattr_size-v1-1-508cac1474fe@kernel.org/))
  * `apparmor: Fix checking address of an array in accum_label_info()` ([`v1`](https://lore.kernel.org/20250120-apparmor-pointer-bool-conversion-label-v1-1-5957d28ffde6@kernel.org/))
  * `apparmor: Remove unused variable 'sock' in __file_sock_perm()` ([`v1`](https://lore.kernel.org/20250120-apparmor-fix-unused-sock-__file_sock_perm-v1-1-8d17bd672c6a@kernel.org/))
  * `ALSA: hda: tas2781-spi: Fix -Wsometimes-uninitialized in tasdevice_spi_switch_book()` ([`v1`](https://lore.kernel.org/20250120-tas2781_hda_spi-fix-wsometimes-uninitialized-v1-1-d7fd104aa63e@kernel.org/))
  * `arm64: Handle .ARM.attributes section in linker scripts` ([`v1`](https://lore.kernel.org/20250124-arm64-handle-arm-attributes-in-linker-script-v1-1-74135b6cf349@kernel.org/))
  * `drm/amd/display: Respect user's CONFIG_FRAME_WARN more for dml files` ([`v1`](https://lore.kernel.org/20250131-amdgpu-respect-config_frame_warn-v1-1-68255384492b@kernel.org/))
  * `scripts/Makefile.extrawarn: Do not show clang's non-kprintf warnings at W=1` ([`v1`](https://lore.kernel.org/20250131-makefile-extrawarn-fix-clang-format-non-kprintf-v1-1-6c6747ada0d4@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] x86/sev: Disable UBSAN on SEV code that may execute very early`](https://lore.kernel.org/20250101223008.GA2803252@ax162/)
* [`Re: [PATCH] kbuild: pacman-pkg: provide versioned linux-api-headers package`](https://lore.kernel.org/20250104050513.GA1889730@ax162/)
* [`Re: [PATCH v3 1/2] objtool: Add --Werror`](https://lore.kernel.org/20250114172429.GA246689@ax162/)
* [`Re: [PATCH] drm/amd/display: mark static functions noinline_for_stack`](https://lore.kernel.org/20250116144049.GA1810277@ax162/)
* [`Re: [PATCH v1] selftests: Handle old glibc without execveat(2)`](https://lore.kernel.org/20250119195706.GB1824045@ax162/)
* [`Re: [PATCH] include/linux: Adjust headers for C23`](https://lore.kernel.org/20250120182048.GA3244701@ax162/)
* [`Re: [PATCH] kbuild: Use -fzero-init-padding-bits=all`](https://lore.kernel.org/20250121215122.GA1517789@ax162/)
* [`Re: [PATCH v3 0/2] objtool: Add option to fail build on vmlinux warnings`](https://lore.kernel.org/20250130183042.GB3394637@ax162/)
* [`Re: [PATCH] kbuild: Use --strip-unneeded with INSTALL_MOD_STRIP`](https://lore.kernel.org/20250131035245.GA47826@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: "Bad or missing .orc_unwind table. Disabling unwinder." Clang 19 problem? 6.12.8 problem?`](https://lore.kernel.org/20250114171816.GA3416405@ax162/)
* [`Re: [PATCH v23 7/8] samples/check-exec: Add an enlighten "inc" interpreter and 28 tests`](https://lore.kernel.org/20250114205645.GA2825031@ax162/)
* [`Re: [sysctl:sysctl-testing 8/16] mm/nommu.c:394:12: error: static declaration of 'sysctl_nr_trim_pages' follows non-static declaration`](https://lore.kernel.org/20250115142355.GA519100@ax162/)
* [`[clang] Clang fails to recognize an integral constant expression accepted by GCC`](https://github.com/llvm/llvm-project/issues/122056#issuecomment-2595720610)
* [`Re: [PATCH v6 0/4] Add Microchip IPC mailbox`](https://lore.kernel.org/20250120121955.GA3525889@ax162/)
* [`Re: [linux-next:master 9185/10024] drivers/pmdomain/mediatek/airoha-cpu-pmdomain.c:59:2: error: write to reserved register 'R7'`](https://lore.kernel.org/20250120172802.GA955657@ax162/)
* [`Re: [morimoto:sound-cleanup-2025-01-16-2 7/23] sound/soc/generic/audio-graph-card2.c:1013:4: error: cannot jump from this goto statement to its label`](https://lore.kernel.org/20250122012933.GA1058871@ax162/)
* [`Re: [PATCH v3] acpi: Fix HED module initialization order when it is built-in`](https://lore.kernel.org/20250129043329.GA2344109@ax162/)
* [`[Clang][counted_by] Refactor __builtin_dynamic_object_size on FAMs`](https://github.com/llvm/llvm-project/pull/122198#issuecomment-2627868702)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Add a step to check if patches apply before invoking TuxSuite`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/800)
* [`generator: Run 'apt-get update' before 'apt-get install'`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/803)
* [`Update stable anchor to 6.13 and add 6.12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/804)
* [`scripts/check-patches-apply.py: Fix strip value for refs with a slash`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/806)
* [`Disable ARM configs with BPF enabled in -next with clang-13`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/807)
* [`patches: Remove stable (January 22, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/808)
* [`Apply .ARM.attributes patch to fix orphan section errors`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/809)
* [`Update DEFAULT_KERNEL_FOR_PGO to 6.13.0 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/287)
* [`ci.sh: Update to release/20.x`](https://github.com/ClangBuiltLinux/tc-build/pull/288)
* [`Update LLVM_TOT_VERSION and corresponding clang-nightly files`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/810)
* [`tc_build: llvm: Move compiler-rt to LLVM_ENABLE_RUNTIMES`](https://github.com/ClangBuiltLinux/tc-build/pull/289)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [19.1.7](https://lore.kernel.org/20250114214100.GA331869@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
