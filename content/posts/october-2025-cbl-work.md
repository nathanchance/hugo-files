---
title: October 2025 ClangBuiltLinux Work
date: 2025-10-31T16:30:00-0700
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

  * `x86/mm: Ensure clear_page() variants always have __kcfi_typeid_ symbols` ([`v1`](https://lore.kernel.org/20251013-x86-fix-clear_page-cfi-full-lto-errors-v1-1-d69534c0be61@kernel.org/))
  * `KMSAN: Restore dynamic check for '-fsanitize=kernel-memory'` ([`v1`](https://lore.kernel.org/20251023-fix-kmsan-check-s390-clang-v1-1-4e6df477a4cc@kernel.org/))
  * `Resolve ARM kCFI build failure in idpf xsk.c` ([`v1`](https://lore.kernel.org/20251025-idpf-fix-arm-kcfi-build-error-v1-0-ec57221153ae@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `scripts/Makefile.extrawarn: Respect CONFIG_WERROR / W=e for hostprogs` ([`v1`](https://lore.kernel.org/20251006-kbuild-hostprogs-werror-fix-v1-1-23cf1ffced5c@kernel.org/))
  * `kbuild: Fixes for fallout from recent modules.builtin.modinfo series` ([`v1`](https://lore.kernel.org/20251008-kbuild-fix-modinfo-regressions-v1-0-9fc776c5887c@kernel.org/))
  * `kbuild: Use '--strip-unneeded-symbol' for removing module device table symbols` ([`v1`](https://lore.kernel.org/20251010-kbuild-fix-mod-device-syms-reloc-err-v1-1-6dc88143af25@kernel.org/))
  * `powerpc/vmlinux.lds: Drop .interp description` ([`v1`](https://lore.kernel.org/20251018-ppc-fix-lld-interp-v1-1-a083de6dccc9@kernel.org/))
  * `jfs: Rename _inline to avoid conflict with clang's '-fms-extensions'` ([`v1`](https://lore.kernel.org/20251023-jfs-fix-conflict-with-clang-ms-ext-v1-1-e219d59a1e68@kernel.org/))
  * `KMSAN: Restore dynamic check for '-fsanitize=kernel-memory'` ([`v1`](https://lore.kernel.org/20251023-fix-kmsan-check-s390-clang-v1-1-4e6df477a4cc@kernel.org/))
  * `kbuild: Rename Makefile.extrawarn to Makefile.warn` ([`v1`](https://lore.kernel.org/20251023-rename-scripts-makefile-extrawarn-v1-1-8f7531542169@kernel.org/))
  * `kbuild: uapi: Drop types.h check from headers_check.pl` ([`v1`](https://lore.kernel.org/20251023-headers-pl-drop-check-sizes-v1-1-18bd21cf8f5e@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `PCI: rcar-host: Add OF Kconfig dependency to avoid objtool no-cfi warning` ([`v1`](https://lore.kernel.org/20251013-rcar_pcie_probe-avoid-nocfi-objtool-warning-v1-1-552876b94f04@kernel.org/), [`v2`](https://lore.kernel.org/20251014-rcar_pcie_probe-avoid-nocfi-objtool-warning-v2-1-6e0204b002c6@kernel.org/))
  * `i2c: designware: Remove i2c_dw_remove_lock_support()` ([`v1`](https://lore.kernel.org/20251013-dw_i2c_plat_remove-avoid-objtool-no-cfi-warning-v1-1-8cc4842967bf@kernel.org/))
  * `smb: client: Fix format specifiers for size_t in parse_dfs_referrals()` ([`v1`](https://lore.kernel.org/20251014-smb-client-fix-wformat-32b-parse_dfs_referrals-v1-1-47fa7db66b71@kernel.org/))
  * `net/mlx5e: Return 1 instead of 0 in invalid case in mlx5e_mpwrq_umr_entry_size()` ([`v1`](https://lore.kernel.org/20251014-mlx5e-avoid-zero-div-from-mlx5e_mpwrq_umr_entry_size-v1-1-dc186b8819ef@kernel.org/))
  * `usb: uhci: Work around bogus clang shift overflow warning from DMA_BIT_MASK(64)` ([`v1`](https://lore.kernel.org/20251014-usb-uhci-avoid-bogus-clang-shift-warning-v1-1-826585eed055@kernel.org/), [`v2`](https://lore.kernel.org/20251015-usb-uhci-avoid-bogus-clang-shift-warning-v2-1-68532d2f6114@kernel.org/))
  * `HID: intel-ish-hid: Fix -Wcast-function-type-strict in devm_ishtp_alloc_workqueue()` ([`v1`](https://lore.kernel.org/20251022-ishtp-fix-function-cast-warn-v1-1-bfb06464f8ca@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH v2] kbuild: uapi: Strip comments before size type check`](https://lore.kernel.org/20251006180201.GA429708@ax162/)
* [`Re: [PATCH] net/mlx5: fix pre-2.40 binutils assembler error`](https://lore.kernel.org/20251006190851.GB2406882@ax162/)
* [`Re: [PATCH 2/2] kbuild: userprogs: also inherit byte order and ABI from kernel`](https://lore.kernel.org/20251006191314.GA2706650@ax162/)
* [`Re: [PATCH AUTOSEL 6.17-5.4] x86/build: Remove cc-option from stack alignment flags`](https://lore.kernel.org/20251006215505.GB3234160@ax162/)
* [`Re: [PATCH] tools headers: kcfi: rename missed CONFIG_CFI_CLANG`](https://lore.kernel.org/20251006220500.GC3234160@ax162/)
* [`Re: [PATCH v2] kbuild: uapi: Strip comments before size type check`](https://lore.kernel.org/20251006180201.GA429708@ax162/)
* [`Re: [PATCH v2 4/5] kbuild: uapi: upgrade check_sizetypes() warning to error`](https://lore.kernel.org/20251006190048.GA2395186@ax162/)
* [`Re: [PATCH v2] gen_init_cpio: Ignore fsync() returning EINVAL on pipes`](https://lore.kernel.org/175985615642.1365513.6759583402363005234.b4-ty@kernel.org/)
* [`Re: [PATCH] scripts/clang-tools: Handle included .c files in gen_compile_commands`](https://lore.kernel.org/20251007163338.GA547361@ax162/)
* [`Re: [PATCH v2 1/1] tick/nohz: avoid showing '(null)' if nohz_full= not set`](https://lore.kernel.org/20251010032819.GA3743688@ax162/)
* [`Re: [PATCH] kbuild: uapi: reuse KBUILD_USERCFLAGS`](https://lore.kernel.org/20251015001921.GA2263607@ax162/)
* [`Re: [PATCH] kbuild: Use objtree for module signing key path`](https://lore.kernel.org/20251015230712.GA3943617@ax162/)
* [`Re: [PATCH v9 2/4] tracing: Add a tracepoint verification check at build time`](https://lore.kernel.org/20251015231219.GB3943617@ax162/)
* [`Re: [PATCH v9 4/4] tracing: Add warnings for unused tracepoints for modules`](https://lore.kernel.org/20251015231928.GC3943617@ax162/)
* [```[support] Don't require VFS in `SourceMgr` for loading includes```](https://github.com/llvm/llvm-project/pull/163862#issuecomment-3415793827)
* [`Re: [PATCH] scripts: Add check-build-warnings.pl for tracking kernel build warnings`](https://lore.kernel.org/20251017212214.GA2776486@ax162/)
* [`Re: [PATCH 2/2] drm/xe/configfs: fix clang warnings for missing parameter name`](https://lore.kernel.org/20251021063957.GA757076@ax162/)
* [`Re: [PATCH] modpost: drop '*_probe' from section check whitelist`](https://lore.kernel.org/20251022203955.GA3256590@ax162/)
* [`Re: [PATCH 1/2] Kbuild: enable -fms-extensions`](https://lore.kernel.org/20251022161505.GA1226098@ax162/)
* [`Re: [PATCH] fs/pipe: stop duplicating union pipe_index declaration`](https://lore.kernel.org/20251023164408.GB2090923@ax162/)
* [`Re: [PATCH] kbuild: align modinfo section for Secureboot Authenticode EDK2 compat`](https://lore.kernel.org/20251026215635.GA2368369@ax162/)
* [`Re: [PATCH v1] pinctrl: mpfs-iomux0: fix compile-time constant warning for LLVM prior to 17`](https://lore.kernel.org/20251029172036.GA1130586@ax162/)
* [`Re: [PATCH] dma-mapping: Allow use of DMA_BIT_MASK(64) in global scope`](https://lore.kernel.org/20251030184728.GA694763@ax162/)
* [`Re: [PATCH] drm/tiny: pixpaper: Prevent inlining of send helpers to reduce stack usage`](https://lore.kernel.org/20251031203446.GD2486902@ax162/)
* [`Re: [PATCH v2] scripts/clang-tools: Handle included .c files in gen_compile_commands`](https://lore.kernel.org/20251031225605.GA2874962@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`BUILD_BUG_ON failed: !__builtin_constant_p(tmo == libeth_xsktmo) with ARCH=arm allmodconfig`](https://github.com/ClangBuiltLinux/linux/issues/2124)
* [`Re: [PATCH net-next V6] net/mlx5: Improve write-combining test reliability for ARM64 Grace CPUs`](https://lore.kernel.org/20251001163655.GA370262@ax162/)
* [`Re: [PATCH v4 1/2] platform/x86: Add Uniwill laptop driver`](https://lore.kernel.org/20251002233627.GA3978676@ax162/)
* [`Semantic conflict around CONFIG_CFI_CLANG between Linus's tree and x86/core`](https://lore.kernel.org/20251003221030.GA1162775@ax162/)
* [```Re: next-20251002: S390: gcc-8-defconfig: symbol `.modinfo' required but not present - no symbols```](https://lore.kernel.org/20251006234114.GA659425@ax162/)
* [`Presence of multiple versions in /usr/lib/gcc/x86_64-linux-gnu in sparc images causes clang warning`](https://gitlab.com/Linaro/tuxmake/-/issues/226)
* [`Crash with full LTO when linking Nouveau driver after LLVM commit bbdcba9b851ab`](https://github.com/ClangBuiltLinux/linux/issues/2126)
* [`Re: [REGRESSION][BISECTED] kbuild: CFLAGS=-w no longer works`](https://lore.kernel.org/20251009174120.GA442207@ax162/)
* [`Re: [tip:x86/core 1/1] vmlinux.o: warning: objtool: rcar_pcie_probe+0x13e: no-cfi indirect call!`](https://lore.kernel.org/20251010032001.GA3741500@ax162/)
* [`Re: Building a allyesconfig kernel fails because macros are being redefined`](https://lore.kernel.org/20251013012423.GA331@ax162/)
* [`Re: [PATCH] arch/powerpc: Remove .interp section in vmlinux`](https://lore.kernel.org/20251015002154.GA2300901@ax162/)
* [`Apply 2f13daee2a72 ("lib/crypto/curve25519-hacl64: Disable KASAN with clang-17 and older") to 6.12`](https://lore.kernel.org/20251015003231.GA2336835@ax162/)
* [`Add support for flag output operand "=@cc" for SystemZ.`](https://github.com/llvm/llvm-project/pull/125970#issuecomment-3407355089)
* [`Re: clang 15 build error`](https://lore.kernel.org/20251015165031.GA1465138@ax162/)
* [`Re: [BUG] kbuild: modules.builtin is empty on architectures without CONFIG_ARCH_VMLINUX_NEEDS_RELOCS`](https://lore.kernel.org/20251015172906.GA1630960@ax162/)
* [```[support] Use VFS in `SourceMgr` for loading includes```](https://github.com/llvm/llvm-project/pull/162903#issuecomment-3412606066)
* [`Re: [PATCHv6] io_uring: add support for IORING_SETUP_SQE_MIXED`](https://lore.kernel.org/20251022120030.GA148714@ax162/)
* [`Re: [PATCH 6.17 000/563] 6.17.3-rc1 review`](https://lore.kernel.org/20251022130852.GA1221362@ax162/)
* [`Re: [PATCH v3 4/4] leds: Add virtualcolor LED group driver`](https://lore.kernel.org/20251022161957.GA1228040@ax162/)
* [`[ARM][KCFI] Add backend support for Kernel Control-Flow Integrity`](https://github.com/llvm/llvm-project/pull/163698#issuecomment-3438623785)
* [`Re: Can't boot kernel 6.17.4+ via rEFInd`](https://lore.kernel.org/20251026211334.GA1659905@ax162/)
* [`Re: [Linaro-TCWG-CI] v6.17.3-130-g86f364ee5842: Failure on aarch64`](https://lore.kernel.org/20251028223221.GA1306180@ax162/)
* [`Re: [PATCH V3 2/5] ublk: implement NUMA-aware memory allocation`](https://lore.kernel.org/20251030175631.GB417112@ax162/)
* [`Re: [PATCH v3] riscv: asm: use .insn for making custom instructions`](https://lore.kernel.org/20251031031009.GA2891125@ax162/)
* [`Re: [tip: objtool/core] objtool/klp: Add --debug option to show cloning decisions`](https://lore.kernel.org/20251031140944.GA3022331@ax162/)
* [`Re: [PATCH] btrfs: make ASSERT no-op in release builds`](https://lore.kernel.org/20251031203746.GE2486902@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update PGO kernel to 6.17 and bump known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/310)
* [`patches: Drop 7fa37ba25a1dfc084e24ea9acc14bf1fad8af14c from mainline and -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/882)
* [`Drop support for LLVM 13 and 14 from mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/883)
* [`Update patches (October 13, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/884)
* [`Python linting updates and fixes`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/885)
* [`[boot-utils] .github/workflows: Lint with Python 3.13 and 3.14`](https://github.com/ClangBuiltLinux/boot-utils/pull/124)
* [`[tc-build] .github/workflows: Lint with Python 3.14`](https://github.com/ClangBuiltLinux/tc-build/pull/311)
* [`patches: Drop 7fa37ba25a1dfc084e24ea9acc14bf1fad8af14c from stable and 6.12`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/886)
* [`Update patches (October 30, 2025)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/887)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and an AMD-based device. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [21.1.3](https://lore.kernel.org/20251008000816.GA1856596@ax162/)
  * [21.1.4](https://lore.kernel.org/20251022111343.GA3803433@ax162/)

* [`[GIT PULL] Kbuild fixes for 6.18 #1`](https://lore.kernel.org/20251011194325.GA1123518@ax162/)
* [`[GIT PULL] Kbuild fixes for 6.18 #2`](https://lore.kernel.org/20251101051443.GA3427600@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
