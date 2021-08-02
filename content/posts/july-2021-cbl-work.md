---
title: "July 2021 ClangBuiltLinux Work"
date: 2021-08-01T00:00:00-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---


## Linux kernel patches

* [`[PATCH] Hexagon: Export raw I/O routines for modules`](https://lore.kernel.org/r/20210708233849.3140194-1-nathan@kernel.org/): Hexagon is a digital signal processor architecture by Qualcomm, which is a little special in terms of the Linux kernel because it is the only architecture that does not have a GCC backend so LLVM has to be used to build it, meaning that it is important to keep it building. This patch fixes an issue with `allmodconfig`, which enables us to build a wide variety of code and verify the compiler is able to handle all of it.

* [`[PATCH] arm64: Restrict ARM64_BTI_KERNEL to clang 12.0.0 and newer`](https://lore.kernel.org/r/20210709000627.3183718-1-nathan@kernel.org/): There was an issue with LLVM's implementation of ARM's Branch Target Identification, a feature of the ARMv8.5 instruction set, which causes a lot of warnings for clang 11.x. To clear up the build, we just disable this option so that users who need access to it will just upgrade their toolchains.

* [`ANDROID: generate_initcall_order.pl: Use two dash long options for llvm-nm`](https://android-review.googlesource.com/q/Iaef9f96af1d75b54eabc4bba38d2a3a58c3c2209): This was an Android specific issue that impacted our CI due to an upstream LLVM change. The benefit of doing this in Android's `kernel/common` tree now is that their update to LLVM 13.x+ will be smooth and all Android devices that track it (which is the majority of devices being updated in the wild) will automatically get that change, resulting in one less bug to triage.

* [`[PATCH 4.9 1/2] iommu/amd: Fix backport of 140456f994195b568ecd7fc2287a34eadffef3ca`](https://lore.kernel.org/r/20210727225650.726875-1-nathan@kernel.org/) and [`[PATCH 4.9 2/2] tipc: Fix backport of b77413446408fdd256599daf00d5be72b5f3e7c6`](https://lore.kernel.org/r/20210727225650.726875-2-nathan@kernel.org/): A series of fixes for the 4.9 series of kernels, which Intel's kernel test robot notified us about. The first one is less likely to happen but I am very surprised nobody reported any issues with the second commit considering the pointer is completely uninitialized. It is important to monitor backports because the kernel moves so rapidly that it is easy to make these kinds of contextual mistakes.

* [`[PATCH] drm/exynos: Always initialize mapping in exynos_drm_register_dma()`](https://lore.kernel.org/r/20210727233656.753002-1-nathan@kernel.org/): A build warning that is harmless in the vast majority of configurations because the hardware that this driver will actually be running on will have one of the two IOMMU configs enabled but it is important to clean up build warnings so that actual issues can be easily caught.

* [`[PATCH] hexagon: Clean up timer-regs.h`](https://lore.kernel.org/r/20210728001729.1960182-1-nathan@kernel.org/): A build warning that was noticed with `ARCH=hexagon allmodconfig`, which builds a large amount of the kernel. This target has uncovered two bugs in the Hexagon backend in LLVM so it is important to keep it as warning free as possible so that errors can be easily caught.

* [`[PATCH] bsg: Fix build error with CONFIG_BLK_DEV_BSG_COMMON=m`](https://lore.kernel.org/r/20210730012108.3385990-1-nathan@kernel.org/): This was not a bug as a result of clang in and of itself but it did impact my ability to build OpenSUSE's config for i386, which I am trying to enable in our CI to increase build coverage. This patch was not accepted as `struct bsg_class_device` was removed in a subsequent patch so this is only a bisect risk.

* [`[PATCH] vmlinux.lds.h: Handle clang's module.{c,d}tor sections](https://lore.kernel.org/r/20210730223815.1382706-1-nathan@kernel.org/): A change in LLVM causes `.text.asan.module_ctor`, `.text.asan.module_d tor`, and `.text.tsan.module_ctor` sections to appear, which have to be explicitly handled in the kernel's linker script to avoid warnings with the linker. As it turns out, not handling them results in panics while running the KASAN KUnit tests, which is usually explained by the fact that the linker can choose to stick the section wherever and if there is a call to it in a section that is too far away, the call goes off into nowhere.



## LLVM patches

* [`[test] Fix tools/gold/X86/comdat-nodeduplicate.ll on non-X86 hosts`](https://github.com/llvm/llvm-project/commit/5060224d9eed8b8359ed5090bb7c577b8575e9e7): A simple text fix that I noticed by building LLVM on my Raspberry Pi 4, which has helped catch a few Linux kernel bugs as well.



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`[PATCH v3 0/2] Kbuild: lto: add make version checking for MODVERSIONS`](https://lore.kernel.org/r/20210702032943.7865-1-lecopzer.chen@mediatek.com/)

* [`Re: [PATCH] clang-tools: Print information when clang-tidy tool is missing`](https://lore.kernel.org/r/YOD%2FbPIizKRSkB8w@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] arm64: drop CROSS_COMPILE for LLVM=1 LLVM_IAS=1`](https://lore.kernel.org/r/YOEFGcTJC6AWFgs1@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH v4] kallsyms: strip LTO suffixes from static functions`](https://lore.kernel.org/r/1fd40e80-283f-62e9-a0fa-84ad68047a23@kernel.org/)

* [`[PATCH v2 0/2] infer CROSS_COMPILE from SRCARCH for LLVM=1 LLVM_IAS=1`](https://lore.kernel.org/r/20210708232522.3118208-1-ndesaulniers@google.com/)

* [`Re: [PATCH] atm: idt77252: clean up trigraph warning on ??) string`](https://lore.kernel.org/r/fd4f465b-86bd-129d-c6d9-e802b7c4815e@kernel.org/)

* [`Re: [PATCH] compiler_attributes.h: move __compiletime_{error|warning}`](https://lore.kernel.org/r/7c7d1639-7997-265e-aa77-ebe3d2fa05e6@kernel.org/)

* [`[PATCH v2 0/3] Fix clang -Wunused-but-set-variable warnings`](https://lore.kernel.org/r/20210726201924.3202278-1-morbo@google.com/)

* [`Re: [PATCH 2/2] mt76: fix build error implicit enumeration conversion`](https://lore.kernel.org/r/YP9Warx1zcUofVhJ@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] nfc: s3fwrn5: fix undefined parameter values in dev_err()`](https://lore.kernel.org/r/YQBDvOG51Tl0ts+g@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] riscv: explicitly use symbol offsets for VDSO`](https://lore.kernel.org/r/YQCnQARTYgAP9hbU@Ryzen-9-3900X.localdomain/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v15 06/12] swiotlb: Use is_swiotlb_force_bounce for swiotlb data bouncing`](https://lore.kernel.org/r/YONOn1FEMufoTy80@Ryzen-9-3900X.localdomain/)

* [`Booting a kernel with -smp under KVM hangs when host kernel is compiled with CFI`](https://github.com/ClangBuiltLinux/linux/issues/1426)

* [`llvm-objdump: error: 'vmlinux': not a dynamic object with pseries_defconfig`](https://github.com/ClangBuiltLinux/linux/issues/1427)

* [`"Not a NOP" warning when booting i386_defconfig`](https://github.com/ClangBuiltLinux/linux/issues/1361)

* [`[SystemZ] Support the 'N' code for the odd register in inline-asm.`](https://reviews.llvm.org/D105502)

* [`-Wimplicit-fallthrough in -next`](https://groups.google.com/g/clang-built-linux/c/ebSSRhQLLG8/m/u3IvdBqfBQAJ)

* [`-Wimplicit-fallthrough needlessly warning about unreachable code`](https://bugs.llvm.org/show_bug.cgi?id=51094)

* [`more 32b ARM targets`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/166)

* [`Re: how can we test the hexagon port in mainline`](https://lore.kernel.org/r/1ee8fc44-3e8c-91c0-7909-a636757dbda4@kernel.org/)

* [`arch/arm64/include/asm/archrandom.h giving build errors`](https://github.com/ClangBuiltLinux/linux/issues/1430)

* [`Re: [PATCH v2 09/79] ALSA: als300: Allocate resources with device-managed APIs`](https://lore.kernel.org/r/YPcnzVns1kz7wtxd@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH v2 14/79] ALSA: cs4281: Allocate resources with device-managed APIs`](https://lore.kernel.org/r/YPcoJ3dkoEkc4xtP@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH v2 39/79] ALSA: korg1212: Allocate resources with device-managed APIs`](https://lore.kernel.org/r/YPcm%2F2ayBvEiHqml@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] Input: serio - make write method mandatory`](https://lore.kernel.org/r/YPd+nl30LwKWpEZa@Ryzen-9-3900X.localdomain/)

* [`Error linking kernel/futex.o`](https://github.com/ClangBuiltLinux/linux/issues/325)

* [`Infinite loop in drivers/hwmon/aspeed-pwm-tacho.c on ARCH=riscv allmodconfig`](https://github.com/ClangBuiltLinux/linux/issues/1431)

* [`Orphan section warnings from .text.asan.module_{c,d}tor`](https://github.com/ClangBuiltLinux/linux/issues/1432)

* [`specified sha does not exist`](https://gitlab.com/Linaro/tuxsuite/-/issues/125)

* [`[CI-NOTIFY]: TCWG Bisect tcwg_kernel/llvm-release-aarch64-next-allnoconfig - Build # 10 - Successful!`](https://groups.google.com/g/clang-built-linux/c/-kX5DNwv6tA/m/Gl7X5h8lAwAJ)

* [`Re: [powerpc][next-20210727] Boot failure - kernel BUG at arch/powerpc/kernel/interrupt.c:98!`](https://lore.kernel.org/r/YQGVZnMe9hFieF8D@Ryzen-9-3900X.localdomain/)

* [`Re: [patch 6/7] slub: fix unreclaimable slab stat for bulk free`](https://lore.kernel.org/r/YQXMHnWRsmfzKK00@archlinux-ax161/)



## Tooling improvements

We maintain a few different repos that help make our lives easier, such as [toolchain build scripts](https://github.com/ClangBuiltLinux/tc-build), [QEMU wrapper scripts](https://github.com/ClangBuiltLinux/boot-utils), and [continuous integration](https://github.com/ClangBuiltLinux/continuous-integration2) so that we catch and triage issues sooner.

* [`Update powerpc64le builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/160)

* [`Disable s390 builds for clang 12 and earlier on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/161)

* [`Enable LLVM_IAS=1 for MIPS`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/170)

* [`Enable more configurations for ARCH=arm`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/171)

* [`Add some distribution configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/172)

* [`Add ARCH=riscv allmodconfig`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/173)

* [`boot-qemu.sh: Add '--no-kvm' option`](https://github.com/ClangBuiltLinux/boot-utils/pull/46)

* [`Upgrade to 5.13.6, update known good revision, and enable Hexagon`](https://github.com/ClangBuiltLinux/tc-build/pull/167)

* [`boot-qemu.sh: Add '--smp' flag`](https://github.com/ClangBuiltLinux/boot-utils/pull/47)

* [`boot test defconfig+sanitizers`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/169) (WIP)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 3 and 4, HP desktop, ASUS laptop, and Hyper-V and VMware platforms on my workstation. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* There are times where I have to test a patch series behind the scenes, which results in a `Tested-by:` tag in the end but no public mailing list posts. This month, that happened with Will Deacon's series [here](https://lore.kernel.org/r/20210719123054.6844-1-will@kernel.org/).



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).