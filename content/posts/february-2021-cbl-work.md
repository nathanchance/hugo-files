---
title: "February 2021 ClangBuiltLinux Work"
date: 2021-02-28T00:01:20-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

As some of you may or may not know by now, I am [now employed](https://www.linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/) by the Linux Foundation to help improve Linux security. The primary way that I am doing that is through the work I do for [ClangBuiltLinux](https://clangbuiltlinux.github.io/), of which I am an [official maintainer](https://git.kernel.org/linus/b9644289657748314dbfe6502c316b3f09e251ed).

## Linux kernel patches

First and foremost, I am a Linux kernel developer, meaning that I love sending kernel patches. Getting familiar with `git send-email` and the standard Linux kernel workflow can be difficult but now that I am used to it, I find other things cumbersome.

Some of the patches that I send in February are as follows:

*  [`arm64: Make CPU_BIG_ENDIAN depend on ld.bfd or ld.lld 13.0.0+`](https://lore.kernel.org/r/20210209005719.803608-1-nathan@kernel.org/): I did not do a whole ton of heavy lifting on this, it is mostly thanks to Fangrui Song that this is possible. Getting proper big endian support for AArch64 is not the biggest deal because it is not super common except in networking but more support is more support. One less configuration option that has to be toggled :)

* [`qemu_fw_cfg: Make fw_cfg_rev_attr a proper kobj_attribute`](https://lore.kernel.org/r/20210211194258.4137998-1-nathan@kernel.org/): This one is particularly nasty. Indirect function calls through function pointers can be a major security threat in C, where they can be used in ROP chains. Clang's [Control Flow Integrity](https://clang.llvm.org/docs/ControlFlowIntegrity.html) is a way to guard this, where indirect function calls have their parameters checked to attempt to make sure that the function call has not been hijacked. This support is currently [out of tree](https://github.com/samitolvanen/linux/commits/clang-cfi) but patches like this are critical for getting it pushed upstream. This patch fixes a CFI failure in the `qemu_fw_cfg` driver, where attempting to read the firmware revision will cause the kernel to panic when it is read.

* [`Makefile: Remove # characters from compiler string`](https://lore.kernel.org/r/20210216213312.30462-1-nathan@kernel.org/): [A report](https://github.com/ClangBuiltLinux/linux/issues/1298) came in that AMD's Optimzing C/C++ Compiler (AOCC, based on LLVM/Clang) could not build the kernel as it would error out due to the `#` symbol in the version string (which starts a comment in Make and is required to be escaped). Kconfig does not support escaping `#` yet so the easiest fix is to just remove that symbol if it is present. AOCC may come in handy later if it has certain optimizations that the kernel can take advantage of

* [`drm/i915: Enable -Wuninitialized`](https://lore.kernel.org/r/20210216212953.24458-1-nathan@kernel.org/): `-Wuninitialized` has become a very important warning over the past year of so because GCC's version (`-Wmaybe-uninitialized`) was [disabled](https://git.kernel.org/linus/78a5255ffb6a1af189a83e493d916ba1c54d8c75) by Linus in 5.7. i915 maintains a separate set of CFLAGS from the rest of the kernel and this was disabled before `-Wuninitialized` became smarter in newer LLVM versions so this commit re-enables it. I had this patch locally for a while with another workaround but Arnd Bergmann pointed out that other workaround is no longer necessary so this can be enabled for everyone!

* [`drm/amd/pm/swsmu: Avoid using structure_size uninitialized in smu_cmn_init_soft_gpu_metrics`](https://lore.kernel.org/r/20210218224849.5591-1-nathan@kernel.org/): Speaking of `-Wuninitialized`... here is an instance where it proves useful. Using uninitialized stack memory is never a good idea and can even be a security issue depending on the context. Linux himself [pointed](https://lore.kernel.org/r/CAHk-=wgiPxXzRNnfaXk7ozSWSu7fFU--kTmVjkDaTB05wwUk_g@mail.gmail.com/) this out and accurately explaining that while the `default:` case never happens, it still should not do this.

## Patch review and input

In open source development, patch review is a currency to barter with. In order to have your own patches reviewed quickly, it is in your best interest to review patches quickly yourself. Below are some of the patches that I provided review on, which helps ensure the kernel's quality. Some of these reviews span multiple messages, which can be viewed at the bottom of the page.

* [`ext4: Enable code path when DX_DEBUG is set`](https://lore.kernel.org/r/20210201004820.GA26230@localhost/)

* [`scripts: switch some more scripts explicitly to Python 3`](https://lore.kernel.org/r/20210201013324.GA2423231@localhost/)

* [`kbuild: fix duplicated flags in DEBUG_CFLAGS`](https://lore.kernel.org/r/20210203173835.GA765175@localhost/)

* [`mm/mremap: fix BUILD_BUG_ON() error in get_extent`](https://lore.kernel.org/r/20210203184840.GA1711681@localhost/)

* [`crypto: octeontx2 - fix -Wpointer-bool-conversion warning`](https://lore.kernel.org/r/20210204212250.GA385551@localhost/)

* [`x86/efi: Remove EFI PGD build time checks`](https://lore.kernel.org/r/20210205185622.GA461042@localhost/)

* [`clang_tools:gen_compile_commands: Change the default source directory`](https://lore.kernel.org/r/20210208195439.GA1097868@ubuntu-m3-large-x86/)

* [`gen_compile_commands: prune some directories`](https://lore.kernel.org/r/20210211171908.GA3820685@ubuntu-m3-large-x86/)

* [`kbuild: remove ld-version macro`](https://lore.kernel.org/r/20210216185742.GA64801@24bbad8f3778/)

* [`kbuild: check the minimum linker version in Kconfig`](https://lore.kernel.org/r/20210216190743.GA18370@24bbad8f3778/)

* [`vmlinux.lds.h: catch more UBSAN symbols into .data`](https://lore.kernel.org/r/20210216185447.GA64303@24bbad8f3778/)

* [`ARM: Implement Clang's SLS mitigation (v1)`](https://lore.kernel.org/r/20210212055508.GA3822196@ubuntu-m3-large-x86/)

* [`ARM: Implement Clang's SLS mitigation (v3)`](https://lore.kernel.org/r/20210219203053.GA53507@24bbad8f3778/)

* [`ARM: kprobes: rewrite test-arm.c in UAL`](https://lore.kernel.org/r/20210211174307.GA2804604@ubuntu-m3-large-x86/)

* [`x86: remove toolchain check for X32 ABI capability`](https://lore.kernel.org/r/20210228064936.zixrhxlthyy6fmid@24bbad8f3778/)

* [`linux/compiler-clang.h: define HAVE_BUILTIN_BSWAP*`](https://lore.kernel.org/r/20210225192941.GA2026@MSI.localdomain/)

* [`Makefile: reuse CC_VERSION_TEXT`](https://lore.kernel.org/r/20210224171740.GA7180@24bbad8f3778/)

* [`firmware_loader: Align .builtin_fw to 8`](https://lore.kernel.org/r/20210205190834.GC461042@localhost/)

* [`iwlwifi: mvm: add terminate entry for dmi_system_id tables`](https://lore.kernel.org/r/20210226210640.GA21320@MSI.localdomain/)

## Issue triage and reporting

As a maintainer, it is important to address issues that are reported to you as well as report issues to other people when you do not have the expertise to deal with them yourself. ClangBuiltLinux is at the intersection of three major projects: the Linux kernel, the LLVM compiler collection, and QEMU (as we use it for boot testing the kernels we build in our CI). We work with a few major continuous integration systems such as KernelCI, 
Continuous Kernel Integration (CKI), and the Oday team from Intel, which results in reports that need to be looked at.

I will not go into too many details but here are some of the issues that I responded to or reported during February.

* [`clang-12: error: unsupported option '--64`](https://github.com/ClangBuiltLinux/linux/issues/1289)

* [`-Wframe-larger-than in= in arch/powerpc/kvm/book3s_hv_nested.c`](https://github.com/ClangBuiltLinux/linux/issues/1292)

* [`__field_overflow in drivers/net/ipa/ipa_main.c with clang-10`](https://github.com/ClangBuiltLinux/linux/issues/1293)

* [`Assertion in mm/memcontrol.c when building x86_64 allyesconfig with -O3`](https://github.com/ClangBuiltLinux/linux/issues/1295)

* [`FAILED unresolved symbol vfs_truncate`](https://github.com/ClangBuiltLinux/linux/issues/1297) ([corresponding LKML thread](https://lore.kernel.org/r/20210209034416.GA1669105@ubuntu-m3-large-x86/))

* [`FAIL: Test report for kernel 5.11.0-rc7 (mainline.kernel.org-clang)`](https://groups.google.com/g/clang-built-linux/c/roX6-wUVhKQ/m/BcUNw3sKBQAJ)

* [`invalid syntax in conditional- Kernel 5.10.14 on OpenSUSE 15.2 with AOCC`](https://github.com/ClangBuiltLinux/linux/issues/1298)

* [`drm/amd/display: Add Freesync HDMI support to DM`](https://lore.kernel.org/r/20210218223158.GA52356@24bbad8f3778/)

* [`gcc invoked even with passing CC=clang to make`](https://github.com/ClangBuiltLinux/linux/issues/1303)

* [`x86 ThinLTO: BUG: unable to handle page fault`](https://groups.google.com/g/clang-built-linux/c/m8KE8TcOBKw/m/iBHtAqpNCwAJ)

* [`FAIL: Test report for kernel 5.11.0 (mainline.kernel.org-clang)`](https://groups.google.com/g/clang-built-linux/c/aAfupPOatvY/m/MRjBRzOYBQAJ)

* [`PANICKED: Test report for kernel 5.11.0-rc6 (mainline.kernel.org-clang)`](https://groups.google.com/g/clang-built-linux/c/de_mNh23FOc/m/SKdJcUlKBgAJ)

* [`Assertion 'ContinueAfterFailure && "Shouldn't have kept evaluating on failure."' failed.`](http://llvm.org/PR49239#c5)

## Tooling patches

ClangBuiltLinux has a few different repos that we maintain to ease our workload. The first of which is the [boot-utils repo](https://github.com/ClangBuiltLinux/boot-utils), which houses a few scripts to make booting the Linux kernels that we build in QEMU easier. QEMU is a rather unwieldy program, as it has a LOT of flags and it is not immediately obvious which ones should and should not be used for testing. We use this locally on our workstations as well as in our continuous integration (more on that in a second). This month I contributed:

* [`boot-utils: Add RISC-V to use '--use-cbl-qemu'`](https://github.com/ClangBuiltLinux/boot-utils/pull/33)

* [`Update to latest stable buildroot and add arm64 big endian`](https://github.com/ClangBuiltLinux/boot-utils/pull/34)

* [`boot-utils: Add support for booting Debian images`](https://github.com/ClangBuiltLinux/boot-utils/pull/35)

* [`boot-utils: Only allow '--use-cbl-qemu' with s390`](https://github.com/ClangBuiltLinux/boot-utils/pull/36)

To help us easily track regressions in the kernel, we have started relying very heavily on [TuxSuite](https://tuxsuite.com/) and GitHub Actions for our [continuous integration](https://github.com/ClangBuiltLinux/continuous-integration2). This month I spent a lot of time trying to get us more coverage of various trees and configurations that have proved problematic in the past. In its current state, ClangBuiltLinux is in the "turning on the lights" phase (starting up), this is a major step in the "keeping them running" phase (stabilizing), and once that is stable, we can focus on adding features and polish as people request them (new development).

* [`Re-enable boot on 4.14 for x86_64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/67)

* [`Add support for testing CONFIG_THUMB2_KERNEL`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/68)

* [`ci2: Rework YAML generation`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/70)

* [`Enable booting MIPS and PowerPC kernels `](https://github.com/ClangBuiltLinux/continuous-integration2/pull/73)

* [`ci2: Regenerate android-4.14 TuxSuite and workflow file`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/74)

* [`ci2: Add all*config builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/75)

* [`Add "all" target to generate.sh`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/77)

* [`Enable some more -next builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/78)

* [`llvm_tot is now 13`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/79)

* [`Disable pseries_defconfig on LLVM 13`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/80)

* [`Add support for LLVM 12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/81)

* [`Enable more some more configs on 5.10 and next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/82)

* [`Disable certain PowerPC builds with LLVM 11`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/83)

* [`Add Sami's LTO/CFI tree`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/84)

* [`Add support for arm64 big endian`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/85)

* [`Enable ld.lld for mips big endian`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/86)

* [`Add support for testing -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/88)

* [`Move back to Ubuntu's qemu-system-riscv64`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/89)

* [`Disable x86_64 all{mod,yes}config for clang-10 on mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/90)

* [`Enable LTO testing on mainline and -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/94)

I maintain [a set of Python build scripts for LLVM and binutils](https://github.com/ClangBuiltLinux/tc-build) so that people can easily test with the latest versions of these tools in a contained environment (by default, the script does not touch anything outside of the repository). A small set of updates to these this month for improved testing and verification:

* [`Linux 5.10.14 and binutils 2.36.1`](https://github.com/ClangBuiltLinux/tc-build/pull/139)

* [`ci.sh: Use release/12.x for build testing`](https://github.com/ClangBuiltLinux/tc-build/pull/140)/

Lastly, we have had to ship a couple of [QEMU binaries](https://github.com/ClangBuiltLinux/qemu-binaries) to get access to features that we need for testing or to work around issues with distribution versions of QEMU. The work in this area this month was concerned with the latter. Ubuntu shipped a broken `qemu-system-riscv64` binary, which I actually fixed upstream in the 5.0.1 stable series, but Ubuntu misses that fix in a set of security backports to 4.2. I shipped a fixed binary in this repo for our CI, [reported the issue to Ubuntu upstream](https://bugs.launchpad.net/bugs/1914883), then reverted it in our repo when it was available.

* [`Add qemu-system-riscv64`](https://github.com/ClangBuiltLinux/qemu-binaries/pull/2)

* [`qemu-binaries: Remove copy of qemu-system-riscv64`](https://github.com/ClangBuiltLinux/qemu-binaries/pull/3)

## Conclusion

I hope this has been interesting to follow along with. I am very excited that I get paid to do this full time now. Going foward, I am going to try [streaming on Twitch](https://twitch.tv/nathanchance) once a week to try and get people involved with Linux kernel development, so that they can see that it is a rewarding path and that it is possible to succeed in it. If you have any questions, feel free to reach out to me using any of the information on my home page!