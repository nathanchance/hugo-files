---
title: November 2025 ClangBuiltLinux Work
date: 2025-11-28T17:30:00-0700
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

  * `riscv: Fix CONFIG_AS_HAS_INSN for new .insn usage` ([`v1`](https://lore.kernel.org/20251107-riscv-fix-new-insn-usage-v1-1-9a186c5928a0@kernel.org/))

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `kbuild: Add '-fms-extensions' to areas with dedicated CFLAGS` ([`v1`](https://lore.kernel.org/20251101-kbuild-ms-extensions-dedicated-cflags-v1-1-38004aba524b@kernel.org/))
  * `lib/crypto: curve25519-hacl64: Fix older clang KASAN workaround for GCC` ([`v1`](https://lore.kernel.org/20251102-curve25519-hacl64-fix-kasan-workaround-v1-1-6ec6738f9741@kernel.org/), [`v2`](https://lore.kernel.org/20251103-curve25519-hacl64-fix-kasan-workaround-v2-1-ab581cbd8035@kernel.org/))
  * `kbuild: Strip trailing padding bytes from modules.builtin.modinfo` ([`v1`](https://lore.kernel.org/20251105-kbuild-fix-builtin-modinfo-for-kmod-v1-1-b419d8ad4606@kernel.org/))
  * `clk: samsung: exynos-clkout: Assign .num before accessing .hws` ([`v1`](https://lore.kernel.org/20251124-exynos-clkout-fix-ubsan-bounds-error-v1-1-224a5282514b@kernel.org/))
  * `tty: vt/keyboard: Split apart vt_do_diacrit()` ([`v1`](https://lore.kernel.org/20251125-tty-vt-keyboard-wa-clang-scope-check-error-v1-1-f5a5ea55c578@kernel.org/))

* Stable backports and fixes: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`[PATCH 5.10] Makefile.compiler: replace cc-ifversion with compiler-specific macros`](https://lore.kernel.org/20251104181427.3261962-1-nathan@kernel.org/)
  * [`[PATCH 5.15] Makefile.compiler: replace cc-ifversion with compiler-specific macros`](https://lore.kernel.org/20251104181446.3262126-1-nathan@kernel.org/)
  * [`[PATCH 6.17.y] kbuild: Strip trailing padding bytes from modules.builtin.modinfo`](https://lore.kernel.org/20251110223818.3951521-1-nathan@kernel.org/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `ARM: dts: omap: am335x-mba335x: Fix stray '/*' in comment` ([`v1`](https://lore.kernel.org/20251105-omap-mba335x-fix-clang-comment-warning-v1-1-9b671ee2cb93@kernel.org/), [`v2`](https://lore.kernel.org/20251105-omap-mba335x-fix-clang-comment-warning-v2-1-f8a0003e1003@kernel.org/))
  * `net: netcp: ethss: Fix type of first parameter in hwtstamp stubs` ([`v1`](https://lore.kernel.org/20251107-netcp_ethss-fix-cpts-stubs-clang-wifpts-v1-1-a80a30c429a8@kernel.org/), [`v2`](https://lore.kernel.org/20251110-netcp_ethss-fix-cpts-stubs-clang-wifpts-v2-1-aa6204ec1f43@kernel.org/))
  * `pinctrl: airoha: Fix AIROHA_PINCTRL_CONFS_DRIVE_E2 in an7583_pinctrl_match_data` ([`v1`](https://lore.kernel.org/20251112-pinctrl-airoha-fix-an7583-drive-e2-confg-usage-v1-1-d2550d55e988@kernel.org/))
  * `backlight: aw99706: Fix unused function warnings from suspend/resume ops` ([`v1`](https://lore.kernel.org/20251120-backlight-aw99706-fix-unused-pm-functions-v1-1-8b9c17c4e783@kernel.org/))
  * `KVM: arm64: Add break to default case in kvm_pgtable_stage2_pte_prot()` ([`v1`](https://lore.kernel.org/20251125-arm64-kvm-hyp-pgtable-fix-c23-ext-warn-v1-1-98b506ddefbf@kernel.org/))
  * `drm/mediatek: mtk_hdmi_v2: Fix return type of mtk_hdmi_v2_tmds_char_rate_valid()` ([`v1`](https://lore.kernel.org/20251125-drm-mediatek-hdmi-v2-wifpts-v1-1-a6c7582cf69a@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] tools/objtool: Copy the __cleanup unused variable fix for older clang`](https://lore.kernel.org/20251101164329.GA3250327@ax162/)
* [`Re: [PATCH] Makefile: Let kernel-doc.py use PYTHON3 override`](https://lore.kernel.org/20251103185609.GB672460@ax162/)
* [`Re: [PATCH 2/2] macintosh/via-pmu-backlight: Include linux/of.h and uapi/linux/fb.h`](https://lore.kernel.org/20251104170104.GA1416336@ax162/)
* [`Re: [PATCH] scripts: headers_install.sh: Remove two outdated config leak ignore entries`](https://lore.kernel.org/20251106043540.GA909744@ax162/)
* [`Re: [PATCH v1 1/1] compiler_types: Warn about unused static inline functions on second`](https://lore.kernel.org/20251106151649.GA1693433@ax162/)
* [`Re: [PATCH v2] Makefile: Let kernel-doc.py use PYTHON3 override`](https://lore.kernel.org/20251107184338.GA502997@ax162/)
* [`Re: [PATCH] kbuild: install-extmod-build: Properly fix CC expansion when ccache is used`](https://lore.kernel.org/20251109234124.GC2977577@ax162/)
* [`Re: [PATCH] Support conditional deps using "depends on X if Y"`](https://lore.kernel.org/20251109232922.GA2977577@ax162/)
* [`Re: [PATCH] selftest/mm: fix pointer comparison in mremap_test`](https://lore.kernel.org/20251110214349.GC302594@ax162/)
* [`Re: [PATCH v2] kbuild: install-extmod-build: Properly fix CC expansion when ccache is used`](https://lore.kernel.org/176292578048.1288921.4035245057559031915.b4-ty@kernel.org/)
* [`Re: [PATCH] fortify: Ignore intermediate *.tmp files`](https://lore.kernel.org/20251112223606.GA3268585@ax162/)
* [`Re: [PATCH] modpost: drop '*_probe' from section check whitelist`](https://lore.kernel.org/20251114041628.GA2566209@ax162/)
* [`Re: [PATCH v2 00/10] kbuild: userprogs: introduce architecture-specific CC_CAN_LINK and userprog flags`](https://lore.kernel.org/20251114042254.GB2566209@ax162/)
* [`Re: [PATCH v3 00/35] Compiler-Based Capability- and Locking-Analysis`](https://lore.kernel.org/20251114043812.GC2566209@ax162/)
* [`Re: [PATCH] kbuild: add target to build a cpio containing modules`](https://lore.kernel.org/20251120063936.GA3321365@ax162/)
* [`Re: [PATCH] kbuild: Enable GCC diagnostic context for value-tracking warnings`](https://lore.kernel.org/20251120064923.GA3320872@ax162/)
* [`Re: [PATCH 1/2] KVM: arm64: Drop useless __GFP_HIGHMEM from kvm struct allocation`](https://lore.kernel.org/20251120070848.GA1879143@ax162/)
* [`Re: [PATCH] kbuild: fix compilation of dtb specified on command-line without make rule`](https://lore.kernel.org/20251121064256.GB571346@ax162/)
* [`Re: [PATCH v2] kbuild: Support directory targets for building DTBs`](https://lore.kernel.org/20251121063033.GA571346@ax162/)
* [`Re: [PATCH v2] kbuild: Enable GCC diagnostic context for value-tracking warnings`](https://lore.kernel.org/20251121222150.GB1674270@ax162/)
* [`Re: [PATCH v2] lib/crypto: blake2b: Limit frame size workaround to GCC`](https://lore.kernel.org/20251123211317.GA3667167@ax162/)
* [`Re: [PATCH v1] kbuild: Add KBUILD_VMLINUX_LIBS_PRELINK`](https://lore.kernel.org/20251121070140.GA780042@ax162/)
* [`Re: [PATCH v4 3/4] rust: add a Kconfig function to test for support of bindgen options`](https://lore.kernel.org/20251124183634.GA1084995@ax162/)
* [`Re: [PATCH 1/1] media: ccs: Avoid possible division by zero`](https://lore.kernel.org/20251125232438.GA193422@ax162/)
* [`Re: [PATCH v2 0/2] kbuild: add target to build a cpio containing modules`](https://lore.kernel.org/20251125235617.GA296211@ax162/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH 5.10 171/332] lib/crypto/curve25519-hacl64: Disable KASAN with clang-17 and older`](https://lore.kernel.org/20251103022044.GA2193668@ax162/)
* [`Re: [mainline]Error while running make modules_install command`](https://lore.kernel.org/20251105005603.GA769905@ax162/)
* [`Re: linux-next: new objtool warnings`](https://lore.kernel.org/20251105170715.GA706366@ax162/)
* [`Re: [PATCH 5.10 239/332] fsdax: switch dax_iomap_rw to use iomap_iter`](https://lore.kernel.org/20251106042725.GA826592@ax162/)
* [`[LoongArch] Add isSafeToMove hook to prevent unsafe instruction motion`](https://github.com/llvm/llvm-project/pull/163725#issuecomment-3514363804)
* [`Re: [akpm-mm:mm-unstable 36/283] mm/hugetlb.c:4753:18: warning: implicit conversion from 'unsigned long long' to 'unsigned long' changes value from 17179869184 to 0`](https://lore.kernel.org/20251114182956.GD2566209@ax162/)
* [`Re: [Linaro-TCWG-CI] v6.18-rc1-73-g3ffeb17a9a27a: Failure on aarch64`](https://lore.kernel.org/20251117162457.GA564441@ax162/)
* [`Re: Objtool segfault inside an VM, based on commit 24172e0d7990`](https://lore.kernel.org/20251117175041.GA703155@ax162/)
* [`Re: [PATCH v3 1/4] mm/vmalloc: warn on invalid vmalloc gfp flags`](https://lore.kernel.org/20251118224448.GA998046@ax162/)
* [`Re: [v2 PATCH] arm64: mm: make linear mapping permission update more robust for patial range`](https://lore.kernel.org/20251118164115.GA3977565@ax162/)
* [`Re: Re: [PATCH] riscv: fix KUnit test_kprobes crash when building with Clang`](https://lore.kernel.org/20251119013808.GA1264583@ax162/)
* [`Re: [PATCH libcrypto 2/2] crypto: chacha20poly1305: statically check fixed array lengths`](https://lore.kernel.org/20251119184500.GA3303394@ax162/)
* [`Re: [tip: sched/core] sched/fair: Skip sched_balance_running cmpxchg when balance is not due`](https://lore.kernel.org/20251121062600.GA256626@ax162/)
* [`Re: [patch V5 09/20] cpumask: Cache num_possible_cpus()`](https://lore.kernel.org/20251122002755.GA2682494@ax162/)
* [`Re: [patch V5 20/20] sched/mmcid: Switch over to the new mechanism`](https://lore.kernel.org/20251122004358.GB2682494@ax162/)
* [`Re: linux-next: boot warning from the final tree`](https://lore.kernel.org/20251121215819.GA1374726@ax162/)
* [`Re: drivers/media/i2c/ccs/ccs.o: error: objtool: ccs_set_selection(): unexpected end of section .text.ccs_set_selection`](https://lore.kernel.org/20251122013414.GA3094872@ax162/)
* [`Re: Bug#1121211: UBSAN: array-index-out-of-bounds in /build/reproducible-path/linux-6.17.8/drivers/clk/samsung/clk-exynos-clkout.c:178:18`](https://lore.kernel.org/20251122203856.GA1099833@ax162/)
* [`Re: [tty:tty-next 27/37] drivers/tty/vt/keyboard.c:1712:7: error: cannot jump from this asm goto statement to one of its possible targets`](https://lore.kernel.org/20251124221343.GA2953435@ax162/)
* [`Re: [REGRESSION] stable-rc/linux-6.12.y: (build) variable 'val' is uninitialized when passed as a const pointer arg...`](https://lore.kernel.org/20251124220404.GA2853001@ax162/)
* [`Re: [PATCH v3 04/15] KVM: arm64: Teach ptdump about FEAT_XNX permissions`](https://lore.kernel.org/20251125173929.GA3256322@ax162/)
* [`Re: [PATCH v6 03/30] objtool: Disassemble code with libopcodes instead of running objdump`](https://lore.kernel.org/20251125181604.GA3595606@ax162/)
* [`Re: [tip:core/rseq 25/39] include/linux/rseq_entry.h:132:3: error: invalid operand for instruction`](https://lore.kernel.org/20251125231834.GA4012217@ax162/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`patches: Drop 7fa37ba25a1dfc084e24ea9acc14bf1fad8af14c from android-mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/888)
* [`Use GitHub mirror of rpms/kernel for Fedora configs`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/889)
* [`boot-qemu.py: Increase default memory to 1G`](https://github.com/ClangBuiltLinux/boot-utils/pull/126)
* [`boot-qemu.py: Allow user to supply their own ramdisk`](https://github.com/ClangBuiltLinux/boot-utils/pull/127)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, four Intel-based devices, and an AMD-based device. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [21.1.5](https://lore.kernel.org/20251105062723.GA2341496@ax162/)
  * [21.1.6](https://lore.kernel.org/20251119033949.GA2155408@ax162/)

* [`[GIT PULL] Kbuild fixes for 6.18 #3`](https://lore.kernel.org/20251109025637.GA2001503@ax162/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
