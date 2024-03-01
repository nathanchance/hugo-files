---
title: February 2024 ClangBuiltLinux Work
date: 2024-02-29T16:30:00-0700
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

  * `x86/coco: Define cc_vendor without CONFIG_ARCH_HAS_CC_PLATFORM` ([`v1`](https://lore.kernel.org/20240202-provide-cc_vendor-without-arch_has_cc_platform-v1-1-09ad5f2a3099@kernel.org/))
  * `power: supply: core: Fix power_supply_init_attrs() stub` ([`v1`](https://lore.kernel.org/20240227-fix-power_supply_init_attrs-stub-v1-1-43365e68d4b3@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `s390: Support linking with ld.lld` ([`v1`](https://lore.kernel.org/20240207-s390-lld-and-orphan-warn-v1-0-8a665b3346ab@kernel.org/))
  * `kbuild: Fix changing ELF file type for output of gen_btf for big endian` ([`v1`](https://lore.kernel.org/20240208-fix-elf-type-btf-vmlinux-bin-o-big-endian-v1-1-cb3112491edc@kernel.org/), [`v2`](https://lore.kernel.org/20240212-fix-elf-type-btf-vmlinux-bin-o-big-endian-v2-1-22c0a6352069@kernel.org/))
  * `s390: Don't allow CONFIG_COMPAT with LD=ld.lld` ([`v1`](https://lore.kernel.org/20240214-s390-compat-lld-dep-v1-1-abf1f4b5e514@kernel.org/))
  * `s390/boot: Add 'alloc' to info.bin .vmlinux.info section flags` ([`v1`](https://lore.kernel.org/20240216-s390-fix-boot-with-llvm-objcopy-v1-1-0ac623daf42b@kernel.org/))
  * `s390/boot: Workaround current 'llvm-objdump -t -j ...' behavior` ([`v1`](https://lore.kernel.org/20240220-s390-work-around-llvm-objdump-t-j-v1-1-47bb0366a831@kernel.org/))
  * `thermal: core: Move initial num_trips assignment before memcpy()` ([`v1`](https://lore.kernel.org/20240226-thermal-fix-fortify-panic-num_trips-v1-1-accc12a341d7@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Please apply 56778b49c9a2cbc32c6b0fbd3ba1a9d64192d3af to linux-6.7.y`](https://lore.kernel.org/20240225194735.GA498058@dev-arch.thelio-3990X/)
  * [`Please apply commit 5b750b22530fe53bf7fd6a30baacd53ada26911b to linux-6.1.y`](https://lore.kernel.org/20240228014555.GA2678858@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `wifi: ath12k: Fix uninitialized use of ret in ath12k_mac_allocate()` ([`v1`](https://lore.kernel.org/20240205-ath12k-mac-wuninitialized-v1-1-3fda7b17357f@kernel.org/))
  * `drm/amd/display: Increase frame-larger-than for all display_mode_vba files` ([`v1`](https://lore.kernel.org/20240205-amdgpu-raise-flt-for-dml-vba-files-v1-1-9bc8c8b98fb4@kernel.org/))
  * `xfrm: Avoid clang fortify warning in copy_to_user_tmpl()` ([`v1`](https://lore.kernel.org/20240221-xfrm-avoid-clang-fortify-warning-copy_to_user_tmpl-v1-1-254a788ab8ba@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`media: Fix warnings building with LLVM=1`](https://lore.kernel.org/20240128-fix-clang-warnings-v1-0-1d946013a421@chromium.org/)
* [`Re: [PATCH 1/2] Compiler Attributes: Add __uninitialized macro`](https://lore.kernel.org/20240206012836.GA2616098@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] s390/fpu: make use of __uninitialized macro`](https://lore.kernel.org/20240206013121.GB2616098@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/4] kbuild: rpm-pkg: do not include depmod-generated files`](https://lore.kernel.org/20240206013508.GA3151678@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/4] kbuild: rpm-pkg: mark installed files in /boot as %ghost`](https://lore.kernel.org/20240206013524.GB3151678@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/6] tools/rtla: Fix Makefile compiler options for clang`](https://lore.kernel.org/20240206154835.GA1433705@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: Use -fmin-function-alignment when available `](https://lore.kernel.org/20240213002746.GB3272429@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] kbuild: Use -fmin-function-alignment when available`](https://lore.kernel.org/20240217000759.GA3808452@dev-arch.thelio-3990X/)
* [`Re: Patch "modpost: Add '.ltext' and '.ltext.*' to TEXT_SECTIONS" has been added to the 6.6-stable tree`](https://lore.kernel.org/20240219190409.GB2348301@dev-arch.thelio-3990X/)
* [`Re: Patch "um: Fix adding '-no-pie' for clang" has been added to the 6.1-stable tree`](https://lore.kernel.org/20240219192315.GC2348301@dev-arch.thelio-3990X/)
* [`Re: Patch "um: Fix adding '-no-pie' for clang" has been added to the 5.15-stable tree`](https://lore.kernel.org/20240219192837.GD2348301@dev-arch.thelio-3990X/)
* [`Re: [PATCH 5.15 380/476] kbuild: Fix changing ELF file type for output of gen_btf for big endian`](https://lore.kernel.org/20240221165424.GC1782141@dev-arch.thelio-3990X/)
* [`Re: [PATCH] arm64: ftrace: Don't forbid CALL_OPS+CC_OPTIMIZE_FOR_SIZE with Clang`](https://lore.kernel.org/20240225195439.GA2972471@dev-arch.thelio-3990X/)
* [`Add support for kernel.org LLVM`](https://gitlab.com/Linaro/tuxmake/-/issues/215#note_1790195360)
* [`Re: [PATCH 0/7] CFI for ARM32 using LLVM`](https://lore.kernel.org/20240227042103.GA902845@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH, RESEND] x86/sev: Fix SEV check in sev_map_percpu_data()`](https://lore.kernel.org/20240201193809.GA2710596@dev-arch.thelio-3990X/)
* [`Re: [PATCH V8 08/12] drm/bridge: imx: add driver for HDMI TX Parallel Video Interface`](https://lore.kernel.org/20240206170632.GA2183819@dev-arch.thelio-3990X/)
* [`Re: [tip: x86/bugs] x86/retpoline: Ensure default return thunk isn't used at runtime`](https://lore.kernel.org/20240215032049.GA3944823@dev-arch.thelio-3990X/)
* [`[AArch64] Add soft-float ABI`](https://github.com/llvm/llvm-project/pull/74460#issuecomment-1948882652)
* [`Re: [PATCH] s390/boot: Add 'alloc' to info.bin .vmlinux.info section flags`](https://lore.kernel.org/20240219231623.GA2565406@dev-arch.thelio-3990X/)
* [`[LIR][SCEVExpander] Restore original flags when aborting transform`](https://github.com/llvm/llvm-project/pull/82362#issuecomment-1960067147)
* [`Re: [PATCH 2/2] pidfd: add pidfdfs`](https://lore.kernel.org/20240222190334.GA412503@dev-arch.thelio-3990X/)
* [`TuxSuite's libelf.so is too old for zstd compressed debug sections`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/716)
* [`New instances of -Wenum-enum-conversion and -Wenum-compare-conditional after LLVM commit 8c2ae42b3e1c`](https://github.com/ClangBuiltLinux/linux/issues/2002)
* [`Re: [PATCH v3 05/24] ALSA: hwdep: Use guard() for locking`](https://lore.kernel.org/20240229210034.GA185642@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Drop android12-5.10 and android13-5.10 patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/705)
* [`patches: mainline: Drop UML series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/706)
* Update for `ruff 0.2.0`
  * [`boot-utils`](https://github.com/ClangBuiltLinux/boot-utils/pull/117)
  * [`continuous-integration2`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/707)
  * [`tc-build`](https://github.com/ClangBuiltLinux/tc-build/pull/261)
* [`Fix known failures (February 5, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/708)
* [`Update patches (February 12, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/709)
* [`docker: clang-android: Update to r510928 (18.0.0)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/360)
* [`Add support for linking certain s390 kernels with ld.lld`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/711)
* [`Use LLVM=1 for ARCH=s390 on linux-next with tip of tree LLVM`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/712)
* [`patches: Update net/xfrm/xfrm_user.c patch with submitted version`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/714)
* [`patches: Remove stable patches that are now released (Feb 25, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/715)
* [`boot-qemu.py: Add support for booting ARCH=arm kernels via EFI`](https://github.com/ClangBuiltLinux/boot-utils/pull/118)
* [`Disable CONFIG_DEBUG_INFO_COMPRESSED_ZSTD for Android for now`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/717)
* [`patches: Drop 6.1 patches that are now applied/released`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/718)
* [`Use kernel.org clang whenever possible`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/719)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I ran the February 7th and 21st ClangBuiltLinux meetings.

* I continue to upload stable LLVM releases to kernel.org to ensure kernel developers have easy access to LLVM for reproducing and fixing issues that they introduce.

  * [LLVM 18.1.0-rc2](https://lore.kernel.org/20240208002841.GA2601476@dev-arch.thelio-3990X/)
  * [LLVM 18.1.0-rc3](https://lore.kernel.org/20240221193642.GA2138843@dev-arch.thelio-3990X/)
  * [LLVM 18.1.0-rc4](https://lore.kernel.org/20240228184609.GB139944@dev-arch.thelio-3990X/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
