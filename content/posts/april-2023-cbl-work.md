---
title: April 2023 ClangBuiltLinux Work
date: 2023-04-28T16:30:00-0700
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

  * `riscv: Adjust dependencies of HAVE_DYNAMIC_FTRACE selection` ([`v1`](https://lore.kernel.org/20230404-riscv-dynamic-ftrace-checks-clang-v1-1-0ce296b7d423@kernel.org/))
  * `MIPS: generic: Do not select CPUs that are unsupported in clang` ([`v1`](https://lore.kernel.org/20230406-mips-clang-generic-selects-fix-v1-1-811690c9fb69@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `Documentation/llvm: Add a note about prebuilt kernel.org toolchains` ([`v1`](https://lore.kernel.org/20230405-korg-llvm-tc-docs-v1-1-420849b2e025@kernel.org/), [`v2`](https://lore.kernel.org/20230405-korg-llvm-tc-docs-v2-1-98d2e4a96c41@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `MIPS: Drop unused positional parameter in local_irq_{dis,en}able` ([`v1`](https://lore.kernel.org/20230404-mips-unused-named-parameters-v1-1-71d6c656f1de@kernel.org/))
  * `powerpc/32: Include thread_info.h in head_booke.h` ([`v1`](https://lore.kernel.org/20230406-wundef-thread_shift_booke-v1-1-8deffa4d84f9@kernel.org/))
  * `MIPS: Fix check_bugs() modpost warning` ([`v1`](https://lore.kernel.org/20230419-mips-check_bugs-init-attribute-v1-1-91e6eed55b89@kernel.org/), [`v2`](https://lore.kernel.org/20230419-mips-check_bugs-init-attribute-v2-0-60a7ee65d4bf@kernel.org/))
  * `ext4: Fix unused iterator variable warnings` ([`v1`](https://lore.kernel.org/20230420-ext4-unused-variables-super-c-v1-1-138b6db6c21c@kernel.org/))
  * `powerpc/boot: Disable power10 features after BOOTAFLAGS assignment` ([`v1`](https://lore.kernel.org/20230427-remove-power10-args-from-boot-aflags-clang-v1-1-9107f7c943bc@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] drm/vblank: Fix for drivers that do not drm_vblank_init()`](https://lore.kernel.org/20230402235325.GA1068285@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: clang: do not use CROSS_COMPILE for target triple`](https://lore.kernel.org/20230403144758.GA3460665@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] drm/vblank: Fix for drivers that do not drm_vblank_init()`](https://lore.kernel.org/20230403162541.GA3195909@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scripts: rust: drop is_rust_module.sh`](https://lore.kernel.org/20230407175318.GA1018455@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/3] kbuild: merge cmd_archive_linux and cmd_archive_perf`](https://lore.kernel.org/20230407180038.GB1018455@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/3] kbuild: do not create intermediate *.tar for source tarballs`](https://lore.kernel.org/20230407181105.GC1018455@dev-arch.thelio-3990X/)
* [`Re: [PATCH 3/3] kbuild: do not create intermediate *.tar for tar packages`](https://lore.kernel.org/20230407181223.GD1018455@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/5] MIPS: Move various toolchain ASE check to Kconfig`](https://lore.kernel.org/20230407185723.GA1511753@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ubsan: remove cc-option test for UBSAN_TRAP`](https://lore.kernel.org/20230407215824.GA1524475@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/2] wifi: iwlwifi: mvm: fix the order of TIMING_MEASUREMENT notifications`](https://lore.kernel.org/20230410162814.GA2775@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: add $(CLANG_CFLAGS) to KBUILD_CPPFLAGS`](https://lore.kernel.org/20230410172554.GA178820@dev-arch.thelio-3990X/)
* [`[StackProtector] don't check stack protector before calling nounwind functions`](https://reviews.llvm.org/D147975)
* [`Re: [PATCH 1/2] start_kernel: add no_stack_protector fn attr`](https://lore.kernel.org/20230412220348.GA1120303@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] start_kernel: omit prevent_tail_call_optimization for newer toolchains`](https://lore.kernel.org/20230412220408.GB1120303@dev-arch.thelio-3990X/)
* [`Re: [PATCH] phy: mediatek: fix returning garbage`](https://lore.kernel.org/20230414154921.GB1931632@dev-arch.thelio-3990X/)
* [`[CodeGen][Shrink-wrap]split restore point`](https://reviews.llvm.org/D42600)
* [`Re: [PATCH] kasan: remove hwasan-kernel-mem-intrinsic-prefix=1 for clang-14`](https://lore.kernel.org/20230414162605.GA2161385@dev-arch.thelio-3990X/)
* [`Re: [PATCH] [v2] kasan: remove hwasan-kernel-mem-intrinsic-prefix=1 for clang-14`](https://lore.kernel.org/20230418161442.GA3753@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: rpm-pkg: remove kernel-drm PROVIDES`](https://lore.kernel.org/20230418183856.GA2635379@dev-arch.thelio-3990X/)
* [`Re: [PATCH] staging: vchiq_arm: Make vchiq_platform_init() static`](https://lore.kernel.org/20230418184419.GB2635379@dev-arch.thelio-3990X/)
* [`Re: [PATCH] soc: ti: pruss: Avoid cast to incompatible function type`](https://lore.kernel.org/20230418184455.GC2635379@dev-arch.thelio-3990X/)
* [`Android rv64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/552)
* [`Re: [PATCH v2 1/2] phy: mediatek: hdmi: mt8195: fix uninitialized variable usage in pll_calc`](https://lore.kernel.org/20230421221330.GA3657732@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 2/7] MIPS: Add toolchain feature dependency for microMIPS smartMIPS`](https://lore.kernel.org/20230424170303.GA1255815@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scripts: Remove ICC-related dead code`](https://lore.kernel.org/20230424171130.GA2606535@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: deb-pkg: specify targets in debian/rules as .PHONY`](https://lore.kernel.org/20230425180802.GA2881732@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: linux-next: Tree for Apr 3`](https://lore.kernel.org/20230403155147.GA239124@dev-arch.thelio-3990X/)
* [`Re: [PATCH] prctl: Add PR_GET_AUXV to copy auxv to userspace`](https://lore.kernel.org/20230404153012.GA1702188@dev-arch.thelio-3990X/)
* [`Re: [stable:linux-5.4.y 5319/9999] ld.lld: error: drivers/nvme/host/core.o: contains a compressed section, but zlib is not available`](https://lore.kernel.org/20230404161735.GA1703291@dev-arch.thelio-3990X/)
* [`[RISCV] Account for LMUL in memory op costs`](https://reviews.llvm.org/D147470)
* [`MIPS: recordmcount failed to run on object file generated by IAS`](https://github.com/ClangBuiltLinux/linux/issues/1830)
* [`Re: [PATCH] kbuild: add $(CLANG_CFLAGS) to KBUILD_CPPFLAGS`](https://lore.kernel.org/20230410171712.GA177836@dev-arch.thelio-3990X/)
* [`Various boot issues after LLVM commit 5f0bccc3d1a7`](https://github.com/ClangBuiltLinux/linux/issues/1833)
* [`Re: linux-next: manual merge of the drm tree with the powerpc tree`](https://lore.kernel.org/20230413184725.GA3183133@dev-arch.thelio-3990X/)
* [`Re: LLVM not detected in bpfutil due to LLVM 16 requiring c++17`](https://lore.kernel.org/20230414154333.GA1931632@dev-arch.thelio-3990X/)
* [`Missing build.log for several builds`](https://gitlab.com/Linaro/tuxsuite/-/issues/189)
* [`arm64 gki_defconfig boot failure on android-4.19-stable after LLVM commit 5ecd36329508`](https://github.com/ClangBuiltLinux/linux/issues/1837)
* [`Re: arch/mips/include/asm/timex.h:75:10: error: instruction requires a CPU feature not currently enabled`](https://lore.kernel.org/20230419231834.GA1269248@dev-arch.thelio-3990X/)
* [`Re: next: mips: ERROR: modpost: Section mismatches detected.`](https://lore.kernel.org/20230420154537.GA706381@dev-arch.thelio-3990X/)
* [`modpost warning from specialized __create_pgd_mapping_locked() after LLVM commit cc7bb7080fc8`](https://github.com/ClangBuiltLinux/linux/issues/1838)
* [`"'-pcrel' / '-prefixed' is not a recognized feature for this target" when building .S files`](https://github.com/ClangBuiltLinux/linux/issues/1839)
* [`Re: [PATCH] tty: tty_io: remove hung_up_tty_fops`](https://lore.kernel.org/20230428162718.GA1099174@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`boot-qemu.py: Attempt to guess architecture from vmlinux without '-a'`](https://github.com/ClangBuiltLinux/boot-utils/pull/97)
* [`Add syncpt_irq patch to android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/547)
* [`tc_build: Switch to absolute imports`](https://github.com/ClangBuiltLinux/tc-build/pull/232)
* [`Update 64-bit big endian PowerPC configuration target`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/551)
* [`boot-qemu.py: Add file strings for aarch64 with CONFIG_RELOCATABLE=n`](https://github.com/ClangBuiltLinux/boot-utils/pull/99)
* [`ruff.toml: Disable S603 and S607 (boot-utils)`](https://github.com/ClangBuiltLinux/boot-utils/pull/100)
* [`ruff.toml: Disable S603 and S607 (tc-build)`](https://github.com/ClangBuiltLinux/tc-build/pull/233)
* [`patches: Drop zicsr/zifencei 5.10 backport`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/555)
* [`docker: Allow clang-android to target riscv`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/303)
* [`Disable RISC-V gki_defconfig with llvm_android for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/557)
* [`Fix ChromeOS boot testing`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/558)
* [`tc_build: kernel: Adjust PowerPC64 configuration target`](https://github.com/ClangBuiltLinux/tc-build/pull/234)
* [`patches: mainline: Add in-flight DRM revert to fix PowerPC breakage`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/559)
* [`Revert #559`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/560)
* [`Update stable anchor to 6.3`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/561)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I uploaded LLVM [16.0.2](https://lore.kernel.org/20230419180635.GA1965688@dev-arch.thelio-3990X/) to [kernel.org](https://mirrors.edge.kernel.org/pub/tools/llvm/) to continue to encourage developers to test with clang/LLVM.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
