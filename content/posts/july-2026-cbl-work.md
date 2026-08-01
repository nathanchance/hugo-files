---
title: July 2026 ClangBuiltLinux Work
date: 2026-07-31T16:30:00-0700
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

  * `x86/boot/compressed: Disable jump tables` ([`v2`](https://lore.kernel.org/20260722-x86-boot-compressed-disable-jt-clang-v2-1-7373d38482fb@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Please apply 90dfeef1cd38 to 6.1 through 6.18`](https://lore.kernel.org/20260725011342.GA2792705@ax162/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `issei: Fix size_t printk specifier in heci_{write,read}_buf()` ([`v1`](https://lore.kernel.org/20260721-issei-fix-size_t-specifier-v1-1-246155b42d48@kernel.org/))



## Patch handling, review, and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v3] selftests: harness: Mark test fixture objects __maybe_unused`](https://lore.kernel.org/20260706213613.GC73349@ax162/)
* [`Re: [PATCH] kconfig: warn on dead default`](https://lore.kernel.org/20260707053143.GA1381193@ax162/)
* [`Re: [PATCH 1/2] riscv: vdso: Do not use LTO for the vDSO`](https://lore.kernel.org/20260706210158.GA73349@ax162/)
* [`Re: [PATCH v4] ARM: breakpoint: CFI breakpoints only on demand`](https://lore.kernel.org/20260708185318.GA2718700@ax162/)
* [`Re: [PATCH] scripts/sorttable: guard long_size under MCOUNT_SORT_ENABLED`](https://lore.kernel.org/20260710002202.GA1577616@ax162/)
* [`Re: [PATCH] x86/build/64: Prevent native builds from generating APX instructions`](https://lore.kernel.org/20260712202538.GA1697833@ax162/)
* [`Re: [PATCH] tracing: ring-buffer: allowlist clang-generated symbols`](https://lore.kernel.org/20260715234005.GB3672352@ax162/)
* [`[Hexagon] Fix HexagonRDFOpt hang during live-in recomputation`](https://github.com/llvm/llvm-project/pull/209986#issuecomment-4998181234)
* [`Re: [PATCH v3 06/11] selftests: Fix arm64 IO barriers to match kernel`](https://lore.kernel.org/20260716232210.GA700430@ax162/)
* [`Re: [PATCH v3] ARM: traps: Implement KCFI trap handler for ARM32`](https://lore.kernel.org/20260721001159.GA3326792@ax162/)
* [`Re: [PATCH v3] ARM: imx: Fix suspend/resume crash with Clang CFI`](https://lore.kernel.org/20260721180950.GA3684684@ax162/)
* [`Re: [PATCH] scripts: headers_install.sh: Normalize __ASSEMBLY__ to __ASSEMBLER__`](https://lore.kernel.org/20260721205455.GA91346@ax162/)
* [`Re: [PATCH v3 1/7] kbuild: support generated asm-headers in subdirectories`](https://lore.kernel.org/20260722172140.GA2947455@ax162/)
* [`Re: [PATCH RESEND] kbuild: Stop modifying $(objtree)/Makefile when building oot-kmods oos`](https://lore.kernel.org/178484858201.4095280.2018856728416028444.b4-review@b4/)
* [`Re: [PATCH] kconfig: fix minor typos in comments`](https://lore.kernel.org/178484869129.4095280.2850128549948384193.b4-review@b4/)
* [`Re: [PATCH v2] kbuild: rpm-pkg: Preserve .BTF section in kernel modules during debuginfo stripping`](https://lore.kernel.org/20260724000015.GA2803569@ax162/)
* [`Re: [PATCH 5/6] s390/Kconfig: Select ARCH_SUPPORTS_CFI`](https://lore.kernel.org/20260725010004.GA1686739@ax162/)
* [`Re: [PATCH v2 0/6] s390: Add kCFI support`](https://lore.kernel.org/20260730231317.GA2423962@ax162/)
* [`Re: [PATCH] Documentation: warn against using int, hex, string options as expressions in Kconfig`](https://lore.kernel.org/178545753473.3004192.2499981065379649802.b4-review@b4/)
* [`Re: [PATCH] kconfig: fix submenu rendering of negative dependencies`](https://lore.kernel.org/178545960208.3004192.10262827405138720875.b4-review@b4/)
* [`Re: [PATCH v2 2/2] kbuild: Move gen_init_cpio and gen_initramfs.sh to scripts/`](https://lore.kernel.org/178552657997.3004192.6811032535007922482.b4-review@b4/)
* [`Re: [PATCH] Bluetooth: hci_sync: add conditional locking annotations`](https://lore.kernel.org/20260731221840.GA4066390@ax162/)
* [`Re: [PATCH] kbuild: fix modules.builtin(.modinfo) targets in the top-level Makefile`](https://lore.kernel.org/178553969779.3652505.9437808895468969032.b4-review@b4/)
* [`Re: [PATCH 0/2] kbuild: link-vmlinux.sh: more reliable 3rd pass linking`](https://lore.kernel.org/178554013759.3652505.7898833065785451675.b4-review@b4/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`RISC-V kCFI boot hang after LLVM commit 434e4e15f6a3`](https://github.com/ClangBuiltLinux/linux/issues/2170)
* [`Re: ld.lld: error: vmlinux.a(af_vsock.o at 1186296) <inline asm>:5:8: unpredictable STXR instruction, status is also a source`](https://lore.kernel.org/20260710155428.GA3149665@ax162/)
* [`Re: [powerpc:fixes-test 10/12] kernel/sched/core.c:7449:1: error: type specifier missing, defaults to 'int'; ISO C99 and later do not support implicit int`](https://lore.kernel.org/20260710223740.GA2667378@ax162/)
* [`Re: ld.lld: error: relocation R_PPC_ADDR16_LO cannot be used against symbol 'init_task'; recompile with -fPIC`](https://lore.kernel.org/20260712204154.GB1697833@ax162/)
* [`[Hexagon] Recompute physreg live-ins after HexagonRDFOpt`](https://github.com/llvm/llvm-project/pull/208050#issuecomment-4987218649)
* [`Re: linux-next: build failure after merge of the tty tree`](https://lore.kernel.org/20260716040347.GA1744016@ax162/)
* [`modpost: vmlinux: section mismatch in reference: __list_add (section: .text.unlikely.) -> dir_list (section: .init.data)`](https://github.com/ClangBuiltLinux/linux/issues/2173)
* [`-Wattribute-warning in drivers/base/arch_numa.c on RISC-V with CONFIG_NR_CPUS > 64`](https://github.com/ClangBuiltLinux/linux/issues/2174)
* [`Re: LLVM 22 needs bindgen 0.72.1`](https://lore.kernel.org/20260720220915.GA3368142@ax162/)
* [`Re: [PATCH v4 2/2] drm: ensure blend mode supported if pixel format with alpha exposed`](https://lore.kernel.org/20260721234802.GA439272@ax162/)
* [`[6.18.40] ld.lld: error: undefined symbol: __scoped_seqlock_bug`](https://github.com/ClangBuiltLinux/linux/issues/2178)
* [`Re: [PULL 0/1] qemu-openbios queue 20260707`](https://lore.kernel.org/20260722011530.GA2116566@ax162/)
* [`Semantic conflict between 04b177544a04 in drm-misc-fixes and 0b6b1bb28482 in -mm`](https://lore.kernel.org/20260722225605.GA1910198@ax162/)
* [`Re: Re: MO flag not working properly for building out-of-tree modules, tries to replace the Makefile in the kernel's source.`](https://lore.kernel.org/20260729231242.GB1120279@ax162/)
* [`Re: [PATCH v2 25/33] ibmvfc: process NVMe/FC rports in work thread`](https://lore.kernel.org/20260730065226.GA1879117@ax162/)
* [`Re: rust compile failure in next-20260730`](https://lore.kernel.org/20260731192522.GA1014697@ax162/)
* [`Re: linux-next: manual merge of the slab tree with the rcu tree`](https://lore.kernel.org/20260731220137.GA3307698@ax162/)
* [`"R_RISCV_HI20 out of range" with clang-21+ after -next commit ee10b1028129`](https://github.com/ClangBuiltLinux/linux/issues/2179)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Further build resource conservation (July 8, 2026)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/933)
* [`tc_build: llvm: Use '-Wno-author' instead of '-Wno-dev' with cmake 4.4+`](https://github.com/ClangBuiltLinux/tc-build/pull/343)
* [`Add support for korg-clang-23`](https://github.com/kernelci/tuxmake/pull/296)
* [`workflows: python_lint: Update setup-python to v7`](https://github.com/ClangBuiltLinux/actions-workflows/pull/18)
* [`workflows: python_lint: Update setup-uv to v9.0.0`](https://github.com/ClangBuiltLinux/actions-workflows/pull/19)
* [`generator: Update LLVM main version to 24`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/934)
* [`Update setup-uv to v9.0.0`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/935)
* [`ruff.toml: Update for RUF201 in ruff 0.15.22`](https://github.com/ClangBuiltLinux/boot-utils/pull/134)
* [`Updates for ruff 0.15.22`](https://github.com/ClangBuiltLinux/tc-build/pull/344)
* [`binutils 2.47`](https://github.com/ClangBuiltLinux/tc-build/pull/345)
* [`boot-qemu.py: Add '-nographic' to ppc32_mac machine`](https://github.com/ClangBuiltLinux/boot-utils/pull/135)
* [`Update korg-clang-23 to 23.1.0-rc2`](https://github.com/kernelci/tuxmake/pull/297)
* [`LLVM 23`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/936)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and two AMD-based devices. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [23.1.0-rc1](https://lore.kernel.org/20260720010346.GA2880389@ax162/)
  * [23.1.0-rc2](https://lore.kernel.org/20260728235838.GA2745605@ax162/)

* I submitted the following pull requests.

  * [`[GIT PULL] Kbuild fixes for 7.2 #1`](https://lore.kernel.org/20260801024558.GA3862889@ax162/)

* I have started prototyping a third version of continuous integration for ClangBuiltLinux due to external constraints that restrict our access to the backend of the current continuous integration.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
