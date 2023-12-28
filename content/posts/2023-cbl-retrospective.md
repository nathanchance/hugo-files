---
title: 2023 ClangBuiltLinux Retrospective
date: 2023-12-28T12:00:00-0700
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

[Just like I did last year](/posts/2022-cbl-retrospective/), I want to do a yearly report/retrospective for 2023 to look at what I (and the whole ClangBuiltLinux team in some cases) accomplished. I do monthly reports but looking at a high level across the year helps put things into perspective and drive improvements going into the new year.

## Linux kernel

This year, I had 129 commits accepted into maintainer trees (not all will be merged into mainline in 2023 but they were written and added in maintainer trees in 2023). They can be viewed on the web or by running the following command in an up-to-date Linux repository locally:

```
$ git log \
    --author='Nathan Chancellor' \
    --oneline \
    --since-as-filter='Jan 1, 2023' \
    --until='Jan 1, 2024' \
    origin/master
```

A similar command will be used to generate all following commit logs, which are included for convenience behind some collapsible Markdown with links.

<details>
<summary>Kernel contributions in 2023 (<a href="https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/log/?qt=author&q=Nathan+Chancellor">git.kernel.org</a>)</summary>
<p><code>
<a href="https://git.kernel.org/linus/e231092d19b86d51ee37e852c9bf6ab9eb3fb059">231092d19b86d</a> ("MAINTAINERS: Add scripts/clang-tools to Kbuild section")</br>
<a href="https://git.kernel.org/linus/20353ebb12324c2ba78a64389c9097d09037715b">0353ebb12324c</a> ("buffer: add cast in grow_buffers() to avoid a multiplication libcall")</br>
<a href="https://git.kernel.org/linus/78af7920d0eb7659a30dde3e3214b2b920f8fdf3">8af7920d0eb76</a> ("s390/traps: only define is_valid_bugaddr() under CONFIG_GENERIC_BUG")</br>
<a href="https://git.kernel.org/linus/c0706cfc7a5e9ddef1949520059798e8aea4c7d3">0706cfc7a5e9d</a> ("s390/dasd: remove dasd_stats_generic_show()")</br>
<a href="https://git.kernel.org/linus/2562a3aeaa71753dcb857c1fc121d7e76300e860">562a3aeaa7175</a> ("hexagon: traps: add internal prototypes for functions only called from asm")</br>
<a href="https://git.kernel.org/linus/d6b0180e6db1cc0699d9df8c8627aade0e2e1b80">6b0180e6db1cc</a> ("hexagon: traps: remove sys_syscall()")</br>
<a href="https://git.kernel.org/linus/2212acda71d93887418146f36d5dd90fb13a2610">212acda71d938</a> ("hexagon: irq: add prototype for arch_do_IRQ()")</br>
<a href="https://git.kernel.org/linus/d9f85d8be96900f659167a637686257e7176ce0e">9f85d8be96900</a> ("hexagon: vm_events: remove unused dummy_handler()")</br>
<a href="https://git.kernel.org/linus/d75eb3344ef1888885fcace3d34f3aceafee9aae">75eb3344ef188</a> ("hexagon: vdso: include asm/elf.h for arch_setup_additional_pages() prototype")</br>
<a href="https://git.kernel.org/linus/54ba0eab469d594dd1ef432a8de48724ae119336">4ba0eab469d59</a> ("hexagon: process: add internal prototype for do_work_pending()")</br>
<a href="https://git.kernel.org/linus/b0f731229a255112bfc82dd81f997a5f3484249e">0f731229a2551</a> ("hexagon: process: include linux/cpu.h for arch_cpu_idle() prototype")</br>
<a href="https://git.kernel.org/linus/9e06373780bd946d4990f84765de4f9bc168ed82">e06373780bd94</a> ("hexagon: reset: include linux/reboot.h for prototypes")</br>
<a href="https://git.kernel.org/linus/cb0085b0d694ab1269ac0c52464a48111c47161e">b0085b0d694ab</a> ("hexagon: signal: switch to SYSCALL_DEFINE0 for sys_rt_sigreturn()")</br>
<a href="https://git.kernel.org/linus/d068b1237e3204ec5d4f7ddcdde54aeef2a9c30b">068b1237e3204</a> ("hexagon: time: include asm/delay.h for prototypes")</br>
<a href="https://git.kernel.org/linus/1f443caea93e76a9e4613d9e370e082354ae3b44">f443caea93e76</a> ("hexagon: time: mark time_init_deferred() as static")</br>
<a href="https://git.kernel.org/linus/3279333097b22e1bab750d0f40837a097ec765fd">279333097b22e</a> ("hexagon: time: include asm/time.h for prototypes")</br>
<a href="https://git.kernel.org/linus/0ebac3e6151c283d39d24a6bbe43f0fe14149899">ebac3e6151c28</a> ("hexagon: vm_tlb: include asm/tlbflush.h for prototypes")</br>
<a href="https://git.kernel.org/linus/8126fafece234f383339bdc3713b7d793006302d">126fafece234f</a> ("hexagon: vm_fault: include asm/vm_fault.h for prototypes")</br>
<a href="https://git.kernel.org/linus/d9d106ce60760ae020f39f5a2d783fe92d401f8f">9d106ce60760a</a> ("hexagon: vm_fault: mark do_page_fault() as static")</br>
<a href="https://git.kernel.org/linus/ef14250ec7d42a0e993bd341db078ecb33900a16">f14250ec7d42a</a> ("hexagon: smp: mark handle_ipi() and start_secondary() as static")</br>
<a href="https://git.kernel.org/linus/bba07109f57d1299cd5551eb948ce182d711c221">ba07109f57d12</a> ("hexagon: mm: include asm/setup.h for setup_arch_memory()'s prototype")</br>
<a href="https://git.kernel.org/linus/600acbea29533db8906ed172b89eb10cd0d5413a">00acbea29533d</a> ("hexagon: mm: mark paging_init() as static")</br>
<a href="https://git.kernel.org/linus/014a5c107d0c45e259f87d3168f6a01e3e195637">14a5c107d0c45</a> ("hexagon: uaccess: remove clear_user_hexagon()")</br>
<a href="https://git.kernel.org/linus/888a50f6f93af2058333786bfcfbbc275bd6120f">88a50f6f93af2</a> ("MAINTAINERS: Add scripts/clang-tools to Kbuild section")</br>
<a href="https://git.kernel.org/linus/e18a38660786dbe8536ecf74563df25936a3532a">18a38660786db</a> ("mmc: sdhci-of-dwcmshc: Use logical OR instead of bitwise OR in dwcmshc_probe()")</br>
<a href="https://git.kernel.org/linus/812cc1da7ffd9e178ef66b8a22113be10fba466c">12cc1da7ffd9e</a> ("drm/bridge: Return NULL instead of plain 0 in drm_dp_hpd_bridge_register() stub")</br>
<a href="https://git.kernel.org/linus/03c0343bdf8d43fee6dfe92a7b66308b60e9e77c">3c0343bdf8d43</a> ("usb: typec: qcom-pmic-typec: Only select DRM_AUX_HPD_BRIDGE with OF")</br>
<a href="https://git.kernel.org/linus/5908cbe82ef77f6019349c450d7f1c8b3c29bb0e">908cbe82ef77f</a> ("usb: typec: nb7vpq904m: Only select DRM_AUX_BRIDGE with OF")</br>
<a href="https://git.kernel.org/linus/5225952d74d43e4c054731c74b8afd700b23a94a">225952d74d43e</a> ("x86/tools: Remove chkobjdump.awk")</br>
<a href="https://git.kernel.org/linus/7192ad2adde8213ad7c7f3b1ff974cccebae4d60">192ad2adde821</a> ("arm64: vdso32: Define BUILD_VDSO32_64 to correct prototypes")</br>
<a href="https://git.kernel.org/linus/71945968d8b128c955204baa33ec03bdd91bdc26">1945968d8b128</a> ("LoongArch: Mark __percpu functions as always inline")</br>
<a href="https://git.kernel.org/linus/7425627b2b2cd671d5bf6541ce50f7cba8a76ad6">425627b2b2cd6</a> ("tcp: Fix -Wc23-extensions in tcp_options_write()")</br>
<a href="https://git.kernel.org/linus/6740ec97bcdbe96ac7df147f986c030eddfebe65">740ec97bcdbe9</a> ("drm/amd/display: Increase frame warning limit with KASAN or KCSAN in dml2")</br>
<a href="https://git.kernel.org/linus/cba4590036855f4e3110d43c14385d2401080dbb">ba4590036855f</a> ("ASoC: codecs: aw88399: Fix -Wuninitialized in aw_dev_set_vcalb()")</br>
<a href="https://git.kernel.org/linus/146a15b873353f8ac28dc281c139ff611a3c4848">46a15b873353f</a> ("arm64: Restrict CPU_BIG_ENDIAN to GNU as or LLVM IAS 15.x or newer")</br>
<a href="https://git.kernel.org/linus/e82f5f40f2b936063361812cad9338ce792dde2f">82f5f40f2b936</a> ("bcachefs: Fix -Wcompare-distinct-pointer-types in bch2_copygc_get_buckets()")</br>
<a href="https://git.kernel.org/linus/53eda6f7130adb194cb3b089bc38fc32d9a1f7d5">3eda6f7130adb</a> ("bcachefs: Fix -Wcompare-distinct-pointer-types in do_encrypt()")</br>
<a href="https://git.kernel.org/linus/1f70225d7791e67084073e54489440d7cf8017e0">f70225d7791e6</a> ("bcachefs: Fix -Wincompatible-function-pointer-types-strict from key_invalid callbacks")</br>
<a href="https://git.kernel.org/linus/0940863fd2186c521d91aaf58b28d872fb1bba6c">940863fd2186c</a> ("bcachefs: Fix -Wformat in bch2_bucket_gens_invalid()")</br>
<a href="https://git.kernel.org/linus/14f63ff3f6617902cd54edb468b906214ab00f34">4f63ff3f66179</a> ("bcachefs: Fix -Wformat in bch2_alloc_v4_invalid()")</br>
<a href="https://git.kernel.org/linus/f7ed15eb177ffd55e97c5817e2ccaacc364be4cd">7ed15eb177ffd</a> ("bcachefs: Fix -Wformat in bch2_btree_key_cache_to_text()")</br>
<a href="https://git.kernel.org/linus/fac1250a8cc3af0e45c07ad59d7e1eabf5213688">ac1250a8cc3af</a> ("bcachefs: Fix -Wformat in bch2_set_bucket_needs_journal_commit()")</br>
<a href="https://git.kernel.org/linus/9040c0d96fd66e84bb9b017b821e31f2876e949c">040c0d96fd66e</a> ("RDMA/bnxt_re: Fix clang -Wimplicit-fallthrough in bnxt_re_handle_cq_async_error()")</br>
<a href="https://git.kernel.org/linus/089dbf6a06f1dcaeed4f8b86d619e8d28b235207">89dbf6a06f1dc</a> ("drm/amd/display: Respect CONFIG_FRAME_WARN=0 in DML2")</br>
<a href="https://git.kernel.org/linus/b8a555dc31e5aa18d976de0bc228006e398a2e7d">8a555dc31e5aa</a> ("eventfs: Use ERR_CAST() in eventfs_create_events_dir()")</br>
<a href="https://git.kernel.org/linus/3d8a18697ad834436d088d65cc66165947cfe600">d8a18697ad834</a> ("remoteproc: st: Fix sometimes uninitialized ret in st_rproc_probe()")</br>
<a href="https://git.kernel.org/linus/78882c7657bb276bab028d92f99fc185b25a2fc1">8882c7657bb27</a> ("scsi: ibmvfc: Use 'unsigned int' for single-bit bitfields in 'struct ibmvfc_host'")</br>
<a href="https://git.kernel.org/linus/41cb1126bed152f7679417834ad7ea39f2252dfb">1cb1126bed152</a> ("ASoC: tegra: Fix -Wuninitialized in tegra210_amx_platform_probe()")</br>
<a href="https://git.kernel.org/linus/184ff4f721638e37a5a5907bf98962b6d9318ef6">84ff4f721638e</a> ("OPP: Fix -Wunsequenced in _of_add_opp_table_v1()")</br>
<a href="https://git.kernel.org/linus/f4ecb3d44a117b16029485325bda1bc98c26de36">4ecb3d44a117b</a> ("mlx5: Fix type of mode parameter in mlx5_dpll_device_mode_get()")</br>
<a href="https://git.kernel.org/linus/26cc115d590c70e66d8399f39e4d9973d26439bc">6cc115d590c70</a> ("ptp: Fix type of mode parameter in ptp_ocp_dpll_mode_get()")</br>
<a href="https://git.kernel.org/linus/f9af5ad0f5b599fadf6fc7c9c2153d7919c7691e">9af5ad0f5b599</a> ("vfio/cdx: Add parentheses between bitwise AND expression and logical NOT")</br>
<a href="https://git.kernel.org/linus/c286c48018dea3c3bea9813477631cb12d6199c6">286c48018dea3</a> ("drm/debugfs: Fix drm_debugfs_remove_files() stub")</br>
<a href="https://git.kernel.org/linus/fc71f615fd08a530d24c7af0a1efa72ec6ea8e34">c71f615fd08a5</a> ("drm/amd/display: Fix -Wuninitialized in dm_helpers_dp_mst_send_payload_allocation()")</br>
<a href="https://git.kernel.org/linus/8ff81bb24f68f747ab2f738c3d493b9c2cad52bf">ff81bb24f68f7</a> ("LoongArch: Drop unused parse_r and parse_v macros")</br>
<a href="https://git.kernel.org/linus/89775a27ff6d0396b44de0d6f44dcbc25221fdda">9775a27ff6d03</a> ("lib/Kconfig.debug: Restrict DEBUG_INFO_SPLIT for RISC-V")</br>
<a href="https://git.kernel.org/linus/d4781807f0505526e293802d8509f31c4dfb8f54">4781807f05055</a> ("scsi: qla2xxx: Fix unused variable warning in qla2xxx_process_purls_pkt()")</br>
<a href="https://git.kernel.org/linus/75d1d3a433f0a0748a89eb074830e9b635a19fd2">5d1d3a433f0a0</a> ("clk: qcom: Fix SM_GPUCC_8450 dependencies")</br>
<a href="https://git.kernel.org/linus/78d84f35d2c3bf4434e9d785e4b4c33fa8e57878">8d84f35d2c3bf</a> ("wifi: rtw89: Fix clang -Wimplicit-fallthrough in rtw89_query_sar()")</br>
<a href="https://git.kernel.org/linus/a74048432fbb30e7a574747f6e1f47aef17010b0">74048432fbb30</a> ("ASoC: cs42l43: Initialize ret in default case in cs42l43_pll_ev()")</br>
<a href="https://git.kernel.org/linus/971fe5095f78b6475c88270aa4b6ee77a791cf25">71fe5095f78b6</a> ("MIPS: VDSO: Conditionally export __vdso_gettimeofday()")</br>
<a href="https://git.kernel.org/linus/f0a3d1de89876f4ca54fccb4103b504d50a8347f">0a3d1de89876f</a> ("x86/hyperv: Add missing 'inline' to hv_snp_boot_ap() stub")</br>
<a href="https://git.kernel.org/linus/98334dc292fddba043fef8f6ebc84b22d42baca9">8334dc292fddb</a> ("watchdog: xilinx_wwdt: Use div_u64() in xilinx_wwdt_start()")</br>
<a href="https://git.kernel.org/linus/2cf2a1acc6ebdffc6363de9156db8737f33c1803">cf2a1acc6ebdf</a> ("rtc: stm32: Use NOIRQ_SYSTEM_SLEEP_PM_OPS()")</br>
<a href="https://git.kernel.org/linus/92382d744176f230101d54f5c017bccd62770f01">2382d744176f2</a> ("lib: test_scanf: Add explicit type cast to result initialization in test_number_prefix()")</br>
<a href="https://git.kernel.org/linus/9c28423d3caae63e665e2b8d704fa41ac823b2a6">c28423d3caae6</a> ("ASoC: SOF: Intel: Initialize chip in hda_sdw_check_wakeen_irq()")</br>
<a href="https://git.kernel.org/linus/1696ec8654016dad3b1baf6c024303e584400453">696ec8654016d</a> ("mISDN: Update parameter type of dsp_cmx_send()")</br>
<a href="https://git.kernel.org/linus/aeedd3a82678d89627bdd020a28e0af35ff45552">eedd3a82678d8</a> ("drm/i915: Avoid -Wconstant-logical-operand in nsecs_to_jiffies_timeout()")</br>
<a href="https://git.kernel.org/linus/b27211db61aed86f095e76aa61eaef3bf3e0b7c2">27211db61aed8</a> ("drm/v3d: Avoid -Wconstant-logical-operand in nsecs_to_jiffies_timeout()")</br>
<a href="https://git.kernel.org/linus/8362bf82fb5441613aac7c6c9dbb6b83def6ad3b">362bf82fb5441</a> ("Input: mcs-touchkey - fix uninitialized use of error in mcs_touchkey_probe()")</br>
<a href="https://git.kernel.org/linus/d9ba2975e98a4bec0a9f8d4be4c1de8883fccb71">9ba2975e98a4b</a> ("ASoC: cs35l45: Select REGMAP_IRQ")</br>
<a href="https://git.kernel.org/linus/44762718b391b5ad7bd226a7a3badfb93248ad3b">4762718b391b5</a> ("drm/amdgpu: Move clocks closer to its only usage in amdgpu_parse_cg_state()")</br>
<a href="https://git.kernel.org/linus/4e3f85d1c071ed174aa5a7477d499d576412df3b">e3f85d1c071ed</a> ("drm/amdgpu: Remove CONFIG_DEBUG_FS guard around body of amdgpu_rap_debugfs_init()")</br>
<a href="https://git.kernel.org/linus/6e68dae946e3a0333fbde5487ce163142ca10ae0">e68dae946e3a0</a> ("clk: ralink: mtmips: Fix uninitialized use of ret in mtmips_register_{fixed,factor}_clocks()")</br>
<a href="https://git.kernel.org/linus/877e91191ccf0782ae18c5dfa7522fb1e5bfba8c">77e91191ccf07</a> ("leds: leds-mt6323: Adjust return/parameter types in wled get/set callbacks")</br>
<a href="https://git.kernel.org/linus/6e6251317c962b7c15ad2d8591f3103990c80701">e6251317c962b</a> ("MIPS: Mark core_vpe_count() as __init")</br>
<a href="https://git.kernel.org/linus/185891f03f712639c082e08fc9986ff214b5d617">85891f03f7126</a> ("coresight: dummy: Update type of mode parameter in dummy_{sink,source}_enable()")</br>
<a href="https://git.kernel.org/linus/4e70da985cef954cdf7813d651c067d2c602ea71">e70da985cef95</a> ("drm/amdgpu: Wrap -Wunused-but-set-variable in cc-option")</br>
<a href="https://git.kernel.org/linus/6c882a573bc1d6130274ef74d1697dd769f6a9e4">c882a573bc1d6</a> ("drm/amdgpu: Fix return types of certain NBIOv7.9 callbacks")</br>
<a href="https://git.kernel.org/linus/093d9b240a1fa261ff8aeb7c7cc484dedacfda53">93d9b240a1fa2</a> ("percpu: Fix self-assignment of __old in raw_cpu_generic_try_cmpxchg()")</br>
<a href="https://git.kernel.org/linus/43fc0a99906e04792786edf8534d8d58d1e9de0c">3fc0a99906e04</a> ("kbuild: Add KBUILD_CPPFLAGS to as-option invocation")</br>
<a href="https://git.kernel.org/linus/cff6e7f50bd315e5b39c4e46c704ac587ceb965f">ff6e7f50bd315</a> ("kbuild: Add CLANG_FLAGS to as-instr")</br>
<a href="https://git.kernel.org/linus/a7e5eb53bf9b800d086e2ebcfebd9a3bb16bd1b0">7e5eb53bf9b80</a> ("powerpc/vdso: Include CLANG_FLAGS explicitly in ldflags-y")</br>
<a href="https://git.kernel.org/linus/08f6554ff90ef189e6b8f0303e57005bddfdd6a7">8f6554ff90ef1</a> ("mips: Include KBUILD_CPPFLAGS in CHECKFLAGS invocation")</br>
<a href="https://git.kernel.org/linus/5c315434fdb6ab43566e6e0f6b9528bb0ad0aca9">c315434fdb6ab</a> ("drm/i915/pxp: Fix size_t format specifier in gsccs_send_message()")</br>
<a href="https://git.kernel.org/linus/1baeef6cd2229e01091c69cef042f6b688e194be">baeef6cd2229e</a> ("drm/i915/gt: Fix parameter in gmch_ggtt_insert_{entries, page}()")</br>
<a href="https://git.kernel.org/linus/4722e2ebe6f2168309b285977c5c96baf910c57b">722e2ebe6f216</a> ("drm/i915/gt: Fix second parameter type of pre-gen8 pte_encode callbacks")</br>
<a href="https://git.kernel.org/linus/2fe1e67e6987b6f05329740da79c8150a2205b0d">fe1e67e6987b6</a> ("x86/csum: Fix clang -Wuninitialized in csum_partial()")</br>
<a href="https://git.kernel.org/linus/35c812050ebdfe5ce576cf04d1d43d02dc2dfe19">5c812050ebdfe</a> ("drm/i915: Fix clang -Wimplicit-fallthrough in intel_async_flip_check_hw()")</br>
<a href="https://git.kernel.org/linus/2b694fc96fe33a7c042e3a142d27d945c8c668b0">b694fc96fe33a</a> ("powerpc/boot: Disable power10 features after BOOTAFLAGS assignment")</br>
<a href="https://git.kernel.org/linus/5c667d5a5a3ec16609229dddf25a46654186b52b">c667d5a5a3ec1</a> ("clk: sp7021: Adjust width of _m in HWM_FIELD_PREP()")</br>
<a href="https://git.kernel.org/linus/b3d6bdfea21ce1bbb45c3e6f297dfaf570d36300">3d6bdfea21ce1</a> ("riscv: Adjust dependencies of HAVE_DYNAMIC_FTRACE selection")</br>
<a href="https://git.kernel.org/linus/856dd6c5981260b4d1aa84b78373ad54a203db48">56dd6c5981260</a> ("ext4: fix unused iterator variable warnings")</br>
<a href="https://git.kernel.org/linus/f4670a1b30f886e843258ca4a69c442811a90302">4670a1b30f886</a> ("MIPS: Sink body of check_bugs_early() into its only call site")</br>
<a href="https://git.kernel.org/linus/95b5baf81001a80a8dd8b35306694db163bcb423">5b5baf81001a8</a> ("MIPS: Mark check_bugs() as __init")</br>
<a href="https://git.kernel.org/linus/8002725b9e3369ce8616d32dc2e7a57870475142">002725b9e3369</a> ("powerpc/32: Include thread_info.h in head_booke.h")</br>
<a href="https://git.kernel.org/linus/dcc11ac9dcaffdce428794f282c100a736244b55">cc11ac9dcaffd</a> ("Documentation/llvm: Add a note about prebuilt kernel.org toolchains")</br>
<a href="https://git.kernel.org/linus/275aca650e76e203e3efc71835d7195e18d2e718">75aca650e76e2</a> ("MIPS: Drop unused positional parameter in local_irq_{dis,en}able")</br>
<a href="https://git.kernel.org/linus/3292004c90c8aba74600a5cd8d10f8186fd48269">292004c90c8ab</a> ("net: ethernet: ti: Fix format specifier in netcp_create_interface()")</br>
<a href="https://git.kernel.org/linus/6cf882d9aa9e8f1f2d63182e179ac4b2e59c00db">cf882d9aa9e8f</a> ("wifi: iwlwifi: mvm: Use 64-bit division helper in iwl_mvm_get_crosstimestamp_fw()")</br>
<a href="https://git.kernel.org/linus/e89c2e815e76471cb507bd95728bf26da7976430">89c2e815e7647</a> ("riscv: Handle zicsr/zifencei issues between clang and binutils")</br>
<a href="https://git.kernel.org/linus/c8384d4a51e7cb0e6587f3143f29099f202c5de1">8384d4a51e7cb</a> ("net: pasemi: Fix return type of pasemi_mac_start_tx()")</br>
<a href="https://git.kernel.org/linus/499183cc3b52613f06cf4ce70809546971c96ed8">99183cc3b5261</a> ("wifi: iwlwifi: Avoid disabling GCC specific flag with clang")</br>
<a href="https://git.kernel.org/linus/a11334d8327b3fd7987cbfb38e956a44c722d88f">11334d8327b3f</a> ("powerpc: Allow CONFIG_PPC64_BIG_ENDIAN_ELF_ABI_V2 with ld.lld 15+")</br>
<a href="https://git.kernel.org/linus/7c3bd8362b06cff0a4044a4975adb7d71db2dfba">c3bd8362b06cf</a> ("powerpc: Fix use of '-mabi=elfv2' with clang")</br>
<a href="https://git.kernel.org/linus/d1c5accacb234c3a9f1609a73b4b2eaa4ef07d1a">1c5accacb234c</a> ("powerpc/boot: Only use '-mabi=elfv2' with CONFIG_PPC64_BOOT_WRAPPER")</br>
<a href="https://git.kernel.org/linus/5cf9d015be160e2d90d29ae74ef1364390e8fce8">cf9d015be160e</a> ("clk: Avoid invalid function names in CLK_OF_DECLARE()")</br>
<a href="https://git.kernel.org/linus/2d5bcdcda8799cf21f9ab84598c946dd320207a2">d5bcdcda8799c</a> ("bpf: Increase size of BTF_ID_LIST without CONFIG_DEBUG_INFO_BTF again")</br>
<a href="https://git.kernel.org/linus/c176060a4c76ed0043cb9c10435af04ed1ad0560">176060a4c76ed</a> ("drm: omapdrm: Do not use helper unininitialized in omap_fbdev_init()")</br>
<a href="https://git.kernel.org/linus/37e1f3acc339b28493eb3dad571c3f01b6af86f6">7e1f3acc339b2</a> ("net/sched: cls_api: Move call to tcf_exts_miss_cookie_base_destroy()")</br>
<a href="https://git.kernel.org/linus/748ea32d2dbd813d3bd958117bde5191182f909a">48ea32d2dbd81</a> ("macintosh: windfarm: Use unsigned type for 1-bit bitfields")</br>
<a href="https://git.kernel.org/linus/4e3feaad6ff8a7a57e3bf3308a93c93e3a2e17a6">e3feaad6ff8a7</a> ("powerpc/vdso: Filter clang's auto var init zero enabler when linking")</br>
<a href="https://git.kernel.org/linus/218674a45930c700486d27b765bf2f1b43f8cbf7">18674a45930c7</a> ("ASoC: mchp-spdifrx: Fix uninitialized use of mr in mchp_spdifrx_hw_params()")</br>
<a href="https://git.kernel.org/linus/5eb6e280432ddc9b755193552f3a070da8d7455c">eb6e280432ddc</a> ("ARM: 9289/1: Allow pre-ARMv5 builds with ld.lld 16.0.0 and newer")</br>
<a href="https://git.kernel.org/linus/8d9acfce33329d1f4b0f0969a9ba884bea7501c6">d9acfce33329d</a> ("kbuild: Stop using '-Qunused-arguments' with clang")</br>
<a href="https://git.kernel.org/linus/db1547c56886742283d7566c872f89cbad76a14c">b1547c5688674</a> ("kbuild: Turn a couple more of clang's unused option warnings into errors")</br>
<a href="https://git.kernel.org/linus/7db038d9790eda558dd6c1dde4cdd58b64789c47">db038d9790eda</a> ("drm/amd/display: Do not add '-mhard-float' to dml_ccflags for clang")</br>
<a href="https://git.kernel.org/linus/66bfe497d044a0dd4505e5179b3874b1a869c0b1">6bfe497d044a0</a> ("s390/purgatory: Remove unused '-MD' and unnecessary '-c' flags")</br>
<a href="https://git.kernel.org/linus/fd8589dce8107e2ce62e92f76089654462dd67b4">d8589dce8107e</a> ("s390/vdso: Drop '-shared' from KBUILD_CFLAGS_64")</br>
<a href="https://git.kernel.org/linus/f8210229f1f3e187ed4c40191b0101a8504b2f80">8210229f1f3e1</a> ("s390/vdso: Drop unused '-s' flag from KBUILD_AFLAGS_64")</br>
<a href="https://git.kernel.org/linus/05e05bfc92d196669a3d087fc34d3998b6ddb758">5e05bfc92d196</a> ("powerpc/vdso: Remove an unsupported flag from vgettimeofday-32.o with clang")</br>
<a href="https://git.kernel.org/linus/f0a42fbab447ed9f55bbd99751183e71f3a1f6ec">0a42fbab447ed</a> ("powerpc/vdso: Improve linker flags")</br>
<a href="https://git.kernel.org/linus/024734d132846dcb27f07deb1ec5be64d4cbfae9">24734d132846d</a> ("powerpc/vdso: Remove unused '-s' flag from ASFLAGS")</br>
<a href="https://git.kernel.org/linus/31f48f16264bc70962fb3e7ec62da64d0a2ba04a">1f48f16264bc7</a> ("powerpc: Remove linker flag from KBUILD_AFLAGS")</br>
<a href="https://git.kernel.org/linus/337ff6bb8960fdc128cabd264aaea3d42ca27a32">37ff6bb8960fd</a> ("MIPS: Prefer cc-option for additions to cflags")</br>
<a href="https://git.kernel.org/linus/80a20d2f8288afcd6036bbab8717061806ace4f2">0a20d2f8288af</a> ("MIPS: Always use -Wa,-msoft-float and eliminate GAS_HAS_SET_HARDFLOAT")</br>
<a href="https://git.kernel.org/linus/2f62847cf6ae49a54515421f67b1badffaa805f3">f62847cf6ae49</a> ("ARM: 9287/1: Reduce __thumb2__ definition to crypto files that require it")</br>
<a href="https://git.kernel.org/linus/27b5de622ea3fe0ad5a31a0ebd9f7a0a276932d1">7b5de622ea3fe</a> ("x86/build: Move '-mindirect-branch-cs-prefix' out of GCC-only block")</br>
<a href="https://git.kernel.org/linus/de1cae22898cf10aacc735a21d799b5bbce4496c">e1cae22898cf1</a> ("ASoC: amd: ps: Fix uninitialized ret in create_acp64_platform_devs()")</br>
</code></p>
</details></br>

