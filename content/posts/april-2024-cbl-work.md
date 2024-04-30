---
title: April 2024 ClangBuiltLinux Work
date: 2024-04-30T16:30:00-0700
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

  * `Bluetooth: Fix type of len in {l2cap,sco}_sock_getsockopt_old()` ([`v1`](https://lore.kernel.org/20240401-bluetooth-fix-len-type-getsockopt_old-v1-1-c6b5448b5374@kernel.org/))
  * `powerpc: Fix fatal warnings flag for LLVM's integrated assembler` ([`v1`](https://lore.kernel.org/20240405-ppc-fix-wa-fatal-warnings-clang-v1-1-bdcd969f2ef0@kernel.org/))
  * `drm/xe: Add xe_guc_ads.c to uses_generated_oob` ([`v1`](https://lore.kernel.org/20240410-drm-xe-fix-xe_guc_ads-using-xe_wa_oob-v1-1-441f2d8e5d83@kernel.org/))

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * [`CHROMIUM: soc: mediatek: Fix seq_putc() call in mtk_apu_hw_logger_seq_show()`](https://chromium-review.googlesource.com/c/chromiumos/third_party/kernel/+/5410370)

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `configs/hardening: Some fixes for UBSAN` ([`v1`](https://lore.kernel.org/20240411-fix-ubsan-in-hardening-config-v1-0-e0177c80ffaa@kernel.org/))
  * `selftests: Make ksft_exit functions return void instead of int` ([`v1`](https://lore.kernel.org/20240417-ksft-exit-int-to-void-v1-1-eff48fdbab39@kernel.org/), [`v2`](https://lore.kernel.org/20240424-ksft-exit-int-to-void-v2-0-c35f3b8c9ca0@kernel.org/))
  * `x86/purgatory: Avoid kexec runtime warning with LLVM 18` ([`v1`](https://lore.kernel.org/20240417-x86-fix-kexec-with-llvm-18-v1-0-5383121e8fb7@kernel.org/))
  * `clk: bcm: Move a couple of __counted_by initializations` ([`v1`](https://lore.kernel.org/20240425-cbl-bcm-assign-counted-by-val-before-access-v1-0-e2db3b82d5ef@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `MIPS: Add prototypes for plat_post_relocation() and relocate_kernel()` ([`v1`](https://lore.kernel.org/20240403-mips-kaslr-missing-prototypes-v1-1-26b2390c1b7a@kernel.org/))
  * `kselftest: Mark functions that unconditionally call exit() as __noreturn` ([`v1`](https://lore.kernel.org/20240411-mark-kselftest-exit-funcs-noreturn-v1-1-b027c948f586@kernel.org/))
  * `bcachefs: Fix format specifier in validate_bset_keys()` ([`v1`](https://lore.kernel.org/20240416-bcachefs-fix-format-specifier-validate_bset_keys-v1-1-3ea2cdf28b12@kernel.org/))
  * `drivers/s390: Fix instances of -Wcast-function-type-strict` ([`v1`](https://lore.kernel.org/20240417-s390-drivers-fix-cast-function-type-v1-0-fd048c9903b0@kernel.org/))
  * `ALSA: scarlett2: Zero initialize ret in scarlett2_ag_target_ctl_get()` ([`v1`](https://lore.kernel.org/20240419-alsa-scarlett2-fix-wsometimes-uninitialized-v1-1-e2ace8642e08@kernel.org/))
  * `bcachefs: Fix type of flags parameter for some ->trigger() implementations` ([`v1`](https://lore.kernel.org/20240423-bcachefs-fix-wifpts-iter_update_trigger_flags-v1-1-c6a521821863@kernel.org/))
  * `bcachefs: Fix format specifiers in bch2_btree_key_cache_to_text()` ([`v1`](https://lore.kernel.org/20240423-bcachefs-fix-wformat-32-bit-pcpu-v1-1-a10549e5271d@kernel.org/))
  * `drm/amd/display: Avoid -Wenum-float-conversion in add_margin_and_round_to_dfs_grainularity()` ([`v1`](https://lore.kernel.org/20240424-amdgpu-display-dcn401-enum-float-conversion-v1-1-43a2b132ef44@kernel.org/))
  * `drm/amd/display: Use frame_warn_flag consistently in dml2 Makefile` ([`v1`](https://lore.kernel.org/20240424-amdgpu-dml2-fix-frame-larger-than-dcn401-v1-0-5659f8fa8816@kernel.org/))



## LLVM patches

* [`[clang] Support __typeof_unqual__ in all C modes`](https://github.com/llvm/llvm-project/pull/87392)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v3] rust: make mutually exclusive with CFI_CLANG`](https://lore.kernel.org/20240404153258.GA852748@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/4] kbuild: turn on -Wextra by default`](https://lore.kernel.org/20240409162521.GB3219862@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ARM: Add a memory clobber to the fmrx instruction`](https://lore.kernel.org/20240409164641.GC3219862@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] ARM: Add a memory clobber to the fmrx instruction`](https://lore.kernel.org/20240410153526.GA3904754@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: buildtar: Add arm support`](https://lore.kernel.org/20240410170450.GA1828262@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/3] selftests: timers: Fix uninitialized variable warning in ksft_min_kernel_version`](https://lore.kernel.org/20240411153945.GA2507795@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] kbuild: buildtar: Add explicit 32-bit arm support`](https://lore.kernel.org/20240412214856.GF2252629@dev-arch.thelio-3990X/)
* [`Re: [PATCH] selftests: Mark ksft_exit_fail_perror() as __noreturn`](https://lore.kernel.org/20240415154107.GA1538232@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ubsan: Add awareness of signed integer overflow traps`](https://lore.kernel.org/20240415183454.GB1011455@dev-arch.thelio-3990X/)
* [`Re: [PATCH 0/2] Enable building of the devel RPM package from Kbuild`](https://lore.kernel.org/20240417144859.GA1471879@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86/purgatory: Switch to the position-independent small code model`](https://lore.kernel.org/20240418203707.GB2962980@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86: enable HAVE_LD_DEAD_CODE_DATA_ELIMINATION`](https://lore.kernel.org/20240422192448.GA19445@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: buildtar: remove warning for the default case`](https://lore.kernel.org/20240422204555.GB770800@dev-arch.thelio-3990X/)
* [`Re: [RFC PATCH 2/2] objtool/powerpc: Enhance objtool to fixup alternate feature relative addresses`](https://lore.kernel.org/0240423002833.GA1436185@dev-arch.thelio-3990X/)
* [`Re: [RFC PATCH 2/9] x86/purgatory: Simplify stack handling`](https://lore.kernel.org/20240424182659.GA2126602@dev-arch.thelio-3990X/)
* [`Re: [PATCH v5 4/7] soc: mediatek: Add MediaTek DVFS Resource Collector (DVFSRC) driver`](https://lore.kernel.org/20240424190405.GA2803128@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ubsan: Avoid i386 UBSAN handler crashes with Clang`](https://lore.kernel.org/20240424192652.GA3341665@dev-arch.thelio-3990X/)
* [`Re: [PATCH v1 0/3] overlayfs: Optimize override/revert creds`](https://lore.kernel.org/20240425174732.GA270911@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] ubsan: Avoid i386 UBSAN handler crashes with Clang`](https://lore.kernel.org/20240426173837.GA2744190@dev-arch.thelio-3990X/)
* [`Re: [PATCH] hardening: Refresh KCFI options, add some more`](https://lore.kernel.org/20240429221650.GA3666021@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`[linux-6.1.y] MIPS: ./scripts/check-local-export: llvm-nm failed`](https://github.com/ClangBuiltLinux/linux/issues/2012)
* [`Re: [PATCH v4 05/13] mm/arch: Provide pud_pfn() fallback`](https://lore.kernel.org/20240402190549.GA706730@dev-arch.thelio-3990X/)
* [`Re: kernel/sched/core.c:961:15: error: incompatible pointer to integer conversion passing 'typeof`](https://lore.kernel.org/20240403160041.GA1252923@dev-arch.thelio-3990X/)
* [`-Wduplicate-decl-specifier and LLVM "error in backend: Unsupported expression in static initializer" crash trying to enable CONFIG_USE_X86_SEG_SUPPORT`](https://github.com/ClangBuiltLinux/linux/issues/2013)
* [`Re: [PATCH v6 01/37] fix missing vmalloc.h includes`](https://lore.kernel.org/20240403211240.GA307137@dev-arch.thelio-3990X/)
* [`-Wframe-larger-than in drivers/media/platform/mediatek/vcodec/decoder/vdec/vdec_vp9_req_lat_if.c with ARCH=loongarch allmodconfig`](https://github.com/ClangBuiltLinux/linux/issues/2014)
* [`Re: [PATCH net-next v11 01/13] net: phy: Introduce ethernet link topology representation`](https://lore.kernel.org/20240409201553.GA4124869@dev-arch.thelio-3990X/)
* [`[RemoveDIs] Add flag to preserve the debug info format of input IR`](https://github.com/llvm/llvm-project/pull/87379#issuecomment-2050192111)
* [`CoCo code breaks with CFI`](https://github.com/ClangBuiltLinux/linux/issues/2015)
* [`kexec_file is broken with LLVM 18 and newer on x86_64`](https://github.com/ClangBuiltLinux/linux/issues/2016)
* [`Kernel BUG() on Linux 6.8 with Clang 17+ and LTO, rt5640 audio fails`](https://github.com/ClangBuiltLinux/linux/issues/2017)
* [`i386 without CONFIG_X86_CMPXCHG64 "error: inline assembly requires more registers than available" in kernel/bpf/core.c after commit 95ece48165c1 in -next`](https://github.com/ClangBuiltLinux/linux/issues/2018)
* [`Fortify warning in net/xfrm/xfrm_user.c with arm64 CONFIG_KASAN=y after LLVM commit 90ba33099cbb1`](https://github.com/ClangBuiltLinux/linux/issues/2019)
* [`BOLT's stage system requirements`](https://github.com/ClangBuiltLinux/tc-build/issues/268)
* [`Re: [PATCH rdma-next v3 4/6] RDMA/mana_ib: enable RoCE on port`](https://lore.kernel.org/20240422193728.GA44715@dev-arch.thelio-3990X/)
* [`Re: [PATCH 10/11] fs/ntfs3: Remove cached label from sbi`](https://lore.kernel.org/20240422204224.GA770800@dev-arch.thelio-3990X/)
* [`KASAN does not seem to work (no "KASAN init done" in dmesg) when kernel is built with clang (kernel 6.9-rc4, ppc32, clang-18)`](https://github.com/ClangBuiltLinux/linux/issues/2020)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update default PGO kernel to 6.8.0 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/265)
* [`Update patches (April 3, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/738)
* [`Add and enable Fedora LTO build`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/739)
* [`Update korg-clang-18 to 18.1.3`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/378)
* [`Disable CONFIG_DRM_WERROR when CONFIG_WERROR is disabled`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/740)
* [`Drop arm64 patches due to 6.9-rc3 fast forward`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/741)
* [`patches: Drop 5.15`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/742)
* [`x86: Update machine and fix '--efi'`](https://github.com/ClangBuiltLinux/boot-utils/pull/119)
* [`Drop '-mhard-float' patch everywhere`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/744)
* [`Update korg-clang-18 to 18.1.4`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/381)
* [`Update patches (April 25, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/745)
* [`patches: Drop Bluetooth patch from -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/746)
* [`Drop RISC-V GKI builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/747)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I ran the [April 3rd](https://github.com/ClangBuiltLinux/meeting-notes/commit/78b53dd4cd10021413f1a424c5d3a74231a81503) and [April 17th](https://github.com/ClangBuiltLinux/meeting-notes/commit/abe04eee07e4e32312f4036d0129cb104bc2e9dc) ClangBuiltLinux meetings.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [18.1.3](https://lore.kernel.org/20240404184653.GA3299356@dev-arch.thelio-3990X/)
  * [18.1.4](https://lore.kernel.org/20240417171206.GA1819161@dev-arch.thelio-3990X/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
