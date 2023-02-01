---
title: January 2023 ClangBuiltLinux Work
date: 2023-01-31T17:30:00-0700
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

* Downstream fixes: These are fixes and improvements that occur in a downstream Linux tree, such as Android or ChromeOS, which our continuous integration regularly tests.

  * [`ANDROID: fuse: Restore upstream type of bitfields in fuse_args`](https://android-review.googlesource.com/c/kernel/common/+/2395938)

* Miscellaneous fixes: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `Remove clang's -Qunused-arguments from KBUILD_CPPFLAGS` ([`v1`](https://lore.kernel.org/20221228-drop-qunused-arguments-v1-0-658cbc8fc592@kernel.org/), [`v2`](https://lore.kernel.org/20221228-drop-qunused-arguments-v2-0-9adbddd20d86@kernel.org/))
  * `ARM: Allow pre-ARMv5 builds with ld.lld 16.0.0 and newer` ([`v1`](https://lore.kernel.org/20230118-v4-v4t-lld-16-v1-1-e3d9a00ae47c@kernel.org/))
  * `x86/build: Move '-mindirect-branch-cs-prefix' out of GCC-only block` ([`resend`](https://lore.kernel.org/20230120165826.2469302-1-nathan@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM (or in the case of the first one, with GCC as the result of a patch series for ClangBuiltLinux). I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `ASoC: amd: ps: Fix uninitialized ret in create_acp64_platform_devs()` ([`v1`](https://lore.kernel.org/20230105-wsometimes-uninitialized-pci-ps-c-v1-1-4022fd077959@kernel.org/), [`v2`](https://lore.kernel.org/20230105-wsometimes-uninitialized-pci-ps-c-v2-1-c50321676325@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * `drm/i915: Fix CFI violations in gt_sysfs` ([`v1`](https://lore.kernel.org/20230116033551.191731-1-nathan@kernel.org/))
  * `offsetof() backports for clang-16+` ([`v1`](https://lore.kernel.org/Y8TWrJpb6Vn6E4+v@dev-arch.thelio-3990X/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`MIPS: fix build from IR files, nan2008 and FpAbi`](https://reviews.llvm.org/D140270)
* [`Re: [PATCH 1/3] KVM: arm64: vgic: Add Apple M2 cpus to the list of broken SEIS implementations`](https://lore.kernel.org/Y7Rs8Lc5u6L2Bz6o@dev-arch.thelio-3990X/)
* [`Re: Linux 6.2-rc2`](https://lore.kernel.org/Y7RzXSKYmAFAXMnr@dev-arch.thelio-3990X/)
* [`Re: [PATCH] ext4: Fix function prototype mismatch for ext4_feat_ktype`](https://lore.kernel.org/Y7TEYceulOsSTQIZ@dev-arch.thelio-3990X/)
* [`Re: [RFC PATCH tip] rseq: Fix: Increase AT_VECTOR_SIZE_BASE to match rseq auxvec entries`](https://lore.kernel.org/Y7XSWdLZqwEXWXml@dev-arch.thelio-3990X/)
* [`Re: [PATCH] crypto: hisilicon: Wipe entire pool on error`](https://lore.kernel.org/Y7hHRYUk6NypdZxT@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] kbuild: rust: remove -v option of scripts/rust_is_available.sh`](https://lore.kernel.org/Y7xKnFsDd5Vlqt4I@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3] kbuild: export top-level LDFLAGS_vmlinux only to scripts/Makefile.vmlinux`](https://lore.kernel.org/Y7xPcBTZin7H+KFe@dev-arch.thelio-3990X/)
* [`Re: [PATCH] efi: tpm: Avoid READ_ONCE() for accessing the event log`](https://lore.kernel.org/Y7xTf1SEPTXiCoPM@dev-arch.thelio-3990X/)
* [`Re: [PATCH] arm64: Fix build with CC=clang, CONFIG_FTRACE=y and CONFIG_STACK_TRACER=y`](https://lore.kernel.org/Y7xT6zNmjtOlDKQX@dev-arch.thelio-3990X/)
* [`[Clang][Sema] Enabled implicit conversion warning for CompoundAssignment operator.`](https://reviews.llvm.org/D139114#4053166)
* [`Re: [RFT PATCH 0/2] arm64: efi: Call SetVaMap() with a 1:1 mapping`](https://lore.kernel.org/Y8bbK30nptwHKn88@dev-arch.thelio-3990X/)
* [`Re: [PATCH] powerpc: add crtsavres.o to always-y instead of extra-y`](https://lore.kernel.org/Y8mPmqDWiRJNdXs%2F@thelio-3990X/)
* [`Re: [PATCH] MIPS: remove CONFIG_MIPS_LD_CAN_LINK_VDSO`](https://lore.kernel.org/Y8q8afLwmEs5RGyR@dev-arch.thelio-3990X/)
* [`Re: [PATCH] arm64: head: Switch endianness before populating the ID map`](https://lore.kernel.org/Y9GgVAxQ3CORw3cA@dev-arch.thelio-3990X/)
* [`Re: [PATCH] udf: remove reporting loc in debug output`](https://lore.kernel.org/Y9QPqPeNCqhO2xev@dev-arch.thelio-3990X/)
* [`Re: [PATCH] gpu: host1x: fix uninitialized variable use`](https://lore.kernel.org/Y9RbvPRP5Yw%2FUMM@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v2] arch: fix broken BuildID for arm64 and riscv`](https://lore.kernel.org/Y7Jal56f6UBh1abE@dev-arch.thelio-3990X/)
* [`Re: [PATCH 02/30] rseq: Introduce feature size and alignment ELF auxiliary vector entries`](https://lore.kernel.org/Y7XJKZhuU9VJZQ11@dev-arch.thelio-3990X/)
* [`Re: next: clang-15: s390x-linux-gnu-ld: BFD (GNU Binutils for Debian) 2.35.2 assertion fail ../../bfd/elf64-s390.c:3349`](https://lore.kernel.org/Y7cRJ8xh%2FLLT9kk0@dev-arch.thelio-3990X/)
* [`Re: [PATCH 4/8] migrate_pages: split unmap_and_move() to _unmap() and _move()`](https://lore.kernel.org/Y7cWb2aP1+wAWR8N@dev-arch.thelio-3990X/)
* [`-Wattribute-warning in drivers/crypto/hisilicon/sgl.c`](https://github.com/ClangBuiltLinux/linux/issues/1780)
* [`-Wattribute-warning in drivers/net/ethernet/mellanox/mlxsw/spectrum_acl_bloom_filter.c`](https://github.com/ClangBuiltLinux/linux/issues/1781)
* [`"Synchronous Exception at..." when booting arm64 kernel built with CONFIG_LTO_CLANG_THIN=y`](https://github.com/ClangBuiltLinux/linux/issues/1782)
* [`nm vmlinux error in arch/arm reappears after upgrade to make 4.4`](https://lore.kernel.org/Y7i8+EjwdnhHtlrr@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 2/2] arm64: efi: Account for the EFI runtime stack in stack unwinder`](https://lore.kernel.org/Y73PAtm6FPuT+1cM@dev-arch.thelio-3990X/)
* [`error: cannot be defined in '__builtin_offsetof' (Kernel 6.1.5)`](https://github.com/ClangBuiltLinux/linux/issues/1791)
* [`Re: [PATCH 4.14 230/338] wifi: brcmfmac: Fix potential shift-out-of-bounds in brcmf_fw_alloc_request()`](https://lore.kernel.org/Y8gccXXyE30sbPSg@dev-arch.thelio-3990X/)
* [`clang 15 fails ppc64 BE kernel build - ld.lld: error: undefined symbol: .early_setup (kernel 6.1-rc5, powerpc64-gentoo-linux-musl)`](https://github.com/ClangBuiltLinux/linux/issues/1761)
* [`Missing cfi_ignorelist.txt in tuxmake images`](https://github.com/ClangBuiltLinux/continuous-integration2/issues/503)
* [`Re: [PATCH 5/6] hw/mips/gt64xxx_pci: Endian-swap using PCI_HOST_BRIDGE MemoryRegionOps`](https://lore.kernel.org/Y88BmxzRqtnpAsWG@dev-arch.thelio-3990X/)
* [`Re: [PATCH v7 4/6] arm64: head: avoid cache invalidation when entering with the MMU on`](https://lore.kernel.org/Y9FZsBEu8hSVVIA8@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 2/2] tools/resolve_btfids: Alter how HOSTCC is forced`](https://lore.kernel.org/Y9lN+H3ModGwwKV6@dev-arch.thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`build-binutils.py: Fix using relative paths for folders`](https://github.com/ClangBuiltLinux/tc-build/pull/217)
* [`patches: mainline: Add tentative fix for 99cb0d917ff`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/497)
* [`Revert OpenSUSE configuration changes`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/498)
* [`Remove -next patches (January 4th, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/499)
* [`4.9 is now EOL`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/500)
* [`Add support for LoongArch`](https://github.com/ClangBuiltLinux/tc-build/pull/219)
* [`Print total script durations`](https://github.com/ClangBuiltLinux/tc-build/pull/222)
* [`Drop mainline patch (January 23, 2023)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/505)
* [`Update kernel to 6.1.7 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/225)
* [`LLVM main is now 17`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/507)
* [`patches: android13-5.10: Remove pahole series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/508)
* [`tc-build: Rewrite`](https://github.com/ClangBuiltLinux/tc-build/pull/226)
* [`docker: install libclang-rt-dev on LLVM 14+`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/290)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
