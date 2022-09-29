---
title: "September 2022 ClangBuiltLinux Work"
date: 2022-09-29T15:55:00-07:00
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

Occasionally, I will forget to link something from the mailing list in this post. To see my full mailing list activity (patches, reviews, and reports), you can view it on [lore.kernel.org](https://lore.kernel.org/all/?q=f:nathan@kernel.org).

## Linux kernel patches

* Build errors: These are patches to fix various build errors that I found through testing different configurations with LLVM or were exposed by our continuous integration setup. The kernel needs to build in order to be run :)

  * `arm64/sysreg: Fix a few missed conversions` ([`v1`](https://lore.kernel.org/20220919160928.3905780-1-nathan@kernel.org/))
  * `platform/x86/amd: pmc: Fix build without debugfs` ([`v1`](https://lore.kernel.org/20220922153100.324922-1-nathan@kernel.org/))
  * `ASoC: Intel: sof_da7219_mx98360a: Access num_codecs through dai_link` ([`v1`](https://lore.kernel.org/20220922153752.336193-1-nathan@kernel.org/))
  * `lib/Kconfig.debug: Add check for non-constant .{s,u}leb128 support to DWARF5` ([`v1`](https://lore.kernel.org/20220928182523.3105953-1-nathan@kernel.org/))

* Miscellaneous fixes: These are fixes and improvements that don't fit into a particular category but are important to ClangBuiltLinux.

  * `drm/i915: Fix CFI violations in gt_sysfs` ([`v1`](https://lore.kernel.org/20220922195127.2607496-1-nathan@kernel.org/))
  * `x86/Kconfig: Drop check for '-mabi=ms' for CONFIG_EFI_STUB` ([`v1`](https://lore.kernel.org/20220929152010.835906-1-nathan@kernel.org/))

* Stable backports: It is important to make sure that the stable trees are as free from issues as possible, as those are the trees that devices and users use; for example, Android and Chrome OS regularly merge from stable, so if there is a problem that will impact those trees that we fixed in mainline, it should be backported.

  * [`Apply 5c5c2baad2b55cc0a4b190266889959642298f79 to 5.10+`](https://lore.kernel.org/Yxyga8k1jee5A9Vs@dev-arch.thelio-3990X/)

* Warning fixes: These are patches to fix various warnings that appear with LLVM. I used to go into detail about the different warnings and what they mean, but the important takeaway for this section is that the kernel should build warning free, as [all developers should be using `CONFIG_WERROR`](https://lore.kernel.org/r/CAHk-=wifoM9VOp-55OZCRcO9MnqQ109UTuCiXeZ-eyX_JcNVGg@mail.gmail.com/), which will turn these all into failures. Maybe these should be in the build failures section...

  * `coresight: cti-sysfs: Mark coresight_cti_reg_store() as __maybe_unused` ([`v1`](https://lore.kernel.org/all/20220901195055.1932340-1-nathan@kernel.org/))
  * `net/mlx5e: Ensure macsec_rule is always initiailized in macsec_fs_{r,t}x_add_rule()` ([`v1`](https://lore.kernel.org/20220908153207.4048871-1-nathan@kernel.org/), [`v2`](https://lore.kernel.org/20220911085748.461033-1-nathan@kernel.org/))
  * `power: supply: bq25890: Fix enum conversion in bq25890_power_supply_set_property()` ([`v1`](https://lore.kernel.org/20220912141553.1743568-1-nathan@kernel.org/))
  * `drm/amd/display: Reduce number of arguments of dml314's CalculateWatermarksAndDRAMSpeedChangeSupport()` ([`v1`](https://lore.kernel.org/20220916210658.3412450-1-nathan@kernel.org/))
  * `drm/amd/display: Reduce number of arguments of dml314's CalculateFlipSchedule()` ([`v1`](https://lore.kernel.org/20220916210658.3412450-2-nathan@kernel.org/))
  * `thermal/intel/int340x: Initialized ret in error path in int340x_thermal_zone_add()` ([`v1`](https://lore.kernel.org/20220923152009.1721739-1-nathan@kernel.org/))
  * `usb: gadget: uvc: Fix argument to sizeof() in uvc_register_video()` ([`v1`](https://lore.kernel.org/20220928201921.3152163-1-nathan@kernel.org/))



## Patch review and input

For the next sections, I link directly to my first response in the thread when possible but there are times where the link is to the main post. My responses can be seen inline by going to the bottom of the thread and clicking on my name.

Reviewing patches that are submitted is incredibly important, as it helps ensure good code quality due to catching mistakes before the patches get accepted and it can help get patches accepted faster, as some maintainers will blindly pick up patches that have been reviewed by someone that they trust.

* [`Re: [PATCH] Makefile.extrawarn: re-enable -Wformat for clang; take 2`](https://lore.kernel.org/YxD0tg4O1Y6MqP0X@dev-arch.thelio-3990X/)
* [`Re: [PATCH 0/2] fortify: Add run-time WARN for cross-field memcpy()`](https://lore.kernel.org/YxEQg+fOpaPuS%2FNH@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4 00/21] KCFI support`](https://lore.kernel.org/YxEh+pLyOyPalW1u@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 2/2] powerpc/math-emu: Remove -w build flag and fix warnings`](https://lore.kernel.org/YxIjM%2FjdLajq4dFk@dev-arch.thelio-3990X/)
* [`Failure with zero call-used registers`](https://github.com/KSPP/linux/issues/192)
* [`Re: [PATCH v3 2/2] RISC-V: Clean up the Zicbom block size probing`](https://lore.kernel.org/YxdocNmsC3hQGNhy@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 2/5] Makefile.compiler: Use KBUILD_AFLAGS for as-option`](https://lore.kernel.org/YxdwbKA5ThYJcPBP@dev-arch.thelio-3990X/)
* [`Re: [PATCH] powerpc/pseries: Fix plpks crash on non-pseries`](https://lore.kernel.org/Yxi6pZf7M8LFeIAw@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4] RISC-V: Clean up the Zicbom block size probing`](https://lore.kernel.org/YyBQASDHnbEd6aMq@dev-arch.thelio-3990X/)
* [`Re: [PATCH] scripts/clang-tools: remove unused module`](https://lore.kernel.org/YyBSgNbJGRBNIL5I@dev-arch.thelio-3990X/)
* [`Re: [PATCH] fortify: Adjust KUnit test for modular build`](https://lore.kernel.org/YyD+lu7Kq%2FGa0N%2FV@dev-arch.thelio-3990X/)
* [`Re: [PATCH] net: davicom: Fix return type of dm9000_start_xmit`](https://lore.kernel.org/YyEEO0ttQhMvLFIC@dev-arch.thelio-3990X/)
* [`Re: [PATCH] net: ethernet: ti: davinci_emac: Fix return type of emac_dev_xmit`](https://lore.kernel.org/YyEEfEHlY5zMjWHg@dev-arch.thelio-3990X/)
* [`Re: [PATCH] net: ethernet: litex: Fix return type of liteeth_start_xmit`](https://lore.kernel.org/YyEErzoi9+8NMRCP@dev-arch.thelio-3990/)
* [`Re: [PATCH] net: korina: Fix return type of korina_send_packet`](https://lore.kernel.org/YyEE6MRhVEKnPPpv@dev-arch.thelio-3990X/)
* [`Re: [PATCH] staging: rtl8192u: Fix return type of ieee80211_xmit`](https://lore.kernel.org/YyENqW9u3LxySbSk@dev-arch.thelio-3990X/)
* [`Re: [PATCH] staging: rtl8712: Fix return type of r8712_xmit_entry`](https://lore.kernel.org/YyEOZ+u%2FT8rP6o9S@dev-arch.thelio-3990X/)
* [`Re: [PATCH] staging: rtl8723bs: Fix rtw_xmit_entry return type`](https://lore.kernel.org/YyEO01+suq0Wl%2F2F@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] staging: r8188eu: Fix return type of rtw_xmit_entry`](https://lore.kernel.org/YyEPaMVRmdGQdoql@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/exynos: Fix return type for mixer_mode_valid and hdmi_mode_vali`](https://lore.kernel.org/YyEP28J5O2Wlh4lS@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/i915: Fix return type of mode_valid function hook`](https://lore.kernel.org/YyEP7W%2FyZAyhNtTX@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/msm: Fix return type of mdp4_lvds_connector_mode_valid`](https://lore.kernel.org/YyEQEdSJLz3MR0%2FK@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm/rockchip: Fix return type of cdn_dp_connector_mode_valid`](https://lore.kernel.org/YyEQOztVBGl9D3u1@dev-arch.thelio-3990X/)
* [`Re: [PATCH] drm: xlnx: Fix return type of zynqmp_dp_connector_mode_valid`](https://lore.kernel.org/YyEQhKlesZjXJzAC@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] staging: octeon: Fix return type of cvm_oct_xmit and cvm_oct_xmit_pow`](https://lore.kernel.org/YyEQr%2F%2FIq7bautrm@dev-arch.thelio-3990X/)
* [`Re: [PATCH] openvswitch: Change the return type for vport_ops.send function hook to int`](https://lore.kernel.org/YyERjpSlz+MVCULd@dev-arch.thelio-3990X/)
* [`Re: [PATCH] x86: Avoid relocation information in final vmlinux`](https://lore.kernel.org/YyEU70K1aY8b%2FEXZ@dev-arch.thelio-3990X/)
* [`Re: [PATCH 2/2] x86/paravirt: add extra clobbers with ZERO_CALL_USED_REGS enabled`](https://lore.kernel.org/YyHn+UbfC7e1XIT3@dev-arch.thelio-3990X/)
* [`Re: [PATCH] kbuild: Add an option to skip vmlinux.bz2 in the rpm's`](https://lore.kernel.org/Yy3uCtztO3uDgecE@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2] Compiler Attributes: Introduce __access_*() function attribute`](https://lore.kernel.org/Yy%2FdLihBWSFzZdyq@dev-arch.thelio-3990X/)
* [`Re: [PATCH v5 29/30] thermal/intel/int340x: Replace parameter to simplify`](https://lore.kernel.org/YzHb50%2F4njPa1TR1@dev-arch.thelio-3990X/)
* [`[C2x] reject type definitions in offsetof`](https://reviews.llvm.org/D133574)
* [`[Clang] Don't warn if deferencing void pointers in unevaluated context`](https://reviews.llvm.org/D134702)
* [`Re: [PATCH -next] Makefile: add implicit enum-conversion check for compile build`](https://lore.kernel.org/YzMsmYTzX2Ni5zGP@dev-arch.thelio-3990X/)
* [`Re: [PATCH -next v2 0/2] Remove unused variables in x86/boot`](https://lore.kernel.org/YzNwG3iWz+qfNCC9@dev-arch.thelio-3990X/)
* [`Re: [PATCH] include/uapi/linux/swab: Fix potentially missing __always_inline`](https://lore.kernel.org/YzTMUpd6HbHmZu8f@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Revert "kbuild: Check if linker supports the -X option"`](https://lore.kernel.org/YzXhVSkCVKZzpf9H@dev-arch.thelio-3990X/)
* [`Re: [PATCH] net: lan966x: Fix return type of lan966x_port_xmit`](https://lore.kernel.org/YzXnqJ+fdogA1GHs@dev-arch.thelio-3990X/)



## Issue triage and reporting

The unfortunate thing about working at the intersection of two projects is we will often find bugs that are not strictly related to the project, which require some triage and reporting back to the original author of the breakage so that they can be fixed and not impact our own testing. Some of these bugs fall into that category while others are issues strictly related to this project.

* [`Re: [kbuild-all] Re: powerpc-linux-objdump: Warning: Unrecognized form: 0x23`](https://lore.kernel.org/YxDj6v5p+wHop0Wm@dev-arch.thelio-3990X/)
* [`Re: [LKP] Re: [mm] d88f8edb09: dmesg.Kernel_panic-not_syncing:Fatal_exception`](https://lore.kernel.org/YxEH6BbPa5VJYGiD@dev-arch.thelio-3990X/)
* [`Re: [jpoimboe:s390 2/2] Unsupported relocation type: 26`](https://lore.kernel.org/YxIdXugKymRtV9zW@dev-arch.thelio-3990X/)
* [`Re: [linux-next:master 4627/4736] ld.lld: error: vmlinux.o:(.debug_info+0x2096020d): relocation R_AARCH64_ABS32 out of range: 4295813883 is not in`](https://lore.kernel.org/YxIvVmJcKzJrIRzD@dev-arch.thelio-3990X/)
* [`Re: drivers/usb/gadget/udc/fsl_qe_udc.c:624:4: warning: unannotated fall-through between switch labels`](https://lore.kernel.org/YxdfyJVLtEaSEoEe@dev-arch.thelio-3990X/)
* [`Re: drivers/gpu/drm/i915/display/intel_display.c:7539:3: warning: unannotated fall-through between switch labels`](https://lore.kernel.org/Yxd0d2EagFOhXe83@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 1/3] powerpc/pseries: define driver for Platform KeyStore`](https://lore.kernel.org/Yxe06fbq18Wv9y3W@dev-arch.thelio-3990X/)
* [`ld.lld: error: undefined symbol: __tsan_mem{cpy,move,set}`](https://github.com/ClangBuiltLinux/linux/issues/1704)
* [`Re: [arm-integrator:virt-to-pfn-v6.0-rc1 21/24] ld.lld: error: arch/powerpc/kernel/vdso/vdso32.lds:244: unknown directive: static`](https://lore.kernel.org/YxoGkuNPskaWQgFE@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Failure after v6.0-rc3-161-g9cb252c4c1c5: net: skb: export skb drop reaons to user by TRACE_DEFINE_ENUM`](https://lore.kernel.org/YxwwUuznBOqxPBR+@dev-arch.thelio-3990X/)
* [```Re: machine_kexec_file.c:undefined reference to `__tsan_memset'```](https://lore.kernel.org/Yx3HnuEDyFG0+G62@dev-arch.thelio-3990X/)
* [`Re: [TCWG CI] Failure after v6.0-rc4-215-ge35ff25f9fda: Merge tag 'linux-kselftest-kunit-fixes-6.0-rc5' of git://git.kernel.org/pub/scm/linux/kernel/git/shuah/linux-kselftest`](https://lore.kernel.org/YyA+F3TOjEbQckeO@dev-arch.thelio-3990X/)
* [`Re: [PATCH 05/15] kbuild: build init/built-in.a just once`](https://lore.kernel.org/YyBAFL9CBsM9gl38@dev-arch.thelio-3990X/)
* [`Re: [peterz-queue:call-depth-tracking 11/59] ld.lld: error: arch/x86/built-in.a(kvm.o at 12382) <inline asm>:48:149: expected newline`](https://lore.kernel.org/YyBKngJE6M11bzrn@dev-arch.thelio-3990X)
* [`Re: [PATCH v2 2/3] fortify: Add KUnit test for FORTIFY_SOURCE internals`](https://lore.kernel.org/YyCOHOchVuE%2FE7vS@dev-arch.thelio-3990X/)
* [`Illegal operation with clang + CONFIG_MARCH_Z13`](https://lore.kernel.org/YyC%2FJvFONhtTYjM%2F@dev-arch.thelio-3990X/)
* [`__mulodi4 from u32_u32__int_overflow_test in lib/overflow_kunit.c`](https://github.com/ClangBuiltLinux/linux/issues/1711)
* [`Re: mainline build failure (new) for x86_64 allmodconfig with clang`](https://lore.kernel.org/YyXK1rJYEc04Sobt@dev-arch.thelio-3990X/)
* [`Re: [PATCH v2 7/8] arm64: alternatives: add alternative_has_feature_*()`](https://lore.kernel.org/YyigTrxhE3IRPzjs@dev-arch.thelio-3990X/)
* [`Re: [kbuild-all] Re: [linux-next:master 6296/7172] ERROR: modpost: "kvm_spurious_fault" [arch/x86/kvm/kvm-intel.ko] undefined!`](https://lore.kernel.org/YyjjT5gQ2hGMH0ni@dev-arch.thelio-3990X/)
* [`argument unused during compilation: '-march=armv6k'`](https://github.com/ClangBuiltLinux/linux/issues/1315)
* [`Re: INFO: task systemd-udevd:94 blocked for more than 120 seconds.`](https://lore.kernel.org/Yy4GoMMwiBaq3oJf@dev-arch.thelio-3990X/)
* [`Re: [PATCH v3 bpf-next 2/3] net: netfilter: add bpf_ct_set_nat_info kfunc helper`](https://lore.kernel.org/Yy4mVv+4X%2FTm3TK4@dev-arch.thelio-3990X/)
* [`Several instances of -Wvoid-ptr-dereference`](https://github.com/ClangBuiltLinux/linux/issues/1720)
* [`[Clang] Warn when trying to deferencing void pointers in C`](https://reviews.llvm.org/D134461)
* [`Re: arch/arm/probes/kprobes/core.c:409:30: error: .fnstart must precede .save or .vsave directives`](https://lore.kernel.org/YzHIwvzhM9DSW9cF@dev-arch.thelio-3990X/)
* [`LLVM/GNU binutils interop: non-constant .uleb128 is not supported`](https://github.com/ClangBuiltLinux/linux/issues/1719)
* [`die__process_unit: DW_TAG_label (0xa) @ <0x7b> not handled!`](https://lore.kernel.org/YzNHYoI4EZOQdCQ0@dev-arch.thelio-3990X/)
* [`Re: [PATCH] Drivers: hv: vmbus: Split memcpy of flex-array`](https://lore.kernel.org/YzNhHZOLB0T8iqHE@dev-arch.thelio-3990X/)
* [`Re: [objtool] ca5e2b42c0: kernel_BUG_at_arch/x86/kernel/jump_label.c`](https://lore.kernel.org/YzRr23Bn6qFDC7j0@dev-arch.thelio-3990X/)
* [`Re: [PATCH v4] usb: gadget: uvc: increase worker prio to WQ_HIGHPRI`](https://lore.kernel.org/YzR2gyyuU6luYRBP@dev-arch.thelio-3990X/)
* [`Fortify source failure in drivers/infiniband/core/cma.c with s390 allmodconfig`](https://github.com/ClangBuiltLinux/linux/issues/1687)
* [`LLVM commit d1ad006a8f64bdc disables CONFIG_EFI_STUB`](https://github.com/ClangBuiltLinux/linux/issues/1725)



## Tooling improvements

These are changes to various tools that we use, such as our continuous integration setup, booting utilities, toolchain building scripts, or other closely related projects such as [AOSP's distribution of LLVM](https://android.googlesource.com/platform/prebuilts/clang/host/linux-x86/) and [TuxMake](https://tuxmake.org).

* [`Disable CONFIG_PSERIES_PLPKS for Fedora's ppc64le configuration`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/421)
* [`boot-utils: Remove skiboot`](https://github.com/ClangBuiltLinux/boot-utils/pull/71)
* [`Disable CONFIG_KCSAN builds for tip of tree LLVM`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/423)
* [`patches: mainline: Drop mchp-spdiftx patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/425)
* [`boot-utils: Flush print commands to ensure output is properly ordered`](https://github.com/ClangBuiltLinux/boot-utils/pull/72)
* [`Revert "generator.yml: Disable CONFIG_PSERIES_PLPKS for Fedora's ppc64le configuration"`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/426)
* [`patches: tip: Drop mchp-spdiftx patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/427)
* [`tc-build: Flush print commands to ensure output is properly ordered`](https://github.com/ClangBuiltLinux/tc-build/pull/203)
* [`Drop -next patches (September 14th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/428)
* [`Drop stable tree patches (September 15th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/429)
* [`Drop mainline patches (September 16th, 2022)`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/430)
* [`Fix location of '--patch-series' in tuxsuite commands`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/432)
* [`Apply drm/amd/display patches for dml314 to mainline`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/433)
* [`Add OpenSUSE's riscv64 configuration`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/437)
* [`Add Alpine Linux configurations`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/438)
* [`utils.py: Handle OpenSUSE riscv64 configuration in get_cbl_name()`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/439)
* [`Enable x86_64 KCSAN build on -next`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/440)
* [`Revert "patches: mainline: Apply drm/amd/display patches for dml314"`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/441)
* [`generate_workflow.py: Do not run TuxSuite workflows on pushes to the main branch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/442)
* [`boot-qemu: Move from 'powernv8' to 'powernv' machine`](https://github.com/ClangBuiltLinux/boot-utils/pull/73)
* [`patches: tip: Remove drm/amd/display patch`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/443)
* [`Disable OpenSUSE's RISC-V configuration when using GNU as`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/444)
* [`boot-utils: Update to latest main`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/446)
* [`Reduce permissions of GITHUB_TOKEN`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/448)
* [`Adjust CFI builds for kCFI`](https://github.com/ClangBuiltLinux/continuous-integration2/pull/449)
* [`Add support for testing with LLVM`](https://github.com/palmer-dabbelt/riscv-systems-ci/pull/3)



## Behind the scenes

* Every day that there is a new [linux-next](https://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git/) release, I rebase and build a few different kernel trees then boot and runtime test them on several different machines, including a Windows Subsystem for Linux instance, a Raspberry Pi 3 and 4, an Intel-based desktop, an AMD-based desktop, an Intel-based laptop, and a SolidRun Honeycomb LX2. This is not always visible because I do not report anything unless there is something broken but it can take up to a few hours each day, depending on the amount of churn and issues uncovered.

* I participated in the 2022 Linux Plumbers Conference in Dublin, Ireland, which was great for interfacing with the greater kernel community and making them more aware of our work.



## Special thanks to:

* [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/).
