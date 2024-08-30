---
title: August 2024 ClangBuiltLinux Work
date: 2024-08-30T16:30:00-0700
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

* Miscellaneous fixes and improvements: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `hexagon: Disable constant extender optimization for LLVM prior to 19.1.0` ([`v1`](https://lore.kernel.org/20240819-hexagon-disable-constant-expander-pass-v1-1-36a734e9527d@kernel.org/))
  * `ACPI: platform-profile: Fix CFI violation when accessing sysfs files` ([`v1`](https://lore.kernel.org/20240819-acpi-platform_profile-fix-cfi-violation-v1-1-479365d848f6@kernel.org/))

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `bcachefs: Fix format specifier in bch2_btree_key_cache_to_text()` ([`v1`](https://lore.kernel.org/20240821-bch2_btree_key_cache_to_text-fix-specifier-v1-1-2d9b89b15fdd@kernel.org/))
  * `ARM: imx: Annotate imx7d_enet_init() as __init` ([`v1`](https://lore.kernel.org/20240822-imx7d-mark-imx7d_enet_init-as-init-v1-1-8dfadee3bed4@kernel.org/))
  * `x86/resctrl: Annotate __get_mem_config_intel() as __init` ([`v1`](https://lore.kernel.org/20240822-x86-restctrl-get_mem_config_intel-init-v1-1-8b0a68a8731a@kernel.org/))
  * `x86/cpu_entry_area: Annotate percpu_setup_exception_stacks() as __init` ([`v1`](https://lore.kernel.org/20240822-x86-percpu_setup_exception_stacks-init-v1-1-57c5921b8209@kernel.org/))
  * `drm/xe: Fix total initialization in xe_ggtt_print_holes()` ([`v1`](https://lore.kernel.org/20240823-drm-xe-fix-total-in-xe_ggtt_print_holes-v1-1-12b02d079327@kernel.org/))
  * `xfs: Fix format specifier for max_folio_size in xfs_fs_fill_super()` ([`v1`](https://lore.kernel.org/20240827-xfs-fix-wformat-bs-gt-ps-v1-1-aec6717609e0@kernel.org/))
  * `hwmon: (oxp-sensors) Add missing breaks to fix -Wimplicit-fallthrough with clang` ([`v1`](https://lore.kernel.org/20240828-hwmon-oxp-sensors-fix-clang-implicit-fallthrough-v1-1-dc48496ac67a@kernel.org/))
  * `xfrm: policy: Restore dir assignments in xfrm_hash_rebuild()` ([`v1`](https://lore.kernel.org/20240829-xfrm-restore-dir-assign-xfrm_hash_rebuild-v1-1-a200865497b1@kernel.org/), [`v2`](https://lore.kernel.org/20240829-xfrm-restore-dir-assign-xfrm_hash_rebuild-v2-1-1cf8958f6e8e@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] kbuild: add debug package to pacman PKGBUILD`](https://lore.kernel.org/20240801183637.GB122261@thelio-3990X/)
* [`Re: [PATCH] scripts: run-clang-tools: add file filtering option`](https://lore.kernel.org/20240802223509.GA781199@thelio-3990X/)
* [`Re: [PATCH v2 2/2] sparc/build: Add SPARC target flags for compiling with clang`](https://lore.kernel.org/20240802224216.GA853635@thelio-3990X/)
* [`Re: [PATCH v2 1/2] sparc/build: Remove all usage of -fcall-used* flags`](https://lore.kernel.org/20240802230417.GB853635@thelio-3990X/)
* [`Re: [PATCH v3 1/4] find: Switch from inline to __always_inline`](https://lore.kernel.org/20240802230715.GA959572@thelio-3990X/)
* [`Re: [PATCH 1/5] Compiler Attributes: update GCC and Clang's `counted_by` URLs`](https://lore.kernel.org/20240804175443.GA2627063@thelio-3990X/)
* [`Re: [PATCH] init/main.c: Initialize early LSMs after arch code`](https://lore.kernel.org/20240806022002.GA1570554@thelio-3990X/)
* [`Re: [PATCH] kbuild: control extra pacman packages with PACMAN_EXTRAPACKAGES`](https://lore.kernel.org/20240806025853.GB1570554@thelio-3990X/)
* [`Re: [PATCH] Documentation/llvm: turn make command for ccache into code block`](https://lore.kernel.org/20240812190451.GA631776@thelio-3990X/)
* [`Re: [PATCH] modpost: simplify modpost_log()`](https://lore.kernel.org/20240812212256.GA3675407@thelio-3990X/)
* [`[HEXAGON] Utilize new mask instruction`](https://github.com/llvm/llvm-project/pull/92365#issuecomment-2286825645)
* [`Re: [PATCH V3] kbuild: control extra pacman packages with PACMAN_EXTRAPACKAGES`](https://lore.kernel.org/20240813185900.GA140556@thelio-3990X/)
* [`Re: [PATCH v2] modpost: simplify modpost_log()`](https://lore.kernel.org/20240816204517.GB3870443@thelio-3990X/)
* [`Re: [PATCH 1/2] kbuild: pacman-pkg: move common commands to a separate function`](https://lore.kernel.org/20240816210012.GC3870443@thelio-3990X/)
* [`Re: [PATCH 2/2] kbuild: pacman-pkg: do not override objtree`](https://lore.kernel.org/20240816210045.GD3870443@thelio-3990X/)
* [`Re: [PATCH 1/1] Documentation: kbuild: explicitly document missing prompt`](https://lore.kernel.org/20240820221528.GC2335251@thelio-3990X/)
* [`Re: [PATCH v2] Documentation: kbuild: explicitly document missing prompt`](https://lore.kernel.org/20240824033306.GC1733394@thelio-3990X/)
* [`Re: [PATCH] kbuild: remove recent dependency on "truncate" program`](https://lore.kernel.org/20240829180106.GA3382912@thelio-3990X/)



## Issue triage, input, and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [gustavoars:testing/wfamnae-next20240729-cbc-2 11/18] include/rdma/uverbs_ioctl.h:643:15: error: static assertion failed due to requirement '__builtin_offsetof(struct uverbs_attr_bundle, attrs) == sizeof(struct uverbs_attr_bundle_hdr)': struct member likely outside of struct_group_tagged()`](https://lore.kernel.org/20240801221427.GA3773553@thelio-3990X/)
* [`Re: [PATCH v2] mm: increase totalram_pages on freeing to buddy system`](https://lore.kernel.org/20240803000748.GA1606016@thelio-3990X/)
* [`[SDag][ARM][RISCV] Allow lowering CTPOP into a libcall`](https://github.com/llvm/llvm-project/pull/101786#issuecomment-2268077506)
* [`[DAG] Add legalization handling for ABDS/ABDU`](https://github.com/llvm/llvm-project/pull/92576#issuecomment-2274413538)
* [`[HEXAGON] Utilize new mask instruction`](https://github.com/llvm/llvm-project/pull/92365#issuecomment-2276056528)
* [`Re: lib/test_bitmap.c:1278:2: error: call to '__compiletime_assert_127' declared with 'error' attribute: BUILD_BUG_ON failed: !__builtin_constant_p(~var)`](https://lore.kernel.org/20240813221221.GA798176@thelio-3990X/)
* [`Re: [PATCH] btrfs: Annotate structs with __counted_by()`](https://lore.kernel.org/20240814180219.GA2542470@thelio-3990X/)
* [`Re: /usr/bin/ld: ../lib/LLVMgold.so: cannot open shared object file: No such file or directory`](https://lore.kernel.org/20240816204438.GA3870443@thelio-3990X/)
* [`Re: [PATCH v2 1/4] mm: Add optional close() to struct vm_special_mapping`](https://lore.kernel.org/20240819185253.GA2333884@thelio-3990X/)
* [`Re: [PATCH 0/2] kmod /usr support`](https://lore.kernel.org/20240821175843.GA2531464@thelio-3990X/)
* [`Apply dbaee836d60a8 to linux-5.10.y`](https://lore.kernel.org/20240827222159.GA2737082@thelio-3990X/)
* [`Re: [PATCH v1 0/2] drm/i915/fence: A couple of build fixes`](https://lore.kernel.org/20240829182255.GA1468662@thelio-3990X/)
* [`Re: [PATCH 6.6 230/341] change alloc_pages name in dma_map_ops to avoid name conflicts`](https://lore.kernel.org/20240830221217.GA3837758@thelio-3990X/)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`patches: Remove arm64-fixes (August 2, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/769)
* [`build-binutils.py: Update to 2.43`](https://github.com/ClangBuiltLinux/tc-build/pull/277)
* [`Apply fix to android-mainline for UBSAN error`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/770)
* [`Add LLVM 19 builds`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/771)
* [`Update korg-clang-19 to 19.1.0-rc2`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/420)
* [`patches: Remove arm64 (August 13, 2024)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/772)
* [`patches: Remove drm/radeon patch from stable trees`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/773)
* [`Update korg-clang-19 to 19.1.0-rc3`](https://gitlab.com/Linaro/tuxmake/-/merge_requests/422)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Raspberry Pi 4, a Raspberry Pi 3, a SolidRun Honeycomb LX2, an Ampere Altra Developer Platform, two Intel-based desktops, an AMD-based desktop, and an Intel-based laptop. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I continue to upload prebuilt, fast versions of LLVM for kernel developers and our continuous integration to use.

  * [19.1.0-rc2](https://lore.kernel.org/20240806054232.GA3815137@thelio-3990X/)
  * [19.1.0-rc3](https://lore.kernel.org/20240820175944.GA2015979@thelio-3990X/)



## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security).
