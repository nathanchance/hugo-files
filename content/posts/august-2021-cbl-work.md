---
title: "August 2021 ClangBuiltLinux Work"
date: 2021-08-31T20:38:00-07:00
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

## Linux kernel patches

* [`[PATCH] dmaengine: idxd: Remove unused status variable in irq_process_work_list()`](https://lore.kernel.org/r/20210802175820.3153920-1-nathan@kernel.org/): Not a clang specific warning but it impacts builds where I use `-Werror`, as kernel builds should be as warning clean as possible.

* [`[PATCH] ASoC: Intel: boards: Fix CONFIG_SND_SOC_SDW_MOCKUP select`](https://lore.kernel.org/r/20210802190351.3201677-1-nathan@kernel.org/): Not a clang specific warning but Kconfig warnings such as this can potentially be build errors later due to incorrect dependency selection so it is important to fix them when they pop up.

* [`[PATCH] scripts/recordmcount.pl: Remove check_objcopy() and $can_use_local`](https://lore.kernel.org/r/20210802210307.3202472-1-nathan@kernel.org/): The kernel has certain version checks in place that do not always work with LLVM tools due to different version outputs. This was an easy cleanup because the kernel checks for certain tool versions before building, which provides an airtight reason to remove other version checks for versions older than the mandatory one.

* [`[PATCH] netfilter: ipset: Fix maximal range check in hash_ipportnet4_uadt()`](https://lore.kernel.org/r/20210803191813.282980-1-nathan@kernel.org/): A fix for a simple programming error but this was important to catch because the fixed patch was destined for the current merge window as it was in the netfilter tree, rather than netfilter-next tree, and it is a security patch.

* [`[PATCH] PCI: Always initialize dev in pciconfig_read`](https://lore.kernel.org/r/20210803200836.500658-1-nathan@kernel.org/): Another small fix for an uninitialized variable warning.

* [`[PATCH] cpuidle: pseries: Mark pseries_idle_proble() as __init`](https://lore.kernel.org/r/20210803211547.1093820-1-nathan@kernel.org/) | [`[PATCH] powerpc/xive: Do not mark xive_request_ipi() as __init`](https://lore.kernel.org/r/20210816185711.21563-1-nathan@kernel.org/): The kernel marks certain functions as "init", meaning that their memory is discarded once the kernel has fully loaded. As a result, the kernel has a tool that checks that all init functions are only called from other init functions and not functions that can run at any point. This commit fixes a warning that clang had because it did not inline a function like GCC did, which is a common reason for these warnings. Whether or not it is an actual issue in practice does not matter, it is important to clean these up to avoid spooky warnings.

* [`[PATCH 0/4] staging: r8188eu: Fix clang warnings`](https://lore.kernel.org/r/20210803223609.1627280-1-nathan@kernel.org/): A new version of another driver in the staging directory was introduced without being compiled with clang so this series cleans up the warnings that it found. Eventually, I hope that compiling with both compilers will become easy enough that everyone does it automatically but that is a long-term and lofty goal :)

* [`[PATCH] net: ethernet: stmmac: Do not use unreachable() in ipq806x_gmac_probe()`](https://lore.kernel.org/r/20210806191339.576318-1-nathan@kernel.org/): `objtool` is an object file validator for x86_64 (and maybe other architectures in the future), which warns when the assembly of a file might have potential issues, which may be compiler bugs. In this particular case, it was easy enough to just fix the code in question but this is most likely related to [another open issue](https://github.com/ClangBuiltLinux/linux/issues/1440) that we currently have.

* [`[PATCH] staging: r8188eu: os_dep: Hoist vmalloc.h include into osdep_service.h`](https://lore.kernel.org/r/20210811005505.3953122-1-nathan@kernel.org/): Another `ARCH=hexagon` build fix, which I have mentioned in previous posts is the only architecture in the tree that only compiles with LLVM, meaning we must keep it building.

* [`[PATCH] mm/hugetlb: Initialize page to NULL in alloc_buddy_huge_page_with_mpol()`](https://lore.kernel.org/r/20210810200632.3812797-1-nathan@kernel.org/): A common theme throughout all of these posts is fixing uninitialized variables, because Linus disabled GCC's `-Wmaybe-uninitialized` several releases ago because there are a lot of false positives due to GCC doing its analysis after inlining and optimization. Fun fact, I actually learned today that clang has a similar warning, `-Wconditional-uninitialized`, which in my testing appears to be just as noisy as GCC's, but it is off by default. `-Wsometimes-uninitialized` was [split off from it](https://github.com/llvm/llvm-project/commit/4323bf8e2e5135c49f814940b2b546298c01ecbc) and added to `-Wuninitialized` because it does not suffer from the same issues ("sometimes" is more definitive than "maybe").

* [`[PATCH 0/3] staging: r8188eu: Fix -Wuninitialized instances from clang`](https://lore.kernel.org/r/20210812204027.338872-1-nathan@kernel.org/): `-Wuninitialized` (a common GCC/clang flag) was disabled for this new driver and someone enabled it without testing it with `clang` so this series cleans up those warnings.

* [`[PATCH] drm/i915/selftest: Fix use of err in igt_reset_{fail, nop}_engine()`](https://lore.kernel.org/r/20210813171158.2665823-1-nathan@kernel.org/): Another `-Wuninitialized` warning :^)

* [`[PATCH] iwlwifi: mvm: Fix bitwise vs logical operator in iwl_mvm_scan_fits()`](https://lore.kernel.org/r/20210814234248.1755703-1-nathan@kernel.org/) | [`[PATCH] staging: rtl8192u: Fix bitwise vs logical operator in TranslateRxSignalStuff819xUsb()`](https://lore.kernel.org/r/20210814235625.1780033-1-nathan@kernel.org/) | [`[PATCH] lib/zstd: Fix bitwise vs logical operators`](https://lore.kernel.org/r/20210815004154.1781834-1-nathan@kernel.org/): [A proposed warning](https://reviews.llvm.org/D108003) in `clang` exposed a few instances where `&` was used instead of `&&` with boolean values. This is not always a bug but it is much clearer to use the logical operators because of short circuiting; `b` will not be evaluated in `if (a && b)` if `a` is false, whereas it will be in `if (a & b)`, which [caused a bug in ChromeOS](https://arstechnica.com/gadgets/2021/07/google-pushed-a-one-character-typo-to-production-bricking-chrome-os-devices/).

* [`[PATCH] bus: ti-sysc: Add break in switch statement in sysc_init_soc()`](https://lore.kernel.org/r/20210815191852.52271-1-nathan@kernel.org/) | [`[PATCH] drm/radeon: Add break to switch statement in radeonfb_create_pinned_object()`](https://lore.kernel.org/r/20210815192959.90142-1-nathan@kernel.org/) | [`[PATCH] scsi: st: Add missing break in switch statement in st_ioctl()`](https://lore.kernel.org/r/20210817235531.172995-1-nathan@kernel.org/): In the quest to enable `-Wimplicit-fallthrough`, which is a little bit more pedantic with clang over GCC ([a known difference which neither party is interested in changing](https://github.com/ClangBuiltLinux/linux/issues/636)), I sent a few patches to mark `case` statements with `break`. This version of the warning [can find bugs](https://lore.kernel.org/r/20201121124019.21116-1-ogabbay@kernel.org/) so it is important to get it enabled so CI systems can start catching them.

* [`Re: Patch "vmlinux.lds.h: Handle clang's module.{c,d}tor sections" has been added to the 5.13-stable tree`](https://lore.kernel.org/r/YRmbLz1ZivIMKgc5@archlinux-ax161/): A stable backport for a commit that I submitted upstream so that Android and ChromeOS do not regress.

* [`[PATCH] fs/ntfs3: Remove unused variable cnt in ntfs_security_init()`](https://lore.kernel.org/r/20210816193041.1164125-1-nathan@kernel.org/): A simple clean up of a variable that was unused.

* [`[PATCH 1/3] kbuild: Remove -Wno-format-invalid-specifier from clang block`](https://lore.kernel.org/r/20210816202056.4586-1-nathan@kernel.org/): Clean up of our clang warning section in the main Makefile, which is important for future travelers.

* [`[PATCH] kbuild: Switch to 'f' variants of integrated assembler flag`](https://lore.kernel.org/r/20210816203635.53864-1-nathan@kernel.org/): A small clean up of the flags used for the integrated assembler, as it has been brought up a few times in various code reviews.

* [`[PATCH] f2fs: Add missing inline to f2fs_sanity_check_cluster() stub`](https://lore.kernel.org/r/20210816234247.139528-1-nathan@kernel.org/): The kernel uses `inline` on stubs to avoid unused function warnings; this patch adds one that was missing.

* [`[PATCH 1/2] ALSA: hda/sigmatel - Sink stac_shutup() into stac_suspend()`](https://lore.kernel.org/r/20210818012705.311963-1-nathan@kernel.org/): `CONFIG_PM=n` builds are not often tested, which can cause warnings with suspend and resume functions. This was uncovered on s390, which [removed support for `CONFIG_PM`](https://git.kernel.org/linus/394216275c7d503d966317da9a01ad6626a6091d).

* [`[PATCH] rtlwifi: rtl8192de: Fix initialization of place in _rtl92c_phy_get_rightchnlplace()`](https://lore.kernel.org/r/20210823222014.764557-1-nathan@kernel.org/)

* [`[PATCH 0/2] Harden clang against unknown flag options`](https://lore.kernel.org/r/20210824022640.2170859-1-nathan@kernel.org/): There are some optimization flags that GCC has which clang does not implement and it issues a warning about that. When these flags are added to the compiler flags unconditionally, it causes all `cc-option` and `cc-disable-warning` calls (which test support for compiler flags) to fail, meaning that certain warnings or options do not get enabled/disabled, annoying developers. This series resolves the particular instance that Intel's Oday bot ran across in randconfig testing and makes sure that we catch these issues quicker in the future.

* [`[PATCH] cxgb4: Properly revert VPD changes`](https://lore.kernel.org/r/20210824205104.2778110-1-nathan@kernel.org/): A botched revert introduced some uninitialized variables so this commit resets the file properly.

* [`[PATCH 0/3] drm/i915: Enable -Wsometimes-uninitialized`](https://lore.kernel.org/r/20210824225427.2065517-1-nathan@kernel.org/): `-Wuninitialized` and `-Wsometimes-uninitialized` were disabled separately for i915 (because it enables `-Wall -Wextra` to help catch bugs), even though the latter is a sub-warning of the former (meaning that if `-Wuninitialized` is disabled, so is `-Wsometimes-uninitialized`). This series cleans up some instances of the warning then enables it again so that i915 gets coverage like the rest of the kernel.

* [`[PATCH] drm/i915: Clean up disabled warnings`](https://lore.kernel.org/r/20210824232237.2085342-1-nathan@kernel.org/): This patch came about from my investigation of the optimization flag issue that I mentioned above. This helps avoid the warnings because we eliminate a few `cc-disable-warning` calls.

* [`[PATCH] cxl/core: Avoid using dev uninitialized in devm_cxl_add_decoder()`](https://lore.kernel.org/r/20210825173301.358381-1-nathan@kernel.org/)

* [`[PATCH] crypto: sm4 - Do not change section of ck and sbox`](https://lore.kernel.org/r/20210825203859.416449-1-nathan@kernel.org/): A warning from GNU as that only appears when compiling with clang. Subtle but important to get right.

* [`[PATCH 1/3] MAINTAINERS: Update ClangBuiltLinux mailing list`](https://lore.kernel.org/r/20210825211823.6406-1-nathan@kernel.org/): We moved mailing lists so that we get archival along with the rest of the kernel through [lore.kernel.org](https://lore.kernel.org/lists.html) and we no longer have to deal with spam ourselves.

* [`[PATCH] x86/setup: Explicitly include acpi.h`](https://lore.kernel.org/r/20210901021510.1561219-1-nathan@kernel.org/): A build error that was introduced during the merge window, which is unfortunate because that is what linux-next is for.



## LLVM patches

* [`[clang] Expose unreachable fallthrough annotation warning`](https://github.com/llvm/llvm-project/commit/9ed4a94d6451046a51ef393cd62f00710820a7e8): The commit message is fairly self explanatory; this is necessary to enable clang's `-Wimplicit-fallthrough`.





## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH 3/3] isystem: delete global -isystem compile option`](https://lore.kernel.org/r/YQg2+C4Z98BMFucg@archlinux-ax161/)

* [`Re: [PATCH v6 3/3] Documentation/llvm: update CROSS_COMPILE inferencing`](https://lore.kernel.org/r/95712f4c-9da5-b7b6-f617-b6b686b6eadc@kernel.org/)

* [`Re: [PATCH v2] compiler_attributes.h: move __compiletime_{error|warning}`](https://lore.kernel.org/r/1847b77a-093a-ce59-5c3b-1a21d3bb66c7@kernel.org/)

* [`Re: [PATCH] slub: fix kmalloc_pagealloc_invalid_free unit test`](https://lore.kernel.org/r/YQiFNwdFb4DRwykI@archlinux-ax161/)

* [`LLVM build of RISCV kernel fails with relocation R_RISCV_PCREL_HI20 out of range`](https://github.com/ClangBuiltLinux/linux/issues/1409)

* [`Re: [PATCH v3] riscv: explicitly use symbol offsets for VDSO`](https://lore.kernel.org/r/615fafba-bdcc-7f13-483d-6f3ef405924c@kernel.org/)

* [`Re: [PATCH] kbuild: check CONFIG_AS_IS_LLVM instead of LLVM_IAS`](https://lore.kernel.org/r/59ce441e-8deb-39ff-700f-4e1c4e871177@kernel.org/)

* [`Re: [PATCH v2] scripts/Makefile.clang: default to LLVM_IAS=1`](https://lore.kernel.org/r/YQ2TGPwjvn8w4rKs@archlinux-ax161/)

* [`Re: [PATCH v2] clang-tools: Print information when clang-tidy tool is missing`](https://lore.kernel.org/r/YRBYHAJSpU5jcTQV@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] Makefile: remove stale cc-option checks`](https://lore.kernel.org/r/YRMFTm3EJWRqwZkM@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] x86/build: remove stale cc-option checks`](https://lore.kernel.org/r/cf568688-01a3-849e-2bcc-1cbe6845c9f8@kernel.org/)

* [`[Clang] Extend -Wbool-operation to warn about bitwise and of bools with side effects`](https://reviews.llvm.org/D108003)

* [`Re: [PATCH] kbuild: Fix 'no symbols' warning when CONFIG_TRIM_UNUSD_KSYMS=y`](https://lore.kernel.org/r/3afe5054-8129-fe42-b5a4-00bd091b1a0c@kernel.org/)

* [`[PATCH 0/7] kbuild: remove cc-option-yn`](https://lore.kernel.org/r/20210817002109.2736222-1-ndesaulniers@google.com/)

* [`Re: [PATCH v2 1/7] Compiler Attributes: Add __alloc_size() for better bounds checking`](https://lore.kernel.org/r/fd4e3b0b-a052-58a7-c816-f055e8404165@kernel.org/)

* [`Re: [PATCH][next] fs/ntfs3: Fix fall-through warnings for Clang`](https://lore.kernel.org/r/6b9a6c49-1b32-c5bc-6313-b0a888e93923@kernel.org/)

* [`Re: [PATCH][next] staging: r8188eu: Fix fall-through warnings for Clang`](https://lore.kernel.org/r/b5136c4b-5fe3-43e6-e893-fe74b2ba913f@kernel.org/)

* [`Re: [GIT PULL] Enable -Wimplicit-fallthrough for Clang for 5.14-rc7`](https://lore.kernel.org/r/9ef3265b-a5e7-d21b-68a8-ad137ac6ebfd@kernel.org/)

* [`Re: [PATCH v3 0/5] Enable -Warray-bounds and -Wzero-length-bounds`](https://lore.kernel.org/r/YS0nJtNDCwfbaubZ@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] ARM: select HAVE_FUTEX_CMPXCHG`](https://lore.kernel.org/r/YS1qYuZ5nM%2FAQeSh@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] powerpc/bug: Cast to unsigned long before passing to inline asm`](https://lore.kernel.org/r/YS6HDZ%2FXMY6HT4It@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] powerpc/ptdump: Fix generic ptdump for 64-bit`](https://lore.kernel.org/r/YS6Jq3VxpxWy%2Fhpo@Ryzen-9-3900X.localdomain/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [CI-NOTIFY]: TCWG Bisect tcwg_kernel/llvm-release-aarch64-next-allmodconfig - Build # 26 - Successful!`](https://groups.google.com/g/clang-built-linux/c/hxwI7QPQXvU/m/KbujekT9BAAJ)

* [`Re: [CI-NOTIFY]: TCWG Bisect tcwg_kernel/llvm-release-arm-stable-allyesconfig - Build # 4 - Successful!`](https://lore.kernel.org/r/a41ca429-9480-9ecf-242b-5e68fade3c10@kernel.org/)

* [`Re: [CI-NOTIFY]: TCWG Bisect tcwg_kernel/llvm-master-aarch64-next-allyesconfig - Build # 14 - Successful!`](https://groups.google.com/g/clang-built-linux/c/f7YJs0lz5Aw/m/fLny2__aAAAJ)

* [`Re: [PATCH v3] ucounts: add missing data type changes`](https://lore.kernel.org/r/YQn+GomdRCoYc%2FE8@Ryzen-9-3900X.localdomain/)

* [`Re: [gustavoars-linux:for-next/clang-fallthrough 1/1] warning: fallthrough annotation in unreachable code/`](https://lore.kernel.org/r/YQw2P8esj8PMNRQn@Ryzen-9-3900X.localdomain/)

* [`warning: objtool: can't find jump dest instruction`](https://github.com/ClangBuiltLinux/linux/issues/1435)

* [`SROA: Enhance speculateSelectInstLoads`](https://reviews.llvm.org/D106667)

* [`Error: make variable 'LLVM_IAS' value invalid '0'`](https://gitlab.com/Linaro/tuxsuite/-/issues/131)

* [`Re: [block:io_uring-fops.v6 58/64] io_uring.c:undefined reference to `__compiletime_assert_833'`](https://lore.kernel.org/r/YRcXr8o56PIYHY27@Ryzen-9-3900X.localdomain/)

* [`Re: [PATCH] mm/mempolicy: fix a race between offset_il_node and mpol_rebind_task`](https://lore.kernel.org/r/YRcdxhnte3d0krHx@Ryzen-9-3900X.localdomain/)

* [`Fedora i686 config "traps: PANIC: double fault, error_code: 0x0" in test_atomic64()`](https://github.com/ClangBuiltLinux/linux/issues/1437)

* [`ERROR: modpost: "__mulodi4" [drivers/block/nbd.ko] undefined!`](https://github.com/ClangBuiltLinux/linux/issues/1438)

* [`-Wshift-count-negative in drivers/net/ethernet/sfc/`](https://github.com/ClangBuiltLinux/linux/issues/1439)

* [`[CVP] processSwitch: Remove default case when switch cover all possible values.`](https://reviews.llvm.org/D106056)

* [`Re: [dhowells-fs:netfs-folio-regions 11/28] fs/netfs/read_helper.c:979:13: warning: variable 'folio' is uninitialized when used here`](https://lore.kernel.org/r/YR64CsQVrynR4V%2Fy@Ryzen-9-3900X.localdomain/)

* [`Re: [linux-next:master 6632/9522] include/linux/pm_opp.h:458:58: warning: unused parameter 'dev'`](https://lore.kernel.org/r/YSQE2f5teuvKLkON@Ryzen-9-3900X.localdomain/)

* [`objtool warning in cfg80211_edmg_chandef_valid() with ThinLTO`](https://lore.kernel.org/r/5913cdf4-9c8e-38f8-8914-d3b8a3565d73@kernel.org/)

* [`Re: [linux-next:master 8858/10077] fs/statfs.c:131:3: warning: 'memcpy' will always overflow; destination buffer has size 64, but size argument is 84`](https://lore.kernel.org/r/2751fd54-f28e-6318-2fc1-3fa5d4a98b2d@kernel.org/)

* [`Re: [PATCH v2 2/2] powerpc/bug: Provide better flexibility to WARN_ON/__WARN_FLAGS() with asm goto`](https://lore.kernel.org/r/YSa1O4fcX1nNKqN%2F@Ryzen-9-3900X.localdomain/)

* [`Fedora i686 config minus CONFIG_FORTIFY_SOURCE error in arch/x86/include/asm/checksum_32.h`](https://github.com/ClangBuiltLinux/linux/issues/1442)

* [`Re: [PATCH v4 4/4] powerpc/ptdump: Convert powerpc to GENERIC_PTDUMP`](https://lore.kernel.org/r/YSvYFTSwP5EkXQZ0@Ryzen-9-3900X.localdomain/)

* [`-Warray-bounds warning with asm goto in impossible switch cases`](https://bugs.llvm.org/show_bug.cgi?id=51682)

* [`Re: [PATCH v5 17/31] target/arm: Enforce alignment for LDM/STM`](https://lore.kernel.org/r/YS19IBEGrIUnUT2p@Ryzen-9-3900X.localdomain/)

* [`Re: [GIT PULL] s390 updates for 5.15 merge window`](https://lore.kernel.org/r/YS2RrUma2oOSYtIc@Ryzen-9-3900X.localdomain/)




## Tooling improvements

* [`Add some distribution configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/172)

* [`Enable sanitizer builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/178)

* [`Update llvm_tot anchor and LLVM_TOT_VERSION file`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/179)

* [`Add separate distribution configuration build set`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/180)

* [`Explicitly specify LLVM_IAS=0`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/181)

* [`Disable CONFIG_ATOMIC64_SELFTEST for Fedora's i686 config`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/182)

* [`Error out if builds.json is not found`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/184)

* [`check_logs.py: Increase timeout for CONFIG_KASAN_SW_TAGS builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/185)

* [`Update to Linux 5.14 for PGO and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/169)

* [`Test CONFIG_KASAN on s390 with clang-13+`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/187)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 3 and 4, HP desktop, ASUS laptop, and Hyper-V and VMware platforms on my workstation. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).
