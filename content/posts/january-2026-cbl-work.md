---
title: January 2026 ClangBuiltLinux Work
date: 2026-01-30T17:30:00-0700
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

  * `scripts/gen-btf.sh: Ensure initial object in gen_btf_o is ELF with correct endianness` ([`v1`](https://lore.kernel.org/20260105-fix-gen-btf-sh-lto-v1-1-18052ea055a9@kernel.org/), [`v2`](https://lore.kernel.org/20260106-fix-gen-btf-sh-lto-v2-1-01d3e1c241c4@kernel.org/))
  * `ALSA: pcm: Revert bufs move in snd_pcm_xfern_frames_ioctl()` ([`v1`](https://lore.kernel.org/20260106-pcm_native-revert-var-move-free-for-old-clang-v1-1-06a03693423d@kernel.org/))
  * `kbuild: uapi: Avoid testing certain headers on ARCH=arm with CC=clang` ([`v1`](https://lore.kernel.org/20260110-uapi-test-disable-headers-arm-clang-unaligned-access-v1-1-b7b0fa541daa@kernel.org/))
  * `compiler-clang.h: Require LLVM 19.1.0 or higher for __typeof_unqual__` ([`v1`](https://lore.kernel.org/20260116-require-llvm-19-1-for-typeof_unqual-v1-1-3b9a4a4b212b@kernel.org/))
  * `drm/xe: Move _THIS_IP_ usage from xe_vm_create() to dedicated function` ([`v1`](https://lore.kernel.org/20260121-xe-vm-fix-clang-goto-error-v1-1-7e121d81512e@kernel.org/))
  * `x86/entry/vdso32: Omit '.cfi_offset eflags' for LLVM < 16` ([`v1`](https://lore.kernel.org/20260123-x86-vdso32-wa-llvm-15-cfi-offset-eflags-v1-1-0f412e3516a4@kernel.org/))

* Kbuild / Kconfig fixes and improvements: These are changes that I have authored as part of maintaining Kbuild and Kconfig.

  * `kbuild: rpm-pkg: Generate debuginfo package manually` ([`v1`](https://lore.kernel.org/20260121-fix-module-signing-binrpm-pkg-v1-1-8fc5832b6cbc@kernel.org/))
  * `scripts/container: Minor fixups suggested by ruff` ([`v1`](https://lore.kernel.org/20260122-scripts-container-ruff-fixes-v1-0-fd1b928c3f10@kernel.org/))
  * `kbuild: Do not run kernel-doc when building external modules` ([`v1`](https://lore.kernel.org/20260130-kbuild-skip-kernel-doc-extmod-v1-1-58443d60131a@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but matter in some way to my other work.

  * `drm/panic: Avoid crash from invalid CONFIG_DRM_PANIC_SCREEN values` ([`v1`](https://lore.kernel.org/20260105-drm_panic-handle-invalid-drm_panic_screen-v1-0-55228bd4b0f8@kernel.org/))
  * `drm/msm/dp: Avoid division by zero in msm_dp_ctrl_config_msa()` ([`v1`](https://lore.kernel.org/20260108-drm-msm-dp_ctrl-avoid-zero-div-v1-1-6a8debcb3033@kernel.org/), [`v2`](https://lore.kernel.org/20260113-drm-msm-dp_ctrl-avoid-zero-div-v2-1-f1aa67bf6e8e@kernel.org/))
  * `riscv: Use 64-bit variable for output in __get_user_asm` ([`v2`](https://lore.kernel.org/20260113-riscv-wa-llvm-asm-goto-outputs-assertion-failure-v2-1-e04aba2e1f9f@kernel.org/), [`v3`](https://lore.kernel.org/20260116-riscv-wa-llvm-asm-goto-outputs-assertion-failure-v3-1-55b5775f989b@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply 70740454377f1ba3ff32f5df4acd965db99d055b to linux-6.12.y`](https://lore.kernel.org/20260113005327.GA2283840@ax162/)
  * [`[PATCH 6.12] drm/amd/display: mark static functions noinline_for_stack`](https://lore.kernel.org/20260119220232.1125319-2-nathan@kernel.org/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `ntfs3: Restore NULL folio initialization in ntfs_writepages()` ([`v1`](https://lore.kernel.org/20260105-ntfs3-restore-null-folio-init-v1-1-432093703dfd@kernel.org/))
  * `drm/panel: ilitek-ili9882t: Remove duplicate initializers in tianma_il79900a_dsc` ([`v1`](https://lore.kernel.org/20260114-panel-ilitek-ili9882t-fix-override-init-v1-1-1d69a2b096df@kernel.org/))
  * `ACPI: APEI: GHES: Disable KASAN instrumentation when compile testing with clang < 18` ([`v1`](https://lore.kernel.org/20260114-ghes-avoid-wflt-clang-older-than-18-v1-1-9c8248bfe4f4@kernel.org/))
  * `riscv: Add intermediate cast to 'unsigned long' in __get_user_asm` ([`v1`](https://lore.kernel.org/20260121-riscv-fix-int-to-pointer-cast-v1-1-b83eebe57c76@kernel.org/))
  * `iommu/amd: Fix type of type parameter to amd_iommufd_hw_info()` ([`v1`](https://lore.kernel.org/20260122-amd-iommufd-fix-wifpts-v1-1-8ee604527875@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v2] rust: Add -fdiagnostics-show-context to bindgen_skip_c_flags`](https://lore.kernel.org/20260105230322.GA1276749@ax162/)
* [`[CodeGen] Check BlockAddress users before marking block as taken`](https://github.com/llvm/llvm-project/pull/174480#issuecomment-3712546815)
* [`[Clang][counted-by][NFC] Add testcase for non-forward record decl`](https://github.com/llvm/llvm-project/pull/153338#pullrequestreview-3629634636)
* [`Re: [PATCH v3] kconfig: Support conditional deps using "depends on X if Y"`](https://lore.kernel.org/176773717252.1961405.1002180889329327680.b4-ty@kernel.org/)
* [`Re: [PATCH 0/5] kbuild: uapi: improvements to header testing`](https://lore.kernel.org/176773745004.1983625.3214132191933293574.b4-ty@kernel.org/)
* [`Re: [PATCH] kconfig: fix static linking of nconf`](https://lore.kernel.org/20260110232043.GA90060@ax162/)
* [`Re: [PATCH v2 1/3] scripts: kconfig: merge_config.sh: refactor from shell/sed/grep to awk`](https://lore.kernel.org/20260111012350.GA1267883@ax162/)
* [`Re: [PATCH] kbuild: uapi: Avoid testing certain headers on ARCH=arm with CC=clang`](https://lore.kernel.org/20260112225307.GA2241363@ax162/)
* [`Re: [PATCH] kbuild: Reject unexpected values for LLVM=`](https://lore.kernel.org/20260112231624.GA2272167@ax162/)
* [`Re: [PATCH] kbuild: Drop superfluous compiler option checks`](https://lore.kernel.org/176842584555.2841457.8759143570481663014.b4-ty@kernel.org/)
* [`Re: [PATCH] rust: kbuild: give `--config-path` to `rustfmt` in `.rsi` target`](https://lore.kernel.org/20260116050046.GA1452322@ax162/)
* [`Re: [PATCH 0/2] kbuild, uapi: Mark inner unions in packed structs as packed`](https://lore.kernel.org/176860117992.3208640.15511985701328893417.b4-ty@kernel.org/)
* [`Re: [PATCH v3 0/2] scripts: introduce containerized builds`](https://lore.kernel.org/20260119213516.GA1051134@ax162/)
* [`Re: [PATCH] kbuild: dummy-tools: Add python3`](https://lore.kernel.org/20260121231651.GA601114@ax162/)
* [`Re: [PATCH v2 0/5] uapi: fix remaining kconfig leaks in UAPI headers`](https://lore.kernel.org/20260121232109.GA985170@ax162/)
* [`Re: [PATCH v2 1/2] Documentation/kbuild: Document gendwarfksyms build dependencies`](https://lore.kernel.org/20260121235616.GA3887168@ax162/)
* [`Re: [PATCH v2] kbuild: Reject unexpected values for LLVM=`](https://lore.kernel.org/176904018061.36280.9070517793890679732.b4-ty@kernel.org/)
* [`Re: [RFC PATCH 2/3] kbuild: Make sure to generate config file`](https://lore.kernel.org/20260121234748.GA2137017@ax162/)
* [`Re: [RFC][PATCH 0/4] locking: Add/convert context analysis bits`](https://lore.kernel.org/20260122185812.GB3461196@ax162/)
* [`Re: [PATCH v4 1/2] scripts: add tool to run containerized builds`](https://lore.kernel.org/20260122184939.GA3461196@ax162/)
* [`[Frontend][Sema] Add CC1-only -fms-anonymous-structs to enable Microsoft anonymous struct/union feature`](https://github.com/llvm/llvm-project/pull/176551#issuecomment-3802073654)
* [`[clang] Use tighter lifetime bounds for C temporary arguments`](https://github.com/llvm/llvm-project/pull/170518#issuecomment-3808125170)
* [`Re: [PATCH] kbuild: Fix permissions of modules.builtin.modinfo`](https://lore.kernel.org/20260127205915.GA3856796@ax162/)
* [`Re: [PATCH v2 00/14] Add SPDX SBOM generation tool`](https://lore.kernel.org/20260127231037.GA3378797@ax162/)
* [`Re: [RFC PATCH 2/2] kbuild: rust: use klint to provide CONFIG_FRAME_WARN`](https://lore.kernel.org/20260127221531.GC3382807@ax162/)
* [`Re: [PATCH v6 1/4] rust: export BINDGEN_TARGET from a separate Makefile`](https://lore.kernel.org/20260129223528.GA844102@ax162/)
* [`Re: [PATCH v2 0/2] Simplify kallsyms offset table generation`](https://lore.kernel.org/176973183159.175491.9742885221630963935.b4-ty@kernel.org/)
* [`Re: [PATCH v3 1/3] scripts: kconfig: merge_config.sh: refactor from shell/sed/grep to awk`](https://lore.kernel.org/176973183385.175491.5659819632977597738.b4-ty@kernel.org/)
* [`Re: [PATCH] kbuild: dummy-tools: Add python3`](https://lore.kernel.org/176973225791.178709.17831414346066426792.b4-ty@kernel.org/)
* [`Re: [PATCH] kbuild: install-extmod-build: Add missing python libraries`](https://lore.kernel.org/20260130011106.GA359714@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [peterz-queue:locking/core 34/39] WARNING: modpost: EXPORT symbol "stack_depot_save_flags" [vmlinux] version generation failed, symbol will not be versioned.`](https://lore.kernel.org/20260107204549.GA846948@ax162/)
* [`[ConstantInt] Disable implicit truncation in ConstantInt::get()`](https://github.com/llvm/llvm-project/pull/171456#issuecomment-3727345950)
* [`FIELD_PREP failure in drivers/hwmon/macsmc-hwmon.c`](https://lore.kernel.org/20260119195817.GA1035354@ax162/)
* [`Re: Since 6.18.x make binrpm-pkg does not sign modules`](https://lore.kernel.org/20260120000454.GA2366369@ax162/)
* [`Re: [PATCH v9 15/17] media: rkvdec: Add H264 support for the VDPU383 variant`](https://lore.kernel.org/20260121230406.GA2625738@ax162/)
* [`Re: [PATCH 13/13] clk: microchip: core: allow driver to be compiled with COMPILE_TEST`](https://lore.kernel.org/20260121231321.GB2625738@ax162/)
* [`Re: [PATCH v2 4/6] KVM: arm64: Account for RES1 bits in DECLARE_FEAT_MAP() and co`](https://lore.kernel.org/20260120211558.GA834868@ax162/)
* [`Re: [rppt:zero-page/v0 1/1] ERROR: modpost: vmlinux: 'empty_zero_page' exported twice. Previous export was in vmlinux`](https://lore.kernel.org/20260122051722.GC3770486@ax162/)
* [`Re: [PATCH v3 4/9] platform/wmi: Add kunit test for the string conversion code`](https://lore.kernel.org/20260122234521.GA413183@ax162/)
* [`Re: make olddefconfig surprises`](https://lore.kernel.org/20260123205359.GA95167@ax162/)
* [`Re: linux-next: manual merge of the jc_docs tree with the kbuild tree`](https://lore.kernel.org/20260126225511.GA866955@ax162/)
* [`clang hangs when building Linux kernel's rkvdec-vdpu383-h264.c for ARCH=hexagon`](https://github.com/llvm/llvm-project/issues/178535)
* [`Re: [RFC] Don't create sframes during build`](https://lore.kernel.org/20260129222357.GA493990@ax162/)
* [`Re: Failure to build with LLVM for the Wii`](https://lore.kernel.org/20260129223136.GA1614447@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update patches for recent drm/amd/display patch merge`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/899)
* [`patches: Drop 6.1 and 6.6 (January 12, 2026)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/900)
* [`Initial set of changes for LLVM main 22 → 23 update`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/902)
* [`Add support for korg-clang-22`](https://github.com/kernelci/tuxmake/pull/235)
* [`patches: Drop drm/amd/display patches from stable and 6.12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/903)
* [`patches: Drop 5.15 (January 21, 2026)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/904)
* [`tc_build: llvm: Handle absence of LLVM_ALL_EXPERIMENTAL_TARGETS`](https://github.com/ClangBuiltLinux/tc-build/pull/315)
* [`docker: clang-android: Update to r584948b (22.0.1)`](https://github.com/kernelci/tuxmake/pull/239)
* [`Update korg-clang-22 to 22.1.0-rc2`](https://github.com/kernelci/tuxmake/pull/240)
* [`Avoid .sframe version mismatch errors`](https://github.com/ClangBuiltLinux/tc-build/pull/316)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [22.1.0-rc1](https://lore.kernel.org/20260117001822.GA1195901@ax162/)
  * [22.1.0-rc2](https://lore.kernel.org/20260128060714.GA1181796@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
