---
title: June 2024 ClangBuiltLinux Work
date: 2024-06-28T16:30:00-0700
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

  * `pinctrl: keembay: Fix func conversion in keembay_build_functions()` ([`v1`](https://lore.kernel.org/20240611-pinctrl-keembay-fix-func-conversion-v1-1-3197f2ded3f7@kernel.org/))
  * `phy: freescale: imx8qm-hsio: Include bitfield.h for FIELD_PREP` ([`v1`](https://lore.kernel.org/20240620-phy-fsl-imx8qm-hsio-add-bitfield-include-v1-1-5c7c09ed87e6@kernel.org/))

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * [`ANDROID: arm64: Place CFI jump table sections in .text`](https://android-review.googlesource.com/c/kernel/common/+/3123672)

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `kbuild: Remove support for Clang's ThinLTO caching` ([`6.1`](https://lore.kernel.org/20240613183322.1088226-1-nathan@kernel.org/), [`5.15`](https://lore.kernel.org/20240613183427.1088868-1-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `hostfs: Add const qualifier to host_root in hostfs_fill_super()` ([`v1`](https://lore.kernel.org/20240611-hostfs-fix-mount-api-conversion-v1-1-ef75bbc77f44@kernel.org/))
  * `drm/amd/display: Disable CONFIG_DRM_AMD_DC_FP for RISC-V with clang` ([`v1`](https://lore.kernel.org/20240614-amdgpu-disable-drm-amd-dc-fp-riscv-clang-v1-1-a6d40617dc9b@kernel.org/))
  * `drm/omap: Restrict compile testing to PAGE_SIZE less than 64KB` ([`v1`](https://lore.kernel.org/20240620-omapdrm-restrict-compile-test-to-sub-64kb-page-size-v1-1-5e56de71ffca@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] usr: shorten cmd_initfs in Makefile`](https://lore.kernel.org/20240603222456.GA1802995@thelio-3990X/)
* [`Re: [PATCH] drm/amd/display: Increase frame-larger-than warning limit`](https://lore.kernel.org/20240603222948.GB1802995@thelio-3990X/)
* [`Re: [PATCH v2 2/2] selftests/lib.mk: silence some clang warnings that gcc already ignores`](https://lore.kernel.org/20240603223609.GC1802995@thelio-3990X/)
* [`Re: [PATCH v2 1/2] selftests/openat2: fix clang build failures: -static-libasan, LOCAL_HDRS`](https://lore.kernel.org/20240603223843.GD1802995@thelio-3990X/)
* [`Re: [PATCH v2 2/2] selftests/fchmodat2: fix clang build failure due to -static-libasan`](https://lore.kernel.org/20240603223936.GF1802995@thelio-3990X/)
* [`Re: [PATCH v2 1/2] selftests/lib.mk: handle both LLVM=1 and CC=clang builds`](https://lore.kernel.org/20240603224706.GA245774@thelio-3990X/)
* [`Re: [PATCH] efi: Add missing __nocfi annotations to runtime wrappers`](https://lore.kernel.org/20240605060618.GB279426@thelio-3990X/)
* [`Re: [PATCH] arch: um: rust: Add i386 support for Rust`](https://lore.kernel.org/20240605062234.GE279426@thelio-3990X/)
* [`Re: [PATCH] selftests/timers: Guard LONG_MAX / LONG_MIN defines`](https://lore.kernel.org/20240605062711.GG279426@thelio-3990X/)
* [`Re: [PATCH v3] drm/msm/a6xx: use __unused__ to fix compiler warnings for gen7_* includes`](https://lore.kernel.org/20240605180553.GA2457302@thelio-3990X/)
* [`Re: [PATCH -next v2] kbuild: explicitly run mksysmap as sed script from link-vmlinux.sh`](https://lore.kernel.org/20240605181534.GA3973798@thelio-3990X/)
* [`Re: [PATCH] powerpc: vdso: fix building with wrong-endian toolchain`](https://lore.kernel.org/20240607144329.GB2483293@thelio-3990X/)
* [`Re: [PATCH] media: c8sectpfe: Add missing parameter names`](https://lore.kernel.org/20240607163530.GC2483293@thelio-3990X/)
* [`Re: [PATCH] kunit/overflow: Adjust for __counted_by with DEFINE_RAW_FLEX()`](https://lore.kernel.org/20240610183318.GA3321@thelio-3990X/)
* [`Re: [PATCH 6/7] arm64: start using 'asm goto' for put_user() when available`](https://lore.kernel.org/20240611215556.GA3021057@thelio-3990X/)
* [`Re: [PATCH] kbuild: move init/build-version to scripts/`](https://lore.kernel.org/20240613000056.GA1596562@thelio-3990X/)
* [`Re: [PATCH] modpost: Enable section warning from *driver to .exit.text`](https://lore.kernel.org/20240613000327.GB1596562@thelio-3990X/)
* [`Re: [PATCH 1/2] kbuild: rpm-pkg: make sure to have versioned 'Obsoletes' for kernel.spec`](https://lore.kernel.org/20240613190113.GA1272931@thelio-3990X/)
* [`Re: [PATCH 4/8] riscv: ftrace: align patchable functions to 4 Byte boundary`](https://lore.kernel.org/20240613190920.GB1272931@thelio-3990X/)
* [`Re: [PATCH 3/8] riscv: ftrace: support fastcc in Clang for WITH_ARGS`](https://lore.kernel.org/20240613223615.GA2739249@thelio-3990X/)
* [`Re: [PATCHv8 bpf-next 3/9] uprobe: Add uretprobe syscall to speed up return probe`](https://lore.kernel.org/20240614174822.GA1185149@thelio-3990X/)
* [`Re: [PATCH] USB: serial: garmin_gps: annotate struct garmin_packet with __counted_by`](https://lore.kernel.org/20240619144320.GA2091442@thelio-3990X/)
* [`Re: [PATCH] sparc/build: Make all compiler flags also clang-compatible`](https://lore.kernel.org/20240621185345.GA416370@thelio-3990X/)
* [`Re: [PATCH v2] kbuild: rpm-pkg: fix build error with CONFIG_MODULES=n`](https://lore.kernel.org/20240624201819.GA783641@thelio-3990X/)
* [`Re: [PATCH] kbuild: deb-dpkg: Check optional env variables before use`](https://lore.kernel.org/20240625170048.GA3877857@thelio-3990X/)
* [`Re: [PATCH v2] of: reserved_mem: Restructure code to call reserved mem init functions earlier`](https://lore.kernel.org/20240626150421.GA3664@fedora-macbook-air-m2/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Boot issue on arm64 and loongarch after LLVM commit 5c214eb0c628c874f2c9496e663be4067e64442a`](https://github.com/ClangBuiltLinux/linux/issues/2031)
* [`Re: [PATCH] loongarch: Only select HAVE_OBJTOOL and allow ORC unwinder if the inline assembler supports R_LARCH_{32,64}_PCREL`](https://lore.kernel.org/20240605054328.GA279426@thelio-3990X/)
* [`Re: [linux-next:master 1323/3381] drivers/gpu/drm/amd/amdgpu/amdgpu_gfx.c:333:4: error: format specifies type 'unsigned char' but the argument has type 'int'`](https://lore.kernel.org/20240605061734.GD279426@thelio-3990X/)
* [`Re: [Bug] Failing kunit test on ARCH=arm and LLVM=1`](https://lore.kernel.org/20240607143329.GA2483293@thelio-3990X/)
* [`Re: [viro-vfs:work.fd 11/19] fs/select.c:860:3: error: cannot jump from this goto statement to its label`](https://lore.kernel.org/20240607202010.GA2270105@thelio-3990X/)
* [`Re: [PATCH 3/9] x86/fpu: Make task_struct::thread constant size`](https://lore.kernel.org/20240610211350.GA1613053@thelio-3990X/)
* [`Re: [PATCH v6 1/4] of: reserved_mem: Restruture how the reserved memory regions are processed`](https://lore.kernel.org/20240610213403.GA1697364@thelio-3990X/)
* [`Re: [PATCH v4 2/2] media: i2c: Add imx283 camera sensor driver`](https://lore.kernel.org/20240611172602.GA2226028@thelio-3990X/)
* [`Re: ANNOUNCE: pahole v1.27 (reproducible builds, BTF kfuncs)`](https://lore.kernel.org/20240613214019.GA1423015@thelio-3990X/)
* [`Re: [PATCH 5.10 000/317] 5.10.219-rc1 review`](https://lore.kernel.org/20240613223523.GB1849801@thelio-3990X/)
* [`[llvm][CodeGen] Add a new software pipeliner 'Window Scheduler'`](https://github.com/llvm/llvm-project/pull/84443#issuecomment-2168734476)
* [`Re: mips allmodconfig build error with llvm-18.1.7-x86_64`](https://lore.kernel.org/20240617174446.GA843124@thelio-3990X/)
* [`Re: [PATCH v16 9/9] drm/i915: Compute CMRR and calculate vtotal`](https://lore.kernel.org/20240619154207.GA1125704@thelio-3990X/)
* [`Big compile time regression for Hexagon Linux kernel build after commit 90ba33099c`](https://github.com/llvm/llvm-project/issues/80185#issuecomment-2187294487)
* [`Re: mm: huge_memory.c:2736:31: error: variable 'page' is uninitialized when used here`](https://lore.kernel.org/20240626170023.GA65296@fedora-macbook-air-m2/)
* [`Re: arch/arm/mm/proc.c:82:6: error: conflicting types for 'cpu_arm920_reset'`](https://lore.kernel.org/20240626172154.GA50752@fedora-macbook-air-m2/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update patches (June 13, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/758)
* [`patches: stable: Drop xpt patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/759)
* [`patches: mainline: Drop merged __counted_by patches`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/760)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [18.1.7](https://lore.kernel.org/20240610180317.GA810381@thelio-3990X/)
  * [18.1.8](https://lore.kernel.org/20240621155610.GA3794848@thelio-3990X/)

* I ran the June 12 and June 26 ClangBuiltLinux meetings.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