I am particularly proud of the work I did as part of [the series](https://git.kernel.org/torvalds/l/8d9acfce33329d1f4b0f0969a9ba884bea7501c6) that culminates in commit [db1547c56886](https://git.kernel.org/linus/db1547c56886742283d7566c872f89cbad76a14c) ("`kbuild: Turn a couple more of clang's unused option warnings into errors`") and commit [8d9acfce3332](https://git.kernel.org/linus/8d9acfce33329d1f4b0f0969a9ba884bea7501c6) ("`kbuild: Stop using '-Qunused-arguments' with clang`"). It feels particularly rewarding to kill off a flag that hides issues, as it means that we cleaned up all the issues that were present and no more are allowed to creep back in because they will break the build. That work forced me to become even more familiar with Kbuild internals and by the end of it, I felt much more confident in being able to debug these errors in the future, which came in handy for [this three patch series](https://git.kernel.org/torvalds/l/cff6e7f50bd315e5b39c4e46c704ac587ceb965f) and commit [43fc0a99906e](https://git.kernel.org/linus/43fc0a99906e04792786edf8534d8d58d1e9de0c) ("`kbuild: Add KBUILD_CPPFLAGS to as-option invocation`").

Another seemingly simple yet memorable fix was commit [093d9b240a1f](https://git.kernel.org/linus/093d9b240a1fa261ff8aeb7c7cc484dedacfda53) ("`percpu: Fix self-assignment of __old in raw_cpu_generic_try_cmpxchg()`"). While the solution was not particularly challenging, the investigation into what was going on was interesting because compiling with `W=2` gave us the smoking gun, which is not something I have ever relied on before. Getting `-Wshadow` cleaned up and enabled would be a nice goal but it has been shot down by high level kernel people in the past, so it is unlikely to happen any time soon in my opinion.

Last year, Nick Desaulniers and I [worked on generating statically linked versions of LLVM/clang](https://github.com/ClangBuiltLinux/containers/commits/main/llvm-project), so that we could distribute them on kernel.org for other developers to use without worrying about dynamic dependencies. Unfortunately, we were not able to complete that work due to other fires and we have not been able to double back to it since. I strongly believe that having accessible toolchains for developers to use is really important because it can ease the friction of testing with another toolchain, either regularly as part of CI or just as part of regular development. Because of that, I [developed tooling to build a version of LLVM/clang in an older distribution container](https://github.com/nathanchance/env/commits/main/python/pgo-llvm-builder) with a relatively minimal set of dynamic dependencies. While that does not eliminate the problem of developers potentially lacking these dynamic dependencies and not being able to run the toolchains, the goal of these toolchains is that they can be used with the most common distributions with a relatively simple list of packages that may already be installed. These toolchains now [live on kernel.org](https://mirrors.edge.kernel.org/pub/tools/llvm/) for all to use; in fact, [the RISC-V folks use them in their Patchwork CI](https://github.com/linux-riscv/docker/blob/2a85bce994a9ee0d780418f1df45eb5f10cbb088/setup-github-ubuntu.sh#L63).

It is important to keep in mind that sending patches is only part of the development process. The others are reporting problems and testing and reviewing solutions to those problems. The kernel keeps track of these through particular tags: `Reported-by`, `Reviewed-by`, and `Tested-by`. In 2023, I provided those tags on 162 patches. The break down of patches that contained:

- `Reported-by`: 55
- `Reviewed-by`: 79
- `Tested-by`: 63

A full list of those commits are below, generated with the following command in an up-to-date Linux checkout:

```
$ git log \
    --extended-regexp \
    --grep='(Report|Review|Test)ed-by: Nathan Chancellor' \
    --oneline \
    --since-as-filter='Jan 1, 2023' \
    --until='Jan 1, 2024' \
    origin/master
```

<details>
<summary><code>Reported-by, Reviewed-by, and Tested-by</code> in 2023</summary>
<p><code>
<a href="https://git.kernel.org/linus/7583ce66ddf7ed4f7aaa14b5e4cf78058ca597e1">583ce66ddf7ed</a> ("docs: rust: remove `CC=clang` mentions")</br>
<a href="https://git.kernel.org/linus/56778b49c9a2cbc32c6b0fbd3ba1a9d64192d3af">6778b49c9a2cb</a> ("kunit: Add a macro to wrap a deferred action function")</br>
<a href="https://git.kernel.org/linus/c9f2b8d45aa453ee58e66a9b0e7a54e170381585">9f2b8d45aa453</a> ("modpost: remove unreachable code after fatal()")</br>
<a href="https://git.kernel.org/linus/5cac96f937021de3b0fbc60cdc6d6c4ee5b2456d">cac96f937021d</a> ("modpost: remove unneeded initializer in section_rel()")</br>
<a href="https://git.kernel.org/linus/16a473f60edc30ffcdf355676263730a6028ec67">6a473f60edc30</a> ("modpost: inform compilers that fatal() never returns")</br>
<a href="https://git.kernel.org/linus/cc87b7c06f2a6a1fbc7e06ccf6123aada4d0b588">c87b7c06f2a6a</a> ("modpost: move __attribute__((format(printf, 2, 3))) to modpost.h")</br>
<a href="https://git.kernel.org/linus/92ef432f027cffe0ff91ff2cbe9258d89ca53968">2ef432f027cff</a> ("kbuild: support W=c and W=e shorthands for Kconfig")</br>
<a href="https://git.kernel.org/linus/fc50065325f8b88d6986f089ae103b5db858ab96">c50065325f8b8</a> ("x86/callthunks: Correct calculation of dest address in is_callthunk()")</br>
<a href="https://git.kernel.org/linus/f4570ebd836363dc7722b8eb8d099b311021af13">4570ebd836363</a> ("x86/tools: objdump_reformat.awk: Allow for spaces")</br>
<a href="https://git.kernel.org/linus/60c2ea7c89e375804171552d8ea53d9084ec3269">0c2ea7c89e375</a> ("x86/tools: objdump_reformat.awk: Ensure regex matches fwait")</br>
<a href="https://git.kernel.org/linus/aa0cbc1b506b090c3a775b547c693ada108cc0d7">a0cbc1b506b09</a> ("LoongArch: Record pc instead of offset in la_abs relocation")</br>
<a href="https://git.kernel.org/linus/9e0be3f50c0e8517d0238b62409c20bcb8cd8785">e0be3f50c0e85</a> ("linux/export: clean up the IA-64 KSYM_FUNC macro")</br>
<a href="https://git.kernel.org/linus/1bfaa37fd3486e66131de9cb87747c84b4c89a05">bfaa37fd3486e</a> ("kbuild: dummy-tools: pretend we understand -fpatchable-function-entry")</br>
<a href="https://git.kernel.org/linus/a09af0551f5cfa850ca4d39be4395bb3526c8bdf">09af0551f5cfa</a> ("leds: pca955x: Fix -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/245561ba6d5de42bf73d501f910b181bc7fa5601">45561ba6d5de4</a> ("lkdtm: Fix CFI_BACKWARD on RISC-V")</br>
<a href="https://git.kernel.org/linus/c40fef858d002fb027033c572ac8bdf8756a2c6b">40fef858d002f</a> ("riscv: Use separate IRQ shadow call stacks")</br>
<a href="https://git.kernel.org/linus/d1584d791a297aa8ed93503382a682a6ecfc4218">1584d791a297a</a> ("riscv: Implement Shadow Call Stack")</br>
<a href="https://git.kernel.org/linus/e609b4f4252a2ad2454736078693571b9fbff019">609b4f4252a2a</a> ("riscv: Move global pointer loading to a macro")</br>
<a href="https://git.kernel.org/linus/82982fdd5133fa7e0b2dfaf746d18d6f29922b82">2982fdd5133fa</a> ("riscv: Deduplicate IRQ stack switching")</br>
<a href="https://git.kernel.org/linus/be97d0db5f44c0674480cb79ac6f5b0529b84c76">e97d0db5f44c0</a> ("riscv: VMAP_STACK overflow detection thread-safe")</br>
<a href="https://git.kernel.org/linus/265cc423155d56030e44068680085adb59800326">65cc423155d56</a> ("bcachefs: Fix -Wself-assign")</br>
<a href="https://git.kernel.org/linus/2d7ce49f58dc95495b3e22e45d2be7de909b2c63">d7ce49f58dc95</a> ("x86/retpoline: Make sure there are no unconverted return thunks due to KCSAN")</br>
<a href="https://git.kernel.org/linus/619102313466eaf8a6ac188e711f5df749dac6d4">19102313466ea</a> ("clk: ralink: mtmips: quiet unused variable warning")</br>
<a href="https://git.kernel.org/linus/a55d4aee76ca72e198a657cb471d2a3b37983072">55d4aee76ca72</a> ("kbuild: make binrpm-pkg always produce kernel-devel package")</br>
<a href="https://git.kernel.org/linus/845346841b77af84c88f1b709c63c14a58a64dc4">45346841b77af</a> ("crypto: skcipher - Add dependency on ecb")</br>
<a href="https://git.kernel.org/linus/2250c7ead8ad95185249d24cf169e4f2b07dcc1a">250c7ead8ad95</a> ("drm/i915: enable W=1 warnings by default")</br>
<a href="https://git.kernel.org/linus/7e1defac4b158cecb4628266f4d89732b4bd9179">e1defac4b158c</a> ("drm/i915: drop -Wall and related disables from cflags as redundant")</br>
<a href="https://git.kernel.org/linus/38c2efa260e6d0eab861c21c6ad1e4d79e1377ac">8c2efa260e6d0</a> ("pmdomain: renesas: rmobile-sysc: fix -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/ffa46bbc5892ebba8a95c839dc302cad7f22c209">fa46bbc5892eb</a> ("kbuild: rpm-pkg: generate kernel.spec in rpmbuild/SPECS/")</br>
<a href="https://git.kernel.org/linus/3e32552652917f10c0aa8ac75cdc8f0b8d257dec">e32552652917f</a> ("x86/boot: Move x86_cache_alignment initialization to correct spot")</br>
<a href="https://git.kernel.org/linus/9077fc228f09c9f975c498c55f5d2e882cd0da59">077fc228f09c9</a> ("bpf: Use kmalloc_size_roundup() to adjust size_index")</br>
<a href="https://git.kernel.org/linus/03dbab3bba5f009d053635c729d1244f2c8bad38">3dbab3bba5f00</a> ("overlayfs: set ctime when setting mtime and atime")</br>
<a href="https://git.kernel.org/linus/eb72d5207008db54c659fd34f341672decc306ae">b72d5207008db</a> ("mfd: cs42l43: Use correct macro for new-style PM runtime ops")</br>
<a href="https://git.kernel.org/linus/ad424743256b0119bd60a9248db4df5d998000a4">d424743256b01</a> ("x86/bitops: Remove unused __sw_hweight64() assembly implementation on x86-32")</br>
<a href="https://git.kernel.org/linus/8f908db77782630c45ba29dac35c434b5ce0b730">f908db7778263</a> ("bpf: Fix BTF_ID symbol generation collision")</br>
<a href="https://git.kernel.org/linus/4ec7b666fb4247bc6b9cdc84fa753d8dc2994d25">ec7b666fb4247</a> ("power: vexpress: fix -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/f7cf22424665043787a96a66a048ff6b2cfd473c">7cf2242466504</a> ("s390/dasd: fix string length handling")</br>
<a href="https://git.kernel.org/linus/a3c6bfba4429123533e9ae96ee50ba45ff8a63f2">3c6bfba442912</a> ("Documentation/llvm: refresh docs")</br>
<a href="https://git.kernel.org/linus/a9415b03f0213fdf8194e128980445a0d692cd5d">9415b03f0213f</a> ("fbdev: neofb: Shorten Neomagic product name in info struct")</br>
<a href="https://git.kernel.org/linus/90bae4d99beb1f31d8bde7c438a36e8875ae6090">0bae4d99beb1f</a> ("powerpc/xmon: Reapply "Relax frame size for clang"")</br>
<a href="https://git.kernel.org/linus/3f301dc292eb122eff61b8b2906e519154b0327f">f301dc292eb12</a> ("LoongArch: Replace -ffreestanding with finer-grained -fno-builtin's")</br>
<a href="https://git.kernel.org/linus/24883eb269f087b5d1068833fced543e020296ca">4883eb269f087</a> ("drm/repaper: fix -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/74f8fc31feb4b756814ec0720f48ccdc1175f774">4f8fc31feb4b7</a> ("riscv: Allow CONFIG_CFI_CLANG to be selected")</br>
<a href="https://git.kernel.org/linus/a72ab0361110db51488c670863551eb01428470e">72ab0361110db</a> ("riscv/purgatory: Disable CFI")</br>
<a href="https://git.kernel.org/linus/af0ead42f69389cd4ed68e1a4c6cde45c0adb35c">f0ead42f69389</a> ("riscv: Add CFI error handling")</br>
<a href="https://git.kernel.org/linus/f3a0c23f2539a69792df4586485225fda5fab988">3a0c23f2539a6</a> ("riscv: Add ftrace_stub_graph")</br>
<a href="https://git.kernel.org/linus/5f59c6855bad1809a4f85ce4db412f9ede45a4a0">f59c6855bad18</a> ("riscv: Add types to indirectly called assembly functions")</br>
<a href="https://git.kernel.org/linus/08d0ce30e0e4fcb5f06c90fe40387b1ce9324833">8d0ce30e0e4fc</a> ("riscv: Implement syscall wrappers")</br>
<a href="https://git.kernel.org/linus/2f4926723ac7343ae11861d0ba2aaa8e04bd3ca1">f4926723ac734</a> ("tty: gdm724x: use min_t() for size_t varable and a constant")</br>
<a href="https://git.kernel.org/linus/1fbda5f4c7c1c8bd51cd3bc3d2ff19176c696b74">fbda5f4c7c1c8</a> ("dmaengine: owl-dma: fix clang -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/349fde599db65d4827820ef6553e3f9ee75b8c7c">49fde599db65d</a> ("arch: enable HAS_LTO_CLANG with KASAN and KCOV")</br>
<a href="https://git.kernel.org/linus/dfd122fe8591d513b5e51313601217b67ae98d13">fd122fe8591d5</a> ("backlight: lp855x: Drop ret variable in brightness change function")</br>
<a href="https://git.kernel.org/linus/a417ab334dccb603f6296cbf04c9f8059bc252eb">417ab334dccb6</a> ("mtd: maps: fix -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/b9e002a34420e5ba25d4283be870c48e0c9e005f">9e002a34420e5</a> ("mtd: rawnand: fix -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/7f3c5d099b6f8452dc4dcfe4179ea48e6a13d0eb">f3c5d099b6f84</a> ("Revert "powerpc/xmon: Relax frame size for clang"")</br>
<a href="https://git.kernel.org/linus/f3add6dec36d9d747929918ba1d7ce8866e1c054">3add6dec36d9d</a> ("net: mdio: fix -Wvoid-pointer-to-enum-cast warning")</br>
<a href="https://git.kernel.org/linus/c8248faf3ca276ebdf60f003b3e04bf764daba91">8248faf3ca276</a> ("Compiler Attributes: counted_by: Adjust name and identifier expansion")</br>
<a href="https://git.kernel.org/linus/6e8d96909a23c8078ee965bd48bb31cbef2de943">e8d96909a23c8</a> ("asm-generic: partially revert "Unify uapi bitsperlong.h for arm64, riscv and loongarch"")</br>
<a href="https://git.kernel.org/linus/ba5ca5e5e6a1d55923e88b4a83da452166f5560e">a5ca5e5e6a1d5</a> ("x86/retpoline: Don't clobber RFLAGS during srso_safe_ret()")</br>
<a href="https://git.kernel.org/linus/41bdc6decda074afc4d8f8ba44c69b08d0e9aff6">1bdc6decda074</a> ("btf, scripts: rust: drop is_rust_module.sh")</br>
<a href="https://git.kernel.org/linus/cbe8ded48b939b9d55d2c5589ab56caa7b530709">be8ded48b939b</a> ("x86/srso: Fix build breakage with the LLVM linker")</br>
<a href="https://git.kernel.org/linus/616bceae250d0bab7ab2cbcb0791d820434ffb71">16bceae250d0b</a> ("drm/exec: use unique instead of local label")</br>
<a href="https://git.kernel.org/linus/bc60c930a43c7c984c80e99282f0d4f7193f3986">c60c930a43c7c</a> ("kbuild: rust_is_available: check that output looks as expected")</br>
<a href="https://git.kernel.org/linus/f295522886a4ebb628cadb2cd74d0661d6292978">295522886a4eb</a> ("kbuild: rust_is_available: handle failures calling `$RUSTC`/`$BINDGEN`")</br>
<a href="https://git.kernel.org/linus/7cd6a3e1f94bab4f2a3425e06f70ab13eb8190d4">cd6a3e1f94bab</a> ("kbuild: rust_is_available: normalize version matching")</br>
<a href="https://git.kernel.org/linus/9eb7e20e0c5cd069457845f965b3e8a7d736ecb7">eb7e20e0c5cd0</a> ("kbuild: rust_is_available: fix confusion when a version appears in the path")</br>
<a href="https://git.kernel.org/linus/e90db5521de2e00b63ba425b3b215f02563efe0a">90db5521de2e0</a> ("kbuild: rust_is_available: check that environment variables are set")</br>
<a href="https://git.kernel.org/linus/52cae7f28ed6c3992489f16bb355f5b623f0912e">2cae7f28ed6c3</a> ("kbuild: rust_is_available: add check for `bindgen` invocation")</br>
<a href="https://git.kernel.org/linus/aac284b1eb420c9317475cacdd21dd0ae3c3eda8">ac284b1eb420c</a> ("kbuild: rust_is_available: print docs reference")</br>
<a href="https://git.kernel.org/linus/eae90172c5b89f08f054b959197397c5cadd658d">ae90172c5b89f</a> ("docs: rust: add paragraph about finding a suitable `libclang`")</br>
<a href="https://git.kernel.org/linus/dee3a6b819c96fc8b1907577f585fd66f5c0fefe">ee3a6b819c96f</a> ("kbuild: rust_is_available: fix version check when CC has multiple arguments")</br>
<a href="https://git.kernel.org/linus/d824d2f98565e7c4cb1b862c230198fbe1a968be">824d2f98565e7</a> ("kbuild: rust_is_available: remove -v option")</br>
<a href="https://git.kernel.org/linus/5e720f8c8c9d959283c3908bbf32a91a01a86547">e720f8c8c9d95</a> ("cpufreq: amd-pstate: fix global sysfs attribute type")</br>
<a href="https://git.kernel.org/linus/d9287ea8ffc9be2ab4c81c32e1ca54478425ba38">9287ea8ffc9be</a> ("kbuild: deb-pkg: split debian/rules")</br>
<a href="https://git.kernel.org/linus/4b970e436523ed34da4ced74ad2b81e5a4f573f2">b970e436523ed</a> ("kbuild: deb-pkg: use Debian compliant shebang for debian/rules")</br>
<a href="https://git.kernel.org/linus/7b27d9ef0f63da736d3a585a5d5098cea62a3ad7">b27d9ef0f63da</a> ("s390/ftrace: use la instead of aghik in return_to_handler()")</br>
<a href="https://git.kernel.org/linus/c40e60f00caf18bc382215c79651777eb40f5f9d">40e60f00caf18</a> ("kbuild: Enable -Wenum-conversion by default")</br>
<a href="https://git.kernel.org/linus/b9b4568843bb6ceee85bf1280473f510701c82e2">9b4568843bb6c</a> ("s390/kexec: make machine_kexec() depend on CONFIG_KEXEC_CORE")</br>
<a href="https://git.kernel.org/linus/1c67921444bf68107f7901d5bcfce954efaa8754">c67921444bf68</a> ("gen_compile_commands: add assembly files to compilation database")</br>
<a href="https://git.kernel.org/linus/6f7cd0371ea79df29791fd56df64b516041d4633">f7cd0371ea79d</a> ("drm/amd/display: Allow building DC with clang on RISC-V")</br>
<a href="https://git.kernel.org/linus/7ee8acd1b803502878992acd6f99e61f1e8c7a25">ee8acd1b80350</a> ("media: verisilicon: fix excessive stack usage")</br>
<a href="https://git.kernel.org/linus/2c5d234d7f55e4ba7f3ee00fb9452ac7c97b4a46">c5d234d7f55e4</a> ("ptp: Make max_phase_adjustment sysfs device attribute invisible when not supported")</br>
<a href="https://git.kernel.org/linus/531b3d1195d096f14e030c4b01ec3a53b80276bf">31b3d1195d096</a> ("MIPS: Loongson: Fix build error when make modules_install")</br>
<a href="https://git.kernel.org/linus/e901f17b0742e36c9d79885a912b666cc1deb210">901f17b0742e3</a> ("NFS: Don't cleanup sysfs superblock entry if uninitialized")</br>
<a href="https://git.kernel.org/linus/5ddc7a3794ddd3470635ebd325fa1dffea5b18c0">ddc7a3794ddd3</a> ("LoongArch: Include KBUILD_CPPFLAGS in CHECKFLAGS invocation")</br>
<a href="https://git.kernel.org/linus/b89673a91a31710a4a957114b0195cfd45feb122">89673a91a3171</a> ("LoongArch: vDSO: Use CLANG_FLAGS instead of filtering out '--target='")</br>
<a href="https://git.kernel.org/linus/938f0c35d7d93a822ab9c9728e3205e8e57409d0">38f0c35d7d93a</a> ("s390/decompressor: fix misaligned symbol build error")</br>
<a href="https://git.kernel.org/linus/17b25a3801d11c28374a37cc672823cedb0f10d3">7b25a3801d11c</a> ("mips: add pte_unmap() to balance pte_offset_map()")</br>
<a href="https://git.kernel.org/linus/8020f0f9316b6961fe384031b4780e764eeb9652">020f0f9316b69</a> ("drm/amd/amdgpu: enable W=1 for amdgpu")</br>
<a href="https://git.kernel.org/linus/9d90161ca5c7234e80e14e563d198f322ca0c1d0">d90161ca5c723</a> ("powerpc/64: Force ELFv2 when building with LLVM linker")</br>
<a href="https://git.kernel.org/linus/3034983db355daefc4463defce802b8e6d86539f">034983db355da</a> ("drm/amdgpu: Mark mmhub_v1_8_mmea_err_status_reg as __maybe_unused")</br>
<a href="https://git.kernel.org/linus/fb9c384625dd604e8a5be1f42b35e83104b90670">b9c384625dd60</a> ("bus: fsl-mc: fsl-mc-allocator: Drop a write-only variable")</br>
<a href="https://git.kernel.org/linus/e0d43ed6389733c518619ec5b0ad2052352e40a4">0d43ed6389733</a> ("bus: fsl-mc: fsl-mc-allocator: Initialize mc_bus_dev before use")</br>
<a href="https://git.kernel.org/linus/e4bb020f3dbb83912eb6799a9d4bb79da4fd77ec">4bb020f3dbb83</a> ("riscv: detect assembler support for .option arch")</br>
<a href="https://git.kernel.org/linus/1caa71a7a600f7781ce05ef1e84701c459653663">caa71a7a600f7</a> ("KVM: arm64: Restore GICv2-on-GICv3 functionality")</br>
<a href="https://git.kernel.org/linus/228020b490eda9133c9cb6f59a5ee1278d8c463f">28020b490eda9</a> ("perf: Re-instate the linear PMU search")</br>
<a href="https://git.kernel.org/linus/feb843a469fb0ab00d2d23cfb9bcc379791011bb">eb843a469fb0a</a> ("kbuild: add $(CLANG_FLAGS) to KBUILD_CPPFLAGS")</br>
<a href="https://git.kernel.org/linus/4ce1e94175696b8f5f6fa29f09f7ef56724ddc2a">ce1e94175696b</a> ("s390/purgatory: Do not use fortified string functions")</br>
<a href="https://git.kernel.org/linus/1c913a1c35aa61cf280173b2bcc133c3953c38fc">c913a1c35aa61</a> ("KVM: arm64: Iterate arm_pmus list to probe for default PMU")</br>
<a href="https://git.kernel.org/linus/dd06e72e68bcb4070ef211be100d2896e236c8fb">d06e72e68bcb4</a> ("Compiler Attributes: Add __counted_by macro")</br>
<a href="https://git.kernel.org/linus/6a038f0183dd5d3e289f6c1fe6962de9b31f8fd2">a038f0183dd5d</a> ("drm/panel: samsung-s6d7aa0: use pointer for drm_mode in panel desc struct")</br>
<a href="https://git.kernel.org/linus/08e4044243a668ea2801cebffcb7c07df568ed3f">8e4044243a668</a> ("ubsan: remove cc-option test for UBSAN_TRAP")</br>
<a href="https://git.kernel.org/linus/413552b360e72604b8c0cf3f60f9e6f01c8ff963">13552b360e726</a> ("soc: ti: pruss: Avoid cast to incompatible function type")</br>
<a href="https://git.kernel.org/linus/dc1d05536f44cee16e46e86316e6718b2c0d8872">c1d05536f44ce</a> ("start_kernel: Omit prevent_tail_call_optimization() for newer toolchains")</br>
<a href="https://git.kernel.org/linus/514ca14ed5444b911de59ed3381dfd195d99fe4b">14ca14ed5444b</a> ("start_kernel: Add __no_stack_protector function attribute")</br>
<a href="https://git.kernel.org/linus/1168f095417643f663caa341211e117db552989f">168f095417643</a> ("jffs2: reduce stack usage in jffs2_build_xattr_subsystem()")</br>
<a href="https://git.kernel.org/linus/714dd3c29a2241fb799586a5b03773103ca50fe5">14dd3c29a2241</a> ("phy: mediatek: hdmi: mt8195: fix uninitialized variable usage in pll_calc")</br>
<a href="https://git.kernel.org/linus/9892bd72efdc9daa7c07ca9f427ac7e5928c7704">892bd72efdc9d</a> ("kbuild: deb-pkg: specify targets in debian/rules as .PHONY")</br>
<a href="https://git.kernel.org/linus/c90b3bbff2a097f4b6861c6d1aac18ed66f15261">90b3bbff2a097</a> ("kbuild: rpm-pkg: remove kernel-drm PROVIDES")</br>
<a href="https://git.kernel.org/linus/1a261a6e10e80cd7c69c3f5bdf47cd41f928fd08">a261a6e10e80c</a> ("scripts: Remove ICC-related dead code")</br>
<a href="https://git.kernel.org/linus/90fd833609c82487a0eca1581becde7ab54d9429">0fd833609c824</a> ("kasan: remove hwasan-kernel-mem-intrinsic-prefix=1 for clang-14")</br>
<a href="https://git.kernel.org/linus/3c65a2704cdd2a0cd0766352e587bae4a6268155">c65a2704cdd2a</a> ("kbuild: do not create intermediate *.tar for tar packages")</br>
<a href="https://git.kernel.org/linus/f8d94c4e403c89ec6b09ba69f65e4547ba99dd07">8d94c4e403c89</a> ("kbuild: do not create intermediate *.tar for source tarballs")</br>
<a href="https://git.kernel.org/linus/f6d8283549bc200e2babdd627239ece3547d634c">6d8283549bc20</a> ("kbuild: merge cmd_archive_linux and cmd_archive_perf")</br>
<a href="https://git.kernel.org/linus/d8f975594da8dc59b463db9bcc0bec3fd961630b">8f975594da8dc</a> ("wifi: iwlwifi: mvm: fix the order of TIMING_MEASUREMENT notifications")</br>
<a href="https://git.kernel.org/linus/52887af5650e35ea78b37602722313ff7b3c0c30">2887af5650e35</a> ("KVM: x86: Revert MSR_IA32_FLUSH_CMD.FLUSH_L1D enabling")</br>
<a href="https://git.kernel.org/linus/6f1ccbf07453eb1ee6bb24d6b531b88dd44ad229">f1ccbf07453eb</a> ("drm/vblank: Fix for drivers that do not drm_vblank_init()")</br>
<a href="https://git.kernel.org/linus/adef41b03b35839e5677aace628d02597f04a616">def41b03b3583</a> ("Revert "net: netcp: MAX_SKB_FRAGS is now 'int'"")</br>
<a href="https://git.kernel.org/linus/08570b7c8db6d9185deccf5bcda773bd6f17172f">8570b7c8db6d9</a> ("gpu: host1x: fix uninitialized variable use")</br>
<a href="https://git.kernel.org/linus/5eb39cde1e2487ba5ec1802dc5e58a77e700d99e">eb39cde1e2487</a> ("kcsan: avoid passing -g for test")</br>
<a href="https://git.kernel.org/linus/2e08ca1802441224f5b7cc6bffbb687f7406de95">e08ca18024412</a> ("kfence: avoid passing -g for test")</br>
<a href="https://git.kernel.org/linus/7d31677bb7b1944ac89e9155110dc1b9acbb3895">d31677bb7b194</a> ("gpu: host1x: fix uninitialized variable use")</br>
<a href="https://git.kernel.org/linus/328839ff93709a517e89ba1de1132c5d138e5dcb">28839ff93709a</a> ("drm/vmwgfx: Fix src/dst_pitch confusion")</br>
<a href="https://git.kernel.org/linus/624c60f326c6e5a80b008e8a5c7feffe8c27dc72">24c60f326c6e5</a> ("selftests: fix LLVM build for i386 and x86_64")</br>
<a href="https://git.kernel.org/linus/95207db8166ab95c42a03fdc5e3abd212c9987dc">5207db8166ab9</a> ("Remove Intel compiler support")</br>
<a href="https://git.kernel.org/linus/acd35dbab871d61021284ff06daccdc0ebb51e61">cd35dbab871d6</a> ("powerpc/vmlinux.lds: Add .text.asan/tsan sections")</br>
<a href="https://git.kernel.org/linus/7adf14d8aca1ea53bf9ccf8463809c82adb8c23a">adf14d8aca1ea</a> ("kbuild: rpm-pkg: remove unneeded KERNELRELEASE from modules/headers_install")</br>
<a href="https://git.kernel.org/linus/29cbe6ecfd97cb599883c68d3c6dbac11d0618b8">9cbe6ecfd97cb</a> ("docs: kbuild: remove description of KBUILD_LDS_MODULE")</br>
<a href="https://git.kernel.org/linus/1c34496e5856886d565665fb64029ecdeb080ffb">c34496e585688</a> ("objtool: Remove instruction::list")</br>
<a href="https://git.kernel.org/linus/6ea17e848a8ba5138b30e936c4b71877bc972c13">ea17e848a8ba5</a> ("x86: Fix FILL_RETURN_BUFFER")</br>
<a href="https://git.kernel.org/linus/a706bb08c81ac878982e41d4b6abcc42258bd39e">706bb08c81ac8</a> ("objtool: Fix overlapping alternatives")</br>
<a href="https://git.kernel.org/linus/c6f5dc28fb3d736fa8d7f7d31e0664a9c772c299">6f5dc28fb3d73</a> ("objtool: Union instruction::{call_dest,jump_table}")</br>
<a href="https://git.kernel.org/linus/0932dbe1f5680481e612cafe0c7d0f1796f68612">932dbe1f56804</a> ("objtool: Remove instruction::reloc")</br>
<a href="https://git.kernel.org/linus/8b2de412158ecdb312c707918432e6650df808cc">b2de412158ecd</a> ("objtool: Shrink instruction::{type,visited}")</br>
<a href="https://git.kernel.org/linus/d54066546121426ecd7ad01a53ae429c4e37a9d5">5406654612142</a> ("objtool: Make instruction::alts a single-linked list")</br>
<a href="https://git.kernel.org/linus/3ee88df1b063962e39d7798ccc3b18fd10cea813">ee88df1b06396</a> ("objtool: Make instruction::stack_ops a single-linked list")</br>
<a href="https://git.kernel.org/linus/20a554638dd2665a88d3d68a68f7981480a27f36">0a554638dd266</a> ("objtool: Change arch_decode_instruction() signature")</br>
<a href="https://git.kernel.org/linus/d99a55a94a0db8eda4a336a6f21730b844b8d2d2">99a55a94a0db8</a> ("ext4: fix function prototype mismatch for ext4_feat_ktype")</br>
<a href="https://git.kernel.org/linus/876e480da2f74715fc70e37723e77ca16a631e35">76e480da2f747</a> ("RDMA/cma: Distinguish between sockaddr_in and sockaddr_in6 by size")</br>
<a href="https://git.kernel.org/linus/d78c8e32890ef7eca79ffd67c96022c7f9d8cce4">78c8e32890ef7</a> ("powerpc/mm: Rearrange if-else block to avoid clang warning")</br>
<a href="https://git.kernel.org/linus/0aee6bec0f44dd344c2637632cb13828990524e2">aee6bec0f44dd</a> ("Documentation/llvm: add Chimera Linux, Google and Meta datacenters")</br>
<a href="https://git.kernel.org/linus/28980db94742f9f2fb0f68ea35f2171b38007bae">8980db94742f9</a> ("EDAC/amd64: Shut up an -Werror,-Wsometimes-uninitialized clang false positive")</br>
<a href="https://git.kernel.org/linus/17c45768fdf970b8a2ea9745783ff6a0512fca11">7c45768fdf970</a> ("Revert "driver core: add error handling for devtmpfs_create_node()"")</br>
<a href="https://git.kernel.org/linus/48c9899affd51f7acfc07a3f4d777b6eeb73a451">8c9899affd51f</a> ("Revert "devtmpfs: add debug info to handle()"")</br>
<a href="https://git.kernel.org/linus/d3583f06782cae72374464f9c29b2056fa0bd012">3583f06782cae</a> ("Revert "devtmpfs: remove return value of devtmpfs_delete_node()"")</br>
<a href="https://git.kernel.org/linus/78f7a3fd6dc66cb788c21d7705977ed13c879351">8f7a3fd6dc66c</a> ("randstruct: disable Clang 15 support")</br>
<a href="https://git.kernel.org/linus/56a2df7615fa050cc67b89245b2a482849077939">6a2df7615fa05</a> ("tools/resolve_btfids: Compile resolve_btfids as host program")</br>
<a href="https://git.kernel.org/linus/a088cf8eee1263462131fe8364e0fa962e17412b">088cf8eee1263</a> ("arm64: kprobes: Drop ID map text from kprobes blacklist")</br>
<a href="https://git.kernel.org/linus/f8d0dd0bc302b237dfa2ef5836e6ee375c0a1773">8d0dd0bc302b2</a> ("udf: remove reporting loc in debug output")</br>
<a href="https://git.kernel.org/linus/aa85923a954e7704bc9d3847dabeb8540aa98d13">a85923a954e77</a> ("crypto: hisilicon: Wipe entire pool on error")</br>
<a href="https://git.kernel.org/linus/118901ad1f25d2334255b3d50512fa20591531cd">18901ad1f25d2</a> ("ext4: Fix function prototype mismatch for ext4_feat_ktype")</br>
<a href="https://git.kernel.org/linus/f048158c428e766fc46bb7dc52c9fc2662a4b4bd">048158c428e76</a> ("MIPS: remove CONFIG_MIPS_LD_CAN_LINK_VDSO")</br>
<a href="https://git.kernel.org/linus/2ced0f30a426c7301350681f838344d5aea711e3">ced0f30a426c7</a> ("arm64: head: Switch endianness before populating the ID map")</br>
<a href="https://git.kernel.org/linus/994f5f7816ff963f49269cfc97f63cb2e4edb84f">94f5f7816ff96</a> ("x86/boot/compressed: prefer cc-option for CFLAGS additions")</br>
<a href="https://git.kernel.org/linus/91ecf7ff1b036f3fe1183809661119b1ee109b19">1ecf7ff1b036f</a> ("kbuild: make W=1 warn files that are tracked but ignored by git")</br>
<a href="https://git.kernel.org/linus/3daed6345d5880464f46adab871d208e1baa2f3a">daed6345d5880</a> ("VMCI: Use threaded irqs instead of tasklets")</br>
<a href="https://git.kernel.org/linus/81e506bec9be1eceaf5a2c654e28ba5176ef48d8">1e506bec9be1e</a> ("mm/thp: check and bail out if page in deferred queue already")</br>
<a href="https://git.kernel.org/linus/d3f450533bbcb6dd4d7d59cadc9b61b7321e4ac1">3f450533bbcb6</a> ("efi: tpm: Avoid READ_ONCE() for accessing the event log")</br>
<a href="https://git.kernel.org/linus/28c8e088427ad30b4260953f3b6f908972b77c2d">8c8e088427ad3</a> ("rseq: Increase AT_VECTOR_SIZE_BASE to match rseq auxvec entries")</br>
<a href="https://git.kernel.org/linus/8debed3efe3a731451ad9a91a7a74eeb18a7f7eb">debed3efe3a73</a> ("kbuild: export top-level LDFLAGS_vmlinux only to scripts/Makefile.vmlinux")</br>
<a href="https://git.kernel.org/linus/a494398bde273143c2352dd373cad8211f7d94b2">494398bde2731</a> ("s390: define RUNTIME_DISCARD_EXIT to fix link error with GNU ld < 2.36")</br>
<a href="https://git.kernel.org/linus/23970a1c9475b305770fd37bebfec7a10f263787">3970a1c9475b3</a> ("udf: initialize newblock to 0")</br>
</code></p>
</details></br>


## LLVM

I am far from a large LLVM contributor but I do have occasional patches there as part of this work. This year, I had 5 patches to LLVM, three of which were actual improvements or changes, rather than just reverts of other changes like in the previous retrospective. I wanted to try and fix more issues in the compiler this year and Nick Desaulniers helpfully pointed me to two issues that were simple to fix and helped me get more familiar with small parts of the LLVM code base.

<details>
<summary>LLVM contributions in 2023 (<a href="https://github.com/llvm/llvm-project/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code>
<a href="https://github.com/llvm/llvm-project/commit/cc2b09bee017147527e7bd1eb5272f4f70a7b900">c2b09bee01714</a> ("[Clang][LoongArch] Generate _mcount instead of mcount (#65657)")</br>
<a href="https://github.com/llvm/llvm-project/commit/a22d385f9656c95f5ce4155ea705aab6f8ef6d82">22d385f9656c9</a> ("[Sema] Do not emit -Wmissing-variable-declarations for register variables")</br>
<a href="https://github.com/llvm/llvm-project/commit/17f4f262fc5c4361cf43e91f2137ff7b2dcadc62">7f4f262fc5c43</a> ("Revert "Reapply [IR] Mark and constant expressions as undesirable"")</br>
<a href="https://github.com/llvm/llvm-project/commit/deecf89a1361cf3407f0fe32e7085127ec0865dc">eecf89a1361cf</a> ("[Clang] Add release note for 877210faa447")</br>
<a href="https://github.com/llvm/llvm-project/commit/877210faa447f4cc7db87812f8ed80e398fedd61">77210faa447f4</a> ("[Sema] Do not emit -Wunused-variable for variables declared with cleanup attribute")</br>
</code></p>
</details></br>


## Tooling

We have a few different repositories in [our GitHub organization](https://github.com/ClangBuiltLinux) that we use for testing and development, which I call "tooling". Tooling is very important for repetitive tasks or tasks where you want to take the human out of the equation so that mistakes are less likely, such as a bisect. Additionally, there are some other repositories that we rely on, like [TuxMake](https://gitlab.com/Linaro/tuxmake), that I consistently contribute to.

The focus for a lot of this year's work on tooling was improved maintainability in a few different aspects. The first major area of work I did was rewriting [boot-utils](https://github.com/ClangBuiltLinux/boot-utils/pull/91) and [tc-build](https://github.com/ClangBuiltLinux/tc-build/pull/226) to be more object oriented and encapsulated. I felt like I was getting lost and experiencing a lack of confidence when modifying these projects, so I challenged myself to rewrite them with all of the extra experience I have gained with Python over the past few years. The end result is much more maintainable and has made adding additional functionality and changes much easier, as evidenced by [the LoongArch addition to boot-utils](https://github.com/ClangBuiltLinux/boot-utils/commit/5ad98994b8dbf8a406fb8042ac545c2daa781f24). The next major area of work we improved on in our tooling is increased linting, primarily through the use of [`ruff`](https://astral.sh/ruff), which has many different rules for keeping our code clean and free of issues before they are committed. Finally, one of the things I am pretty proud of changing in our tooling was [eliminating the initial ramdisk images](https://github.com/ClangBuiltLinux/boot-utils/pull/105) that were checked into boot-utils and improving the tooling around generating them. While the versions of those images up until that point will still be in the repository's history (which increases its size when cloning), any future updates that we have to do to the images will not impact the repository's size, which makes updating the images slightly "cheaper", meaning we can potentially make them more useful as we need.

Like previously, I have included the `git log` output with direct links to commits, along with a web link to browse the history there.

<details>
<summary>boot-utils (<a href="https://github.com/ClangBuiltLinux/boot-utils/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/af9130e5a42a78ccdf34bd90f2b8b242d8282128">f9130e5a42a78</a> ("boot-qemu.py: Allow memory value to be customized")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/732146d9faf61fd3972afe7339b1c4e8f4bb885a">32146d9faf61f</a> ("utils.py: Do not query GitHub's API at all with '--gh-json-file'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/4bc72645aa41dca4c36c9a28c52bbfe6261ed71a">bc72645aa41dc</a> ("boot-qemu.py: Workaround PLW1509 warning from Ruff")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/44f12ea606eea1c67df4f094ddb0118db4a42f52">4f12ea606eea1</a> ("boot-qemu.py: Fix downloading LoongArch firmware")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/2b705206d260278b37b82a7b737af989eb612114">b705206d26027</a> ("Revert "DO-NOT-MERGE: Check in loongarch image"")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/09087cf8d5f9bbbe1d5673ffa6ea4a08e8a24a8f">9087cf8d5f9bb</a> ("DO-NOT-MERGE: Check in loongarch image")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/5ad98994b8dbf8a406fb8042ac545c2daa781f24">ad98994b8dbf8</a> ("boot-qemu.py: Add support for LoongArch")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/a500d1b017d738b3933da8f40c773b34e6647050">500d1b017d738</a> ("buildroot: Add support for LoongArch")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/30e89f87bebe15e7ee3a3c5e38b3ed8a18d442bf">0e89f87bebe15</a> ("buildroot: Add support for applying local patches")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/aff63bfb4ffa57724fe088a2473ada245a7f6206">ff63bfb4ffa57</a> ("buildroot: Upgrade to 2023.02.2")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/2940648942b1e8aaf61e96b26aefa8138f75e6ec">940648942b1e8</a> ("boot-qemu.py: Handle dtb location shuffle in linux-next")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/98f47f10513dce98b07480ac28be5f8636bf6205">8f47f10513dce</a> ("boot-utils: Add '--gh-json-file'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/929044427808a45a5482f808b46972ea7ea170c3">29044427808a4</a> ("utils: Limit scope of limit variable")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/3c6a83f7e876a06f819d40658939d07e7a799929">c6a83f7e876a0</a> ("utils: Link to GitHub's authentication documentation")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/26e906bb7f1fc02516b555834a049b3bff02867d">6e906bb7f1fc0</a> ("images: Remove")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/b2afa0224a8a49e930ac9eca027ab07b3c2fa147">2afa0224a8a49</a> ("boot-utils: Download rootfs images from GitHub releases")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/858d555b6a3baf48f7b0a83482f462781c3bdeec">58d555b6a3baf</a> ("boot-qemu.py: Fix check in _prepare_initrd()")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/8583db5068e20de14f444c980d5b52e2f6b75220">583db5068e20d</a> ("buildroot/rebuild.py: Add support for uploading images to a GitHub release")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/eb62d3959c99a60f80452c92a99fffa71d5b4ff8">b62d3959c99a6</a> ("buildroot: Rewrite build script in Python")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/b6b306ec7468008c6a65cb24bd251fd7375e310a">6b306ec746800</a> ("utils: Expand relative paths in get_full_kernel_path()")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/3a42d6a098552ba8a58d3fa81b5c46000fe7fc72">a42d6a098552b</a> ("boot-qemu.py: Add ppc64 big endian ELFv2 file strings to file_rosetta")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/81a58619ca334c7f58fa4bc77e44356aee3ed60a">1a58619ca334c</a> ("boot-qemu.py: Add file strings for aarch64 with CONFIG_RELOCATABLE=n")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/533f03c2edb26c9ce6cb07a63b5cd6873bd0dff5">33f03c2edb26c</a> ("ruff.toml: Disable S603 and S607")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/4e6963d09ecc5f74bc4d507248d352ee4a2606a9">e6963d09ecc5f</a> ("boot-qemu.py: Add quotes around file in error")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/0008bf44bf397d3a5fefd296d875263d285019ab">008bf44bf397d</a> ("boot-qemu.py: Attempt to guess architecture from vmlinux without '-a'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/8c6388892b52bf4aeb184dd732edc2618fa0d51e">c6388892b52bf</a> ("boot-qemu.py: Do not add '-no-reboot' unconditionally")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/25d75a6fc02ecee981fd4aae9d38cda008ef0b08">5d75a6fc02ece</a> ("boot-qemu.py: Avoid .with_stem() for Python 3.8 compatibility")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/bea5c7d481b96dd5aabca786af1be64b3685dcee">ea5c7d481b96d</a> ("boot-qemu.py: Eliminate _can_use_kvm() for arm64 and x86")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/2ca85ba5d2bfd0fd4226ca7b06993df20694b7b5">ca85ba5d2bfd0</a> ("boot-qemu.py: Add comment around why firmware files are being copied")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/9cdf19f21bcded24591d66f39c1228a2929d628e">cdf19f21bcded</a> ("boot-qemu.py: Add a note above QEMU 6.2.50 check to point out gitlab links")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/29206b1aaf4e09feed35593f1aecdfc03f70b479">9206b1aaf4e09</a> ("boot-qemu.py: Pull large sections of ARM64QEMURunner.run() into new methods")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/09a59ca6be112027bfac117c325c35847eb6272d">9a59ca6be1120</a> ("boot-qemu.py: Do not make gdb imply interactive")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/b845151d1e2e9c1296e0aaa83a4cc4dae861c5e5">845151d1e2e9c</a> ("boot-qemu.py: Make supports_efi a member instead of method")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/cd70025276cf65bee01166d332c276909c279924">d70025276cf65</a> ("boot-qemu.py: Remove trailing comma from function call")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/fc98ddcb9651ca1c1f6a367b56973100fbd669d9">c98ddcb9651ca</a> ("boot-qemu.py: Use find_first_file() for kernel image")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/4909dd4decfa362156ba78ac1826791f0661a570">909dd4decfa36</a> ("boot-qemu.py: Add support for m68k")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/04b984e11cab6fdfb66ace15305cbcbcedd0e328">4b984e11cab6f</a> ("boot-qemu.py: Add support for s390")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/80ce9f4f8729327c9eed1a996a36664a518e0721">0ce9f4f872932</a> ("boot-qemu.py: Add support for riscv")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/521125e5199921753009bf6e0134ef607c5677d1">21125e5199921</a> ("boot-qemu.py: Add support for powerpc")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/7d28e84ce162825f9d3f44f907c3d24b04f7ccd2">d28e84ce16282</a> ("boot-qemu.py: Add support for mips")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/81286a405aecd120c3763bbd792b0d3c94f735b9">1286a405aecd1</a> ("boot-qemu.py: Add support for arm32")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/d5353c9263d0fb0d3b8959b72d8686b21dc31723">5353c9263d0fb</a> ("boot-qemu.py: Add support for arm64")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/22a0fa1849ffd680f4b5d9b8c99fbe77c5a83ab0">2a0fa1849ffd6</a> ("boot-qemu.py: Implement '--gdb'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/7a1363b2899420e4003365c3a40f0e09a791702d">a1363b2899420</a> ("boot-qemu.py: Implement '--efi'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/b4d6205b6c993d35377033173b0acd463a2e87f5">4d6205b6c993d</a> ("boot-qemu.py: Implement '--smp'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/358761bd48e96406f119368ba3ab0bdee4e73ec5">58761bd48e964</a> ("boot-qemu.py: Implement '--append'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/c8c6eb42aefa4ff4a27b1d4d5d0df9c5a28a65a7">8c6eb42aefa4f</a> ("boot-qemu.py: Implement '--interactive' and '--timeout'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/17f4157818a6f1fb688038941041a9b639c4920e">7f4157818a6f1</a> ("boot-qemu.py: Initial rewrite")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/0fdee4ebcafe1b6e03822224834c1b217cd21d55">fdee4ebcafe1b</a> ("boot-qemu.py: Remove current implementation")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/80411d25cb551b84b690506a93780a8300969385">0411d25cb551b</a> ("boot-qemu.py + utils.py: Refactor finding EFI images")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/6bbfed72828944a61af12ab3adf3bb2f9d17e27b">bbfed72828944</a> ("utils.py: Combine an else + if into elif")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/4d004f30f20905c89644711343d360bd9781849e">d004f30f20905</a> ("boot-qemu.py: Combine an else + if into elif")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/a5ecf73ed6e1c6fc23ae5be5be6d7e461bd770ed">5ecf73ed6e1c6</a> ("Selectively revert bdd0ce438bdabd20b4406151cfd96ccd35c2ad21")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/54e6b62cc55f0c12d8c99127fd7c1081bab91253">4e6b62cc55f0c</a> ("ruff.toml: Add link to all rules")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/66ba09e9d9def8a44687072e8cdaf41e0d54e18f">6ba09e9d9def8</a> ("boot-qemu.py: Use contextlib.suppress() instead of try except")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/b55011d9f053baef10f88bf17e6874f89ff1b5b6">55011d9f053ba</a> ("boot-qemu.py: Use unpacking instead of concatentation")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/a23ded710b53e49c6addab0ca5b3c84820b85960">23ded710b53e4</a> ("boot-qemu.py: Use ternary operators in a couple of places")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/a7a1c4a3b9767b1df3d5756399df32ced5c381cb">7a1c4a3b9767b</a> ("boot-qemu.py: Use .open() with pathlib object")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/bdd0ce438bdabd20b4406151cfd96ccd35c2ad21">dd0ce438bdabd</a> ("Run 'ruff --fix' for COM warnings")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/b47f417d6bcc385cac4d1be9ae5c0f4ec569708c">47f417d6bcc38</a> ("boot-{qemu,uml}.py: Remove 'noqa: E251'")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/eba30e4d0e154d703817d3ace18510b7a1450a19">ba30e4d0e154d</a> ("Add a ruff configuration file")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/0709eae174f22ea61e410614857d1e197b29f864">709eae174f22e</a> ("boot-qemu.py: Use context manager for pathlib open() calls")</br>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/66e18834da3fbaa0008bbb4896b4bd575bfd844d">6e18834da3fba</a> ("boot-qemu.py: Use more specific exception class")</br>
</code></p>
</details></br>

<details>
<summary>continuous-integration2 (<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ebe4ed2444f9a69d4cea5fe0bffd86556de89c4f">be4ed2444f9a6</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/83b2c2b972bb38e64661266d60c363d649a0e838">3b2c2b972bb38</a> ("Revert "TEMPORARY: generator.yml: Delay LLVM ToT builds until next Tuesday"")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4e78de52ae93588de06921c5fe4dae38f5424606">e78de52ae9358</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b88c8f059adecf868d5ef8407edc90148477c1bf">88c8f059adecf</a> ("generate_workflow.py: Add spacing to context expressions")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/630fd9bdc220d43c75f17266d9c261effcb963e8">30fd9bdc220d4</a> ("generate_workflow.py: Make formatting of cache job match others")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b093dec9d57316e795f9880e5cbd3fe9622b4512">093dec9d57316</a> ("generate_workflow.py: Install full requirements.txt during cache setup")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0a06e80320be4af51ab36f7b9fed3f8fa0a21f31">a06e80320be4a</a> ("generate_workflow.py: Disable yapf for cond in tuxsuite_setups()")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/eeb533a2f66aefe122074d71abb83d3cbaa2dad5">eb533a2f66aef</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5a6380f003cce19be2a64427aa6308dae5e44051">a6380f003cce1</a> ("generate_workflow.py: Apply tuxsuite if condition to all steps")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a99079205f9b55e5346f1c1247fcacd65f5dbd3f">99079205f9b55</a> ("Revert "Temporarily change cron string to test caching"")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a37fd93399b6655e27c24380c9e95ed6cf09f7aa">37fd93399b665</a> ("Temporarily change cron string to test caching")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/148c1a12225d43e08cec5c1df94feb458514fd14">48c1a12225d43</a> ("patches: Drop SRSO patches from android13-5.10")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e50cb9b8aa233eda0ea3a46b233c852ae633e31f">50cb9b8aa233e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/35bf042fb048bac198b1256c0634fae5b98fd7f3">5bf042fb048ba</a> ("patches: android-4.19: Drop RESERVE_BRK() patches")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/84638d472141674f2ca801bc19054be840452634">4638d47214167</a> ("patches: stable: Drop LoongArch percpu patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/94dcd11a9ece2dddff59e0ead9fc6726c8b42467">4dcd11a9ece2d</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e0069ac3bbfa858665eeff144f87ee1896e1c38d">0069ac3bbfa85</a> ("TEMPORARY: generator.yml: Delay LLVM ToT builds until next Tuesday")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6d312f4c5c6d9ed2e2f50b6c5085f396c243ac5e">d312f4c5c6d9e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8925ee8751dedfd51e08ecabfc7d24be273210ef">925ee8751dedf</a> ("patches: tip: Drop current list of patches")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1b3e4bb6679dc76693fa41fd6f198bb1fce1f326">b3e4bb6679dc7</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/475889aeb57b9855de177cc2f583ddef80f36977">75889aeb57b98</a> ("patches: mainline: Remove -Wuninitialized ASoC patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/26495455561a327d9e0e2ad706aa617cfd9dee68">6495455561a32</a> ("patches: mainline: Drop LoongArch percpu __always_inline patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/691d5c2c462c604e158f1bdd82a7231857a09a67">91d5c2c462c60</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/71e9094f04d552d77b1152bedd90be460070efdc">1e9094f04d552</a> ("patches: Remove android-4.14")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0925e020923ab268de2a72d43bf9cfcc71311a23">925e020923ab2</a> ("patches: mainline: Drop -Wc23-extensions patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fce1e84b02b97077e9e6ebb3e6c3e853708bd988">ce1e84b02b970</a> ("patches: mainline: Drop AMDGPU -Wframe-larger-than bump patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/dbf63b1ee0efdf842510ce259b645e49358bf081">bf63b1ee0efdf</a> ("utils.py: Handle virtconfig")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e53077db7abfc30626196efeb9c4ec7a75231875">53077db7abfc3</a> ("patches: Fix android-4.14 and android-4.19 brk backports")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6a7f520db8898512ef5d5684daf176f11343ab2f">a7f520db88985</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1b29a828bf3952daad15237b623a76f8ae7ded53">b29a828bf3952</a> ("patches: Copy mainline patches to -tip")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5f5a524342a4f803bf873645f232634231584757">f5a524342a4f8</a> ("patches: mainline: Add 1dc05a274a7b13fd61b6c43f0136153752e6f731")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1577cb1d5141854f672a9d6ac6e1fc1ff41a7fb6">577cb1d514185</a> ("patches: mainline: Add cba4590036855f4e3110d43c14385d2401080dbb")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/35a682614ce40b3310feab20603b9c99deaf53bf">5a682614ce40b</a> ("patches: mainline: Use v2 of AMDGPU fix")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/69f644e749aea73ed8bdc9b3225569d9fe244dec">9f644e749aea7</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/061b24fef624f89f5c7e4dc5bd593dc61ae2923a">61b24fef624f8</a> ("patches: Add patch for LoongArch build failures in stable and mainline")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/43af0671aca444daf2823111ccc5c36fadd88848">3af0671aca444</a> ("patches: mainline: Add patch for -Wframe-larger-than in AMDGPU")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/abc8ffa081323b3723b70800a8075e19752007cf">bc8ffa081323b</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1ec9b2d0a66d92ad513d271caf44a694ead13b92">ec9b2d0a66d92</a> ("patches: Add fix for -Wc23-extensions in net/ipv4/tcp_output.c")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/bdbe4e90c9fd19c8f815819fa5ccf5e593a896ab">dbe4e90c9fd19</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/eb09c2baa7f60e5b50b5ed688aff56489924984b">b09c2baa7f60e</a> ("generator.yml: Update stable anchor to 6.6")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d5c9f6935dc1fcd639ffaa5bae0de2ec014387e1">5c9f6935dc1fc</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/79de88895d862245d28660bdfd4e0959570bd5f6">9de88895d8622</a> ("patches: Add backports for new ld.lld issue in Android")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cba43166a4c0157b31cd7530740192063f2ef27a">ba43166a4c015</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8a43c9409eed526545ea5ecc94fec049c5a5f939">a43c9409eed52</a> ("generator.yml: Disable arm64 big endian builds prior to LLVM 15")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9b0e025fc9dbfa63a4f2331649862d58fe3cd5be">b0e025fc9dbfa</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/862bc821433b3b597e3c9d416f166551165806ce">62bc821433b3b</a> ("patches: Drop 5.1 drm/mediatek fixup patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f83f858c4f3703779f14d1ae9e38ef3fbbec6f24">83f858c4f3703</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b319ab6d05433eea00208158510a90968bd7e2ed">319ab6d05433e</a> ("patches: next: Remove COUNT_ARGS() implementation patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/20a9c518090af14bf6cda50f25e87993480384d4">0a9c518090af1</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f61c9310428b728b3230520b240cdcd54a902b99">61c9310428b72</a> ("patches: Drop 5.15 fix up patch for drivers/interconnect/core.c")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2a39df479e2e0da7fc8dc46c4663266ed272eb28">a39df479e2e0d</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5bc85651b7a38cc716a1384c4d554bc61fbcd6d1">bc85651b7a38c</a> ("patches: Drop test_scanf change from all stable trees")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0b6d7c31d827e582741727d2e15935a7c36a31f7">b6d7c31d827e5</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8b735bb5e517f0637c44f9c83f86fb095d9c4094">b735bb5e517f0</a> ("patches: Drop test_scanf.c patch from -tip")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/71df229ab4b17af3fe783fa1e25cfac7ea57a411">1df229ab4b17a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/73d9b7dc67c033cfd04733226ddc0c03cde42c57">3d9b7dc67c033</a> ("generator.yml: Add android15-6.1")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8cb89197bef1734e1eb4b967b1455dd64e4b5d19">cb89197bef173</a> ("generator.yml: Add missing AOSP LLVM builds for android14-6.1")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7d026264e289b69a81592ef7876b33c17fc9e44f">d026264e289b6</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/66ea416ac3a4388afb05d14383abf88585ee4e17">6ea416ac3a438</a> ("generator.yml: Update LoongArch LTO configuration")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9e033ef8513ee0eaf8645299b2cca89f65f4010a">e033ef8513ee0</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0b8df326bd8f98cd74cbbc02f5cc1efad7a9c240">b8df326bd8f98</a> ("generator.yml: Add LoongArch builds for LLVM 17")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/497f7bbaab74c6d927a87886f7ae3a35223be1d2">97f7bbaab74c6</a> ("patches: arm64-fixes: Update for 6.5-rc3 fast forward")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d0f3622aa9e567e7f41c890b9da5849fef29e95f">0f3622aa9e567</a> ("workflows: Avoid hard coded Ubuntu version")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/bf464da60df3bfa6f720bdbef7070a5df05597ca">f464da60df3bf</a> ("workflows: Update manually managed files to use actions/checkout@v4")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b95ccd8396fd11c4d175402063b044a671d5bce2">95ccd8396fd11</a> ("ci: Regenerate GitHub Actions workflow files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/99339b91136dd1278c87a405f96a365c3f9bc1ae">9339b91136dd1</a> ("generate_workflow.py: Update to actions/checkout@v4")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f7992d5c2b873e4e8358cf226f635a68711bb93d">7992d5c2b873e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ef1d16780f8a32b44559dcaf95886656f5f0866d">f1d16780f8a32</a> ("patches: mainline: Drop test_scanf.c patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ab2767d56698acfd4595764b9f1d38292d88c14d">b2767d56698ac</a> ("patches: android-mainline: Drop raid1-10.c patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6302d0cb053b30603ab10455753deb001523b11f">302d0cb053b30</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5669302bb5889a35ee7c1dd0ca6eef7aab57f109">669302bb5889a</a> ("Add support for LoongArch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/24d1cbe38aecd807bf0f9a1514dd1b59985aac68">4d1cbe38aecd8</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4f21be99dfacb94871fe8e9fabef6265713d5c72">f21be99dfacb9</a> ("generator.yml: Apply 3f93770 to stable")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9fc2932c4b1e3153ecda5bc1258eecb3e5b54b6c">fc2932c4b1e31</a> ("generator.yml: Update stable anchor to 6.5")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/47bbca7965fbdbda462d0e46f9593fa2d57d33ba">7bbca7965fbdb</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9a6ddc78537b3e3389138c94cc0f40591704488c">a6ddc78537b3e</a> ("generator.yml: Stop enabling CONFIG_PPC_DISABLE_WERROR=y for clang-14+")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4d0ac1642b485ea78abe06f0b9ae88c7f74050f8">d0ac1642b485e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/db07e111529e00c7770615dcc47ac729acb5f9b1">b07e111529e00</a> ("generator.yml: Drop Fedora's armv7hl configuration")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b48178c170aa10a0bcaaa3202163720b31a2412f">48178c170aa10</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/09cffcc2b01afcb37155bd15df0883bb9025342a">9cffcc2b01afc</a> ("patches: Add fix for new -Wconstant-conversion in clang-18")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/278af623e9df649e7f58b8a3aeb7207c9c430b95">78af623e9df64</a> ("README: Update for new clang-17 workflows")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4de4b96a5142703da956a469933cb548d4ed355f">de4b96a514270</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cfa47bbc73097dfb40f17d68fbf5473652d2acb5">fa47bbc73097d</a> ("generator: Re-enable powerpc 32-bit builds with LLVM 16+")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e9c9a3806e3baf240afe6bbefccd76a9335784ef">9c9a3806e3baf</a> ("generator.yml: Add llvm_16 and move llvm_latest to 17")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d13e659916b5a3ef9b91f6a8a63c353ea4707a6d">13e659916b5a3</a> ("patches: arm64: Update for 6.5-rc3 fast forward")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/52d831c3df24a8d67bbccb43e2902261504fccca">2d831c3df24a8</a> ("README: Update badges for new workflows")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e18433538c1d5932eb5c05fd481664aeed08cef9">18433538c1d59</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/abb0bbe02fc37f4de19aa5196232a38cef95371e">bb0bbe02fc37f</a> ("LLVM tip of tree is now 18")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d3a9209c477afded083ebc0f1fc6b6b9b23d085f">3a9209c477afd</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1f24982d981838fc21a099ea74be4cb5747daa6a">f24982d981838</a> ("patches: mainline: Drop verisilicon patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5b09328d910457134dffbdbf2d234e1266fd62e8">b09328d910457</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/12c9058c9c04bd33c344ef7aea4ad29bd39b3964">2c9058c9c04bd</a> ("patches: stable: Remove "move '--target' to KBUILD_CPPFLAGS" series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1b974062e75a8034355045866cb6edc23eaea297">b974062e75a80</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f42ed1dc229561ebebea766e6178cbd93b842063">42ed1dc229561</a> ("patches: Drop SCSI -Warray-bounds patch from mainline and -tip")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/20f98093ed77e0bb5b69439b974041a904ca4682">0f98093ed77e0</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0f7478fbb27324e866d0c03cb3f3ed4078369cf3">f7478fbb27324</a> ("patches: next: Drop verisilicon patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/be026087a4599ca6821d890634eba18645b6ab7d">e026087a4599c</a> ("patches: Update verisilicon patch with accepted version")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/52809e1926b133e6f84e1934bbe7d65b5435b7a5">2809e1926b133</a> ("patches: arm64-fixes: Update after 6.5-rc1 fast forward")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2476d3646608182c9a80ef58a2337d85503ebf9e">476d364660818</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3a465a4d2d12042745bd38b944a5df75bf8e7e06">a465a4d2d1204</a> ("patches: Add SCSI -Warray-bounds patch to -tip")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e4d8fd7b5c8a0e001801f427aa215f6ba6ae69fc">4d8fd7b5c8a0e</a> ("patches: mainline: Update SCSI patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c7332f5b7d029c727d79a408e494bbf99316af26">7332f5b7d029c</a> ("scripts/build-local.py: Add comment above check for versioned toolchains")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6df04b9c0590400b11c8b9e707887f05a134c74b">df04b9c059040</a> ("scripts/build-local.py: Allow use of ccache")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d86bdb77ce3eac6d1713361db65d0e6967c10d41">86bdb77ce3eac</a> ("scripts/build-local.py: Improve interrupt behavior")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/30cfddfed19c8b8ff0e66f9a1f00b79a0c484247">0cfddfed19c8b</a> ("scripts/build-local.py: Only require runtime when using versioned toolchains")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1c62b0b0a7be4cc4b0358bf68a7c2bb6c75b701c">c62b0b0a7be4c</a> ("patches: next: Drop SCSI patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4b187d769276d3685124fede17244dbd0e1853b1">b187d769276d3</a> ("patches: mainline: Add verisilicon -Wframe-larger-than fix")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1874eac48972406701840678e0da808bae7f5784">874eac4897240</a> ("patches: mainline: Drop md/raid1-10 patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0c5644dcc5d3c087e334c1c3016e8dc9f8d1da96">c5644dcc5d3c0</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3f937703f16ed9549abf92362411eb377f64afce">f937703f16ed9</a> ("generator.yml: Switch to LLVM=1 for ppc64 on -next/mainline")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0a57fca64f88c34f94214f15c68fc66564d7ee5a">a57fca64f88c3</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f95f73521846d2194ef2f8c3c69c4620b7141dc5">95f73521846d2</a> ("patches: tip: Add patch for RANDSTRUCT raid1-10.c error")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e258bc3f4d30226e138081d1598fba58fdc3ebff">258bc3f4d3022</a> ("patches: mainline: Drop '--target=' KBUILD_CPPFLAGS series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/13a6117e329292795b781f2d100f0ef90c94f975">3a6117e329292</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/806efcbfd3b1926e814519fb8c1061c14d386911">06efcbfd3b192</a> ("generator.yml: Apply dfc1013 to stable")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7b04d6b78f0266c61fb7030e2fc4ce454473fd42">b04d6b78f0266</a> ("generator.yml: Update stable anchor to 6.4")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7a6081d2a27c420c0c2a298bf9ef08bbef131bcd">a6081d2a27c42</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ebbc8f3f0b8bc1f27c56657929a3c59214a07307">bbc8f3f0b8bc1</a> ("patches: stable: Apply series to resolve arm64 big endian failures")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7e3b054e267cc997e140e582e63e07837ec270b0">e3b054e267cc9</a> ("patches: next: Add patch for -Wframe-larger-than in drivers/media/platform/verisilicon/rockchip_vpu981_hw_av1_dec.c")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c4d10560fc06ee4dc9a0d435687489fde4089a01">4d10560fc06ee</a> ("patches: next: Update SCSI patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/dc0e9c56c82057d0c4dd20458ce43944c8ba0680">c0e9c56c82057</a> ("patches: mainline: Add patch for drivers/scsi/aacraid")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b74e3dabfb7f7866f08ba2a6ab03b89edaf9f704">74e3dabfb7f78</a> ("patches: mainline: Add patch for RANDSTRUCT raid1-10.c error")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a7fe8735790adafb59fafe5b231fc5b3501697c4">7fe8735790ada</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e115cb3fe607cd4979b1658ee860139c3dda8aae">115cb3fe607cd</a> ("generator.yml: Update RISC-V gki_defconfig")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7c3248ee2d9cd52b7bcddafb0b8711eca4b70a26">c3248ee2d9cd5</a> ("check_logs.py: Remove unnecessary conversion to fix updated RUF010")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/daf3849c59f4cccf992665934450602f9447b11f">af3849c59f4cc</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8b80152d923d4f4012d39adb7d45a15711a2fb29">b80152d923d4f</a> ("patches: android-mainline: Drop upstream ext4 patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/85c2476dda8b24e8c058bc3fc6be7a3ab391a635">5c2476dda8b24</a> ("check_logs.py: Fix fetch_dtb() after refactoring")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/66e4a285290e2f2f620141f564b2059dff7504d2">6e4a285290e2f</a> ("check_logs.py: Make fetching boot-utils a little more obvious")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9c7edf5f8c9c68b4d023b5c4b044cd3eb9dd7ae9">c7edf5f8c9c68</a> ("check_logs.py: Improve downloading files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e948e213042a71ebf28086fafbd0e12ee88d31fd">948e213042a71</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4d941540d8b50e38f8d473297713a97c514a9330">d941540d8b50e</a> ("generator: Update Arch Linux x86_64 configuration link")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3c2d56b1a3a951425a1d531dece969a202c9418f">c2d56b1a3a951</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d1b6b8e786efb648a794f7f4d9ce5d775c66570c">1b6b8e786efb6</a> ("boot-utils: Remove submodule")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/279e5f3f761f63307a570fbec73fb544c5deb371">79e5f3f761f63</a> ("ci: Wire up '--gh-json-file'")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/15bdfb4a62d55c87f1f78fa883c9c829ccb3fa9a">5bdfb4a62d55c</a> ("check_logs.py: Download boot-utils straight from GitHub")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4292a9bf309cbcdcbe9748b076131193ee915eef">292a9bf309cbc</a> ("ci: Use pathlib and absolute paths everywhere")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/538765a95cd894bcbf32642c083fb8b8f23dfdea">38765a95cd894</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/dfc1013d101bae1e62e0b806a5538f74008f95ee">fc1013d101bae</a> ("generator.yml: Add PowerPC allmodconfig build for mainline with LLVM 15+")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/caf80ae49173150956f729e44e443d63e09a86ee">af80ae4917315</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/eeca8f0bb17e8d0cbf5ff1ae9858236e3ba591d7">eca8f0bb17e8d</a> ("generator.yml: Update stable anchor to 6.3")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c01b3b58fcc864ba88fac273b52228effd919f72">01b3b58fcc864</a> ("Revert #559")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/87093847d08ec1eff7fb99fe0dd6413ecbe64a25">7093847d08ec1</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/af2a06f06d3a6444a96b2ef0e51abeb35ef4784b">f2a06f06d3a64</a> ("patches: mainline: Add in-flight DRM revert to fix PowerPC breakage")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/50937a724626d5ae551f48cb0e0854eb18e67020">0937a724626d5</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5a8dc022dcdeccea755f55e575273defed0e1db4">a8dc022dcdecc</a> ("generator.yml: Disable CONFIG_SECURITY_CHROMIUMOS for CrOS builds")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/825f799ea6d93bead096af8e18fc247aaef2c6d3">25f799ea6d93b</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6e032947baa13b31b4c9095b6b7a804c4a402f1e">e032947baa13b</a> ("generator.yml: Disable RISC-V gki_defconfig with llvm_android for now")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/15aaf310db4226ea629e430caaf4e66d72faa918">5aaf310db4226</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c88c164bb7d9e3efa6418e81ae6b3e8ea7d2e8e3">88c164bb7d9e3</a> ("patches: Drop zicsr/zifencei 5.10 backport")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a9ad5367161abd85ce202622a0fb08c334c96103">9ad5367161abd</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5974403b653d2760371311860db9691ca696d436">974403b653d27</a> ("Update 64-bit big endian PowerPC configuration target")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3f0e5f094a9af0ee21094cf906f2a5f868ee2a7f">f0e5f094a9af0</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3fbec65f0f447e730501f4638e0add1fb752885e">fbec65f0f447e</a> ("patches: Add syncpt_irq patch to android-mainline")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5b308c3602995d818910d42289d367aa6301d5da">b308c3602995d</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/86451574eed0255f373fe42d7ea4c10f0c71a0a3">6451574eed025</a> ("patches: Add backport of e89c2e815e76 to 5.10")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ec1c96cc6478661ff9b196f80cd8c0c3ef2b4c91">c1c96cc647866</a> ("patches: Add arm64")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ad29e30d1893214dcf0e7b19da623ba9c8b1fa96">d29e30d189321</a> ("patches: arm64-fixes: Sync with upstream patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/acafc3b4dc98a6ec9b3c923408c8b4781a87fa44">cafc3b4dc98a6</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e6bdab7f63f360018a75edaec6a7b2ba2d45b2da">6bdab7f63f360</a> ("generator.yml: Disable x86_64 allyesconfig builds on stable")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3233a3455525405394b38dd12fea89dcb7a1ee3d">233a345552540</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ead965aaf5a3faec7a3cdde0203fbee6429100df">ad965aaf5a3fa</a> ("generator.yml: Turn x86_64 allyesconfig builds back on")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/32359d36990901a6e8a3a54ae625cb3955d21e96">2359d36990901</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/91ffd05bd595fe822249d7f52d996bba6967c4c2">1ffd05bd595fe</a> ("patches: Drop android12-5.4")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e4e706e6969c254430ab258a58ac27f692e45224">4e706e6969c25</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c7488227e2d0f009f3b85a3f7fe742f488846a96">7488227e2d0f0</a> ("patches: Drop majority of Android patches")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/63a79b73e8d8753d29ce0c71aa886731ea09f654">3a79b73e8d875</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e4abceae206b5c05e631a6aa2f3b20b1d4428ea9">4abceae206b5c</a> ("generator: Add PowerPC allmodconfig build")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d9668711c594f340382b949b031ed116251753e6">9668711c594f3</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b199b7d774d68e129d0327ddb6167b087ae316eb">199b7d774d68e</a> ("patches: Drop android14-6.1")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/83064d992c0fc559b2caaa3377d0f889daa45cf2">3064d992c0fc5</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0c52139074b76b3c41622ba1af6e0c7bb2110b94">c52139074b76b</a> ("patches: Drop mainline")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b65c29338461f059012cfbaf35f75b3ccf9c8b93">65c29338461f0</a> ("boot-utils: Update")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d833409e52b14dd508587fdd80d53c516330d68b">833409e52b14d</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/493ad7b7663870aff8f537b57f9af2c44be7033f">93ad7b7663870</a> ("patches: stable: Remove sockaddr patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3d56a6948c19ba3010a66cea5e764d63dd831aa7">d56a6948c19ba</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1b7fa85c9e23571fcf4ab5c9b57c9b52e244a9f4">b7fa85c9e2357</a> ("patches: Add host1x fix to arm64-fixes tree")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f0e331a7c6d81cc536fcd8a283aa18a096f591ec">0e331a7c6d81c</a> ("patches: Drop android-mainline")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/54ff83df15afc74a1325ac2eed1071292676b299">4ff83df15afc7</a> ("README: Add 6.1 builds")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/50d30bec77e9840bf5caf59a7bda8ae8fc578973">0d30bec77e984</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ecdaaa41075e40c7611713157cc7cac821885d0a">cdaaa41075e40</a> ("generator: Add 6.1 builds")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d02cbdfeb2a205f55ee7843bb4fc2147f99931bc">02cbdfeb2a205</a> ("generator: Update stable anchor to 6.2")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1db576e4e648b32e9f1a1593d2c4aaf03f13bc50">db576e4e648b3</a> ("patches: mainline: Drop memintristics series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4f9c88ce481290e8c18d4318549a7cfeb92d7981">f9c88ce481290</a> ("patches: android14-6.1: Drop alternatives series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4dde30ae0a0be9bf5671d7fc6106c882fd610cff">dde30ae0a0be9</a> ("scripts/estimate-builds.py: Use separate variable for total builds")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4334205a69a025523c713beb0c4f192d33a39f4a">334205a69a025</a> ("scripts/estimate-builds.py: Use an if statement instead of del to skip total")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/764d704dc520931fe41dfbe6bb4f7934868072b9">64d704dc52093</a> ("scripts/estimate-builds.py: Use more descriptive variable names")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/610c1f043f3ce157e68788fe4fd93f088a0e25ea">10c1f043f3ce1</a> ("scripts/estimate-builds.py: Use unpacking assignment for builds_per_tree iteration")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d409f9327545ccc13408cd59f8794d7e84758549">409f9327545cc</a> ("scripts/estimate-builds.py: Add comment around use of key plus reverse")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c3234a6841f9c339387a511cee69400606ab24b3">3234a6841f9c3</a> ("scripts/estimate-builds.py: Remove unnecessary trailing comma")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e500f351ecfae53c84cf87f6cae10388ef903f13">500f351ecfae5</a> ("scripts/estimate-builds.py: Use defaultdict")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e89a021ac9d4b4f01ed7f747f66c43109606db15">89a021ac9d4b4</a> ("scripts/estimate-builds.py: Place tree_name and tree_llvm_ver closer to initial use")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1fdb09ab746283677b21aa19f7b0b4ce9785d456">fdb09ab746283</a> ("scripts/estimate-builds.py: Hoist now and week_from_now out of for loop")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/46289191dd110577b259563204d9e5506bc62927">6289191dd1105</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/37a0980f01c34046e476007fdd94fb6691b2453e">7a0980f01c340</a> ("patches: Drop next series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4bd35f7852a339d3af074196a51807db7e58ee9d">bd35f7852a339</a> ("Add script to print the number of builds done per week per workflow")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/570df43330b57e261c4d610964c00618917e3161">70df43330b57e</a> ("ruff.toml: Add link to codes")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b1bfde82d1220321ee46074b2bde7233f0e2d439">1bfde82d12203</a> ("check_logs.py: Avoid overwriting loop variable")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d079d65fe9539f5ccdcf2058f458c7972d0061c8">079d65fe9539f</a> ("generate.py: Use unpacking instead of concatentation")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0b53d79443522a0154cc558a0b0c28001f261944">b53d79443522a</a> ("Add a ruff configuration file")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/97c8051553fd3365d6a94f482c76cca4bd0e63a4">7c8051553fd33</a> ("patches: stable: Drop alternatives series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0df94fff7239b6b38fbc28620627f10d96b38683">df94fff7239b6</a> ("patches: mainline: Add fix for host1x -Wuninitialized warning")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cf87a2de025f7aa7f87c98f0282b7821fe148e0d">f87a2de025f7a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/364df6459bb7a85076c2d26177d98bbeeeeefbef">64df6459bb7a8</a> ("patches: mainline: Drop alternatives series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4b662a8411227fbf29612ca5c94a0b577e4716ba">b662a8411227f</a> ("patches: Add a series to fix KASAN KUnit tests in mainline and -next")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e58a2daf8e53386c418025da7ac1c8ad7be6b9e6">58a2daf8e5338</a> ("generate_tuxsuite.py: Use simpler expression to get patches_folder from patches_flag")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/849dfd4916a92d2d2281bb5682b1348f4605fc67">49dfd4916a92d</a> ("generate_tuxsuite.py: Add small comment around string manipulation")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4cb798b007df22ce470547e58824029f1e73cacf">cb798b007df22</a> ("scripts/build-local.py: Use simpler expression for checking build result")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e3a1493447355709daeb9bae4f34c9987a40446b">3a14934473557</a> ("scripts/build-local.py: Remove exist_ok=True from mkdir()")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/38b6ca77caa22c260704538979317f7a3cb016b9">8b6ca77caa22c</a> ("scripts/build-local.py: Remove unused current_jobs")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ca24531b0468699c59c431ea5af2a441e9958e8c">a24531b046869</a> ("tuxsuite: Regenerate")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1bdf45d36b3a44258df07a4d7f006b81251d626d">bdf45d36b3a44</a> ("generate_tuxsuite.py: Add information about running locally")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b86e4ad4839bbf889e4aafd6e1f46fc41fd54cdd">86e4ad4839bbf</a> (".gitignore: Ignore tuxmake folder")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/444449a65d02bf20beded347c57ab2e3b10c65d0">44449a65d02bf</a> ("Add a script to build TuxSuite configurations locally")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b6c394e42094f4b9321aadb81ab646062fecef78">6c394e42094f4</a> ("README: Update for new builds")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c90e7e6a76508430068f992cb7b564046d2a6fcc">90e7e6a765084</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/57b8a35121f0098e4bb689f900cc3f0f794fbdfd">7b8a35121f009</a> ("generator: Add LLVM 16 builds")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8bfca870a6d595bc6adcced854cce15564358294">bfca870a6d595</a> ("generator: Move LLVM latest builds to LLVM 15 anchor")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/33ab4f662cc1c5dfb44db5fae666b9977d5a8ba2">3ab4f662cc1c5</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fedee2ef291f93e9e52312f127cdb4cd7ffe4669">edee2ef291f93</a> ("patches: Add workaround for cma.c issue in stable")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/354a8f6b6896930586c9dfe3c4e7c7512d94bd64">54a8f6b689693</a> ("patches: Add series for handling clang's conditional tail calls in static calls on x86")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/da0cfecc7c4d2f6c19f61e293dbd6987d6f199da">a0cfecc7c4d2f</a> ("README: Update position of markdown badges")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4269da741538ea973f71232cb465e5062c62a1b6">269da741538ea</a> ("scripts/markdown-badges.py: Stop relying on parse_version()")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f701d0da97562c8f8302e08ffdaca72a739d5e7a">701d0da97562c</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/eb1367c7f348958f0f4671d4330e3c6fa334a6b6">b1367c7f34895</a> ("generator: Enable KCSAN builds on stable")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/04d9bccfe1ddac9fb2129de25d1d09df86cc5431">4d9bccfe1ddac</a> ("Replace Exception with more specific RuntimeError")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b1fbc5ca34ca37a0c0c953812e4931a372579bd0">1fbc5ca34ca37</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/190118ae33fd4bf0998277d93f59f7345a775951">90118ae33fd4b</a> ("generator.yml: Drop s390 builds for LLVM < 13 on 5.10")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b877355fee2dd0b1c11587a4592426941082de16">877355fee2dd0</a> ("patches: android13-5.10: Remove pahole series")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a987b9d1ffd813709a70e370453db8ea45919104">987b9d1ffd813</a> ("README: Update for new workflows")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/82a802f0de81ae5e0108dae96f12e3bd9971feef">2a802f0de81ae</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/adaa433d93e4458125c34eb1a023d397974650ea">daa433d93e445</a> ("LLVM tip of tree is now 17")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c56dd963f54974635f236dd8ddbfa766fe0b7e92">56dd963f54974</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b91ad95c206c670f16745e387178c9010e3e9231">91ad95c206c67</a> ("patches: Drop mainline patch")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d7c0be8f2b758a47928f15488401e17fd58fa664">7c0be8f2b758a</a> ("scripts: check-matrix: Add comment above for loop explaining sys.exit() positioning")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/421a7fbde123856877e83380e7a6cf185dfdbe5d">21a7fbde12385</a> ("4.9 is now EOL")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a51242b35c2c0ddd3e3728adbc047c69b844ac15">51242b35c2c0d</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4c3e90881a82154c1ff8bc4dd999d22d77a6a6a3">c3e90881a8215</a> ("patches: next: Remove")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/75abc6fd36bcbe3833c81cee4187fd1986c7ed90">5abc6fd36bcbe</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8ed05deb488409d256ed881e34e90788e095ca0e">ed05deb488409</a> ("Revert "patches: Add patch for CONFIG_DRM_OFDRM=m and CONFIG_FB_OF=y"")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e8cf7d5e64a5263fb54b68b34fa34f120ec30bac">8cf7d5e64a526</a> ("Revert "generator.yml: Disable CONFIG_IPMMU_VMSA for OpenSUSE's RISC-V config"")</br>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f9c5b38e5d1fb464cccc80b53424044fb3e1504f">9c5b38e5d1fb4</a> ("patches: mainline: Add tentative fix for 99cb0d917ff")</br>
</code></p>
</details></br>

<details>
<summary>tc-build (<a href="https://github.com/ClangBuiltLinux/tc-build/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/6b42b6ce9a9785db1aa2ea5b45a2db76df03b4cb">b42b6ce9a9785</a> ("build-binutils.py: Fix error with x86-64-v{2,3,4} as value to '-m'")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/17103fcfce954da45d1325c40cb590442efe3e29">7103fcfce954d</a> ("build-llvm.py: Fix PGO using LLVM as training data without '--full-toolchain'")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/fc439f99b759fd9406a88c720827c98bf54d5be0">c439f99b759fd</a> ("tc_build: llvm: Update BOLT flags")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/c6765eb7fec5d7988d17d0cd5ea75928110f161d">6765eb7fec5d7</a> ("tc_build: llvm: Move use of build_targets out of ninja_cmd")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/3add5cbaa8033b25f48f31204dc524003b61cdc0">add5cbaa8033b</a> ("ci: Slim up LLVM build")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/504c8b6ff577af38f64f5e29859c804999d4358d">04c8b6ff577af</a> ("ci: Move to release/17.x")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/5275321cf9d27485c185aa577832577a2448dd5c">275321cf9d274</a> ("build-llvm.py: Update GOOD_REVISION to 0f8615f4dc568f4d7cbf73580eef3e78f64f3bd0")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/3f398ab34910a4a806e1294dbf41dd27ee5d293f">f398ab34910a4</a> ("build-llvm.py: Update DEFAULT_KERNEL_FOR_PGO to 6.6.0")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1b43d4626747027599403c13ee4fed0bac5a255c">b43d462674702</a> ("build-llvm.py: Add '--build-targets'")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/e916997047aa8197957980adca94c88e77869ec6">916997047aa81</a> ("tc_build: llvm: Only add tools to distribution_components with LLVM_BUILD_TOOLS=ON")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/e9a493d9abeef2346817ec141264acfc89e37be1">9a493d9abeef2</a> ("tc_build: llvm: Fix distribution_components with disabled projects")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/2711ccdfed2761706cc6b561c06e63e0bd849f71">711ccdfed2761</a> ("tc_build: llvm: Only add llvm-profdata and profile to components when building runtimes")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/0c91f4d591aeff5b8619468052240f887cdb28f4">c91f4d591aeff</a> ("tc_build: llvm: Use a separate list variable for distribution components")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/fb3ec05c0f4cd3112e1b2799a980e5cc00c49cd7">b3ec05c0f4cd3</a> ("workflows: Update to actions/checkout@v4")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/6074dcea467443e377b7a5b5743e88d2773e97c8">074dcea467443</a> ("build-llvm.py: Update GOOD_REVISION to e280e406c2e34ce29e1e71da7cd3a284ea112ce8")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/7d3969ff889ddfd968e0aeef3086a4e7da187c22">d3969ff889ddf</a> ("tc_build: kernel: Add support for ARCH=powerpc allmodconfig")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/5bec135c351fa4e168dada9d75cfe672123f7b5a">bec135c351fa4</a> ("tc_build: kernel: Build ppc64_guest_defconfig with LLVM=1")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/a28e3df2cc83dfba26b213f912f78890055d208a">28e3df2cc83df</a> ("build-llvm.py: Update DEFAULT_KERNEL_FOR_PGO to 6.5.0")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/4a1ae46d5aa66fb1a2c5e16d857a5096020422ea">a1ae46d5aa66f</a> ("tc_build: utils: Fix new Ruff warning (PIE808)")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/bea38c6d70af6fc91b47c9379ab0eca64463c51f">ea38c6d70af6f</a> ("tc_build: llvm: Switch to Path.glob()")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/490e3e07081bca1ded549e788ef52a2307583327">90e3e07081bca</a> ("build-binutils.py: 2.41")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/5fe43f285c44a47986c0a26c474af402c5b190fc">fe43f285c44a4</a> ("build-llvm.py: Update GOOD_REVISION to b5983a38cbf4eb405fe9583ab89e15db1dcfa173")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/78196400c4e846497f5512957cf47d13b5c05e5d">8196400c4e846</a> ("build-llvm.py: Update DEFAULT_KERNEL_FOR_PGO to 6.4.0")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1248eec37f77f0f0b22056c4f0253ecd474561ce">248eec37f77f0</a> ("build-llvm.py: Update GOOD_REVISION to 012ea747ed0275c499f69c82ac0f635f4c76f746")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/ec660a5dd9469354171e47988705dc8a80b1d263">c660a5dd94693</a> ("build-llvm.py: Update DEFAULT_KERNEL_FOR_PGO to 6.3.0")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/644a001cc1119f0d51d66a8e465a783597f0ce88">44a001cc1119f</a> ("tc_build: kernel: Adjust PowerPC64 configuration target")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/82d6f859a470e08101aa2445a03f42bc047ea2dc">2d6f859a470e0</a> ("ruff.toml: Disable S603 and S607")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b848280b6020567f1108ca4ff123858a36c82065">848280b602056</a> ("tc_build: Switch to absolute imports")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/e347cbc5a66b5b83bf5dfd592f68d1f4b50fb034">347cbc5a66b5b</a> ("binutils: Add support for LoongArch")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/e25d8ee4c62d616993a88a2587b4b53f2c198dfd">25d8ee4c62d61</a> ("tc_build: llvm: Enable LoongArch backend if it is in LLVM_ALL_TARGETS")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b5acae355e61dd96e59d14bb54f1f4cc857f429e">5acae355e61dd</a> ("build-llvm.py: Update GOOD_REVISION to 08b0977a1925cf0a2cf6f87fcbf1d656e873f7c5")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/c933dec65d26ec24d2257e7cf6c5b9ed53cdfc4f">933dec65d26ec</a> ("build-llvm.py: Update DEFAULT_KERNEL_FOR_PGO to 6.2.8")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/fc9db17da81e5dbaa8da800d1ab63c004dc2bb23">c9db17da81e5d</a> ("build-llvm.py: Switch to dict() over dictionary comprehension")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/be7a9782138edb2b0323d8f49e46c71f894137cc">e7a9782138edb</a> ("tc_build: llvm: Ensure local_ref is always assigned")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/0c1452ee9c5ea3d21ecfb5c08b682b75a9610f9f">c1452ee9c5ea3</a> ("tc_build: kernel: Simplify adding builders in LLVMKernelBuilder")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/01b65b97a480b3eeeab0c6759d2bfef6836f7189">1b65b97a480b3</a> ("tc_build: source: Use an identifier for magic number")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/4fd6541ae36f1fdb420c988810dc8b26255c01c2">fd6541ae36f1f</a> ("tc_build: llvm: Simplify host_target_is_enabled()")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/09d14a6b4f7d167d9d5620dfb2fb58e9a8269dff">9d14a6b4f7d16</a> ("tc_build: llvm: Fix omission of CMAKE_RANLIB")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/dd6ad72eaefab469bd5be79a894c5463faa351a8">d6ad72eaefab4</a> ("tc_build: llvm: Allow opting out of default target triple")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/3a3c8e9e83eba70608d964e3b33db65a0199bfa4">a3c8e9e83eba7</a> ("Add '--install-targets'")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/df566f1c1b52167c3c4e2b035af49f0014f2ef81">f566f1c1b5216</a> ("tc_build: kernel: Use ld.bfd for powerpc64le builds with ld.lld 11.x")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b4ca572c36f76f86dd65ef66bf7cd86b0e06f508">4ca572c36f76f</a> ("tc_build: llvm: Handle lack of commit 95bfd0849f7fb")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/e630799fff44839febefb587fc72a2ac6956e380">630799fff4483</a> ("tc_build: kernel: Restrict integrated assembler with RISC-V")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/486eeb2f24e11b4eef5d4b3fdc9bc6074fc217d6">86eeb2f24e11b</a> ("tc_build: kernel: Ensure -Werror gets disabled on PowerPC")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/9fbccf3823a83ebca00568d1139ceaf899a51bed">fbccf3823a83e</a> ("tc_build: kernel: Fix function name in S390KernelBuilder")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/3c23920ab569c00b5ea12e4df3c34a102602cfea">c23920ab569c0</a> ("build-llvm.py: Refactor else + if into elif to reduce indentation")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/9ae08cd307e68360f3eb24b054bbbcee36f28352">ae08cd307e683</a> ("build-*.py Fix new ruff warnings around iteration variables")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/15882f604ab302a140bb2339c2116c2653701fe6">5882f604ab302</a> ("tc_build: llvm: Combine nested if statement")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/5cf3ec43db6bcd139005bd73050c35dce8e8fa1b">cf3ec43db6bcd</a> ("tc_build: llvm: Ignore exception using contextlib.suppress()")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/d1d73c7b56b5bdb4126c69b293166da47c96db5e">1d73c7b56b5bd</a> ("tc_build: llvm: Use unpacking instead of concatentation")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/d6bbf229e2394cf57610c0ecf49c042dda1bdc7b">6bbf229e2394c</a> ("build-llvm.py: Use comprehension instead of constructor plus generator")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/69413a542409363417a31e0347649a33b2735d34">9413a54240936</a> ("build-llvm.py: Use ternary expression for targets assignment")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/c596c8e050bd92b062c9a066872dd421ba4fa619">596c8e050bd92</a> ("tc-build: Run 'ruff --fix --select COM .' and adjust formatting for yapf")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/d3c49cf6a743f32a1c6245ed27bb513dcffdc59f">3c49cf6a743f3</a> ("Add a ruff configuration file")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/888565c2e5424b47c22a2966c68b997a1eca1673">88565c2e5424b</a> ("build-binutils.py: Swap short flags for '--binutils-folder' and '--build-folder'")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/2dc673338d8b51d40fe06388e417aadee875be9b">dc673338d8b51</a> ("tc_build: llvm: Switch to upstream cmake compiler launcher variables")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/8bc3594fde9585df0cedeeb9e79a64412ad4c686">bc3594fde9585</a> ("tc-build: Modular rewrite")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/96de64e7ab98c9743a3d4a56827876cebb0b00e4">6de64e7ab98c9</a> ("tc-build: Remove current implementation")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1a2705cffe4b41302fd5b3ad210e23f5531037ef">a2705cffe4b41</a> ("tc-build: Print total script durations")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/5e95db2260af2c6a23b687316c3edbeb892a4ba7">e95db2260af2c</a> ("tc-build: Add custom duration printing function")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b4b54af57322fc547d44f6a51e3793c752f8d335">4b54af57322fc</a> ("build-llvm.py: Update known good revision")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/f82e2a638ad9ab0d6ee3dc6012d33bf924a04594">82e2a638ad9ab</a> ("kernel: Add patch to workaround __read_overflow() error with s390 allmodconfig")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/ef5044edbc77aa51f7d2a9013e0c6e17cedfa913">f5044edbc77aa</a> ("kernel: Update to Linux 6.1.7")</br>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/c6706c62b194421d25a3e95373afcdf539b34582">6706c62b19442</a> ("build-binutils.py: Fix using relative paths for folders")</br>
</code></p>
</details></br>

<details>
<summary>TuxMake (<a href="https://gitlab.com/Linaro/tuxmake/-/commits/master?author=Nathan%20Chancellor">GitLab</a>)</summary>
<p><code>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/4864e072fb8a4a337ebf53ae49336e9ee8dd80a7">864e072fb8a4a</a> ("docker: clang-android: Update to r498229b (17.0.4)")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/0c5d02c99fe86445d16425ea080b1e197a37c194">c5d02c99fe864</a> ("runtime: docker: Enable LoongArch for clang-17")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/a1e8a14b9c825ff933ce18c8916508422429cf21">1e8a14b9c825f</a> ("runtime: docker: Add clang-17")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/a0456922e6c0fd3b1fc12c7f50d6ceed9338fa18">0456922e6c0fd</a> ("arch: add support for building LoongArch")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/010e7b410f4cf9a0472ba1e72d046c3ff8f60389">10e7b410f4cf9</a> ("docker: clang-android: Update to r498229 (17.0.3)")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/d54374f996c10bc3818f5c5124725e6382fda38a">54374f996c10b</a> ("wrapper: Handle 5.18 updates to LLVM variable")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/e1ea5b1137399fe5055d91f39f9b3a0f6672ff4d">1ea5b1137399f</a> ("docker: Allow clang-android to target riscv")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/8d172bd539ea104559518c502e03eab99c35868c">d172bd539ea10</a> ("docker: clang-android: Update to r487747 (17.0.0)")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/e879dce240241c1f705efc4f99b86fecb381b1f3">879dce240241c</a> ("docker: install libclang-rt-dev on LLVM 14+")</br>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/3d633373c9abb83937a6928641aa4816439926e1">d633373c9abb8</a> (".gitlab-ci.yml: Stop creating GCC symlink for Arch Linux")</br>
</code></p>
</details></br>

## Behind the scenes

There are always things that require time but do not always show tangible results. There are a few things that I think fall under this category:

- __Issue tracker management:__ Keeping a clean and accurate issue tracker is critical for few reason.
  1. It gives a good high level overview of the "health" of the project. We want our issue tracker to be an accurate representation of how much help we need (since we always need it...)
  2. It helps assign priority to certain issues. If we have a lot of open but resolved issues, it can be hard to decide what needs to be worked on next.
  3. The issue tracker is a wonderful historical reference. We use the issue tracker to keep track of mailing list posts and such so it is important that those links are as acccurate as possible and have as much information as possible in case we have to look back five years later to wonder why we did something the way that we did.
- __Mailing list reading:__ We are not Cc'd on every issue related to `clang`, even though sometimes it is the compiler's problem or a known difference between the toolchains that we have already figured out. By monitoring the mailing list for certain phrases, we can provide assistance without being initially notified, [which developers do appreciate](https://lore.kernel.org/YtsY5xwmlQ6kFtUz@google.com/).
- __Hardware testing:__ Every linux-next release, I built and boot kernels on a variety of hardware to test for issues, as some problems only show up on bare metal or with a full distribution. I wrote a script to drive a full distribution QEMU on a variety of architectures to make some of that testing and debugging easier but bare metal is always an important testing target, since that is where the kernel will run the majority of the time.
- __Conferences__: I attended the [2023 US LLVM Developers' Meeting in Santa Clara, California](https://llvm.org/devmtg/2023-10/) and [Linux Plumbers Conference in Richmond, Virginia](https://lpc.events/event/17/). It was great to interact with many great people in each of those communities and talk about various issues, as these events are usually the only times of year when people are together in person.


## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://www.linuxfoundation.org) for [sponsoring my work](https://www.linuxfoundation.org/press/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security). I am in a very fortunate position thanks to the work of many great and suppportive folks at those organizations and I look forward to continuing to contribute under this umbrella for 2024!

To view individual monthly reports, click on one of the links below:

- [January 2023](/posts/january-2023-cbl-work/)
- [February 2023](/posts/february-2023-cbl-work/)
- [March 2023](/posts/march-2023-cbl-work/)
- [April 2023](/posts/april-2023-cbl-work/)
- [May 2023](/posts/may-2023-cbl-work/)
- [June 2023](/posts/june-2023-cbl-work/)
- [July 2023](/posts/july-2023-cbl-work/)
- [August 2023](/posts/august-2023-cbl-work/)
- [September 2023](/posts/september-2023-cbl-work/)
- [October 2023](/posts/october-2023-cbl-work/)
- [November 2023](/posts/november-2023-cbl-work/)
- [December 2023](/posts/december-2023-cbl-work/)
