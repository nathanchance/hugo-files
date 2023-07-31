---
title: July 2023 ClangBuiltLinux Work
date: 2023-07-31T16:30:00-0700
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

  * `ASoC: cs35l45: Select REGMAP_IRQ` ([`v1`](https://lore.kernel.org/20230703-cs35l45-select-regmap_irq-v1-1-37d7e838b614@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply clang '--target=' KBUILD_CPPFLAGS shuffle to linux-6.4.y and linux-6.3.y`](https://lore.kernel.org/20230703150339.GA1975402@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `Avoid -Wconstant-logical-operand in nsecs_to_jiffies_timeout()` ([`v1`](https://lore.kernel.org/20230718-nsecs_to_jiffies_timeout-constant-logical-operand-v1-0-36ed8fc8faea@kernel.org/))
  * `Input: mcs-touchkey - Fix uninitialized use of error in mcs_touchkey_probe()` ([`v1`](https://lore.kernel.org/20230725-mcs_touchkey-fix-wuninitialized-v1-1-615db39af51c@kernel.org/))



## LLVM patches

* [`Revert "Reapply [IR] Mark and constant expressions as undesirable"`](https://github.com/llvm/llvm-project/commit/17f4f262fc5c4361cf43e91f2137ff7b2dcadc62)



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kbuild: Enable -Wenum-conversion by default`](https://lore.kernel.org/20230705162026.GA2951@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: rpm-pkg: Add dtbs support`](https://lore.kernel.org/20230712220832.GA669032@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 18/21] compiler.h: RFC - s/__LINE__/__COUNTER__/ in __UNIQUE_ID fallback`](https://lore.kernel.org/20230713171901.GA4036397@dev-arch.thelio-3990X/)
* [`Re: [PATCH] lib/bitmap: waive const_eval test as it breaks the build`](https://lore.kernel.org/20230717203105.GA2212488@dev-arch.thelio-3990X/)
* [`[Clang] Fix -Wconstant-logical-operand when LHS is a constant`](https://reviews.llvm.org/D142609)
* [`Re: [PATCH] arch: enable HAS_LTO_CLANG with KASAN and KCOV`](https://lore.kernel.org/20230717204554.GB2212488@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/amd/display: Allow building DC with clang on RISC-V`](https://lore.kernel.org/20230718163953.GA1279879@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: rust: avoid creating temporary files`](https://lore.kernel.org/20230718164303.GB1279879@dev-arch.thelio-3990X/)
* [`Re: [PATCH] gen_compile_commands: add assembly files to compilation database`](https://lore.kernel.org/20230719174639.GA1034565@dev-arch.thelio-3990X/)
* [`[clang][JumpDiagnostics] ignore non-asm goto target scopes`](https://reviews.llvm.org/D155342)
* [`add prereduce v0.1`](https://github.com/ClangBuiltLinux/misc-scripts/pull/2)
* [`Re: [PATCH] riscv: Handle zicsr/zifencei issue between gcc and binutils`](https://lore.kernel.org/20230725172344.GA1445373@dev-arch.thelio-3990X/)
* [`Add reduce scripts v2`](https://github.com/ClangBuiltLinux/misc-scripts/pull/4)
* [`Re: [PATCH] s390/ftrace: use la instead of aghik in return_to_handler()`](https://lore.kernel.org/20230726152046.GA3828563@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] drm: fix indirect goto into statement expression UB`](https://lore.kernel.org/20230728171757.GA433645@dev-arch.thelio-3990X/)
* [`Re: [PATCH 1/2] drm/exec: use unique instead of local label`](https://lore.kernel.org/20230731153119.GA773004@dev-arch.thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [PATCH v4 12/13] s390/kexec: refactor for kernel/Kconfig.kexec`](https://lore.kernel.org/20230705154958.GA3643511@dev-arch.thelio-3990X/)
* [`Hang while building 32-bit arm Linux 5.4 kernel after 9485d983ac0c`](https://github.com/llvm/llvm-project/issues/63699)
* [`Re: [RFC PATCH 0/2] x86: kprobes: Fix CFI_CLANG related issues`](https://lore.kernel.org/20230710155703.GA4021842@dev-arch.thelio-3990X/)
* [`Re: [peterz-queue:sched/maybe 1/14] ERROR: modpost: vmlinux: local symbol '__put_task_struct_rcu_cb' was exported`](https://lore.kernel.org/20230710185657.GA1505918@dev-arch.thelio-3990X/)
* [`error: cannot jump from this asm goto statement to one of its possible targets`](https://github.com/ClangBuiltLinux/linux/issues/1886)
* [`Re: [akpm-mm:mm-unstable 86/125] drivers/irqchip/irq-al-fic.c:281:2: error: call to undeclared function 'iounmap'; ISO C99 and later do not support implicit function declarations`](https://lore.kernel.org/20230711172008.GA369569@dev-arch.thelio-3990X/)
* [`LLVM commit b0cc947b5d0a7 introduces "error: cannot jump from this asm goto statement to one of its possible targets" for ppc64_defconfig`](https://github.com/ClangBuiltLinux/linux/issues/1887)
* [`[compiler-rt] Move crt into builtins`](https://reviews.llvm.org/D153989)
* [`Re: [PATCH 12/18] mtd: nand: omap: Use devm_platform_get_and_ioremap_resource()`](https://lore.kernel.org/20230714152701.GA1300718@dev-arch.thelio-3990X/)
* [`Re: stable-rc 6.1: x86: clang build failed - block/blk-cgroup.c:1237:6: error: variable 'ret' is used uninitialized whenever 'if' condition is true`](https://lore.kernel.org/20230717132426.GA2561862@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 2/2] regulator: max77857: Add ADI MAX77857/59/MAX77831 Regulator Support`](https://lore.kernel.org/20230718155502.GA3542993@dev-arch.thelio-3990X/)
* [`Re: [akpm-mm:mm-unstable 209/215] mm/mmap.c:2430:29: error: assigning to 'lockdep_map_p' from incompatible type 'void *'`](https://lore.kernel.org/20230718172105.GA1714004@dev-arch.thelio-3990X/)
* [`Reapply [IR] Mark and constant expressions as undesirable`](https://reviews.llvm.org/rG086ee99564af)
* [`"error: cannot jump from this indirect goto statement to one of its possible targets" in drivers/gpu/drm/tests/drm_exec_test.c`](https://github.com/ClangBuiltLinux/linux/issues/1890)
* [`Re: [PATCH 1/6] drm: execution context for GEM buffers v7`](https://lore.kernel.org/20230722220637.GA138486@dev-arch.thelio-3990X/)
* [`Re: Fwd: UBSAN: index 1 is out of range for type 'xen_netif_rx_sring_entry [1]'`](https://lore.kernel.org/20230723000657.GA878540@dev-arch.thelio-3990X/)
* [`Build error around 'aghik' after commit 1256e70a082a ("s390/ftrace: enable HAVE_FUNCTION_GRAPH_RETVAL")`](https://lore.kernel.org/20230725211105.GA224840@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 3/3] mm: Batch-zap large anonymous folio PTE mappings`](https://lore.kernel.org/20230726161942.GA1123863@dev-arch.thelio-3990X/)
* [`-Wframe-larger-than no longer shows information about spills/variables`](https://github.com/ClangBuiltLinux/linux/issues/1894)
* [`Re: [EXTERNAL] fs/smb/client/sess.c:160:5: warning: stack frame size (1152) exceeds limit (1024) in 'cifs_try_adding_channels'`](https://lore.kernel.org/20230727164003.GA79803@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 1/2] asm-generic: Unify uapi bitsperlong.h for arm64, riscv and loongarch`](https://lore.kernel.org/20230727213648.GA354736@dev-arch.thelio-3990X/)
* [`"relocation R_LARCH_B26 out of range" when building LoongArch allyesconfig with KCOV`](https://github.com/ClangBuiltLinux/linux/issues/1895)
* [`CONFIG_FORTIFY_SOURCE may not be working for strcpy() and strlcpy() on LoongArch with clang`](https://github.com/ClangBuiltLinux/linux/issues/1897)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Update default PGO kernel to 6.4.0 and update known good revision`](https://github.com/ClangBuiltLinux/tc-build/pull/237)
* [`patches: mainline: Drop '--target=' KBUILD_CPPFLAGS series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/594)
* [`Add patch for RANDSTRUCT raid1-10.c error to -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/595)
* [`wrapper: Handle 5.18 updates to LLVM variable`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/315)
* [`patches: mainline: Drop md/raid1-10 patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/596)
* [`patches: mainline: Add verisilicon -Wframe-larger-than fix`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/597)
* [`patches: next: Drop SCSI patch `](https://github.com/ClangBuiltLinux/continuous-integration2/pull/598)
* [`boot-qemu.py: Fix downloading LoongArch firmware`](https://github.com/ClangBuiltLinux/boot-utils/pull/110)
* [`Update SCSI -Warray-bounds patch application`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/601)
* [`docker: clang-android: Update to r498229 (17.0.3)`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/324)
* [`arch: add support for building LoongArch`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/325)
* [`Update arm64-fixes patches and update verisilicon patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/602)
* [`Drop verisilicon patch from -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/603)
* [`Drop SCSI -Warray-bounds patch from mainline and -tip`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/604)
* [`patches: stable: Remove "move '--target' to KBUILD_CPPFLAGS" series`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/606)
* [`patches: mainline: Drop verisilicon patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/607)
* [`Update LLVM tip of tree anchor to 18`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/609)
* [`patches: arm64: Update for 6.5-rc3 fast forward`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/611)
* [`runtime: docker: Add clang-17`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/326)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, an Intel-based desktop, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I built, qualified, and uploaded LLVM [17.0.0-rc1](https://lore.kernel.org/20230731180916.GA2401456@dev-arch.thelio-3990X/) to kernel.org for wider testing.



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
