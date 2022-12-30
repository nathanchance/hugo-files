---
title: 2022 ClangBuiltLinux Retrospective
date: 2022-12-30T11:00:00-0700
toc: false
images:
tags:
  - clangbuiltlinux
  - linux
  - linuxfoundation
  - maintainer
---

I have been contracting for the Linux Foundation for two years now, going onto the third, and it dawned on me that I have never done a retrospective or yearly report. This is useful for looking back on the year's worth of accomplishments, both to understand how much I have evolved and to look for areas that I would like to improve upon going forward. I have struggled with imposter syndrome for as long as I have been involved with the kernel community, so looking back to give credit where credit is due for particular solutions is a good way to try and combat that.


## Linux kernel

This year, I had 132 commits accepted and merged into mainline, which can either be viewed on [git.kernel.org](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/log/?qt=author&q=Nathan+Chancellor) or in a Linux repository locally using the following command, which I have included the output of in the collapsible section below:

<details>
<summary><code>git log --author='Nathan Chancellor' --since='Jan 1, 2022' --before='Jan 1, 2023'</code></summary>
<p><code><a href="https://git.kernel.org/linus/d6a9fb87e9d18f3394a9845546bbe868efdccfd2">d6a9fb87e9d1</a> ("security: Restrict CONFIG_ZERO_CALL_USED_REGS to gcc or clang > 15.0.6")<br/>
<a href="https://git.kernel.org/linus/19331e84c3873256537d446afec1f6c507f8c4ef">19331e84c387</a> ("modpost: Include '.text.*' in TEXT_SECTIONS")<br/>
<a href="https://git.kernel.org/linus/0d24f1b7cc65ee73ea8d04e0d10f77a7cb7a83f3">0d24f1b7cc65</a> ("padata: Mark padata_work_init() as __ref")<br/>
<a href="https://git.kernel.org/linus/45be2ad007a9c6bea70249c4cf3e4905afe4caeb">45be2ad007a9</a> ("x86/vdso: Conditionally export __vdso_sgx_enter_enclave()")<br/>
<a href="https://git.kernel.org/linus/1aba7930c63ea57b4918dc61dcc315f451cec21e">1aba7930c63e</a> ("media: rzg2l-cru: Remove unnecessary shadowing of ret in rzg2l_csi2_s_stream()")<br/>
<a href="https://git.kernel.org/linus/9e913888647be447c3d114042428f02d24676390">9e913888647b</a> ("hwmon: (smpro-hwmon) Improve switch statments in smpro_is_visible()")<br/>
<a href="https://git.kernel.org/linus/59e2cf8d21e05391c42628eb9fb5bb40f9d9698f">59e2cf8d21e0</a> ("ARM: 9275/1: Drop '-mthumb' from AFLAGS_ISA")<br/>
<a href="https://git.kernel.org/linus/0ad811cc08a937d875cbad0149c1bab17f84ba05">0ad811cc08a9</a> ("drm/sti: Fix return type of sti_{dvo,hda,hdmi}_connector_mode_valid()")<br/>
<a href="https://git.kernel.org/linus/96d845a67b7e406cfed7880a724c8ca6121e022e">96d845a67b7e</a> ("drm/fsl-dcu: Fix return type of fsl_dcu_drm_connector_mode_valid()")<br/>
<a href="https://git.kernel.org/linus/cab75e1c8e42ebb4bb386247a928af6ab412e82b">cab75e1c8e42</a> ("cpufreq: ACPI: Remove unused variables 'acpi_cpufreq_online' and 'ret'")<br/>
<a href="https://git.kernel.org/linus/913a144164d8f09fab7e4175d693168b29d5843b">913a144164d8</a> ("HSI: ssi_protocol: Fix return type of ssip_pn_xmit()")<br/>
<a href="https://git.kernel.org/linus/890d637523eec9d730e3885532fa1228ba678880">890d637523ee</a> ("drm/mediatek: Fix return type of mtk_hdmi_bridge_mode_valid()")<br/>
<a href="https://git.kernel.org/linus/75b5e47201329537c8b88531a59aab2cbcec8d61">75b5e4720132</a> ("fs/ntfs3: Eliminate unnecessary ternary operator in ntfs_d_compare()")<br/>
<a href="https://git.kernel.org/linus/fcae44fd36d052e956e69a64642fc03820968d78">fcae44fd36d0</a> ("RISC-V: vdso: Do not add missing symbols to version section in linker script")<br/>
<a href="https://git.kernel.org/linus/000f8870a47bdc36730357883b6aef42bced91ee">000f8870a47b</a> ("vmlinux.lds.h: Fix placement of '.data..decrypted' section")<br/>
<a href="https://git.kernel.org/linus/3d75e766b58a7410d4e835c534e1b4664a8f62d0">3d75e766b58a</a> ("scsi: elx: libefc: Fix second parameter type in state callbacks")<br/>
<a href="https://git.kernel.org/linus/a679120edfcf3d63f066f53afd425d51b480e533">a679120edfcf</a> ("bpf: Add explicit cast to 'void *' for __BPF_DISPATCHER_UPDATE()")<br/>
<a href="https://git.kernel.org/linus/bb16db8393658e0978c3f0d30ae069e878264fa3">bb16db839365</a> ("s390/lcs: Fix return type of lcs_start_xmit()")<br/>
<a href="https://git.kernel.org/linus/88d86d18d7cf7e9137c95f9d212bb9fff8a1b4be">88d86d18d7cf</a> ("s390/netiucv: Fix return type of netiucv_tx()")<br/>
<a href="https://git.kernel.org/linus/aa5bf80c3c067b82b4362cd6e8e2194623bcaca6">aa5bf80c3c06</a> ("s390/ctcm: Fix return type of ctc{mp,}m_tx()")<br/>
<a href="https://git.kernel.org/linus/8e0aa1ff44ca30b0b9df95dfa72003522d9a088d">8e0aa1ff44ca</a> ("net: ethernet: renesas: Fix return type of rswitch_start_xmit()")<br/>
<a href="https://git.kernel.org/linus/e4d0ef752081e7aa6ffb7ccac11c499c732a2e05">e4d0ef752081</a> ("drm/amdgpu: Fix type of second parameter in odn_edit_dpm_table() callback")<br/>
<a href="https://git.kernel.org/linus/f0d0f1087333714ee683cc134a95afe331d7ddd9">f0d0f1087333</a> ("drm/amdgpu: Fix type of second parameter in trans_msg() callback")<br/>
<a href="https://git.kernel.org/linus/c5733e5b15d91ab679646ec3149e192996a27d5d">c5733e5b15d9</a> ("hamradio: baycom_epp: Fix return type of baycom_send_packet()")<br/>
<a href="https://git.kernel.org/linus/63fe6ff674a96cfcfc0fa8df1051a27aa31c70b4">63fe6ff674a9</a> ("net: ethernet: ti: Fix return type of netcp_ndo_start_xmit()")<br/>
<a href="https://git.kernel.org/linus/6c4e4d35203301906afb53c6d1e1302d4c793c05">6c4e4d352033</a> ("drm/meson: Fix return type of meson_encoder_cvbs_mode_valid()")<br/>
<a href="https://git.kernel.org/linus/a8a4f0467d706fc22d286dfa973946e5944b793c">a8a4f0467d70</a> ("drm/i915: Fix CFI violations in gt_sysfs")<br/>
<a href="https://git.kernel.org/linus/0d6d7c61ffeedc782b651a080ad6543ad45314b6">0d6d7c61ffee</a> ("fs/ntfs3: Don't use uni1 uninitialized in ntfs_d_compare()")<br/>
<a href="https://git.kernel.org/linus/e688ba3e276422aa88eae7a54186a95320836081">e688ba3e2764</a> ("drm/amdkfd: Fix type of reset_type parameter in hqd_destroy() callback")<br/>
<a href="https://git.kernel.org/linus/e299b00adf3d4505132e624894f549422ad05eeb">e299b00adf3d</a> ("drm/amdkfd: Fix type of reset_type parameter in hqd_destroy() callback")<br/>
<a href="https://git.kernel.org/linus/33806e7cb8d50379f55c3e8f335e91e1b359dc7b">33806e7cb8d5</a> ("x86/Kconfig: Drop check for -mabi=ms for CONFIG_EFI_STUB")<br/>
<a href="https://git.kernel.org/linus/0a6de78cff600cb991f2a1b7ed376935871796a0">0a6de78cff60</a> ("lib/Kconfig.debug: Add check for non-constant .{s,u}leb128 support to DWARF5")<br/>
<a href="https://git.kernel.org/linus/2130b87b2273389cafe6765bf09ef564cda01407">2130b87b2273</a> ("drm/amd/display: Fix build breakage with CONFIG_DEBUG_FS=n")<br/>
<a href="https://git.kernel.org/linus/d2995249a2f72333a4ab4922ff3c42a76c023791">d2995249a2f7</a> ("arm64: alternatives: Use vdso/bits.h instead of linux/bits.h")<br/>
<a href="https://git.kernel.org/linus/a15e17acce5aaae54243f55a7349c2225450b9bc">a15e17acce5a</a> ("usb: gadget: uvc: Fix argument to sizeof() in uvc_register_video()")<br/>
<a href="https://git.kernel.org/linus/0402cca4828dd9556d36ddef67710993b7063f7c">0402cca4828d</a> ("ASoC: Intel: sof_da7219_mx98360a: Access num_codecs through dai_link")<br/>
<a href="https://git.kernel.org/linus/9af48b262675561eefd6edc11b4b02854e6a18ae">9af48b262675</a> ("platform/x86/amd: pmc: Fix build without debugfs")<br/>
<a href="https://git.kernel.org/linus/f525ed19437d376736bed64ee7bc4afee82f2ba9">f525ed19437d</a> ("drm/amd/display: Reduce number of arguments of dml314's CalculateFlipSchedule()")<br/>
<a href="https://git.kernel.org/linus/faed5d0182480556cddb8343d9bad968387848f4">faed5d018248</a> ("drm/amd/display: Reduce number of arguments of dml314's CalculateWatermarksAndDRAMSpeedChangeSupport()")<br/>
<a href="https://git.kernel.org/linus/db74cd6337d2691ea932e36b84683090f0712ec1">db74cd6337d2</a> ("arm64/sysreg: Fix a few missed conversions")<br/>
<a href="https://git.kernel.org/linus/25ea501ed85dc3c224db73fb79d38b6109c1ad99">25ea501ed85d</a> ("drm/amd/display: Reduce number of arguments of dml314's CalculateFlipSchedule()")<br/>
<a href="https://git.kernel.org/linus/ca07f4f5a98b96211a2a8fe51b35c039720be888">ca07f4f5a98b</a> ("drm/amd/display: Reduce number of arguments of dml314's CalculateWatermarksAndDRAMSpeedChangeSupport()")<br/>
<a href="https://git.kernel.org/linus/2e50e9bf328fb781c9fcd5dc6531458dd02d1626">2e50e9bf328f</a> ("net/mlx5e: Ensure macsec_rule is always initiailized in macsec_fs_{r,t}x_add_rule()")<br/>
<a href="https://git.kernel.org/linus/682493e401a2c5f7a5f6d851939f7c03536970ba">682493e401a2</a> ("drm/msm/dsi: Remove use of device_node in dsi_host_parse_dt()")<br/>
<a href="https://git.kernel.org/linus/55cafd4ba42cf495268f955dd38e277fc4b4381e">55cafd4ba42c</a> ("power: supply: bq25890: Fix enum conversion in bq25890_power_supply_set_property()")<br/>
<a href="https://git.kernel.org/linus/b0f4b23fc3dbd8c5398e9ea9cf1f16a00d9006a2">b0f4b23fc3db</a> ("drm/amd/display: Mark dml30's UseMinimumDCFCLK() as noinline for stack usage")<br/>
<a href="https://git.kernel.org/linus/1dbec5b4b0ef319d6961d3ecb7384b4f9ef9d358">1dbec5b4b0ef</a> ("drm/amd/display: Reduce number of arguments of dml31's CalculateFlipSchedule()")<br/>
<a href="https://git.kernel.org/linus/ab2ac59c32dbec068954de30eda741d012be3c74">ab2ac59c32db</a> ("drm/amd/display: Reduce number of arguments of dml31's CalculateWatermarksAndDRAMSpeedChangeSupport()")<br/>
<a href="https://git.kernel.org/linus/3b4e83a232244e2fe911bd39b322e0dc19b22434">3b4e83a23224</a> ("drm/amd/display: Reduce number of arguments of dml32_CalculatePrefetchSchedule()")<br/>
<a href="https://git.kernel.org/linus/1df7e569522486e58307929a726ec8f303c5abf4">1df7e5695224</a> ("drm/amd/display: Reduce number of arguments of dml32_CalculateWatermarksMALLUseAndDRAMSpeedChangeSupport()")<br/>
<a href="https://git.kernel.org/linus/41012d715d5d7b9751ae84b8fb255e404ac9c5d0">41012d715d5d</a> ("drm/amd/display: Mark dml30's UseMinimumDCFCLK() as noinline for stack usage")<br/>
<a href="https://git.kernel.org/linus/21485d3da659b66c37d99071623af83ee1c6733d">21485d3da659</a> ("drm/amd/display: Reduce number of arguments of dml31's CalculateFlipSchedule()")<br/>
<a href="https://git.kernel.org/linus/37934d4118e22bceb80141804391975078f31734">37934d4118e2</a> ("drm/amd/display: Reduce number of arguments of dml31's CalculateWatermarksAndDRAMSpeedChangeSupport()")<br/>
<a href="https://git.kernel.org/linus/a3fef74b1d48d89d4d911fcd7c2630d0eb6a0012">a3fef74b1d48</a> ("drm/amd/display: Reduce number of arguments of dml32_CalculatePrefetchSchedule()")<br/>
<a href="https://git.kernel.org/linus/c4be0ac987f21e12e7ad23bc480e826d8c30de20">c4be0ac987f2</a> ("drm/amd/display: Reduce number of arguments of dml32_CalculateWatermarksMALLUseAndDRAMSpeedChangeSupport()")<br/>
<a href="https://git.kernel.org/linus/269e633dad16c951449c99cc3bfce46110f5029e">269e633dad16</a> ("coresight: cti-sysfs: Mark coresight_cti_reg_store() as __maybe_unused")<br/>
<a href="https://git.kernel.org/linus/cfe0d370e0788625ce0df3239aad07a2506c1796">cfe0d370e078</a> ("powerpc/math_emu/efp: Include module.h")<br/>
<a href="https://git.kernel.org/linus/6cf07810e9ef8535d60160d13bf0fd05f2af38e7">6cf07810e9ef</a> ("powerpc/papr_scm: Ensure rc is always initialized in papr_scm_pmu_register()")<br/>
<a href="https://git.kernel.org/linus/92f97c00f0ca538fe820c0b18b6f957a69410689">92f97c00f0ca</a> ("net/mlx5e: Do not use err uninitialized in mlx5e_rep_add_meta_tunnel_rule()")<br/>
<a href="https://git.kernel.org/linus/7d3ac70d82080f7a934402d66c5238e1d99be412">7d3ac70d8208</a> ("ASoC: codes: src4xxx: Avoid clang -Wsometimes-uninitialized in src4xxx_hw_params()")<br/>
<a href="https://git.kernel.org/linus/5c5c2baad2b55cc0a4b190266889959642298f79">5c5c2baad2b5</a> ("ASoC: mchp-spdiftx: Fix clang -Wbitfield-constant-conversion")<br/>
<a href="https://git.kernel.org/linus/370655bc183b1824ba623e621b58e8c2616c839c">370655bc183b</a> ("scripts/Makefile.extrawarn: Do not disable clang's -Wformat-zero-length")<br/>
<a href="https://git.kernel.org/linus/eab9100d9898cbd37882b04415b12156f8942f18">eab9100d9898</a> ("ASoC: mchp-spdiftx: Fix clang -Wbitfield-constant-conversion")<br/>
<a href="https://git.kernel.org/linus/f9929f69de94212f98b3ad72a3e81c3bd3d333e0">f9929f69de94</a> ("drm/simpledrm: Fix return type of simpledrm_simple_display_pipe_mode_valid()")<br/>
<a href="https://git.kernel.org/linus/78acd4ca433425e6dd4032cfc2156c60e34931f2">78acd4ca4334</a> ("usb: cdns3: Don't use priv_dev uninitialized in cdns3_gadget_ep_enable()")<br/>
<a href="https://git.kernel.org/linus/74de14fe05dd6b151d73cb0c73c8ec874cbdcde6">74de14fe05dd</a> ("MIPS: tlbex: Explicitly compare _PAGE_NO_EXEC against 0")<br/>
<a href="https://git.kernel.org/linus/0c09bc33aa8e9dc867300acaadc318c2f0d85a1e">0c09bc33aa8e</a> ("drm/simpledrm: Fix return type of simpledrm_simple_display_pipe_mode_valid()")<br/>
<a href="https://git.kernel.org/linus/d81677410f172c2b946393c09b46ff9e8dc1b6ec">d81677410f17</a> ("ASoC: amd: acp: Fix initialization of ext_intr_stat1 in i2s_irq_handler()")<br/>
<a href="https://git.kernel.org/linus/db886979683a8360ced9b24ab1125ad0c4d2cf76">db886979683a</a> ("x86/speculation: Use DECLARE_PER_CPU for x86_spec_ctrl_current")<br/>
<a href="https://git.kernel.org/linus/33f32e5072b6cc84d1b130a3ad485849bcec907a">33f32e5072b6</a> ("bpf, arm64: Mark dummy_tramp as global")<br/>
<a href="https://git.kernel.org/linus/c3c0ed75ffbff5c70667030b5139bbb75b0a30f5">c3c0ed75ffbf</a> ("mmc: sdhci-brcmstb: Initialize base_clk to NULL in sdhci_brcmstb_probe()")<br/>
<a href="https://git.kernel.org/linus/1b8667812b3a1304f3db736ac4905d6ad77d721e">1b8667812b3a</a> ("x86/Kconfig: Fix CONFIG_CC_HAS_SANE_STACKPROTECTOR when cross compiling with clang")<br/>
<a href="https://git.kernel.org/linus/c749d676a33d99ee4c40d69ac2bf280270d890ad">c749d676a33d</a> ("soc: mediatek: SVS: Use DEFINE_SIMPLE_DEV_PM_OPS for svs_pm_ops")<br/>
<a href="https://git.kernel.org/linus/9e07352ef7791b08c3784acb2822dd9363ec4eae">9e07352ef779</a> ("arm64: vdso32: Add DWARF_DEBUG")<br/>
<a href="https://git.kernel.org/linus/859716b4131feb42fd2e3d87fbc25ec28152c029">859716b4131f</a> ("arm64: vdso32: Shuffle .ARM.exidx section above ELF_DETAILS")<br/>
<a href="https://git.kernel.org/linus/10a9035c36d00586ad4bdb838f8800be951db8d2">10a9035c36d0</a> ("drm/amd/display: Fix indentation in dcn32_get_vco_frequency_from_reg()")<br/>
<a href="https://git.kernel.org/linus/e83031564137cf37e07c2d10ad468046ff48a0cf">e83031564137</a> ("riscv: Fix ALT_THEAD_PMA's asm parameters")<br/>
<a href="https://git.kernel.org/linus/bd476c1306ea989d6d9eb65295572e98d93edeb6">bd476c1306ea</a> ("misc: rtsx: Fix clang -Wsometimes-uninitialized in rts5261_init_from_hw()")<br/>
<a href="https://git.kernel.org/linus/61114e734ccb804bc12561ab4020745e02c468c2">61114e734ccb</a> ("riscv: Move alternative length validation into subsection")<br/>
<a href="https://git.kernel.org/linus/48a75b979940f8aa838143e976455aa0e629e638">48a75b979940</a> ("ath6kl: Use cc-disable-warning to disable -Wdangling-pointer")<br/>
<a href="https://git.kernel.org/linus/79f9fbe303520d2c32b70f04f2bb02cc2baaa4c3">79f9fbe30352</a> ("mailbox: qcom-ipcc: Fix -Wunused-function with CONFIG_PM_SLEEP=n")<br/>
<a href="https://git.kernel.org/linus/4406b12214f6592909b63dabdea86d69f1b5ba2e">4406b12214f6</a> ("powerpc/vdso: Link with ld.lld when requested")<br/>
<a href="https://git.kernel.org/linus/e247172854a57d1a7213bb835ecb4a40ce9bb2b9">e247172854a5</a> ("powerpc/vdso: Remove unused ENTRY in linker scripts")<br/>
<a href="https://git.kernel.org/linus/58606220a2f1407a7516c547f09a1ba7b4350a73">58606220a2f1</a> ("drm/i915: Fix CFI violation with show_dynamic_id()")<br/>
<a href="https://git.kernel.org/linus/6977262c2eee111645668fe9e235ef2f5694abf7">6977262c2eee</a> ("i2c: at91: Initialize dma_buf in at91_twi_xfer()")<br/>
<a href="https://git.kernel.org/linus/18fb42db05a0b93ab5dd5eab5315e50eaa3ca620">18fb42db05a0</a> ("drm/i915: Fix CFI violation with show_dynamic_id()")<br/>
<a href="https://git.kernel.org/linus/acd51562e07d17aaf4ac652f1dc55c743685bf41">acd51562e07d</a> ("platform/x86: amd-pmc: Shuffle location of amd_pmc_get_smu_version()")<br/>
<a href="https://git.kernel.org/linus/45bd8951806eb5e857772c593de021b09057950d">45bd8951806e</a> ("arm64: Improve HAVE_DYNAMIC_FTRACE_WITH_REGS selection for clang")<br/>
<a href="https://git.kernel.org/linus/390d645877ffd6dcb55f162d618045b2779217b3">390d645877ff</a> ("drm/msm/gpu: Avoid -Wunused-function with !CONFIG_PM_SLEEP")<br/>
<a href="https://git.kernel.org/linus/6d4a6b515c39f1f8763093e0f828959b2fbc2f45">6d4a6b515c39</a> ("btrfs: remove unused variable in btrfs_{start,write}_dirty_block_groups()")<br/>
<a href="https://git.kernel.org/linus/83a1cde5c74bfb44b49cb2a940d044bb2380f4ea">83a1cde5c74b</a> ("ARM: davinci: da850-evm: Avoid NULL pointer dereference")<br/>
<a href="https://git.kernel.org/linus/1e39036de5fce1b0c61204c546ca04aff6eafc71">1e39036de5fc</a> ("Revert "um: clang: Strip out -mno-global-merge from USER_CFLAGS"")<br/>
<a href="https://git.kernel.org/linus/cf300b83c793c25c6b485fdaf7a4447d8ea4c655">cf300b83c793</a> ("kbuild: Remove '-mno-global-merge'")<br/>
<a href="https://git.kernel.org/linus/e9c281928c24dfeb86b11c31b53757b6a127f8aa">e9c281928c24</a> ("kbuild: Make $(LLVM) more flexible")<br/>
<a href="https://git.kernel.org/linus/07ea4ab1f9b83953ff5c3f6ccfb84d581bfe0046">07ea4ab1f9b8</a> ("KVM: x86: Fix clang -Wimplicit-fallthrough in do_host_cpuid()")<br/>
<a href="https://git.kernel.org/linus/6b49f3409a090c8e9d1f46ff2705c479b45a54d4">6b49f3409a09</a> ("dt-bindings: kbuild: Make DT_SCHEMA_LINT a recursive variable")<br/>
<a href="https://git.kernel.org/linus/a4812d47deff0642b3315f0528d579f0a99c45c2">a4812d47deff</a> ("mm/page_alloc: mark pagesets as __maybe_unused")<br/>
<a href="https://git.kernel.org/linus/f6a2c2b2de817078ac5a7e58c10e746165e7825d">f6a2c2b2de81</a> ("x86/Kconfig: Only allow CONFIG_X86_KERNEL_IBT with ld.lld >= 14.0.0")<br/>
<a href="https://git.kernel.org/linus/262448f3d18959d175b10e28a3b65f41d1d7313f">262448f3d189</a> ("x86/Kconfig: Only enable CONFIG_CC_HAS_IBT for clang >= 14.0.0")<br/>
<a href="https://git.kernel.org/linus/a860f266a0e19f271b839451d291a6acf6ddcfe8">a860f266a0e1</a> ("drm/selftest: plane_helper: Put test structures in static storage")<br/>
<a href="https://git.kernel.org/linus/aaeed6ecc1253ce1463fa1aca0b70a4ccbc9fa75">aaeed6ecc125</a> ("x86/Kconfig: Do not allow CONFIG_X86_X32_ABI=y with llvm-objcopy")<br/>
<a href="https://git.kernel.org/linus/d0583229bcf5090495d0621892d5da93d9fb6e3c">d0583229bcf5</a> ("i2c: designware: Mark dw_i2c_plat_{suspend,resume}() as __maybe_unused")<br/>
<a href="https://git.kernel.org/linus/f0a4e62bb5343b3b7163bc851cfb4bebfefcc4e7">f0a4e62bb534</a> ("io_uring: Fix use of uninitialized ret in io_eventfd_register()")<br/>
<a href="https://git.kernel.org/linus/36168e387fa7d0f1fe0cd5cf76c8cea7aee714fa">36168e387fa7</a> ("ARM: Do not use NOCROSSREFS directive with ld.lld")<br/>
<a href="https://git.kernel.org/linus/52c9f93a9c482251cb0d224faa602ba26d462be8">52c9f93a9c48</a> ("arm64: Do not include __READ_ONCE() block in assembly files")<br/>
<a href="https://git.kernel.org/linus/bf127df3cceada8693888fc86a3121c38ef25701">bf127df3ccea</a> ("clocksource/drivers/imx-tpm: Move tpm_read_sched_clock() under CONFIG_ARM")<br/>
<a href="https://git.kernel.org/linus/f014a00bbeb09cea16017b82448d32a468a6b96f">f014a00bbeb0</a> ("compiler-clang.h: Add __diag infrastructure for clang")<br/>
<a href="https://git.kernel.org/linus/185897d03ca3c4c98eff5cbf151671c5f88165fb">185897d03ca3</a> ("iio: accel: adxl367: Fix handled initialization in adxl367_irq_handler()")<br/>
<a href="https://git.kernel.org/linus/ab2f993c01f261aa3eeb8842842ff38bff7806b6">ab2f993c01f2</a> ("ftrace: Remove unused ftrace_startup_enable() stub")<br/>
<a href="https://git.kernel.org/linus/3b2f68f196a5b23c61777078f5c7d0d19626b5e4">3b2f68f196a5</a> ("drm/stm: Avoid using val uninitialized in ltdc_set_ycbcr_config()")<br/>
<a href="https://git.kernel.org/linus/b63c54d978236dd6014cf2ffba96d626e97c915c">b63c54d97823</a> ("drm/amdkfd: Use proper enum in pm_unmap_queues_v9()")<br/>
<a href="https://git.kernel.org/linus/c47c7ab9b53635860c6b48736efdd22822d726d7">c47c7ab9b536</a> ("MIPS: Malta: Enable BLK_DEV_INITRD")<br/>
<a href="https://git.kernel.org/linus/1cf5f151d25fcca94689efd91afa0253621fb33a">1cf5f151d25f</a> ("Makefile.extrawarn: Move -Wunaligned-access to W=1")<br/>
<a href="https://git.kernel.org/linus/345be4275cad454ae7e25884369a9c6c25e56279">345be4275cad</a> ("thermal: netlink: Fix parameter type of thermal_genl_cpu_capability_event() stub")<br/>
<a href="https://git.kernel.org/linus/d49fc69293f2022785f6ee1c3c7d565271abe393">d49fc69293f2</a> ("MIPS: Loongson{2ef,64}: Wrap -mno-branch-likely with cc-option")<br/>
<a href="https://git.kernel.org/linus/0e96ea5c3eb5904e5dc2f3d414e2aba361933b87">0e96ea5c3eb5</a> ("MIPS: Loongson64: Clean up use of cc-ifversion")<br/>
<a href="https://git.kernel.org/linus/7f3bdbc3f13146eb9d07de81ea71f551587a384b">7f3bdbc3f131</a> ("tools/resolve_btfids: Do not print any commands when building silently")<br/>
<a href="https://git.kernel.org/linus/42d9b379e3e1790eafb87c799c9edfd0b37a37c7">42d9b379e3e1</a> ("lib/Kconfig.debug: Allow BTF + DWARF5 with pahole 1.21+")<br/>
<a href="https://git.kernel.org/linus/6323c81350b73a4569cf52df85f80273faa64071">6323c81350b7</a> ("lib/Kconfig.debug: Use CONFIG_PAHOLE_VERSION")<br/>
<a href="https://git.kernel.org/linus/2d6c9810eb8915c4ddede707b8e167a1d919e1ca">2d6c9810eb89</a> ("scripts/pahole-flags.sh: Use pahole-version.sh")<br/>
<a href="https://git.kernel.org/linus/613fe169237785a4bb1d06397b52606b2967da53">613fe1692377</a> ("kbuild: Add CONFIG_PAHOLE_VERSION")<br/>
<a href="https://git.kernel.org/linus/f67644b4f282d42acf5ad9b0175ef5671314ab12">f67644b4f282</a> ("MAINTAINERS: Add scripts/pahole-flags.sh to BPF section")<br/>
<a href="https://git.kernel.org/linus/bbd2e05fad3e692ff2495895975bd0fce02bdbae">bbd2e05fad3e</a> ("lib/Kconfig.debug: make TEST_KMOD depend on PAGE_SIZE_LESS_THAN_256KB")<br/>
<a href="https://git.kernel.org/linus/e9009095998a8de4491692e89ca303fb74047c9e">e9009095998a</a> ("btrfs: use generic Kconfig option for 256kB page size limit")<br/>
<a href="https://git.kernel.org/linus/e4bbd20d8c2b9fb5a937bf132775f5257ccb0412">e4bbd20d8c2b</a> ("arch/Kconfig: split PAGE_SIZE_LESS_THAN_256KB from PAGE_SIZE_LESS_THAN_64KB")<br/>
<a href="https://git.kernel.org/linus/a89eeb9937a0124e609e9355cd48cdfe35c8b8b7">a89eeb9937a0</a> ("media: atomisp: Do not define input_system_cfg2400_t twice")<br/>
<a href="https://git.kernel.org/linus/4ccdcc8ffd955490feec05380223db6a48961eb5">4ccdcc8ffd95</a> ("iwlwifi: mvm: Use div_s64 instead of do_div in iwl_mvm_ftm_rtt_smoothing()")<br/>
<a href="https://git.kernel.org/linus/751971af2e3615dc5bd12674080bc795505fefeb">751971af2e36</a> ("csky: Fix function name in csky_alignment() and die()")<br/>
<a href="https://git.kernel.org/linus/ab4ababdf77ccc56c7301c751dff49c79709c51c">ab4ababdf77c</a> ("h8300: Fix build errors from do_exit() to make_task_dead() transition")<br/>
<a href="https://git.kernel.org/linus/4f0712ccec09c071e221242a2db9a6779a55a949">4f0712ccec09</a> ("hexagon: Fix function name in die()")<br/>
<a href="https://git.kernel.org/linus/4e31bfa37662f72e8e7e3ae46eb5f845a5854229">4e31bfa37662</a> ("clk: visconti: Remove pointless NULL check in visconti_pll_add_lookup()")<br/>
<a href="https://git.kernel.org/linus/be185c2988b48db65348d94168c793bdbc8d23c3">be185c2988b4</a> ("cxl/core: Remove cxld_const_init in cxl_decoder_alloc()")<br/>
</code></p>
</details></br>

You will notice that I spend a lot of time fixing warnings, which have been a higher priority ever since `CONFIG_WERROR` was introduced last year. `allmodconfig` enables `CONFIG_WERROR`, which is a common testing target for developers and continuous integration, so it is important to keep this working. As a direct result of that work and the work of others such as Arnd Bergmann, we were able to stop disabling `CONFIG_WERROR` for [arm64 allmodconfig](https://github.com/ClangBuiltLinux/continuous-integration2/pull/412) and [x86_64 allmodconfig](https://github.com/ClangBuiltLinux/continuous-integration2/pull/385) this year, which gives us leverage when things break. Linus himself has voiced displeasure when things break with `clang` ([example](https://lore.kernel.org/CAHk-=wjS6ptr5=JqmmyEb_qTjDz_68+S=h1o1bL1fEyArVOymA@mail.gmail.com/)) but that can only happen when the build with `clang` continues to work. Additionally, ever since GCC's `-Wmaybe-uninitialized` was [globally disabled for a normal kernel build in 5.7](https://git.kernel.org/linus/78a5255ffb6a1af189a83e493d916ba1c54d8c75), `clang`'s `-Wuninitialized` and `-Wsometimes-uninitialized` have become a constant nuisance since developers will introduce them inadvertently and not realize it because GCC never told them about it. Some developers have started integrating `clang` testing into their workflows to help catch these, such as the netdev folks, who I regularly see tell contributors their patch has errors with `clang`, but it is a slow process; until then, we'll be fixing them ourselves to keep the builds green.

There are a few different commits that I am particularly proud of due to how they tested me and forced me to dig deeper for an answer:

- `a8a4f0467d70 ("drm/i915: Fix CFI violations in gt_sysfs")`: This one was particularly wild becuase it seemed so simple at first. I booted up my test Intel desktop, saw the failure, debugged for a bit, wrote the patch, and was so confident it was right that I considered not even testing the patch before sending it. I learned very quickly that is never a good idea since my patch was broken in its current state. As I read the code and started to fully understand what was going on, I started to worry I was not going to be able to fix it but at the end of the day, I am surrounded by people who know more than I do that I can ask for help and I have started to write development journals so that I am sure I understand the problem before starting a solution. This one was fun to have people review and test because it was so large but I am thankful for all the review that I did get, since it helped warp the patch into something that is worthwhile.

- `52c9f93a9c48 ("arm64: Do not include __READ_ONCE() block in assembly files")`: This one was fun to debug because I got to the point of `git diff`ing preprocessed assembly files :) Debugging header include issues is probably one of my least favorite things but this was high priority because the commit that introduced this breakage was a part of the Spectre-BHB fixes, which were going to be rapidly backported to the stable trees and potentially break downstream consumers of Clang's LTO, like Android. I felt really good once I had gotten the commit message written because it felt like it was going to make a real, tangible impact somewhere.

- `e9c281928c24 ("kbuild: Make $(LLVM) more flexible")`: While I cannot claim credit for the implementation of this commit, I am proud of this commit because it is addressing a core Linux kernel developer concern that was brought up to us, which allows them to continue to test with LLVM. It is very important to listen to developers and maintainers when they complain about our project because they are the ones we have to get buy in from. Our project could collapse if maintainers decide they do not want to put up with the idiosyncracies of building the kernel with LLVM or even worse, refuse to let the kernel be built with LLVM, [like `ld.gold`](https://git.kernel.org/linus/75959d44f9dc8e44410667009724e4e238515502). That is an obvious extreme that is unlikely at this point, especially when core maintainers have [openly accepted our approach to maintaining LLVM support](https://lore.kernel.org/87v8nbiaz7.ffs@tglx/), but it is important to remember that since the kernel and LLVM are always evolving, we have to be present and responsive at all times.

It is important to keep in mind that sending patches is only part of the development process. The others are reporting problems and testing and reviewing solutions to those problems. The kernel keeps track of these through particular tags, which I have summarized below (I did not do much filtering so there will likely be duplicates across the lists).

<details>
<summary><code>Reported-by</code></summary>
<p><code><a href="https://git.kernel.org/linus/980411a4d1bb925d28cd9e8d8301dc982ece788d">980411a4d1bb</a> ("powerpc/code-patching: Fix oops with DEBUG_VM enabled")<br/>
<a href="https://git.kernel.org/linus/0ba09b1733878afe838fe35c310715fda3d46428">0ba09b173387</a> ("Revert "mm: align larger anonymous mappings on THP boundaries"")<br/>
<a href="https://git.kernel.org/linus/d03407183d97554dfffea70f385b5bdd520f846c">d03407183d97</a> ("wifi: ath10k: fix QCOM_SMEM dependency")<br/>
<a href="https://git.kernel.org/linus/beb3d47d1d3d7185bb401af628ad32ee204a9526">beb3d47d1d3d</a> ("bpf: Fix a BTF_ID_LIST bug with CONFIG_DEBUG_INFO_BTF not set")<br/>
<a href="https://git.kernel.org/linus/32d495b0c3305546f4773b9aafcd4e90188ddb9e">32d495b0c330</a> ("Revert "arm64/mm: Drop redundant BUG_ON(!pgtable_alloc)"")<br/>
<a href="https://git.kernel.org/linus/e287bd005ad9d85dd6271dd795d3ecfb6bca46ad">e287bd005ad9</a> ("KVM: SVM: restore host save area from assembly")<br/>
<a href="https://git.kernel.org/linus/80ddf5ce1c9291cb175d52ed1227134ad48c47ee">80ddf5ce1c92</a> ("s390: always build relocatable kernel")<br/>
<a href="https://git.kernel.org/linus/3220022038b9a3845eea762af85f1c5694b9f861">3220022038b9</a> ("ARM: 9256/1: NWFPE: avoid compiler-generated __aeabi_uldivmod")<br/>
<a href="https://git.kernel.org/linus/03e9491fff252a7435e109333ec51ca2d619b759">03e9491fff25</a> ("pinctrl: qcom: lpass-lpi: Add missed bitfield.h")<br/>
<a href="https://git.kernel.org/linus/f8fbf0dc702bf15b8b0ea1731a353bdb7faee8fd">f8fbf0dc702b</a> ("ASoC: SOF: fix compilation issue with readb/writeb helpers")<br/>
<a href="https://git.kernel.org/linus/fb3041d61f6867158088c627c2790f94e208d1ea">fb3041d61f68</a> ("kbuild: fix SIGPIPE error message for AR=gcc-ar and AR=llvm-ar")<br/>
<a href="https://git.kernel.org/linus/0e5b9f25b27a7a92880f88f5dba3edf726ec5f61">0e5b9f25b27a</a> ("overflow: disable failing tests for older clang versions")<br/>
<a href="https://git.kernel.org/linus/fb2d14add4f813c73bd9d28b750315ccb3f5f0ea">fb2d14add4f8</a> ("Drivers: hv: vmbus: Split memcpy of flex-array")<br/>
<a href="https://git.kernel.org/linus/37dcc673d065d9823576cd9f2484a72531e1cba6">37dcc673d065</a> ("frontswap: don't call ->init if no ops are registered")<br/>
<a href="https://git.kernel.org/linus/0072dc1b53c39fb7c4cfc5c9e5d5a30622198613">0072dc1b53c3</a> ("arm64: avoid BUILD_BUG_ON() in alternative-macros")<br/>
<a href="https://git.kernel.org/linus/06c1c49d0cd1d6cec5b78963109ba728e49e0063">06c1c49d0cd1</a> ("fortify: Adjust KUnit test for modular build")<br/>
<a href="https://git.kernel.org/linus/a66de5283e16602b74658289360505ceeb308c90">a66de5283e16</a> ("powerpc/pseries: Fix plpks crash on non-pseries")<br/>
<a href="https://git.kernel.org/linus/7245fc5bb7a966852d5bd7779d1f5855530b461a">7245fc5bb7a9</a> ("powerpc/math-emu: Remove -w build flag and fix warnings")<br/>
<a href="https://git.kernel.org/linus/ea522b806162ca947427f8305310d3c8a3d42d7a">ea522b806162</a> ("platform/x86/amd/pmf: Fix clang unused variable warning")<br/>
<a href="https://git.kernel.org/linus/b7bf23c0865faac61564425ddc96a4a79ebf19b0">b7bf23c0865f</a> ("ASoC: SOF: ipc3-topology: Fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/b5276c924497705ca927ad85a763c37f2de98349">b5276c924497</a> ("drivers: lkdtm: fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/b4909252da9be56fe1e0a23c2c1908c5630525fa">b4909252da9b</a> ("drivers: lkdtm: fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/b321823a03dc81a5b83b063e6e1904d612b53266">b321823a03dc</a> ("io_uring: fix io_poll_remove_all clang warnings")<br/>
<a href="https://git.kernel.org/linus/f066b8f7d961b0f3ec08678a27f2f1a2f0148380">f066b8f7d961</a> ("drivers: iommu: fix clang -wformat warning")<br/>
<a href="https://git.kernel.org/linus/1e744351bcb9c4cee81300de5a6097100d835386">1e744351bcb9</a> ("ASoC: Intel: avs: Use lookup table to create modules")<br/>
<a href="https://git.kernel.org/linus/9950f11211331180269867aef848c7cf56861742">9950f1121133</a> ("can: pch_can: pch_can_error(): initialize errc before using it")<br/>
<a href="https://git.kernel.org/linus/bdd0d7e290e0e4c8f7545fff89770abbd22bd51a">bdd0d7e290e0</a> ("drm/amd/display: fix non-x86/PPC64 compilation")<br/>
<a href="https://git.kernel.org/linus/2fd26970cf66bd52dc42843c46968040caa8c9a1">2fd26970cf66</a> ("Revert "kernfs: Change kernfs_notify_list to llist."")<br/>
<a href="https://git.kernel.org/linus/c0c87382c1a6985cd12a49a62a893361e5fd1b8f">c0c87382c1a6</a> ("drm/amdgpu/display: fix build when CONFIG_DEBUG_FS is not set")<br/>
<a href="https://git.kernel.org/linus/90f4b5499cdd94be3c1e856375ecd7d5f9c4cecc">90f4b5499cdd</a> ("rtw88: 8821c: fix access const table of channel parameters")<br/>
<a href="https://git.kernel.org/linus/9be4cbd09da820a20d400670a45fc1571f6a13b8">9be4cbd09da8</a> ("driver core: Set default deferred_probe_timeout back to 0.")<br/>
<a href="https://git.kernel.org/linus/5ee76c256e928455212ab759c51d198fedbe7523">5ee76c256e92</a> ("driver core: Fix wait_for_device_probe() & deferred_probe_timeout interaction")<br/>
<a href="https://git.kernel.org/linus/e15db62bc5648ab459a570862f654e787c498faf">e15db62bc564</a> ("swiotlb: fix setting ->force_bounce")<br/>
<a href="https://git.kernel.org/linus/ead165fa1042247b033afad7be4be9b815d04ade">ead165fa1042</a> ("objtool: Fix symbol creation")<br/>
<a href="https://git.kernel.org/linus/5eefe17c7ae41bac4d2d281669e8357a10f4d5a4">5eefe17c7ae4</a> ("libbpf: Clean up ringbuf size adjustment implementation")<br/>
<a href="https://git.kernel.org/linus/9d79799193b728b62c9899d931b5009da1f89b67">9d79799193b7</a> ("fbcon: Fix delayed takeover locking")<br/>
<a href="https://git.kernel.org/linus/60210a3d86dc57ce4a76a366e7841dda746a33f7">60210a3d86dc</a> ("riscv module: remove (NOLOAD)")<br/>
<a href="https://git.kernel.org/linus/faf79934e65aff90284725518a5ec3c2241c65ae">faf79934e65a</a> ("s390/alternatives: avoid using jgnop mnemonic")<br/>
<a href="https://git.kernel.org/linus/b847bd64ea9f484510e27065cb2bccc58d9b829b">b847bd64ea9f</a> ("MIPS: Only use current_stack_pointer on GCC")<br/>
<a href="https://git.kernel.org/linus/d55957fb299b74829c438f77fe29896e3aed39fc">d55957fb299b</a> ("drm/amdkfd: bail out early if no get_atc_vmid_pasid_mapping_info")<br/>
<a href="https://git.kernel.org/linus/4013e26670c590944abdab56c4fa797527b74325">4013e26670c5</a> ("arm64: module: remove (NOLOAD) from linker script")<br/>
<a href="https://git.kernel.org/linus/5790597d7113faabb1714d3d1efa268e36eb4811">5790597d7113</a> ("spi: Fix warning for Clang build and simplify code")<br/>
<a href="https://git.kernel.org/linus/6bf625a4140f24b490766043b307f8252519578b">6bf625a4140f</a> ("Drivers: hv: vmbus: Rework use of DMA_BIT_MASK(64)")<br/>
<a href="https://git.kernel.org/linus/b7892f7d5cb2b8187c603dd8ea3a7c44059ccfc2">b7892f7d5cb2</a> ("tools: Ignore errors from `which' when searching a GCC toolchain")<br/>
<a href="https://git.kernel.org/linus/25d2e41cf59bd6ccd23adc2965a157053bc3ed5c">25d2e41cf59b</a> ("pinctrl: thunderbay: rework loops looking for groups names")<br/>
<a href="https://git.kernel.org/linus/2056e2989bf47ad7274ecc5e9dda2add53c112f9">2056e2989bf4</a> ("x86/sgx: Fix NULL pointer dereference on non-SGX systems")<br/>
<a href="https://git.kernel.org/linus/5d9224fb076e9a2023e0b06d6a164d644612c0c0">5d9224fb076e</a> ("scsi: hisi_sas: Remove unused variable and check in hisi_sas_send_ata_reset_each_phy()")<br/>
</code></p>
</details>

<details>
<summary><code>Reviewed-by</code></summary>
<p><code><a href="https://git.kernel.org/linus/6a7ee50f8f56dc181e1150cc101896053b02d220">6a7ee50f8f56</a> ("ARM: disallow pre-ARMv5 builds with ld.lld")<br/>
<a href="https://git.kernel.org/linus/87d599fc3955e59b1ed30f350321a4be5353f945">87d599fc3955</a> ("kbuild: ensure Make >= 3.82 is used")<br/>
<a href="https://git.kernel.org/linus/fccb3d3eda8d19b893e1fd18e8c70b78784b2a72">fccb3d3eda8d</a> ("kbuild: add test-{ge,gt,le,lt} macros")<br/>
<a href="https://git.kernel.org/linus/80b6093b55e31c2c40ff082fb32523d4e852954f">80b6093b55e3</a> ("kbuild: add -Wundef to KBUILD_CPPFLAGS for W=1 builds")<br/>
<a href="https://git.kernel.org/linus/efa80b028c7a9c74fd875517aa0fc9fd8d610ed0">efa80b028c7a</a> ("kbuild: move -Werror from KBUILD_CFLAGS to KBUILD_CPPFLAGS")<br/>
<a href="https://git.kernel.org/linus/9f8fe647797a4bc049bc7cceaf3a63584678ba04">9f8fe647797a</a> ("Makefile.debug: support for -gz=zstd")<br/>
<a href="https://git.kernel.org/linus/23df39fc6a36183af5e6e4f47523f1ad2cdc1d30">23df39fc6a36</a> ("locking: Fix qspinlock/x86 inline asm error")<br/>
<a href="https://git.kernel.org/linus/612d80784fdc0c2e2ee2e2d901a55ef2f72ebf4b">612d80784fdc</a> ("MIPS: fix duplicate definitions for exported symbols")<br/>
<a href="https://git.kernel.org/linus/f39556bc2530c83a22bc11b73c7a46df9a340685">f39556bc2530</a> ("compiler-gcc: document minimum version for `__no_sanitize_coverage__`")<br/>
<a href="https://git.kernel.org/linus/689540cbda7f69594ae5e13fef4c8239519d8b66">689540cbda7f</a> ("compiler-gcc: remove attribute support check for `__no_sanitize_undefined__`")<br/>
<a href="https://git.kernel.org/linus/095ac0763ac507dd4e1a71ad9784f49f51498483">095ac0763ac5</a> ("compiler-gcc: remove attribute support check for `__no_sanitize_thread__`")<br/>
<a href="https://git.kernel.org/linus/ae37a9a2c2d0960d643d782b426ea1aa9c05727a">ae37a9a2c2d0</a> ("compiler-gcc: remove attribute support check for `__no_sanitize_address__`")<br/>
<a href="https://git.kernel.org/linus/6e2be1f2ebcea42ed6044432f72f32434e60b34d">6e2be1f2ebce</a> ("compiler-gcc: be consistent with underscores use for `no_sanitize`")<br/>
<a href="https://git.kernel.org/linus/1d2e9b67b001f8c0d10138797b9df4779609c03c">1d2e9b67b001</a> ("ARM: 9265/1: pass -march= only to compiler")<br/>
<a href="https://git.kernel.org/linus/26b12e084bce78f339bf9069ff25e0128313d9bc">26b12e084bce</a> ("ARM: 9264/1: only use -mtp=cp15 for the compiler")<br/>
<a href="https://git.kernel.org/linus/5aa4860eb50f3700e3175759f608bcfb68d0d6ae">5aa4860eb50f</a> ("ARM: 9262/1: remove lazy evaluation in Makefile")<br/>
<a href="https://git.kernel.org/linus/e1789d7c752ed001cf1a4bbbd624f70a7dd3c6db">e1789d7c752e</a> ("kbuild: upgrade the orphan section warning to an error if CONFIG_WERROR is set")<br/>
<a href="https://git.kernel.org/linus/fc007fb815ab5395c3962c09b79a1630b0fbed9c">fc007fb815ab</a> ("drm/imx: imx-tve: Fix return type of imx_tve_connector_mode_valid")<br/>
<a href="https://git.kernel.org/linus/aae538cd03bc8fc35979653d9180922d146da0ca">aae538cd03bc</a> ("riscv: fix detection of toolchain Zihintpause support")<br/>
<a href="https://git.kernel.org/linus/b8c86872d1dc171d8f1c137917d6913cae2fa4f2">b8c86872d1dc</a> ("riscv: fix detection of toolchain Zicbom support")<br/>
<a href="https://git.kernel.org/linus/80e5acb6dd72b25a6e6527443b9e9c1c3a7bcef6">80e5acb6dd72</a> ("wifi: rtl8xxxu: Fix reads of uninitialized variables hw_ctrl_s1, sw_ctrl_s1")<br/>
<a href="https://git.kernel.org/linus/8e5bad7dccec2014f24497b57d8a8ee0b752c290">8e5bad7dccec</a> ("sched: Introduce struct balance_callback to avoid CFI mismatches")<br/>
<a href="https://git.kernel.org/linus/c67a85bee78db74c6889a5ca645c3763ad23d863">c67a85bee78d</a> ("kbuild: add -fno-discard-value-names to cmd_cc_ll_c")<br/>
<a href="https://git.kernel.org/linus/bb1435f3f575b5213eaf27434efa3971f51c01de">bb1435f3f575</a> ("Kconfig.debug: add toolchain checks for DEBUG_INFO_DWARF_TOOLCHAIN_DEFAULT")<br/>
<a href="https://git.kernel.org/linus/4f001a21080ff2e2f0e1c3692f5e119aedbb3bc1">4f001a21080f</a> ("Kconfig.debug: simplify the dependency of DEBUG_INFO_DWARF4/5")<br/>
<a href="https://git.kernel.org/linus/450a580fc4b5e7f7fb8d9b1a0208bf0d1efc53a8">450a580fc4b5</a> ("net: lan966x: Fix return type of lan966x_port_xmit")<br/>
<a href="https://git.kernel.org/linus/0f1fe9d6168306cd003d26dfdbb3bf3e79643e78">0f1fe9d61683</a> ("Revert "kbuild: Check if linker supports the -X option"")<br/>
<a href="https://git.kernel.org/linus/0b33a33bd15d5bab73b87152b220a8d0153a4587">0b33a33bd15d</a> ("drm/msm: Fix return type of mdp4_lvds_connector_mode_valid")<br/>
<a href="https://git.kernel.org/linus/88b61e3bff93f99712718db785b4aa0c1165f35c">88b61e3bff93</a> ("Makefile.compiler: replace cc-ifversion with compiler-specific macros")<br/>
<a href="https://git.kernel.org/linus/9512d5f8e34fb7c92be6179dc48ba5f0b9b922ac">9512d5f8e34f</a> ("staging: r8188eu: Fix return type of rtw_xmit_entry")<br/>
<a href="https://git.kernel.org/linus/b77599043f00fce9253d0f22522c5d5b521555ce">b77599043f00</a> ("staging: octeon: Fix return type of cvm_oct_xmit and cvm_oct_xmit_pow")<br/>
<a href="https://git.kernel.org/linus/2851349ac351010a2649e0ff86a1e3d68fe5d683">2851349ac351</a> ("staging: rtl8192u: Fix return type of ieee80211_xmit")<br/>
<a href="https://git.kernel.org/linus/32ef9e5054ec0321b9336058c58ec749e9c6b0fe">32ef9e5054ec</a> ("Makefile.debug: re-enable debug info for .S files")<br/>
<a href="https://git.kernel.org/linus/61f2b7c7497ba96cdde5bbaeb9e07f4c48f41f97">61f2b7c7497b</a> ("Makefile.debug: set -g unconditional on CONFIG_DEBUG_INFO_SPLIT")<br/>
<a href="https://git.kernel.org/linus/237fe72749425f2cd3132bf54fa6b98807c27938">237fe7274942</a> ("scripts/clang-tools: remove unused module")<br/>
<a href="https://git.kernel.org/linus/8bb7c4f8c9276eb61f7f39cc107f67a0e81de610">8bb7c4f8c927</a> ("openvswitch: Change the return type for vport_ops.send function hook to int")<br/>
<a href="https://git.kernel.org/linus/106c67ce46f3c82dd276e983668a91d6ed631173">106c67ce46f3</a> ("net: korina: Fix return type of korina_send_packet")<br/>
<a href="https://git.kernel.org/linus/40662333dd7c64664247a6138bc33f3974e3a331">40662333dd7c</a> ("net: ethernet: litex: Fix return type of liteeth_start_xmit")<br/>
<a href="https://git.kernel.org/linus/5972ca946098487c5155fe13654743f9010f5ed5">5972ca946098</a> ("net: ethernet: ti: davinci_emac: Fix return type of emac_dev_xmit")<br/>
<a href="https://git.kernel.org/linus/0191580b000d50089a0b351f7cdbec4866e3d0d2">0191580b000d</a> ("net: davicom: Fix return type of dm9000_start_xmit")<br/>
<a href="https://git.kernel.org/linus/21f0b7dabf9c358e75a539b5554c0375bf1abe0a">21f0b7dabf9c</a> ("drm/i915: Fix return type of mode_valid function hook")<br/>
<a href="https://git.kernel.org/linus/b0b9408f132623dc88e78adb5282f74e4b64bb57">b0b9408f1326</a> ("drm/rockchip: Fix return type of cdn_dp_connector_mode_valid")<br/>
<a href="https://git.kernel.org/linus/7245fc5bb7a966852d5bd7779d1f5855530b461a">7245fc5bb7a9</a> ("powerpc/math-emu: Remove -w build flag and fix warnings")<br/>
<a href="https://git.kernel.org/linus/b0839b281c427e844143dba3893e25c83cdd6c17">b0839b281c42</a> ("Makefile.extrawarn: re-enable -Wformat for clang; take 2")<br/>
<a href="https://git.kernel.org/linus/4ffb4d25ef1251d57881da183d6bec7f2dfe1e32">4ffb4d25ef12</a> ("wifi: rtw88: fix uninitialized use of primary channel index")<br/>
<a href="https://git.kernel.org/linus/ea522b806162ca947427f8305310d3c8a3d42d7a">ea522b806162</a> ("platform/x86/amd/pmf: Fix clang unused variable warning")<br/>
<a href="https://git.kernel.org/linus/a0a12c3ed057af57552bf6c0aeaca6835693df04">a0a12c3ed057</a> ("asm goto: eradicate CC_HAS_ASM_GOTO")<br/>
<a href="https://git.kernel.org/linus/c9a1dd673f28da9624776e75b78ae04125544852">c9a1dd673f28</a> ("rtc: zynqmp: initialize fract_tick")<br/>
<a href="https://git.kernel.org/linus/b7bf23c0865faac61564425ddc96a4a79ebf19b0">b7bf23c0865f</a> ("ASoC: SOF: ipc3-topology: Fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/b5276c924497705ca927ad85a763c37f2de98349">b5276c924497</a> ("drivers: lkdtm: fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/b4909252da9be56fe1e0a23c2c1908c5630525fa">b4909252da9b</a> ("drivers: lkdtm: fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/f066b8f7d961b0f3ec08678a27f2f1a2f0148380">f066b8f7d961</a> ("drivers: iommu: fix clang -wformat warning")<br/>
<a href="https://git.kernel.org/linus/9950f11211331180269867aef848c7cf56861742">9950f1121133</a> ("can: pch_can: pch_can_error(): initialize errc before using it")<br/>
<a href="https://git.kernel.org/linus/aa8c7cdbae58b695ed79a0129b6b8c887b25969f">aa8c7cdbae58</a> ("netfilter: xt_TPROXY: remove pr_debug invocations")<br/>
<a href="https://git.kernel.org/linus/5b47d2364652979dc43df14f7af19346a001b770">5b47d2364652</a> ("net: rxrpc: fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/ffff4913c7e22cc2bd5570df73e0e154bf111215">ffff4913c7e2</a> ("eeprom: idt_89hpesx: fix clang -Wformat warnings")<br/>
<a href="https://git.kernel.org/linus/291810be4227564403807e663f3ec8d3b3d6ba34">291810be4227</a> ("Documentation/llvm: Update Supported Arch table")<br/>
<a href="https://git.kernel.org/linus/d30dfd490f7dc4cb6a7c11a647bd1ff7a22139e7">d30dfd490f7d</a> ("include/uapi/linux/swab.h: move explicit cast outside ternary")<br/>
<a href="https://git.kernel.org/linus/01ece65132e2980ece4eca91105dfc9eed504881">01ece65132e2</a> ("drm/ssd130x: Only define a SPI device ID table when built as a module")<br/>
<a href="https://git.kernel.org/linus/0e900029655327bb5326ced02eff97667a079039">0e9000296553</a> ("ipc/sem: remove redundant assignments")<br/>
<a href="https://git.kernel.org/linus/7374fa33dc2dd76b71999f8fd236e73b21161030">7374fa33dc2d</a> ("init/Kconfig: remove USELIB syscall by default")<br/>
<a href="https://git.kernel.org/linus/e6f3b3c9c109ed57230996cf4a4c1b8ae7e36a81">e6f3b3c9c109</a> ("cfi: Use __builtin_function_start")<br/>
<a href="https://git.kernel.org/linus/1754cea1763e2bdc6a2153220440fe9aa9e0f2c9">1754cea1763e</a> ("drm/amd/display: fix 64 bit divide in freesync code")<br/>
<a href="https://git.kernel.org/linus/334865b2915c33080624e0d06f1c3e917036472c">334865b2915c</a> ("x86/extable: Prefer local labels in .set directives")<br/>
<a href="https://git.kernel.org/linus/22ef7ee3eeb2a41e07f611754ab9a2663232fedf">22ef7ee3eeb2</a> ("PCI: hv: Remove unused hv_set_msi_entry_from_desc()")<br/>
<a href="https://git.kernel.org/linus/9fbed27a7a1101c926718dfa9b49aff1d04477b5">9fbed27a7a11</a> ("kbuild: add --target to correctly cross-compile UAPI headers with Clang")<br/>
<a href="https://git.kernel.org/linus/b027471adaf955efde6153d67f391fe1604b7292">b027471adaf9</a> ("Revert "ubsan, kcsan: Don't combine sanitizer with kcov on clang"")<br/>
<a href="https://git.kernel.org/linus/14e83077d55ff4b88fe39f5e98fb8230c2ccb4fb">14e83077d55f</a> ("include: drop pointless __compiler_offsetof indirection")<br/>
<a href="https://git.kernel.org/linus/f9b3cd24578401e7a392974b3353277286e49cee">f9b3cd245784</a> ("Kconfig.debug: make DEBUG_INFO selectable from a choice")<br/>
<a href="https://git.kernel.org/linus/ffea9fb319360b9ead8befac6bb2db2b54fd53e6">ffea9fb31936</a> ("sched/headers: ARM needs asm/paravirt_api_clock.h too")<br/>
<a href="https://git.kernel.org/linus/c7500c1b53bfc083e8968cdce13a5a9d1ca9bf83">c7500c1b53bf</a> ("um: Allow builds with Clang")<br/>
<a href="https://git.kernel.org/linus/a8ae15ead9c9d10671c3f76cb0749dec6e571ce7">a8ae15ead9c9</a> ("ASoC: atmel: mchp-pdmc: Fix `-Wpointer-bool-conversion` warning")<br/>
<a href="https://git.kernel.org/linus/b847bd64ea9f484510e27065cb2bccc58d9b829b">b847bd64ea9f</a> ("MIPS: Only use current_stack_pointer on GCC")<br/>
<a href="https://git.kernel.org/linus/1e24078113ae69c741cb1b03375a9f1490db7308">1e24078113ae</a> ("Kbuild: use -std=gnu11 for KBUILD_USERCFLAGS")<br/>
<a href="https://git.kernel.org/linus/e8c07082a810fbb9db303a2b66b66b8d7e588b53">e8c07082a810</a> ("Kbuild: move to -std=gnu11")<br/>
<a href="https://git.kernel.org/linus/4d94f910e79a349b00a4f8aab6f3ae87129d8c5a">4d94f910e79a</a> ("Kbuild: use -Wdeclaration-after-statement")<br/>
<a href="https://git.kernel.org/linus/1344794a59db2bd44b4919d2d75300fd3b1c2cd7">1344794a59db</a> ("Kbuild: add -Wno-shift-negative-value where -Wextra is used")<br/>
<a href="https://git.kernel.org/linus/6580c5c18fb3df2b11c5e0452372f815deeff895">6580c5c18fb3</a> ("um: clang: Strip out -mno-global-merge from USER_CFLAGS")<br/>
<a href="https://git.kernel.org/linus/afcf5441b9ff22ac57244cd45ff102ebc2e32d1a">afcf5441b9ff</a> ("arm64: Add gcc Shadow Call Stack support")<br/>
<a href="https://git.kernel.org/linus/330f4c53d3c2d8b11d86ec03a964b86dc81452f5">330f4c53d3c2</a> ("ARM: fix build error when BPF_SYSCALL is disabled")<br/>
<a href="https://git.kernel.org/linus/925088781eede43cf6616a1197c31dee451b7948">925088781eed</a> ("KVM: x86: Fix pointer mistmatch warning when patching RET0 static calls")<br/>
<a href="https://git.kernel.org/linus/d4c858643263cfde13f7d937eaff95c2ed87cdf1">d4c858643263</a> ("kallsyms: ignore all local labels prefixed by '.L'")<br/>
<a href="https://git.kernel.org/linus/baf682144ecacae4b98597daa636ce7b2b3143f6">baf682144eca</a> ("drm/i915: fix build issue when using clang")<br/>
<a href="https://git.kernel.org/linus/efa90c11f62e6b7252fb75efe2787056872a627c">efa90c11f62e</a> ("stack: Constrain and fix stack offset randomization with Clang builds")<br/>
<a href="https://git.kernel.org/linus/8cb37a5974a48569aab8a1736d21399fddbdbdb2">8cb37a5974a4</a> ("stack: Introduce CONFIG_RANDOMIZE_KSTACK_OFFSET")<br/>
<a href="https://git.kernel.org/linus/818ab43fc56ad978cbb7c0ffdc9a332fd2f23a23">818ab43fc56a</a> ("fortify: Update compile-time tests for Clang 14")<br/>
<a href="https://git.kernel.org/linus/6bf625a4140f24b490766043b307f8252519578b">6bf625a4140f</a> ("Drivers: hv: vmbus: Rework use of DMA_BIT_MASK(64)")<br/>
<a href="https://git.kernel.org/linus/b7892f7d5cb2b8187c603dd8ea3a7c44059ccfc2">b7892f7d5cb2</a> ("tools: Ignore errors from `which' when searching a GCC toolchain")<br/>
<a href="https://git.kernel.org/linus/b470947c3672f7eb7c4c271d510383d896831cc2">b470947c3672</a> ("usb: dwc3: xilinx: fix uninitialized return value")<br/>
<a href="https://git.kernel.org/linus/bece04b5b41dd7730dd06aec0d6b15c53d1fbb5a">bece04b5b41d</a> ("kcov: fix generic Kconfig dependencies if ARCH_WANTS_NO_INSTR")<br/>
<a href="https://git.kernel.org/linus/35140d399db2b67153fc53b51a97ddb8ba3b5956">35140d399db2</a> ("script/sorttable: Fix some initialization problems")<br/>
</code></p>
</details>

<details>
<summary><code>Tested-by</code></summary>
<p><code><a href="https://git.kernel.org/linus/e287bd005ad9d85dd6271dd795d3ecfb6bca46ad">e287bd005ad9</a> ("KVM: SVM: restore host save area from assembly")<br/>
<a href="https://git.kernel.org/linus/defbab270d45e32b068e7e73c3567232d745c60f">defbab270d45</a> ("include/uapi/linux/swab: Fix potentially missing __always_inline")<br/>
<a href="https://git.kernel.org/linus/1d2e9b67b001f8c0d10138797b9df4779609c03c">1d2e9b67b001</a> ("ARM: 9265/1: pass -march= only to compiler")<br/>
<a href="https://git.kernel.org/linus/a2faac39866d0313f3ca59c36a9f4e077faf4f53">a2faac39866d</a> ("ARM: 9263/1: use .arch directives instead of assembler command line flags")<br/>
<a href="https://git.kernel.org/linus/80ddf5ce1c9291cb175d52ed1227134ad48c47ee">80ddf5ce1c92</a> ("s390: always build relocatable kernel")<br/>
<a href="https://git.kernel.org/linus/3220022038b9a3845eea762af85f1c5694b9f861">3220022038b9</a> ("ARM: 9256/1: NWFPE: avoid compiler-generated __aeabi_uldivmod")<br/>
<a href="https://git.kernel.org/linus/bce5a1e8a34006a5e80213ede5e5c465d53f1dce">bce5a1e8a340</a> ("x86/mem: Move memmove to out of line assembler")<br/>
<a href="https://git.kernel.org/linus/e1789d7c752ed001cf1a4bbbd624f70a7dd3c6db">e1789d7c752e</a> ("kbuild: upgrade the orphan section warning to an error if CONFIG_WERROR is set")<br/>
<a href="https://git.kernel.org/linus/62e1cbfc5d795381a0f237ae7ee229a92d51cf9e">62e1cbfc5d79</a> ("fortify: Short-circuit known-safe calls to strscpy()")<br/>
<a href="https://git.kernel.org/linus/41eefc46a3a4682976afb5f8c4b9734ed6bfd406">41eefc46a3a4</a> ("string: Convert strscpy() self-test to KUnit")<br/>
<a href="https://git.kernel.org/linus/fb3041d61f6867158088c627c2790f94e208d1ea">fb3041d61f68</a> ("kbuild: fix SIGPIPE error message for AR=gcc-ar and AR=llvm-ar")<br/>
<a href="https://git.kernel.org/linus/3cebf80e9a0d3adcb174053be32c88a640b3344b">3cebf80e9a0d</a> ("riscv: Pass -mno-relax only on lld < 15.0.0")<br/>
<a href="https://git.kernel.org/linus/820dc0523e05c12810bb6bf4e56ce26e4c1948a2">820dc0523e05</a> ("net: netfilter: move bpf_ct_set_nat_info kfunc in nf_nat_bpf.c")<br/>
<a href="https://git.kernel.org/linus/fb2d14add4f813c73bd9d28b750315ccb3f5f0ea">fb2d14add4f8</a> ("Drivers: hv: vmbus: Split memcpy of flex-array")<br/>
<a href="https://git.kernel.org/linus/3c516f89e17e56b4738f05588e51267e295b5e63">3c516f89e17e</a> ("x86: Add support for CONFIG_CFI_CLANG")<br/>
<a href="https://git.kernel.org/linus/a4b7a12c5594fe5e6ab2a5aa514a9ae3c0b85573">a4b7a12c5594</a> ("x86/purgatory: Disable CFI")<br/>
<a href="https://git.kernel.org/linus/ccace936eec7b805e1ab9268a6d163a00047b3a9">ccace936eec7</a> ("x86: Add types to indirectly called assembly functions")<br/>
<a href="https://git.kernel.org/linus/ca7e10bff196f69a450b9072a7b757713d3bb2dd">ca7e10bff196</a> ("x86/tools/relocs: Ignore __kcfi_typeid_ relocations")<br/>
<a href="https://git.kernel.org/linus/dfb352ab1162f73b8c6dc98150fa32cf5aa2f623">dfb352ab1162</a> ("kallsyms: Drop CONFIG_CFI_CLANG workarounds")<br/>
<a href="https://git.kernel.org/linus/3c68a92d17add767109441f4040391b9e8a14a98">3c68a92d17ad</a> ("objtool: Disable CFI warnings")<br/>
<a href="https://git.kernel.org/linus/5659b598b4dcb352b1a567c55fc5a658bc80076c">5659b598b4dc</a> ("treewide: Drop __cficanonical")<br/>
<a href="https://git.kernel.org/linus/4b24356312fbe1bace72f9905d529b14fc34c1c3">4b24356312fb</a> ("treewide: Drop WARN_ON_FUNCTION_MISMATCH")<br/>
<a href="https://git.kernel.org/linus/607289a7cd7a3ca42b8a6877fcb6072e6eb20c34">607289a7cd7a</a> ("treewide: Drop function_nocfi")<br/>
<a href="https://git.kernel.org/linus/5dbbb3eaa2a784342f3206b77381b516181d089c">5dbbb3eaa2a7</a> ("init: Drop __nocfi from __init")<br/>
<a href="https://git.kernel.org/linus/5f20997c194e8b74254cbdb113b2b09bc1c0c734">5f20997c194e</a> ("arm64: Drop unneeded __nocfi attributes")<br/>
<a href="https://git.kernel.org/linus/b26e484b8bb3a992ef30e851d771973a3dd2336b">b26e484b8bb3</a> ("arm64: Add CFI error handling")<br/>
<a href="https://git.kernel.org/linus/c50d32859e70f6dbccb7d151408eb10afbbb7965">c50d32859e70</a> ("arm64: Add types to indirect called assembly functions")<br/>
<a href="https://git.kernel.org/linus/44f665b69c67f0a17a0c8748030ed30205532149">44f665b69c67</a> ("psci: Fix the function type for psci_initcall_t")<br/>
<a href="https://git.kernel.org/linus/cf90d0383560de12330de8cf3f831b14cdd45914">cf90d0383560</a> ("lkdtm: Emit an indirect call for CFI tests")<br/>
<a href="https://git.kernel.org/linus/e84e008e7b02c015047e76261726da1550130a59">e84e008e7b02</a> ("cfi: Add type helper macros")<br/>
<a href="https://git.kernel.org/linus/89245600941e4e0f87d77f60ee269b5e61ef4e49">89245600941e</a> ("cfi: Switch to -fsanitize=kcfi")<br/>
<a href="https://git.kernel.org/linus/92efda8eb15295a07f450828b2db14485bfc09c2">92efda8eb152</a> ("cfi: Drop __CFI_ADDRESSABLE")<br/>
<a href="https://git.kernel.org/linus/9fca7115827b2e5f48d84e50bceb4edfd4cb6375">9fca7115827b</a> ("cfi: Remove CONFIG_CFI_CLANG_SHADOW")<br/>
<a href="https://git.kernel.org/linus/d0f9562ee43a135b941715d9e5e607de88898aca">d0f9562ee43a</a> ("scripts/kallsyms: Ignore __kcfi_typeid_")<br/>
<a href="https://git.kernel.org/linus/f143ff397a3f991e8b48542f77aad900845f436e">f143ff397a3f</a> ("treewide: Filter out CC_FLAGS_CFI")<br/>
<a href="https://git.kernel.org/linus/0072dc1b53c39fb7c4cfc5c9e5d5a30622198613">0072dc1b53c3</a> ("arm64: avoid BUILD_BUG_ON() in alternative-macros")<br/>
<a href="https://git.kernel.org/linus/a66de5283e16602b74658289360505ceeb308c90">a66de5283e16</a> ("powerpc/pseries: Fix plpks crash on non-pseries")<br/>
<a href="https://git.kernel.org/linus/b5276c924497705ca927ad85a763c37f2de98349">b5276c924497</a> ("drivers: lkdtm: fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/b4909252da9be56fe1e0a23c2c1908c5630525fa">b4909252da9b</a> ("drivers: lkdtm: fix clang -Wformat warning")<br/>
<a href="https://git.kernel.org/linus/29589ca09a74cfc0c50ad002e298bf4b8e69e0bd">29589ca09a74</a> ("ARM: 9208/1: entry: add .ltorg directive to keep literals in range")<br/>
<a href="https://git.kernel.org/linus/bdd0d7e290e0e4c8f7545fff89770abbd22bd51a">bdd0d7e290e0</a> ("drm/amd/display: fix non-x86/PPC64 compilation")<br/>
<a href="https://git.kernel.org/linus/51bae889fe111e418321ff0e6bb5f67e64cb9042">51bae889fe11</a> ("af_unix: Put pathname sockets in the global hash table.")<br/>
<a href="https://git.kernel.org/linus/1e70212e031528918066a631c9fdccda93a1ffaa">1e70212e0315</a> ("hinic: Replace memcpy() with direct assignment")<br/>
<a href="https://git.kernel.org/linus/2c0ab32b73cfe39a609192f338464e948fc39117">2c0ab32b73cf</a> ("hinic: Replace memcpy() with direct assignment")<br/>
<a href="https://git.kernel.org/linus/c0c87382c1a6985cd12a49a62a893361e5fd1b8f">c0c87382c1a6</a> ("drm/amdgpu/display: fix build when CONFIG_DEBUG_FS is not set")<br/>
<a href="https://git.kernel.org/linus/90f4b5499cdd94be3c1e856375ecd7d5f9c4cecc">90f4b5499cdd</a> ("rtw88: 8821c: fix access const table of channel parameters")<br/>
<a href="https://git.kernel.org/linus/e15db62bc5648ab459a570862f654e787c498faf">e15db62bc564</a> ("swiotlb: fix setting ->force_bounce")<br/>
<a href="https://git.kernel.org/linus/f6b66ca4f38b1169313383aec7fa0a8446205ebb">f6b66ca4f38b</a> ("kbuild: rebuild multi-object modules when objtool is updated")<br/>
<a href="https://git.kernel.org/linus/ebd191b38c5ea177318543a08e544cf2f7df944d">ebd191b38c5e</a> ("kbuild: add cmd_and_savecmd macro")<br/>
<a href="https://git.kernel.org/linus/c6031b1dbbbfec03891bf1baefa2e0803d705601">c6031b1dbbbf</a> ("kbuild: make *.mod rule robust against too long argument error")<br/>
<a href="https://git.kernel.org/linus/cd968b97c49214e6557381bddddacbd0e0fb696e">cd968b97c492</a> ("kbuild: make built-in.a rule robust against too long argument error")<br/>
<a href="https://git.kernel.org/linus/31cb50b5590fe911077b8463ad01144fac8fa4f3">31cb50b5590f</a> ("kbuild: check static EXPORT_SYMBOL* by script instead of modpost")<br/>
<a href="https://git.kernel.org/linus/c25e1c55822f9b3b53ccbf88b85644317a525752">c25e1c55822f</a> ("kbuild: do not create *.prelink.o for Clang LTO or IBT")<br/>
<a href="https://git.kernel.org/linus/5ce2176b81f77366bd02c27509b83049f0020544">5ce2176b81f7</a> ("genksyms: adjust the output format to modpost")<br/>
<a href="https://git.kernel.org/linus/7375cbcf2343a9337b19846e76dfd94c3af98a27">7375cbcf2343</a> ("kbuild: stop merging *.symversions")<br/>
<a href="https://git.kernel.org/linus/7b4537199a4a8480b8c3ba37a2d44765ce76cd9b">7b4537199a4a</a> ("kbuild: link symbol CRCs at final link, removing CONFIG_MODULE_REL_CRCS")<br/>
<a href="https://git.kernel.org/linus/f292d875d0dc700b3af0bef04c5abc1dc7b3b62c">f292d875d0dc</a> ("modpost: extract symbol versions from *.cmd files")<br/>
<a href="https://git.kernel.org/linus/69c4cc99bbcbf3ef2e1901b569954e9226180840">69c4cc99bbcb</a> ("modpost: add sym_find_with_module() helper")<br/>
<a href="https://git.kernel.org/linus/ead165fa1042247b033afad7be4be9b815d04ade">ead165fa1042</a> ("objtool: Fix symbol creation")<br/>
<a href="https://git.kernel.org/linus/1a6dd9996699889313327be03981716a8337656b">1a6dd9996699</a> ("can: mcp251xfd: silence clang's -Wunaligned-access warning")<br/>
<a href="https://git.kernel.org/linus/8218827b73c6e41029438a2d3cc573286beee914">8218827b73c6</a> ("scripts/min-tool-version.sh: raise minimum clang version to 14.0.0 for s390")<br/>
<a href="https://git.kernel.org/linus/bb31074db95f735004203b307e63e2e0d4ef9c26">bb31074db95f</a> ("s390/boot: do not emit debug info for assembly with llvm's IAS")<br/>
<a href="https://git.kernel.org/linus/e9953b729b789c0e2984859e3b2170b7fa8520d5">e9953b729b78</a> ("s390/boot: workaround llvm IAS bug")<br/>
<a href="https://git.kernel.org/linus/e6ed91fd0768b914558dad5eeda2407a7d871f52">e6ed91fd0768</a> ("s390/alternatives: remove padding generation code")<br/>
<a href="https://git.kernel.org/linus/fad442d3abde47aef97d0d822807ab6e2555784a">fad442d3abde</a> ("s390/alternatives: provide identical sized orginal/alternative sequences")<br/>
<a href="https://git.kernel.org/linus/2a66c3124afd2782015d160f8bad693488ce68de">2a66c3124afd</a> ("modpost: change the license of EXPORT_SYMBOL to bool type")<br/>
<a href="https://git.kernel.org/linus/78e9e56af3858bf2c52c065daa6c8bee0d72048c">78e9e56af385</a> ("kbuild: record symbol versions in *.cmd files")<br/>
<a href="https://git.kernel.org/linus/e493f472752000968f5b30aac10391288cfbf5b1">e493f4727520</a> ("kbuild: generate a list of objects in vmlinux")<br/>
<a href="https://git.kernel.org/linus/a44abaca0e196cfeef2374ed663b97daa1ad112a">a44abaca0e19</a> ("modpost: move *.mod.c generation to write_mod_c_files()")<br/>
<a href="https://git.kernel.org/linus/7fedac9698b3a56571064eb3b23063f09c93eb94">7fedac9698b3</a> ("modpost: merge add_{intree_flag,retpoline,staging_flag} to add_header")<br/>
<a href="https://git.kernel.org/linus/9d79799193b728b62c9899d931b5009da1f89b67">9d79799193b7</a> ("fbcon: Fix delayed takeover locking")<br/>
<a href="https://git.kernel.org/linus/334865b2915c33080624e0d06f1c3e917036472c">334865b2915c</a> ("x86/extable: Prefer local labels in .set directives")<br/>
<a href="https://git.kernel.org/linus/6c7cb60bff7aec24b834343ff433125f469886a3">6c7cb60bff7a</a> ("ARM: fix Thumb2 regression with Spectre BHB")<br/>
<a href="https://git.kernel.org/linus/4013e26670c590944abdab56c4fa797527b74325">4013e26670c5</a> ("arm64: module: remove (NOLOAD) from linker script")<br/>
<a href="https://git.kernel.org/linus/925088781eede43cf6616a1197c31dee451b7948">925088781eed</a> ("KVM: x86: Fix pointer mistmatch warning when patching RET0 static calls")<br/>
<a href="https://git.kernel.org/linus/cc188a73addc8188d73ad11901b697acdc7fd0b0">cc188a73addc</a> ("drm/amd/pm: fix enabled features retrieving on Renoir and Cyan Skillfish")<br/>
<a href="https://git.kernel.org/linus/a69cb445f7d129abf7c50d48c8a8eca7c8d5df15">a69cb445f7d1</a> ("crypto: arm/xor - make vectorized C code Clang-friendly")<br/>
<a href="https://git.kernel.org/linus/297565aa22cfa80ab0f88c3569693aea0b6afb6d">297565aa22cf</a> ("lib/xor: make xor prototypes more friendly to compiler vectorization")<br/>
<a href="https://git.kernel.org/linus/6bf625a4140f24b490766043b307f8252519578b">6bf625a4140f</a> ("Drivers: hv: vmbus: Rework use of DMA_BIT_MASK(64)")<br/>
<a href="https://git.kernel.org/linus/d2a02e3c8bb6b347818518edff5a4b40ff52d6d8">d2a02e3c8bb6</a> ("lib/crypto: blake2s: avoid indirect calls to compression function for Clang CFI")<br/>
<a href="https://git.kernel.org/linus/b7892f7d5cb2b8187c603dd8ea3a7c44059ccfc2">b7892f7d5cb2</a> ("tools: Ignore errors from `which' when searching a GCC toolchain")<br/>
<a href="https://git.kernel.org/linus/35140d399db2b67153fc53b51a97ddb8ba3b5956">35140d399db2</a> ("script/sorttable: Fix some initialization problems")<br/>
<a href="https://git.kernel.org/linus/2056e2989bf47ad7274ecc5e9dda2add53c112f9">2056e2989bf4</a> ("x86/sgx: Fix NULL pointer dereference on non-SGX systems")<br/>
<a href="https://git.kernel.org/linus/5fe41793bc78d9bb47fea37d1a16984ad6cf294b">5fe41793bc78</a> ("ARM: 9176/1: avoid literal references in inline assembly")<br/>
</code></p>
</details></br>

As a direct result of this, I appeared in the top testers in the [5.19 development cycle](https://lwn.net/Articles/902854/) and [6.1 development cycle](https://lwn.net/Articles/915435/).



## LLVM

I am far from a large LLVM contributor but I do have occasional patches there as part of this work. This year, I had 4 patches to LLVM, which can be viewed on [GitHub](https://github.com/llvm/llvm-project/commits/main?author=nathanchance). They are just reverts of patches that caused issue for us, which I was a little bit more aggressive about this year due to our use of LLVM ToT in continuous integration.



## Tooling

We have a few different repositories in [our GitHub organization](https://github.com/ClangBuiltLinux) that we use for testing and development, which I call "tooling". Tooling is very important for repetitive tasks or tasks where you want to take the human out of the equation so that mistakes are less likely, such as a bisect. Additionally, there are some other repositories that we rely on, like [TuxMake](https://gitlab.com/Linaro/tuxmake), that I consistently contribute to.

A common theme this year is moving more and more infrastructure to Python where possible, as it gives us more flexibility in writing scripts that can do complicated things without worrying about how we are handling input, because we actually have types and data structures in Python, unlike shell. In particular, moving boot-utils to Python has increased what we are able to do with it, such as [passing along a folder from the host to the VM](https://github.com/ClangBuiltLinux/boot-utils/pull/82).

Like previously, I have included the `git log` output with direct links to commits, along with a web link to browse the history there.

<details>
<summary>boot-utils (<a href="https://github.com/ClangBuiltLinux/boot-utils/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code><a href="https://github.com/ClangBuiltLinux/boot-utils/commit/772eb24d1c0186620c86bba81ff0e0fd64b25464">772eb24</a> ("boot-qemu.py: Check gdb_bin earlier")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/867976c2befc21009f7f5d24872218eae4a273e1">867976c</a> ("boot-qemu.py: Simplify can_use_kvm()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/581efc6c5066eb7d2f2ae21bac94260f263cd07f">581efc6</a> ("boot-qemu.py: Update unsupported EFI boot message")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/3da9dd2b9413b729a2c7232835a38cddc827287f">3da9dd2</a> ("github workflows: Switch to new Python linting workflow")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/098b8c19fd77eba099101381111393ae3d29b5e9">098b8c1</a> ("Silence module name warnings")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/7b8d05b6748f80a804b2fbdd4e919247fecccf88">7b8d05b</a> ("boot-qemu.py: Add support for booting arm64 and x86_64 guests under UEFI")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/6249ca49161d17cf5fbdd2bd4a3ab753691ccda9">6249ca4</a> ("Avoid redefining outer names")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/58116c5ee5f3638ceb17ff50b657b0daaa7688f3">58116c5</a> ("Add explicit check parameter to subprocess.run() calls")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/8b5b27469ab0db53fef4a56292bdc532e10da0d3">8b5b274</a> ("Use Popen() in context manager in launch_qemu()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/05b4f3b213c0d084cd2c05847ecba78e078de0e1">05b4f3b</a> ("Update 'with open()' in get_smp_value()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/8ebd7cb01f3f3d3f968fa8540e70b76f04e9dfe0">8ebd7cb</a> ("Use read_text() from pathlib in can_use_kvm()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/a7bd42c3ce332571bde844644bb7a364bd64af3e">a7bd42c</a> ("Use 'in' for certain comparisons")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/24133b31e142f29aa6f344217aef198764b1f67c">24133b3</a> ("Use sys.exit() over exit()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/664844373c2dc33ff96af0368468cf9c9d7a65ee">6648443</a> ("Silence flake8 E251 warnings")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/6e80b3b320f8bef4b1dfc71d2b5fcb9b437340fc">6e80b3b</a> ("boot-qemu.py: Use tuples for Linux and QEMU versions")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/a1403e9384c6fb4a330645d7c0d7411d26d9f6c6">a1403e9</a> ("boot-qemu.py: Combine aarch64 machine flags")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/ca70cb361316839456843520047ef95b49c17e33">ca70cb3</a> ("Eliminate calling as_posix() on path objects")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/6b301861a8f399374c11b92d0aaea54c833b1865">6b30186</a> ("Switch to f-strings")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/604db5b6fd98c1d7f94baa8478222b82eef68c52">604db5b</a> ("boot-qemu.py: Fix get_linux_ver_code() when CONFIG_LOCALVERSION_AUTO is not set")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/6a2833c73f1f20b4b26894f6aacc378f0fad4e2f">6a2833c</a> ("boot-qemu: Move from 'powernv8' to 'powernv' machine")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/09fd7a3f6d00f42deb20b1ef0cfa6e3fc13c974e">09fd7a3</a> ("boot-utils: Flush print commands to ensure output is properly ordered")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/a0540ceaccd97e92d2c2eb0ea504e762a174171b">a0540ce</a> ("boot-utils: Remove skiboot")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/d2b3324d8836839268c21b6a58db0053591d029f">d2b3324</a> ("boot-utils.py: Fix searching for Linux kernel version numbers")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/8e7d6717a126f335519848207dd970862b76f661">8e7d671</a> ("boot-uml.py: Fix stray 'i' in decomp_rootfs()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/d23d9941f99ed04d100ef57ea8f306ec3f18c196">d23d994</a> ("boot-qemu.py: Move pretty_print_qemu_cmd() out of try block in launch_qemu()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/312aaefd30d3aa167c095c9788605d8631d2552e">312aaef</a> ("boot-qemu.py: Use 'break' instead of 'exit(0)' in launch_qemu()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/9dc0d2048b01e39860fda2a849859c43931ba140">9dc0d20</a> ("boot-qemu.py: print_version_code() -> create_version_code()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/8ba5012653af7f0f74cf181e93ab3ca71e74d68e">8ba5012</a> ("boot-qemu.py: Use more specific arguments for can_use_kvm()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/314d18dc9b2d48d6ba7df5a46d4e0dcf7799c791">314d18d</a> ("boot-qemu.py: Eliminate unnecessary local variable in can_use_kvm()")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/961f44063165dc16669e3c5e8d2863af737c6bd8">961f440</a> ("README: Remove stray 'i'")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/9883bf88383a113de31e585b82d7ff883fd19aa8">9883bf8</a> ("debian: Remove")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/aedb4a4c10fbd61d7b59d6d5c57365204eb5d441">aedb4a4</a> ("README: Rewrite")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/7f88af34915f62dc43371d7eb5ac2525ab1e8351">7f88af3</a> ("github: Add yapf step")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/6a14843e9748a1159cd991d1d65f948028b7a92c">6a14843</a> ("boot-utils: Rewrite core scripts in Python")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/e3da2ab0c6f36f1d2814af632834527dd1939547">e3da2ab</a> ("boot-uml.sh: Remove exec hack")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/81fe57a4b18d4680a9d84fdaaff75c87e745902f">81fe57a</a> ("boot-qemu.sh: Fix aarch64 KVM after cb0698a")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/cb0698ab47731038291726e9a1d62723592c9c13">cb0698a</a> ("boot-qemu.sh: Use implementation defined pointer authentication algorithm")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/4dd7e0b80e6e9274e8e82762f09b0290c03194ce">4dd7e0b</a> ("boot-qemu.sh: Use different CPU for arm64 with new QEMU + old kernel")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/2a315ccfca4abc39e5b12e0ff76823c484257e04">2a315cc</a> ("utils.sh: Make get_full_kernel_path() more robust with UML")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/9d37a7b70f50cde3d317b29dd6402cb12e1c19fa">9d37a7b</a> ("boot-uml.sh: Add link to issue for 'exec' workaround")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/bd4b962ee12e00f666eef12e3413a79d334a0685">bd4b962</a> ("boot-utils: Add boot-uml.sh")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/80e9a83feee413cbc2a62d4ac766558229631b62">80e9a83</a> ("buildroot: Build an ext4 image for x86_64")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/8603a5cc54ca67c6cd1cb74051a1dfcf9e0ccd5d">8603a5c</a> ("images: Update")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/c51a603ad0f48eac6e0bfcd58867dcbfa5c8d7b6">c51a603</a> ("buildroot: Upgrade to 2022.02")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/11d3e43628b96821c5a2446ca64ddc2220940d1d">11d3e43</a> ("boot-qemu.sh: arm64: Pass 'lpa2=off' when necessary")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/d1daa35e64d5f828ff77c2fd68d6795205856acc">d1daa35</a> ("boot-qemu.sh: Share version printing code between Linux and QEMU")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/e53f47e3b304dbf497bd0e447a3cfe7b548acad1">e53f47e</a> ("boot-qemu.sh: Add a function to get the kernel version as a number")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/4320279166f7626f96f4d6dc4e2e91db757e0817">4320279</a> ("boot-qemu.sh: Add a function to get the QEMU version as a number")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/56210943f80056bd6ac39b2603acb21cc0c6fbea">5621094</a> ("boot-qemu.sh: Move kernel image path logic to its own function")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/ff6f235f6667662456a20c008b295934168b81d1">ff6f235</a> ("boot-qemu.sh: Add a few comments in the arm64 Debian block")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/e623485fe68f0a8a15b4022fbaceaa672680d838">e623485</a> ("boot-qemu.sh: Use '-smp' for other instances of KVM")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/066e331200f369c198e0152f0193d6e01185a5e7">066e331</a> ("boot-qemu.sh: Clamp '-smp' based on CONFIG_NR_CPUS when available")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/08686522331f5c97734f91bdca220d8cb34d148e">0868652</a> ("boot-qemu.sh: Do not set '-cpu' for 32-bit x86")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/49af93716d5cd9ed728eac967ba4ff26f7793487">49af937</a> ("debian/build.sh: Update default version to bullseye")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/53a74dd5fd214df6dbb52e5197ab66c65ef120e2">53a74dd</a> ("debian/build.sh: Use 'uname -n' instead of 'hostname'")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/da1a94d6b3c1c37b51f324211083b0568cf4065e">da1a94d</a> ("boot-qemu.sh: Add support for booting pmac32_defconfig")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/6931ca3e321cc22a6b5a4fa6fee00df96e892aa1">6931ca3</a> ("boot-qemu.sh: Add 'arm' as an alias for 'arm32_v7'")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/ad66a0da39b3c85227b71c664fba7f01fec425ff">ad66a0d</a> ("boot-qemu.sh: Support booting ARMv7 kernels under KVM on AArch64 hosts")<br/>
<a href="https://github.com/ClangBuiltLinux/boot-utils/commit/76687ffad6eaa7bfae9733dc72ee2d76acc56914">76687ff</a> ("boot-utils: Remove '--use-cbl-qemu'")<br/>
</code></p>
</details>

<details>
<summary>containers (<a href="https://github.com/ClangBuiltLinux/containers/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code><a href="https://github.com/ClangBuiltLinux/containers/commit/9a511dd1ad89e71179566873c8180b6898a1bda0">9a511dd</a> ("github workflows: Switch to more robust Python linting workflow")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/6533047c36b6fbc88d38528fbd1b983e25ec9e0f">6533047</a> ("ci/install-vm.py: Disable pylint warning around invalid module name")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/4784e520a97497b5de1cb23775dd9af7b7a80ae5">4784e52</a> ("ci/install-vm.py: Switch to f-strings")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/533a9f54bc16393e7aade7bb4e9d1044367cd5e4">533a9f5</a> ("ci/install-vm.py: Shorten description to avoid flake8 warning")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/252785f8f4ad7e0f0f2961e60d730bb94c7125b7">252785f</a> ("Revert "qemu: Temporarily switch to testing repo for updated QEMU packages"")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/879be368a87b7c1605ebba2b143a2ca4562d0512">879be36</a> ("qemu: Temporarily switch to testing repo for updated QEMU packages")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/0e45694a3137510d38aae642d5a9aa7cee275eaa">0e45694</a> ("qemu: Install binutils")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/978f85d63b831727580d75140f734196e1cbe499">978f85d</a> ("github: build-test-llvm-project: Add build-args")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/dc4ac4d6c72dc8086d35c12df3233d11c968da5f">dc4ac4d</a> ("llvm-project: README: Update tag names for new structure")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/369b8cbb26878d50aecb6b8366cd04a0fbd33bb2">369b8cb</a> ("llvm-project/Makefile: Add support for building epochs")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/afa077c860efdd1acde1001140b6e3b045ebe9ab">afa077c</a> ("llvm-project/Makefile: Allow user to use Podman via DOCKER variable")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/c4f312d855d5c3369897d70089250c281c6c9f6c">c4f312d</a> ("ci/test-clang.sh: Capitalize Docker override variable")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/dcc15cd6d56aae71beff2906a58d2e5f80982907">dcc15cd</a> ("github: Make tag specific to architecture")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/1a9c88212c90e0ac32f71d6fa73987d7274ef71a">1a9c882</a> ("github: Fix llvm-project epoch three workflow for AArch64")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/42700038fda4067b5ab81e231aaf6ba89468bba2">4270003</a> ("llvm-project: Add a space between '-D' and key + value")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/4c4f7a2edf9a83253763e694e91f3d2941d3db7a">4c4f7a2</a> ("ci/test-clang.sh: Skip toolchain archive creation when run locally")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/32025954b42edb6a85d1cc0f4df9ae357abc4a04">3202595</a> ("ci/test-clang.sh: Add 'z' option to '--volume'")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/e0552956d3c0a431b09b76bcb722eca0b082ca12">e055295</a> ("ci/test-clang.sh: Allow user to use Podman")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/7f6804df8fb9e201c931ca67e22d89f3c980a2a1">7f6804d</a> ("llvm-project: Specify registry for Alpine image")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/57e0170990005004f1528052eeb13ada328011fb">57e0170</a> ("github: workflows: Support arm64")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/a4a26de3fcaa24b32d778ae02548afd4a3fb22f6">a4a26de</a> ("llvm-project: Make build process architecture agnostic")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/ac1cef5b16c31a1e659d5b0bb57b7790f0c201c6">ac1cef5</a> ("ci/configure-vm.sh: Link to "Create self-hosted runner" page")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/3b23bc0f03ba217a9dd34d949b59d3d593679079">3b23bc0</a> ("ci/install-vm.py: Explicitly connect to qemu:///system")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/2a75b0eb044d157cc6db0f488b9dcf780765b026">2a75b0e</a> ("github: workflows: Add linting workflow")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/9be6c0d7f749fc48976102d82689d5e7aa9aa7ba">9be6c0d</a> ("ci/test-clang-docker.sh: Appease shfmt")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/61a17233ec582c58b5849620a24ea36e835f63da">61a1723</a> ("ci: Add a simple Python script to install virtual machine using libvirt")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/c3435a88fbb674ca82dc173fa3d1f92ab41847c7">c3435a8</a> ("ci/configure-vm.sh: Appease shfmt")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/b06e2b6804bcf698fcb2f5db922ac3614fef3cc0">b06e2b6</a> ("ci/configure-vm.sh: Add OS checks to update_install_packages()")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/3f8c937af2928367872ac843f4ded7594b1c2f99">3f8c937</a> ("ci/configure-vm.sh: Check filesystem type before calling xfs_growfs")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/0dd7075a3ba84eb52eecd77d882fbc16a0589ac6">0dd7075</a> ("github: workflows: Switch to self-hosted runners")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/f87c1e39a772dbcd1b2a2517fb7cc0984d93a8a5">f87c1e3</a> ("github: actions: build-test-llvm-project: Pull images before building")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/989e2becf62da58ad0d287f4957c04e5c51f7573">989e2be</a> ("ci: Add script to configure virtual machine")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/51314436a5d5d5110b472ab3c131cf5e81a4e47f">5131443</a> ("Upload toolchain tarball when build is successful")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/ebd6906a8a2ddecbce5ce52069617483eba16f26">ebd6906</a> ("github: workflows: Rename llvm-project workflows")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/ccfda3bd63612b9bf5df213eeb1b79f2e1f19e22">ccfda3b</a> ("github: Move to a composite action for llvm-project builds")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/d0b65f0918c87635bf8695adcbdaacc68b1c7530">d0b65f0</a> ("github: workflows: Shorten 'GitHub Container Registry' to ghcr.io")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/4372995bcda53316dfa83015107b3ce95719d1ca">4372995</a> ("github: workflows: Move 'docker login' step to right before image push")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/2a612087a917fe0642ccc4badc817e0a0b19ae7c">2a61208</a> ("github: workflows: Space out steps")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/50fab280c153fab864a8455f2e4e7cba93d1dc62">50fab28</a> ("ci/test-clang-docker.sh: Use '--sysroot' for Arch Linux")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/162cc51a36827e94e2d538a35916171a156c871f">162cc51</a> ("ci/test-clang.sh: Clarify Arch Linux comment")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/a6a0b028bc0c6d13b290ea19f7142c6406bd35af">a6a0b02</a> ("github: workflows: Run 'docker push' in a separate step")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/30f74d6ea015f20eeb5c9295e719dc61dac35fe9">30f74d6</a> ("github: workflows: Update Docker actions")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/5f2bbadc55a717f16a372efb98b6f5b772fd7553">5f2bbad</a> ("ci: Test statically linked clang in Arch Linux containers")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/179aaa1399e18c519f66fb0eca20ee849a3e97b0">179aaa1</a> ("ci/test-clang-docker.sh: Add LD_LIBRARY_PATH test")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/b8a02d204216d65c3c86d8e0774828ac2e2e2752">b8a02d2</a> ("ci/test-clang.sh: Let docker cp create bin, include, and lib directories")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/55125a5086cd353263a06b1917736fe2ead0d194">55125a5</a> ("ci/test-clang-docker.sh: Define CC and CXX explicitly")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/5cc84afddae54ec7c09798603a53528ad89dc4fc">5cc84af</a> ("ci/test-clang.sh: Split docker run command over multiple lines")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/22c734e153fa0f87dba1ff5a6de2725cb21fad56">22c734e</a> ("ci/test-clang.sh: Use long flag for volume")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/33cc658456d4eaa9caf88482e137f97d769fe766">33cc658</a> ("Test statically linked clang outside of Alpine Linux")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/d79ebcb27c92946ab3dee83b6b2673052832c94d">d79ebcb</a> ("qemu: Update package list")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/9fa1423564516f3e2324e4df370ae7639d039fce">9fa1423</a> ("workflows: Bump actions/checkout to v3")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/040e7f819b1ad2cea6164144158829608a326c1d">040e7f8</a> ("qemu: Install python-yaml")<br/>
<a href="https://github.com/ClangBuiltLinux/containers/commit/a5ae59e0adc4c6d9f09c50148738dee3b36a8111">a5ae59e</a> ("qemu: Install git")<br/>
</code></p>
</details>

<details>
<summary>continuous-integration2 (<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code><a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/65e2acc4f6c2f283b828932f9b4534bdb8188c2d">65e2acc</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9374a7e38510964e58e0f2d3af893d4ad87097ef">9374a7e</a> ("generator.yml: Disable CONFIG_IPMMU_VMSA for OpenSUSE's RISC-V config")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3bc3f5cf0980d22923cdffbe1ff683fef05195f9">3bc3f5c</a> ("patches: Add patch for CONFIG_DRM_OFDRM=m and CONFIG_FB_OF=y")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1913bdcdac3d0a8a9b660b5f798f878ef3d9649e">1913bdc</a> ("patches: next: Update for next-20221226")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/93c4ff9e9344df487418273f5612065e4221b18c">93c4ff9</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/35fdd94c1503290de5dffd5198ca8fbc053b8903">35fdd94</a> ("patches: next: Add fixes during break")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ef2533b6fbaab273e1a9ae707ef83628aa53c3f5">ef2533b</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/eea13597718c78b7d4b5229ef9977c9409551dfd">eea1359</a> ("patches: Drop mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/457ddd5e80ecb034f72a8b604b4cdc4a029cd86f">457ddd5</a> ("patches/stable: Drop x86 vDSO patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/64ca63e58b997a97f2ededff536e343e5cf31b3e">64ca63e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f7a5027d6a30b1e61c4333a42e19e22082fef997">f7a5027</a> ("patches: Drop android14-5.15, chromeos-5.15, and android-mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9160c4f8f2b571185cddef9f4bb9362e1113533c">9160c4f</a> ("README: Add android14-6.1 to matrix status")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/91cc42eafd28d448516457be317c7e17b25766e8">91cc42e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fe25476fea9a8f0dbc2bd3d2de660e918205f457">fe25476</a> ("generator.yml: Add android14-6.1")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/21a153633676424595b3683689c3f65f9a1d2c81">21a1536</a> ("patches: Remove 45be2ad007a9c6bea patch from 5.15")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ed98cd4002aec529081b24acd75209cacf8fb478">ed98cd4</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4497a19c6ebb47918b9b5d0fecb9e7e2ae6d6b34">4497a19</a> ("generator.yml: Re-enable x86_64 allyesconfig for LLVM < 15")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b94a3f658c378b657ef9634ab9d1e43ea51f5eed">b94a3f6</a> ("generator.yml: Disable x86_64 allyesconfig with LLVM 15+ on mainline and next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c920d17ed86051a8e88f9c615c5feaf5902d005d">c920d17</a> ("patches/mainline: Remove ARM patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0a1020bda1c6dd225d242fca3132f3e1766a4d15">0a1020b</a> ("patches/mainline: Add fixes for warnings in drivers/staging/media")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2def315740a14c505cafc0d402a0df8df3e2c2ab">2def315</a> ("boot-utils: Update to latest main")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c32904f5581c13488220e645bf709b48895f9376">c32904f</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/764d90f9930fb32e25724b938d9222b6a26e8e4b">764d90f</a> ("generator.yml: Update stable anchor to 6.1")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/874edd70fc525da0f72b467c1bdf706be28e09ff">874edd7</a> ("generator.yml: Add arm64 kCFI build without LTO to mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7322449d6d74d46b0583fec9193e29e53015a30f">7322449</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/076600ea1b22af6b9b9603376cc585bfe0630252">076600e</a> ("generator.yml: Re-enable x86_64 KCSAN build on mainline with LLVM ToT")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5647d262d4d5ed7c71f00d53c9d374901dd0ec17">5647d26</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f03d0db63d78a7f1b2f4a5362ab6d0a8c1ca6423">f03d0db</a> ("patches: Add x86 vDSO patch for tip of tree ld.lld default change")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/110c2647034b25f1509fca99ecaf549c56a4e752">110c264</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/59929062824ce1b7020affd4ad30467deadfefb0">5992906</a> ("generator.yml: Disable x86_64 allyesconfig builds on -tip")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c41893ee15b90eeeecf8826f771a6f3558aa31df">c41893e</a> ("workflows: lint: Use updated Python linting workflow")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/633667f69b01dd2bcfa64e1d397cd7825cbd0609">633667f</a> ("scripts/markdown-badges.py: Use more specific variable name")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/61bbc19184e9dd6cc552c26efc3d576887aed5fb">61bbc19</a> ("Add requirements.txt")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/96d2530e8de7c99737144abdf5239eb3375a3868">96d2530</a> ("generate_tuxsuite.py: Rename unused data parameter")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/beada580c6de70e32ebdff6639787879b460500f">beada58</a> ("check_logs.py: Remove TODO")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8c686690e9da9f2a51417c0aed81407314e392c5">8c68669</a> ("check_logs.py: Use longer initial build variable name")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/686a3b008fa45f8348927775e8f9f0338fb93130">686a3b0</a> ("check_logs.py: Remove len() on list")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/47cfa066bac24101a8fbf5e214d9fa973bf74acc">47cfa06</a> ("check_logs.py: Use 'check=True' in print_clang_info()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b52059601c5421fa9a013aae5f0175003170038d">b520596</a> ("check_logs.py: Use 'not in' in fetch_dtb()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3b829385489c4861d30b677bdf472360a6406703">3b82938</a> ("check_log.py: Eliminate unnecessary pass statements")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5a724ad7dcc30f9fd0bd2e98acc26a841c07225a">5a724ad</a> ("Use f-strings in a few more locations")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/dcd3c549660f0c4a856485857f26f3277d6daff6">dcd3c54</a> ("utils.py: Raise an exception in get_repo_ref()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c9c2122fd2a0a8eee8f4d00b135c90c1aff9acce">c9c2122</a> ("scripts/markdown-badges.py: Use .items()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b4dcc2c93acbcd97974c34c4ce4e9bac59343257">b4dcc2c</a> ("generate_workflow.py: Switch to sys.exit()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f108dc0a3c5a6426791d85266d2d1f64eb19e372">f108dc0</a> ("Switch to a literal empty dictionary over dict()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f28e35904f871bcec071e3c012e96588603bf8ae">f28e359</a> ("Eliminate pylint warning 'consider-using-with'")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b1dc37dc3afe5a9ce4aa634694f527de8cf67b1e">b1dc37d</a> ("Eliminate invalid name warnings from pylint")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/25852ee9f753395b4d1f9bac5a377dba2eece4ca">25852ee</a> ("Add explicit encoding parameter to open()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7bf6487ea293cd430959ae942a3c95bec92ff794">7bf6487</a> ("generate_*.py: Use a shorter initial variable name for llvm_version")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/06429bd9a09fd18afdfe240c41e63d1b85db9b8f">06429bd</a> ("generate_*.py: Use a longer initial variable name for config")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/674c7d39c9367f5fc2461081b6777b4c97dc26bc">674c7d3</a> ("get_config() -> get_config_from_generator()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9b5c99289882a232fc5e0d3a140229facbf561b9">9b5c992</a> ("generate_workflow.py: Fix unidiomatic typecheck warning")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/754b74f5cef86048ec5c014d36986ef3ac72649d">754b74f</a> ("scripts/markdown-badges.py: Fix comparisons to None")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7727b4a9c24b1a87d729bca2614cbbbcc3b0b816">7727b4a</a> ("scripts/markdown-badges.py: Place imports on separate lines")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6a04b39ad92fe776adf6acf0add55364de28788a">6a04b39</a> ("generate_*.py: Add another space to 'yapf: disable' comments")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3eb778f5bc705d4a090d00d826f782adc20f56bc">3eb778f</a> ("generate_workflow.py: Remove unused pathlib import")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f49f49b10702cd9cd6385bf74b6620aa9139daea">f49f49b</a> ("check_logs.py: Fix flake8 E713")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/804a66df46249d9ffbb94f49b746494331c5d426">804a66d</a> ("patches: android13-5.10: Add upstream series for pahole 1.24 compatability")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/66bac28e33801acec0757717aabd484b92db42e6">66bac28</a> ("README: Update markdown badges")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2df4a09555900eee0711b59c6a5dbcd195e9b27d">2df4a09</a> ("scripts/markdown-badges.py: Add "Check clang version" badges")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/db191db84ce82ced03ddc6b791f931f447221d74">db191db</a> ("scripts: Remove markdown-badges.sh")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/47b9ab9d053049b3b666bffb7d47fb2eb84e7803">47b9ab9</a> ("ci: Regenerate GitHub Actions workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/86d46c4e824621172117db8897a483e1de9d0866">86d46c4</a> ("generate_workflow.py: Move timeout from step to job scope")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2027dfde0e37697a3a4bce5c778ede8dcd3a9305">2027dfd</a> ("boot-utils: Update to latest main")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/49e96e7a5b8e72b9cccf8a9b2becca18efd6cc78">49e96e7</a> ("utils.py: Use f-string for zero-sized exception")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5a4758e7e3ec8d7e70afc20ab0c30b1ea95dbd70">5a4758e</a> ("check_logs.py: Consistently use double quotes")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/82b750da1ae9f4bb444505726fbf4915a38427a1">82b750d</a> ("utils: Eliminate as_posix()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6aca9c29fdbd0c4810228834933c8d27100c401f">6aca9c2</a> ("Use f-strings everywhere")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d51683cc62a918b36543076b1e3ba7e9a3e55d92">d51683c</a> ("check_logs.py: Add explicit check for an unknown build result")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/47c79a26387bd4e2b63608c3eacccff14ae8ca6a">47c79a2</a> ("check_logs.py: Add explicit check for timed out build")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b0a640e8abd340eb48381ed629a65dcf74bf284f">b0a640e</a> ("check_logs.py: Check 'tuxbuild_status' instead of 'result'")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/25e29380128fe9db2c7c19936445656d101830ab">25e2938</a> ("utils.py: Add a check for an empty builds.json")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1893d9bdfdc67fd5cd7dfe83ce809a05e5995d78">1893d9b</a> ("patches: mainline: Drop BPF patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d41de32557d09f2e7a705c990e22421f6bf7059f">d41de32</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fb9b19dca4c7673eed114f24bc517d7b9562f6bc">fb9b19d</a> ("generator.yml: Disable LLVM 14 s390 builds on mainline and -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5e0bb0a24883f7843672864702fd26542b02eb0b">5e0bb0a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a6b672e06cf43453f592605f68d2ed6656bdc419">a6b672e</a> ("patches: next: Remove")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9636d17d1a82f238ec2aff9fc92e4721ab1cfa39">9636d17</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/79aa35df165f8cca45f3dd56fe71270e2b9fdba5">79aa35d</a> ("patches: Drop arm64")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d1e5510fc2730763b9cbe3b7dadd6215396ac3f1">d1e5510</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/16e2698d5bb0c71da1e16df0ca1432a9ae5da707">16e2698</a> ("generator.yml: Disable ppc44x_defconfig builds on stable")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/374bb59f900bf7217c4ca82e0a66d57211fdbee1">374bb59</a> ("patches/next: Update bpf patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d5a99cbe5330a6e27fb504ced853766926ca7716">d5a99cb</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5e74e6afa107f5b35ce160be33470d8f4a7d49db">5e74e6a</a> ("Revert "generator.yml: Disable OpenSUSE's RISC-V configuration when using GNU as"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3f47a62a25415af17e5c5b2e24cf0e6d7682562c">3f47a62</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/53e2932ee1a4cda58159ff7b83db3d4206e6866b">53e2932</a> ("generator.yml: Update stable anchor to linux-6.0.y")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e558c3a3d5221f6d3e0dabd57a66b3cbb16fd2c7">e558c3a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c46f318ae0a45dfaee8d019715c9e7999960eee1">c46f318</a> ("generate_workflow.py: Increase 'tuxsuite plan' timeout limit")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3144e8c5d0270289df56104f32e5038272fe4392">3144e8c</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/624e7fd0c6876cf2d2af1f248a7a7d7fe2a537e5">624e7fd</a> ("patches: Remove arm64-fixes patches")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/67b88685949e49f359379005fd85bba8cea1fe88">67b8868</a> ("patches: Apply 2120635108b35ecad9c59c8b44f6cbdf4f98214e to arm64 trees")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/434c9aeb2b0213a36fab2d67a5acb2e791d76980">434c9ae</a> ("patches: Apply patch for bpf recordmcount issue")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f77c34087a103f9f446572917693d616903d0e1b">f77c340</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e81fd382bd099fed78fcef6516b2ae3e94a98c5d">e81fd38</a> ("generator.yml: CONFIG_KASAN_KUNIT_TEST requires tracing now")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/86c55265d513f65c181119beba6775b908fd4ee6">86c5526</a> ("check_logs.py: Increase attempts to verify build status to 9")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d19b0b218e63f62a40464c640479a06cb30f173d">d19b0b2</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b68b35557ac25300b61addc9fae6abd1c39cb4db">b68b355</a> ("patches: Rename folders to display name, rather than anchor name")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7d69e4071252c898e92d6d35b2dc6f6812b4aac9">7d69e40</a> ("scripts/check-patches.sh: Add a check for invalid patch folders")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a459cc72a732b26ac80657f354ce2a8ee35754ca">a459cc7</a> ("boot-utils: Update")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/78e6f8cd84619313b5270156de007fd598c54cff">78e6f8c</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/56edb14ed1988cd376db5bae6d7df4796e8ce919">56edb14</a> ("generator.yml: Add x86_64 kCFI and kCFI + LTO builds for mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4398ef5b97c11fa80ffc1c1666ffbc7811b2f132">4398ef5</a> ("generator.yml: Disable CFI builds for LLVM < 16 in mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/26db2787cc4590da110b2fe637da217c91ec9327">26db278</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e747c007e7adedfd50f1b7e1883725183fa82e26">e747c00</a> ("generator.yml: Add x86_64 kCFI and kCFI + LTO builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6e4dbac162d856322aa2fa022a50dd20cfee9728">6e4dbac</a> ("generator.yml: Adjust CFI builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6fa21d9870be0cc8ae0f69a058b3822e287aacfe">6fa21d9</a> ("generator.yml: Disable CFI builds for LLVM < 16 in -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/08701ae61f8cd340120101ce23143d332bc48f45">08701ae</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0e1ead76104fb45e03b141be5b06ece00deca4ba">0e1ead7</a> ("generate_workflow.py: Reduce permissions of GITHUB_TOKEN")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a64adb66bd5372c5dec9de879ae4968685eb52c5">a64adb6</a> ("boot-utils: Update to latest main")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5d6ea90541954fbb1610bc334e102655810ee221">5d6ea90</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d94a2d59cf76bf21418bcfd510e9b737c689f820">d94a2d5</a> ("generator.yml: Disable OpenSUSE's RISC-V configuration when using GNU as")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/252fdda60404613052a8c6eb27ac98c0cfabde73">252fdda</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f55199fb27e64f3bb68098761440716aee57341c">f55199f</a> ("patches: tip: Remove drm/amd/display patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d6a4476a55f552b1a9460ca9bd146603fed9cb15">d6a4476</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/029b661a3842d7bcd61715d237191b1d3a76429e">029b661</a> ("generate_workflow.py: Do not run TuxSuite workflows on pushes to the main branch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0f01d5f0704113dab1903d240693e13e46b57996">0f01d5f</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/53459020202045f6713dde22927365027db404e3">5345902</a> ("generator.yml: Add Alpine configurations and builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fda5b7a290562317f1c1eae2c97e642feea7c710">fda5b7a</a> ("utils.py: Handle Alpine Linux configurations in get_cbl_name()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/90652bd781f9202c8fcc2e334ba972095673ca06">90652bd</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/96341f4819f4986412f9f3ab245818ad757ab7ff">96341f4</a> ("Revert "patches: mainline: Apply drm/amd/display patches for dml314"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0c453d6230b180c07f78bc7d5cfe3b1c87802847">0c453d6</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2bf47d4c9d3f6bb6c5c540652685afdbecf55b1c">2bf47d4</a> ("generator.yml: Enable x86_64 KCSAN build on -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/91aba15f90c311f712f5a3cc9081dbcdcf5f5b78">91aba15</a> ("utils.py: Handle OpenSUSE riscv64 configuration in get_cbl_name()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/58ec5d2866ded5fa8c5a572931e9690c9c31ca71">58ec5d2</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ed80ab11a23451432fd98da88cd489491b513273">ed80ab1</a> ("generator.yml: Add OpenSUSE's riscv64 configuration")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7523c573172b2d42b7020ff6a0361a8714ed386a">7523c57</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fc1158b82bab45f82255c0aa98e91dfa79b962bc">fc1158b</a> ("patches: mainline: Apply drm/amd/display patches for dml314")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2703f0e37d57e634fb924134da13e01f35ac8fd6">2703f0e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f32c5d8e63787992bd2c385049504b8069eadb4c">f32c5d8</a> ("Fix location of '--patch-series' in tuxsuite commands")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/85f418fae9c31761aeae1f6c8b5ac3fd1b08466f">85f418f</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ae34aec1f5baeda0b150a203943ce45fbf15e737">ae34aec</a> ("patches: mainline: Drop drm/amd/display patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f6d24917b212433e1f67195969bfcc37f03ed963">f6d2491</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/40b9b960078f2592f6a8871d740f6148118031ac">40b9b96</a> ("patches: Remove stable versions of mchp-spdiftx patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5831e726452b36d658bbb87f57ca08130dd63ae3">5831e72</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d05ee7a3fcb6113062621495fee6bff8f11ac1c9">d05ee7a</a> ("patches: next: Drop the hack drm/amd/display patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5cd98761b0bab3bd77f4a10551f8bb758366d238">5cd9876</a> ("patches: tip: Drop mchp-spdiftx patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/53672378db4bc0aa151e5dfe5f96962a062aa236">5367237</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/332513d00c49c64645a860cad8fb47b09c770d14">332513d</a> ("Revert "generator.yml: Disable CONFIG_PSERIES_PLPKS for Fedora's ppc64le configuration"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/dde7b4bbb06e7f8838946011cc450f921c49f5d5">dde7b4b</a> ("patches: mainline: Drop mchp-spdiftx patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/664a45ac211028070450ad20a9930dc39cef183c">664a45a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/59678cbbae1a81d34e7d1cbc970c5fecda40bcbe">59678cb</a> ("generator.yml: Disable x86_64 KCSAN builds for tip of tree LLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d877c5a9aa9bad3059ccda4901b39bf1c035ad79">d877c5a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/011484125709760985028bcdf1e87baadd433741">0114841</a> ("generator.yml: Disable CONFIG_PSERIES_PLPKS for Fedora's ppc64le configuration")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d6ca216f6575d64f1fc8e3a57510e0abf75d6d6e">d6ca216</a> ("README: Update badges for new workflows")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b72b6e455eff8da9f89c37c51783a5296bc792fb">b72b6e4</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9cee08a496868dfed49834310e8580c2a8527188">9cee08a</a> ("generator.yml: Add support for android14-5.15")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/694baf5a1b591a81fd97769abc1fe0f8cc5a0af4">694baf5</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d6f52263e578fb49f03992308a3ff2c7683675b1">d6f5226</a> ("generator.yml: Disable CONFIG_WERROR for hexagon allmodconfig")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ed752084e700ffe5682c544f7a385096ff4b89eb">ed75208</a> ("patches: arm64: Update mchp-spdiftx.c patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9acd7f11394656b1b464a9a4caa5e6f31d1be6ae">9acd7f1</a> ("Revert "patches: arm64: Add a couple more patches to fix warnings"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ee5cf0173dfb54c35d9cf87729ca247f6be82393">ee5cf01</a> ("patches: arm64: Add a couple more patches to fix warnings")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9467b04302a91471aee4c44fa3ae5bc9601940b2">9467b04</a> ("README: Update badges for new workflows")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2a1725b84d2547efca08a0a7374d6d7467246a17">2a1725b</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a300efe62c9dff0959839da1934f733f931f6f04">a300efe</a> ("generator.yml: Add support for LLVM 15")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2c4d66a7fe8006373ba1a60c321aacbb0a023bae">2c4d66a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2eeef6ff6b5dc4c4cebafe818a5822aab0fa34f0">2eeef6f</a> ("generator.yml: Stop disabling CONFIG_WERROR for arm64")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/da35570a871b54d241b090294cb0adc4ffd16d46">da35570</a> ("patches: Add mchp-spdiftx.c to arm64 trees")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/67d2e4dee3cdd8cd5534924481cadbb8648b03f5">67d2e4d</a> ("patches: Add mchp-spdiftx.c to -tip trees")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cb78b287479a8dfbcaa43c6706f0ba3421febd83">cb78b28</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1688240823e7854788dde9395414d91128526c87">1688240</a> ("patches: Add pending -Wbitfield-constant-conversion mchp-spdiftx.c patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7061bdcd18ebf9343ff218a8bad5f3681de19219">7061bdc</a> ("patches: mainline/tip: Drop i915 patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ac9be55c726b203d0da07e205f0a77ca66880410">ac9be55</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/35686434d70c131a19e67acf9a3e1b8d21647aec">3568643</a> ("Revert "generator.yml: Enable CONFIG_PPC_DISABLE_WERROR for powernv_defconfig"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1024d45c0ff580559aec900d1adc34541c7a3f70">1024d45</a> ("patches: mainline: Drop -Wformat patches")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6828e7957ef5051c65c2040a37104650691fb2fd">6828e79</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ea273d888580fee29917e970a777787d653daa10">ea273d8</a> ("generator.yml: Re-enable boot for ppc44x_defconfig")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0c5fd9a97b4c7f3d9ad06e6e0ec2410bf0d81b37">0c5fd9a</a> ("generator.yml: Re-enable x86_64 all{mod,yes}config builds on -next with AOSP LLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9c90630f49207eb89d467018d39ebde2f64bdd70">9c90630</a> ("generator.yml: Remove commented out s390 builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d5bef249a17864717313b54f65b719c192422f5e">d5bef24</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b8af73488587c21f542b5e79cddcac52f129ed93">b8af734</a> ("generator.yml: Upgrade stable anchor to 5.19 branch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/eb2995e0c2ff75e113b10c0a4ac607d842b75a9b">eb2995e</a> ("patches: mainline: Add a couple in-flight -Wformat fixes")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b5573cb47a7b214519d7d59439dfc172daef5e4c">b5573cb</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5ca511d8c93eea164ee243f81e75c47499694c82">5ca511d</a> ("patches: Copy mainline to tip")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5b65a9faab2cf511b97ad4f97722ee78c347a951">5b65a9f</a> ("utils.py: Adjust for new builds.json output with 'tuxsuite plan'")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/205261eeef6a7e14426cd66a07c6e0dcf96ddf50">205261e</a> ("patches: next: Drop rtc patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ce792ce47e5e2a9aeaccdbab266c6d9d11d62b2f">ce792ce</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4ea2e8dc613bb8ddd94d534a331c078505f9d410">4ea2e8d</a> ("generator.yml: Disable ppc44x_defconfig builds on mainline and -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6ed9b50b0ad8f1ab6965e3cac14be46f389fe1ac">6ed9b50</a> ("generator.yml: Enable CONFIG_PPC_DISABLE_WERROR for powernv_defconfig")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/62899be73be4079f157b7b79aca6f66cc1c541f5">62899be</a> ("generator.yml: Fix architecture indentation")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2519a93e3266a0ea5a236e565305767abb07e777">2519a93</a> ("patches: next: Add a patch for a warning in drivers/rtc/rtc-zynqmp.c")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/401f990ef3d2e747f2ff70632bc00869fccdb810">401f990</a> ("patches: mainline/next: Add a hack patch for new warnings in AMDGPU")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/02177f7e329bb942913c6f35106bfd6f0fd6d583">02177f7</a> ("patches: mainline: Add patch for i386_defconfig warning")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b74e01c3e5b02125598a3a730614ee6e0da4f926">b74e01c</a> ("boot-utils: Update for another 'boot-qemu.py' fix")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7cb5f24fda655709c839efe700b058cfdfeac3a7">7cb5f24</a> ("boot-utils: Update to fix 'boot-uml.py'")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/77204edc13b4364cba35802c5daf22fd161f4c1c">77204ed</a> ("ci: Update boot-utils for Python rewrite")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fe49a8b04354f50545dcd87b39dd8e6a14a2caf8">fe49a8b</a> ("README: Fix badges after #389")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/784105248b402ac1678cde1b6cb38ef453addfdf">7841052</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/617494f3c5b276bdca7135a9458c9c724ec856fe">617494f</a> ("ci: Update llvm_tot and LLVM_TOT_VERSION")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/30bf95648bacc8048488782d0b9d283e550d2ecc">30bf956</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8daf93b4a260c72932ee655e3af7ead8a3b97223">8daf93b</a> ("generator.yml: Disable CONFIG_FPE_NWFPE on arm all{mod,yes}config")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/149f8b0988bd21c920805c515aa9fc8bf1f81102">149f8b0</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7048736cab11df7d8d91f34f094f01d916c788f7">7048736</a> ("generator.yml: Stop disabling CONFIG_WERROR on x86_64")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c85da5bcc840118a759ccea23748df46c0232e0c">c85da5b</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/26dff9b80bf7c6d1a430aaea517745f185be2b19">26dff9b</a> ("Revert "ci: Disable ChromeOS jobs again"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6c5400e0eed8c8a7bb56374b47f3f51efbecc2cf">6c5400e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e5c13318fba1141ade79ce0ae288998395577e66">e5c1331</a> ("patches/mainline: Drop RISC-V patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/22dab9a939c8d6ad8b62252fd813a6a974f09285">22dab9a</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/64b5c1dc976fdf72098d659e7a4713991638bd02">64b5c1d</a> ("ci: Disable ChromeOS jobs again")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ff017c18f1915ad1a3e0bdb87d8f7ddc74244398">ff017c1</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ff06444666c0e8af9871626760a985005077792f">ff06444</a> ("patches: Remove -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c55e1e97910930c27c76aabe32c5daaf6432e7cb">c55e1e9</a> ("patches: mainline: Drop watchdog patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c0985bb106a6c65126852a1708ddf4814ca5467f">c0985bb</a> ("patches: mainline: Add a fix for ARCH=arm allmodconfig")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f67e556ecdb0f0db8109189fb2cf943e72c8415e">f67e556</a> ("boot-utils: Update")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/87dbb5fa712237195a7c9502b0e0b59eff5400ba">87dbb5f</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7a1d9cec84556cffcd5163836fd96659bbb74623">7a1d9ce</a> ("Revert "Revert "generator.yml: Add support for ChromeOS""")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/be746607ee67989792d0d9f30ba4ffdbc92cd5a7">be74660</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/40ec857f879bcd65d122947a623c5df1a1bd9fa7">40ec857</a> ("patches: Drop objtool patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1afbdc45c0fe2abdfbf6c48b85b2972d102d4316">1afbdc4</a> ("patches: Drop a RISC-V patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e2fb7955d54df2f24413a83c61081ff209d7c2fd">e2fb795</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9ddc577e9a0f01728f1f53e21f6a6640304c265d">9ddc577</a> ("patches: Add RISC-V inline asm fixes to mainline and -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f51a5b9f0134bd2d23b160cf35f4e36094f69e46">f51a5b9</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fd538799ae5ec5ec5b0fda26d2a805f8bb8df1b0">fd53879</a> ("generate_workflow.py: Bump GitHub Actions versions")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/40d3690330da9604a0c1abfc9709c05982a9801b">40d3690</a> ("patches: 5.15: Fix objtool patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4a2306855cfbb3168d7b049a9aedbd633d4b79d6">4a23068</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f379b9853af2e78417a432766a150679910b7352">f379b98</a> ("generator.yml: Apply 0060c1341cf04124a580f0763a8d6b943283828d to mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1cc632e7d88068b35e0a52d87dbcb11ed1237b44">1cc632e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/38df0d31c868869500da083422c0f31541d99cc5">38df0d3</a> ("generator.yml: Upgrade stable anchor to 5.18 branch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1ab781ec376f82c22cb2b58f0e27189fb9edc8f2">1ab781e</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/06365e80f9dce5117a8d625a8978cb8d88c0c890">06365e8</a> ("patches: Add an objtool patch for 5.15 and latest stable")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a1cea169c6022adeee721cd72d4cda4963312840">a1cea16</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4eca7d165b8d7c5c73d9737055a0e399b3bfe98c">4eca7d1</a> ("generator.yml: Use the integrated assembler with s390 on -next and mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fe24c46691041cc006d688582ff391575e7cbcdf">fe24c46</a> ("generator.yml: Disable s390 builds for LLVM 13 on -next and mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/211e2b2dc3f70d8aac0e94ee6be175264e90f685">211e2b2</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3f90aa4bada421bd6191c03950c174d823646fd8">3f90aa4</a> ("Revert "generator.yml: Add support for ChromeOS"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/010f393344fe61d36e8b6f026ea4c90ec0d86b91">010f393</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cbb5159c8331e72961ab0498dc90096effe39152">cbb5159</a> ("generate_{tuxsuite,workflow}.py: Consider CrOS configs defconfigs")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4d61fbaf1b2f75bbe23ac5575b9412b1a02a759f">4d61fba</a> ("README: Update badges")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0666c843943f28b4bfb0b7ab73ef35b9504dc6a2">0666c84</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/02ff1b5302ca516a9971f52ea2ffe792a4c10ca0">02ff1b5</a> ("utils.py: Support ChromeOS configs in get_cbl_name()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f2ef7cab183db66ef6ce39b9534401f97cfc646d">f2ef7ca</a> ("generator.yml: Add support for ChromeOS")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a64539ddb722d69d0a4fd48ff3a29355ec2d5c99">a64539d</a> ("boot-utils: Upgrade for implementation defined pointer authentication")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/bdc9611e04cbb548396ca700b42834d93df39a2f">bdc9611</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a4f0fed3c772cca78b043f148f3a8544326d78d7">a4f0fed</a> ("generator.yml: Build powerpc64le configs with LLVM=1 with LLVM 14")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1d17bdd5cd0ef6a35bf6fbffff188cd2629e8af8">1d17bdd</a> ("github: workflows: Fix clang-version")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0296cdb1c6373230cabda951885e62d9d827d276">0296cdb</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0060c1341cf04124a580f0763a8d6b943283828d">0060c13</a> ("generator.yml: Disable arm64 CFI builds on -next for clang-{12,13}")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/543eb60a6fdb97fcb2b45bc5c0361ce741f0b1ed">543eb60</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/70e65ee5e60ffde80d4b2a7e69ed134dc77a3f24">70e65ee</a> ("generator.yml: Build ARCH=hexagon allmodconfig on linux-5.15.y")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e095eac0e5d5d52e6a94cd8ce382ae6820155503">e095eac</a> ("scripts/check-patches.sh: Ignore patch files check if patches folder does not exist")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2ed6be99dd88ef459baf0c6191d04b93e7d7e376">2ed6be9</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/65811872171176301f03fd72e217cb9927a37d19">6581187</a> ("patches: mainline: Remove btrfs patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2e612b5168dd66409fd48d0b1a24d9f97a6614cd">2e612b5</a> ("ci: Regenerate GitHub Actions workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6e815e6c2977b42f35e1c7eaf9b4308f4e904044">6e815e6</a> ("generate_workflow.py: Add '--ipc=host' to remove 'noexec' from /dev/shm")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e974de42d50c663035a248aa834901bfc7387c7f">e974de4</a> ("check_logs.py: Fix name of variable in run_boot()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2952bc392f67705aed4ca52b4ceab362538ad01c">2952bc3</a> ("patches: mainline: Drop KVM patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a7538321094c19cf0087ccb3f79f8f20dd423a40">a753832</a> ("patches/mainline: Add btrfs patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6f565287b7a75521499e06a5ffa98bd13bbc111e">6f56528</a> ("check_logs.py: Set execute bit for UML images before running 'boot-uml.sh'")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c3930e38c2d97c6efb50e8e18d668112d927b7cb">c3930e3</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4deee3f03f3f6b320c2508397dc321fde9d62fd0">4deee3f</a> ("generator.yml: Enable ARCH=um builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c4e2a773357d0e68d4aabaaf75459cbda649c3ec">c4e2a77</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4b87e4aead69ecc6f73827b4fb6e9e58261b12e3">4b87e4a</a> ("patches: next: Drop UML patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0bbaed95ff61451d1d11b441ae7a74e8ccfffc66">0bbaed9</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/61dbc4aea74d5c704c015d5227a562ea4530a68c">61dbc4a</a> ("generator.yml: Remove Fedora i686 builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/79f4f512535c765bd24360570ceede68b18f7b67">79f4f51</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2d75a2dee7198d70b7491b09c5d06eef3a129f2a">2d75a2d</a> ("generator.yml: Apply a9d8571b91a5b19cd6a48780ed2231938a25283f to mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8876748f3799e910fb368221cab94176a175ee7d">8876748</a> ("patches: mainline: Drop PowerPC patches")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/092296b7816d794d3636b5f9c9113f629cd169c9">092296b</a> ("patches: mainline: Add -Wimplicit-fallthrough fix for KVM")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5615ba80891a4092ca7f737af7ef8b9866d3b41c">5615ba8</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a9d8571b91a5b19cd6a48780ed2231938a25283f">a9d8571</a> ("generator.yml: Disable ARCH=arm allyesconfig for old LLVM versions on -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c80e504d03cf86813d932caf51cad6b9c01095d8">c80e504</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c422ff44c37b60cab2435e4d81672c99cc840f5f">c422ff4</a> ("generator.yml: Disable x86_64 all{mod,yes}config for AOSP LLVM on -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/32458240b9cbe89176bc5404e368c0fa5c0f6938">3245824</a> ("generator.yml: Enable disabled ARM builds for AOSP LLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/48edb0611e1cd4a3f01d64a007bbdae8827884b8">48edb06</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6cd5336f7a571ffb47360f822a6bb1d2ff9e8775">6cd5336</a> ("generator.yml: Disable ARCH=um builds for now")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/10031321afa57c2caff7601bdcc9629ff851b7fd">1003132</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3881b88c35f0ce6edde679ec08c31978875e7cfd">3881b88</a> ("generator.yml: Boot test ARCH=um kernels")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e4d38d9ec3fa8765527906764fc0e5e4eb6ec003">e4d38d9</a> ("patches: next: Add patch to fix booting ARCH=um")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/97a7d64079e3caeff20c4b391391d8b9f9156f74">97a7d64</a> ("check_logs.py: Add support for boot-uml.sh")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d045522881213d206b9a21593c2f07be508a8116">d045522</a> ("utils.py: Add support for ARCH=um to get_image_name()")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8e3cc283cca09d67b921d9658b576f24e3f107af">8e3cc28</a> ("boot-utils: Update for boot-uml.sh")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d8e2797eeb7d3d8c88664988178be1c180b2f10b">d8e2797</a> ("generator.yml: Add ARCH=um builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2fb817f63c31dabe469a89f13da0dd86dac5b79b">2fb817f</a> ("generator.yml: Add information for ARCH=um builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2d1133971c3bbc070be887b6b7b087567feb609f">2d11339</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d071029b76105e9f1c6c46c581fdb862c37dbda8">d071029</a> ("generator.yml: Undo 4eec5c448d09646659806afdd42b080a7002620f for stable")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/119f2edc2f0e8bbc210e4ac870ed9bf0cb0979fd">119f2ed</a> ("README.md: Add badges for latest stable release")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/41a2523cc3eac92bff105a51052c9be015c05444">41a2523</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/59366d50105351065b920558aa22f96129e82bd4">59366d5</a> ("generator.yml: Add builds for latest stable release")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a3afcfeefcadfdf94ec198fcb6500d452a246d28">a3afcfe</a> ("generator.yml: Add anchors for latest stable release")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7529e585279a30149e09a08e1cf800c50ae85400">7529e58</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cc99069693f9816c0d52efed2478262d9acb9a46">cc99069</a> ("patches: Drop -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/92079452a9c821de94251a1279ee419931b929b3">9207945</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/131a2136c0331d7b7ef8b19ddbf9b64c524a45ee">131a213</a> ("patches: Drop arm64-fixes")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/510aca380d423eb143382288dbf6cbf9b7873a96">510aca3</a> ("boot-utils: Update to latest main branch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/983b029aada41c32b091ad2d18da336c7a197b98">983b029</a> ("check_logs.py: Use proper syntax for removing problem matcher")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/742c0396e66cc6fa72538355f71bee162a6c4694">742c039</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4eec5c448d09646659806afdd42b080a7002620f">4eec5c4</a> ("generator.yml: Build powerpc64le configs with LLVM=1 with LLVM ToT")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5954e74dcd501c195bdbbede98ed76aaf86c80ab">5954e74</a> ("patches: mainline: Add patch to drop stabs annotations")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/c6bbee742bfc77f787bc9abcd2acb5fcc3baac0e">c6bbee7</a> ("patches: mainline: Add fix for booting ppc64le kernels linked with ld.lld")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/49b3aa04912b8961e3b1959f9b82b7a49230b55c">49b3aa0</a> ("patches: next: Drop KVM patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/47da3fcb2a870d30101d4f1ec7ede29faa99df0c">47da3fc</a> ("Revert "Merge pull request #321 from nathanchance/ppc64le-llvm-ias"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/27363b083ad6c296fd9760ac780b27ffb109bcb0">27363b0</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/d84502ad14ed8833073b17202bc3232f64b3b5df">d84502a</a> ("workflows: Bump actions/checkout to v3")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6096efd8909275c740759d55a3bddc054f375b71">6096efd</a> ("generate_workflow.py: Bump actions/checkout to v3")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3f7574a6e59db3904a541b14cbba9b7c1fb12824">3f7574a</a> ("README.md: Update workflow badges with new alternative text")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9a40358bf62313f08da8ff8aa7d1438ec440b36a">9a40358</a> ("scripts/markdown-badges.sh: Make badge alternative text more descriptive")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/593ecf618e0e2f1d21d3060c490c1c1f9e37a224">593ecf6</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b574dab4cf100f5a86db1d2cf585b4aff1ecd659">b574dab</a> ("generator.yml: Build powerpc64le configs with LLVM=1 with LLVM ToT")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6489d0ceaa3d02beb8d39bf8755f2ad7eefb7187">6489d0c</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/42254178bcb6b950cc959e9e0ec80687a3b7d9f9">4225417</a> ("patches: Remove android-4.14")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/57763f42a58ed12cae45c1d197611a829536ed27">57763f4</a> ("patches: next: Drop drm patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/90ce82e9734228b36ebf408aa82514ce514ea1d3">90ce82e</a> ("patches: next: Replace warning hiding patch with proper fix")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8a223f3725406089f8868273c2a931f452e088dd">8a223f3</a> ("ci: Update messages and documentation for scripts move")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cf6640b9b8ee4f104cc80c83d1d07c5a7d3d20f1">cf6640b</a> ("check_logs.py: Fix up script call after scripts move")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/56020605bd4677be789220f801adb0fc9cd1d723">5602060</a> ("ci: Regenerate GitHub Actions workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1ae274616ca5f774545a8fcf5fc2cb0f3190a08b">1ae2746</a> ("generator.yml: Build tip and arm64 trees at 6pm UTC")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/cbbf4051ebc0c68ad3b3a3f542540c8b2dd05ba7">cbbf405</a> ("generator.yml: Build Android trees at 6am UTC")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/963754b2661526488d4f1ebc332f055f78418f1d">963754b</a> ("generator.yml: Build stable trees at noon UTC")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3d678f0bf2655b80bf7044516e413bda73f214e7">3d678f0</a> ("generator.yml: Test certain Linux + clang combinations less often")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/745e016675fef1131f209ed49fba3f6835075644">745e016</a> ("generator.yml: Rename daily anchor to daily_midnight")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/aca6f15bf8c387bd045fccf297c8afcdebad9c62">aca6f15</a> ("ci: Separate cron schedule by LLVM version")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/954e4d58f1b91a3d3758fbdbf0c10ba9976ca7ca">954e4d5</a> ("generator.yml: Hoist llvm_version key into llvm_versions entries")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/476fcd176a08831eb6cbf82004333f54c6a426f0">476fcd1</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1b22e54e3baa39d66c3d49fca364cb15a299a362">1b22e54</a> ("patches: mainline: Drop netfilter patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/89b3dd21706d0038a8d097269ae968a02787d15a">89b3dd2</a> ("patches: next: Add a patch to fix build with KVM_WERROR")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/317e4a1e000a4af6de271c51680c3bd404bb3c0a">317e4a1</a> ("patches: next: Drop net/netfilter/xt_socket.c patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/20330590d8f8349344dda96ad324beb4d13564cf">2033059</a> ("ci: Regenerate GitHub Actions workflow and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/208c6931c4cb3c786c634cd52d55629a584855bb">208c693</a> ("patches: next: Add a hack patch to hide a warning on Hexagon")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b948bdf3866df96d13cd56680e75c3d017e736ad">b948bdf</a> ("generator.yml: Disable certain ARM builds with Android LLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/343b6dff1db02e75ff1e9fd7f6f783fad2310fd0">343b6df</a> ("patches: next: Add patch for drm_panel_dp_aux_backlight() link failure")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5412a2f8bd8429ffefa39214464c1f5e7ac56b3f">5412a2f</a> ("patches: mainline/next: Add patch for nf_defrag_ipv6_disable() error")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/95345f0cdf6eaa92287519af4d308cdd229ef64b">95345f0</a> ("patches: android-4.14: Revert -O2 linker patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1c6961d6324d258981e0f6b968580801001c8f39">1c6961d</a> ("Move shell scripts to scripts/")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/4005e87083b69cece63448736c64b9a0d413b49c">4005e87</a> ("README.md: Update badges for clang-14 workflows")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/46850c55a072667adf284688d8741a7e574fc148">46850c5</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7188cf1afe4e23d09609886150cff124808c359d">7188cf1</a> ("generator.yml: Add LLVM 13 builds and update llvm_latest to 14")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/72ef030cbe66f712e6978bfa0f38c5b002d9f7fa">72ef030</a> ("README.md: Update badges with markdown-badges.sh")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/880fe0f1a703b5768e3fd273ab192634ee8abeb2">880fe0f</a> ("markdown-badges.sh: Fix badge syntax")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/172353105ae677b357c0b78ba9419788a6ed5ffc">1723531</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/21f7ff229d10ec90be9afd0aff0633013c7c0f0e">21f7ff2</a> ("generate_workflow.py: Include toolchain version in workflow name")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/89bcbbbda626ecea2a69de89d844f59af8bda2b3">89bcbbb</a> ("README.md: Update badges for new workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9a7c0c9a81c6380592ade5e4d83f5674fed758be">9a7c0c9</a> ("ci: Regenerate GitHub Actions and TuxSuite files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3581467cee613a1b54c92cfa605de0f8f03cab53">3581467</a> ("ci: Split up generated files by tree and toolchain version")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6e71ae1ab5e2e18381655b5f20ac3e3e032e2a79">6e71ae1</a> ("ci: Write to GitHub Actions and TuxSuite files directly, rather than stdout")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/57892768e2c325b9ab3aa1b33a03aec296ec4d92">5789276</a> ("ci: Read from generator.yml rather than stdin")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7e342700201b1db3d0caea8dcf7de1290f496f4b">7e34270</a> ("README: Update badge order from markdown-badges.sh")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ba47a9d57694221fc0c0a5336bae53ddd385f99d">ba47a9d</a> ("markdown-badges.sh: Initial revision")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2cb780d35ba7726032f232315f13d7840666d21a">2cb780d</a> ("generate.sh: Add a '--check' mode")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a9e88e34ec8d1a4ceea733958033ddc4552dfd0a">a9e88e3</a> ("github: Move matrix-check.yml and patch-check.yml into lint.yml")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/be4199a628ff076941435652bcc5a016795bed93">be4199a</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/04e43751a4b284fb3351085fcf35de530dfeab96">04e4375</a> ("generator.yml: Enable DEBUG_INFO_BTF for Android and Fedora configs")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7251390297c481913e4a43c94e0aa2af9e9bc2b1">7251390</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ee899d0091639ac2ef5e1399df81a56e13e54f9c">ee899d0</a> ("patches: android-mainline: Drop CFI patch")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1cf7ff4bd4e8c90041da81d8ba2bf6fa3008fe1f">1cf7ff4</a> ("ci: Remove 4.4 TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ee9ac3944b0d6a03570d096121b9be366447dd47">ee9ac39</a> ("generator.yml: Remove 4.4 builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0910713ce0d1097c964df29ce8ce617d33314106">0910713</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/583f4c15eb92f9d338d7cef95bd55b3536dbc403">583f4c1</a> ("generator.yml: Update llvm_tot")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/dd2c9c7cd297dd5aa3914db931bdc09767714e87">dd2c9c7</a> ("generate.sh: Update LLVM_TOT_VERSION before running Python scripts")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/45e4f27cb1b21d487a1e138f24c7a1832b57d589">45e4f27</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3546b407df6f0ee76d1beeb70efe7f675fd42f00">3546b40</a> ("patches: Remove mainline series")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0005c17b06acbb44efec7f8765c4dd1986c6924f">0005c17</a> ("patches: Remove max20086-regulator.c patch from mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8d0800cc0af914a2ca75b8545896ac38e7fa3533">8d0800c</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/5d80d870222cd1861052da6075a777daea461fb6">5d80d87</a> ("generator.yml: Fix CONFIG_DEBUG_INFO_BTF=n for Fedora configs")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/a6de49d98a3133251d7f381e0202744cd19c2b9d">a6de49d</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/953e3a35ec7aac8ba424f64bac015865d6ffc03a">953e3a3</a> ("generator.yml: Disable CONFIG_DEBUG_INFO_BTF for Fedora configs")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ccf2b2fb0c7a9013fef7d0556504dd6b6afea2bc">ccf2b2f</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/89b3c45750f8fcc6bbf2ea193ba47dd9eda0dee0">89b3c45</a> ("patches: Add blake2s CFI workaround patch for android-mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/97614e68a4e255eb10c04ae19969cc7787337d8e">97614e6</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ab5b685d2e01a34d73e4a135d86887309fe09910">ab5b685</a> ("patches: Add pinctl-thunderbay.c patches to arm64-fixes")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/87c5fb0018892cbaeafdd3140ee0f34f980a79aa">87c5fb0</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/06ac1aabde4e8b9b364b72f42c3c9f4614302bfc">06ac1aa</a> ("generator.yml: Add android13-5.15 builds")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/0e87536f1575aca7cb15ab37f5adb0c5a1fb91dc">0e87536</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/21107c08f090413113972951414b26c136ab311a">21107c0</a> ("generator.yml: Disable CONFIG_DEBUG_INFO_BTF for gki_defconfig")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b62e273e966906d34a22f2c9b3acaf3e6c275573">b62e273</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f2c13729d9ccbb993abde8397b78244dbd1b1319">f2c1372</a> ("generator.yml: Enable x86_64 GCOV build with AOSP LLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/dba2158a9fc2d417fdbd2b417c60b51b93d9219e">dba2158</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/09d56faffed973e86619d9eac804fd2a3db7bd32">09d56fa</a> ("generator.yml: Enable arm64 big endian build on -next with AOSP LLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/931d7f05550be5ceef9df7467823e51b8934a97d">931d7f0</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1d78fba39c450ff20d84d643326d8e5eddbacb05">1d78fba</a> ("patches: Drop android12-5.10")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/48b1e530c968ca3fd516a08ea2353d9ad0ff7c18">48b1e53</a> ("patches: mainline: Add a patch for drivers/regulator/max20086-regulator.c")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ec557b2ccd215744edfeeb853d7d7f920cb44c7b">ec557b2</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e46d6ae465a6851b98dcfc69a4b3639145b959ba">e46d6ae</a> ("patches: Remove tip")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/bff9efed6fca2a5cbd28f0d300f718b33893fcd9">bff9efe</a> ("Revert "patches/mainline: Add a SCSI patch from -next"")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/ed03ce962d2041e686a90828d91d71838b395a25">ed03ce9</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/3b585a00361843992074575344a090e818a65022">3b585a0</a> ("patches: Remove -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9f2e252a57fd910e1adc561d524ffd4b47522151">9f2e252</a> ("patches/mainline: Add a SCSI patch from -next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/b6400a0e6ad28fed4373f28a10c7c8c5b6f1e103">b6400a0</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/e8bce11b2dfa3f45345cc638407f1ce52e03beed">e8bce11</a> ("generator.yml: Drop clang-10 builds on mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9bfefd76707fded2ce9df5dbe091017450f05851">9bfefd7</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/bf629ee3a81273015e9b5fdc5304224ea5224457">bf629ee</a> ("check-patches.sh: Check for patches but no '--patch-series'")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2b3ef49a3897035d986868ac0c0cb7d7914eab6c">2b3ef49</a> ("patches: Add a series to fix ARCH=arm64 with mainline and next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8b06b10e5ecf096cc01d7084f4450656ba800c4f">8b06b10</a> ("Revert #274")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9df6888a184ccaad331300442ac80b54d1a99a17">9df6888</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/fc946697e3abc69999f297eb003607ca13bce3d9">fc94669</a> ("patches: Apply hack patch to fix the build on android-mainline")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/f82da84ed92e0a0320ce5e4a68c1738013908117">f82da84</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/7f679ba564d72c1f2abc0fdd43715bb3c12e50fc">7f679ba</a> ("patches: Drop 5.15 and 5.10")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/9aa436120a803a39b66199cf15dfa99690a64f2e">9aa4361</a> ("ci: Regenerate TuxSuite and workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/2422dbd4c2f1a7a6abf62cf368d6156cd126b012">2422dbd</a> ("patches: Drop mainline and next")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/1492aca30dab59ea2b11390629140be50563a821">1492aca</a> ("patches: android12-5.10: Add patch to fix Thumb2 kernel boot on newer QEMU")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/6fd8ef04697e036c5ed38d96a9b0c6c1e3946a07">6fd8ef0</a> ("README: Remove lto-cfi{,-tip} badges")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/519183c24fa5ce4de95ef995e14b20e05ace5365">519183c</a> ("ci: Remove CFI workflow files")<br/>
<a href="https://github.com/ClangBuiltLinux/continuous-integration2/commit/8c6643d7a1e1e89cce061d6ea33fdd3765b38bfe">8c6643d</a> ("generator.yml: Remove CFI tree builds")<br/>
</code></p>
</details>

<details>
<summary>tc-build (<a href="https://github.com/ClangBuiltLinux/tc-build/commits/main?author=nathanchance">GitHub</a>)</summary>
<p><code><a href="https://github.com/ClangBuiltLinux/tc-build/commit/ceaeaacb02f85cefb7f3d5f43224eabd61165616">ceaeaac</a> ("build-binutils.py: Disable internationalization unconditionally")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/ae85dae7a82e12f92c3e716ca7f6ea73b7fbd4d3">ae85dae</a> ("build-binutils.py: Disable gprofng on musl-based distros")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/9c908bb6bff8ef44683a7f884e1a78796a525f51">9c908bb</a> ("build-binutils.py: Unwrap option lists and use unpacking for appending")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/6ae83728a8d1ba5b0c1969c35aaef06ae37c4209">6ae8372</a> ("build-llvm.py: Disable COMPILER_RT_BUILD_GWP_ASAN when execinfo.h is not present")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/30a0244b22e7e6105ebd497c95900bf94669f4b6">30a0244</a> ("build-llvm.py: Fix using relative paths for folders")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/219c476c04f31031f932e9ef9539167e7401dc1e">219c476</a> ("utils.py: Make create_gitignore() even simpler")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/f2c4182149a35c5c48f2f67578fa4dd357e5c6cd">f2c4182</a> ("build-binutils.py: Dynamically figure out good hash")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/2b448da9a29884bdb5d27596530fbccf4dbf4c86">2b448da</a> ("tc-build: Stop building the gold plugin")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/977cde6dcdef8a1cdbea65ca37f99a29cfc29420">977cde6</a> ("workflows: lint: Use updated Python linting workflow")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/457d148c84ff56494c93a261dbd3e81b8ce77f2f">457d148</a> ("Refactor some subprocess.run() calls to use variable for command")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/522c6bfc89db17e524caedc09a58442280f272cb">522c6bf</a> ("build-llvm.py: Add encoding parameter to open() in do_bolt()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b1f4ad742315289e775a765a1ca1ce63b8a7c9f1">b1f4ad7</a> ("build-llvm.py: Remove unnecessary self assignment")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/356b8e4af0b29c5269860077a91e4858672ea590">356b8e4</a> ("build-llvm.py: Avoid clashing function name")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1a038ede910bb2dd7d544404b22609db08983694">1a038ed</a> ("build-llvm.py: Use .items() and list comprehension in invoke_cmake()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/8144211b57d2679009676796fe60622e664d802c">8144211</a> ("build-llvm.py: Simplify get_final_stage()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/f051eb9725adcfe5db31b75dbbe1ff8229b3d38e">f051eb9</a> ("build-llvm.py: Use 'check=True' in ref_exists()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1ef55f41b9705562bb95ead44a2c6f7c84d5b05e">1ef55f4</a> ("build-llvm.py: Use request.urlopen() as a context manager")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1135eb7b112bfc6fa4a1b676150cd2258b6a7ddb">1135eb7</a> ("build-llvm.py: Use sys.exit() instead of exit()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/85054c1b6fafe4111be2693bb64dc484b02c40e8">85054c1</a> ("build-llvm.py: Rewrite linker_test()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b890dc0b9d80fe4ce140b6782ae25cb956d465f1">b890dc0</a> ("build-llvm.py: Adjust imports for pylint")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/5c0a71f0a493d8fee6f49b8da294f717398e36b9">5c0a71f</a> ("utils.py: Clean up remaing variable warnings from pylint")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/465b0c4a18842aa94ff95c8b091c1f8cc4935971">465b0c4</a> ("build-binutils.py: Remove unnecessary elif after return")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/aab3bfe1443f043b1b2b3e8fa2801fb5939e981b">aab3bfe</a> ("Silence pylint invalid warning for module names")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b6dcc8bcee7ceb1c37f4da838904140bf90f7fb2">b6dcc8b</a> ("utils.py: Remove unused pathlib import")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/ab837554ce99a3b531e62276378e0906447b930e">ab83755</a> ("build-llvm.py: Fix bare except in can_use_perf()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1a2ddd49edfb017e94a5d76c5b54b57b9dd874a7">1a2ddd4</a> ("build-llvm.py: Fix 'in' expression in stage_specific_cmake_defines()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b62fedfad3fe9eb8703704e6ab8fd7d81f8644a4">b62fedf</a> ("build-llvm.py: Use raw string in versioned_binaries()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/a91199521d9f41693eb01750bf01477823ebccd2">a911995</a> ("build-llvm.py: Use f-strings in more places")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/67f18bf54b6be5882571cf1d68ec9d39fdbdcd9e">67f18bf</a> ("build-llvm.py: Eliminate os.path usage")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/c49e8b4f3fe83aa48fe025303a64c7237cdc158e">c49e8b4</a> ("Eliminate all calls to as_posix()")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/c100cde1e3009f71a5b5e06a45f020f7d9b1f599">c100cde</a> ("Switch to f-strings")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/47839bfb3348aa4c05cea33c9d9f33110a10a7fe">47839bf</a> ("build-binutils.py: Add ability to skip installing")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/c1cbd224f2b145f5e5e572ba0c12ac41f0d4014e">c1cbd22</a> ("build-binutils.py: Allow alternative binutils source locations")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/9a2ee8766ce49fdb99786ec9d4c828c5a3d0128d">9a2ee87</a> ("build-binutils.py: Use pathlib exclusively")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/f515296304499c93a16ce281502efaa6ca986413">f515296</a> ("build-llvm.py: Update known good revision")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/a55171735c2a5301d7a29060708e7b0de2c3bcec">a551717</a> ("kernel: Update to Linux 6.0")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/94737df09a295d86d48450e555b1f07ebe48e4b7">94737df</a> ("kernel: Add patch to fix ARCH=arm allmodconfig with latest LLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/ca7cf65cff302025b760a24e58d0d311f070a49d">ca7cf65</a> ("kernel/build.sh: ppc64le and s390 can use the integrated assembler")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/7bca2a27ba2a9dce66aa7da9ff1f7add28c8f3ea">7bca2a2</a> ("kernel/build.sh: s390 cannot be built with LLVM < 14.0.0")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/eecd43cc7780c4889e9c2c4207c4a94ff53d711f">eecd43c</a> ("kernel/build.sh: Make llvm_version a global variable")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/155311eeb8bcc68f71e29a4404e57df5f9caafa5">155311e</a> ("kernel/build.sh: Use pmac32_defconfig for 32-bit PowerPC builds")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/0807d275532dc64a210a014d38e18df88a4997cc">0807d27</a> ("build-llvm.py: Adjust BOLT + PGO warning for upstream fix")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/82fa6d29e8c03ed7845b07899825542eea7be94f">82fa6d2</a> ("build-llvm.py: Fix 'LLVM source location does not exist' error message")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/390e4fe72a46c5f77a897cbaad3082f5b5d8fe14">390e4fe</a> ("tc-build: Flush print commands to ensure output is properly ordered")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/64206bec988fd3c86790bd141185971639b3f88f">64206be</a> ("utils.py: Update binutils to 2.39")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/0c73e12933eaf9658375c10a51a6f322f4342a8c">0c73e12</a> ("kernel/build.sh: Include kernel version in "building kernels" header")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/e6a5f02bbe18436c3c71a1bf5f08c8af70c8c65a">e6a5f02</a> ("kernel/build.sh: Fix header")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/f7c343d75306a8279f18579698a1e28d5dde4754">f7c343d</a> ("ci.sh: Use release/14.x for testing")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/9d7b675ee9a6d51f436936ecd1258557a2c182eb">9d7b675</a> ("kernel/build.sh: Support all*config for Hexagon, RISC-V, and s390")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/27aea0a34658dae9f214215e0cd91ae2428f5d07">27aea0a</a> ("kernel/build.sh: Update style")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/6f0c46a32dc2f86afda4503b2c0e15dced80c561">6f0c46a</a> ("ci.sh: Update style")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/17f8f1f0343702bebeeb58d73bb3fb7cdf42c035">17f8f1f</a> ("build-llvm.py: Add LLVM_DEFAULT_TARGET_TRIPLE when possible")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/3ac781a6ef8a991bfa8612c382a60e792bc70f66">3ac781a</a> ("build-llvm.py: Update known good revision")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/0347e9a97821d58495af960948ff82504725b659">0347e9a</a> ("kernel: Update to Linux 5.18")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/3466af887bf05a51cc756bed0ed0d899a6c13a21">3466af8</a> ("build-llvm.py: Introduce "slim" PGO")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/1b2cc135dabb9a9b308b97f7796eeb9633c948e1">1b2cc13</a> ("build-llvm.py: Add a note about LLVM commit 7d7771f34d14 for BOLT")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b8ef75cc05e67f220dd5ec50d98c65ac633ea743">b8ef75c</a> ("tc-build: Initial BOLT support")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/a68e42e6404279df64a444a6861dc2008529cf0c">a68e42e</a> ("build-llvm.py: Dynamically link the instrumented stage against libLLVM")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/42a879eb2f22af6d1b85983c12fac68b2e9a5e03">42a879e</a> ("build-llvm.py: Update known good revision")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/b2b28addf81ee7176b9c7e50ee019078748ca438">b2b28ad</a> ("kernel: Update to Linux 5.17")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/a2500dbcf44289153ca9baf247363cab8f59c502">a2500db</a> ("workflows: Bump actions/checkout to v3")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/d8e519692ad67bbf7f8af6ec4267cb0f270f9f47">d8e5196</a> ("build-llvm.py: Update known good revision")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/22c3211620dcfbcc1157f0b7d335a16e27887f74">22c3211</a> ("kernel: Update to Linux 5.16")<br/>
<a href="https://github.com/ClangBuiltLinux/tc-build/commit/42bd8c3562fb5ae0d3f6867ba8fa1a4286a963d2">42bd8c3</a> ("build-llvm.py: Appease new yapf")<br/>
</code></p>
</details>

<details>
<summary>TuxMake</summary>
<p><code><a href="https://gitlab.com/Linaro/tuxmake/-/commit/969446df28311c9d15f227f2fd0a12ab1463457f">969446d</a> ("docker: clang-android: Update to r458507 (15.0.1)")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/6a573a6b74b0c831a32dda16ca5b0fa076ae207d">6a573a6</a> (".gitlab-ci.yml: Wire up Arch Linux integration tests")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/d21087b041f0878578fc331f7da470f412f0e5dc">d21087b</a> ("test/integration: Look for Arch Linux's shunit2")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/f9b1fa9371bbfe5fa2af79d0ea945033a3c3bc9c">f9b1fa9</a> ("ci: Wire up Arch Linux package building")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/7090583eab63d85004e2547362cef6140bd6d486">7090583</a> ("fakelinux: Don't let CFLAGS leak in from environment")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/976972c7c63f12a32c7a302f53fa7ca2c008ac8d">976972c</a> ("metadata: os: Fallback to /usr/lib/os-release")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/1da18760092f308e315698cb056e9166521c143f">1da1876</a> ("docker: clang-android: Update to r450784e (14.0.7)")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/2235eb57eaabb07c0dde36e76818f45978cfdc73">2235eb5</a> ("docker: clang-android: Update to r450784d (14.0.6)")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/b8f7902e0594772e469ec2143856e78fb02d415a">b8f7902</a> ("docker: clang-android: Update to r450784b (14.0.4)")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/a9cdf0326cd61552fcb4e2929aae03adb01bc670">a9cdf03</a> ("docker: clang-android: Update to r450784")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/99d0ac958b8ea968d589b41959c6a5b3d765bbfe">99d0ac9</a> ("docker: clang-android: Update to r445002")<br/>
<a href="https://gitlab.com/Linaro/tuxmake/-/commit/80e373b7a225b962ffac31885a4f9b023edeeb28">80e373b</a> ("docker: clang-android: Update to r437112b")<br/>
</code></p>
</details>



## Behind the scenes

There are always things that require time but do not always show tangible results. There are three things that I think fall under this category:

- __Issue tracker management:__ Keeping a clean and accurate issue tracker is critical for few reason.
  1. It gives a good high level overview of the "health" of the project. We want our issue tracker to be an accurate representation of how much help we need (since we always need it...)
  2. It helps assign priority to certain issues. If we have a lot of open but resolved issues, it can be hard to decide what needs to be worked on next.
  3. The issue tracker is a wonderful historical reference. We use the issue tracker to keep track of mailing list posts and such so it is important that those links are as acccurate as possible and have as much information as possible in case we have to look back five years later to wonder why we did something the way that we did.
- __Mailing list reading:__ We are not Cc'd on every issue related to `clang`, even though sometimes it is the compiler's problem or a known difference between the toolchains that we have already figured out. By monitoring the mailing list for certain phrases, we can provide assistance without being initially notified, [which developers do appreciate](https://lore.kernel.org/YtsY5xwmlQ6kFtUz@google.com/).
- __Hardware testing:__ Every linux-next release, I built and boot kernels on a variety of hardware to test for issues, as some problems only show up on bare metal or with a full distribution. I now have a script to drive a full distribution QEMU on a variety of architectures to make some of that testing and debugging easier but bare metal is always an important testing target, since that is where the kernel will run the majority of the time.


## Special thanks

Special thanks to [Google](https://www.google.com/) and [the Linux Foundation](https://linuxfoundation.org/) for [sponsoring my work](https://linuxfoundation.org/en/press-release/google-funds-linux-kernel-developers-to-focus-exclusively-on-security/). I am in a very fortunate position thanks to the work of many great and suppportive folks at those organizations and I look forward to continuing to contribute under this umbrella for 2023!

To view individual monthly reports, click on one of the links below:

- [January 2022](/posts/january-2022-cbl-work/)
- [February 2022](/posts/february-2022-cbl-work/)
- [March 2022](/posts/march-2022-cbl-work/)
- [April 2022](/posts/april-2022-cbl-work/)
- [May 2022](/posts/may-2022-cbl-work/)
- [June 2022](/posts/june-2022-cbl-work/)
- [July 2022](/posts/july-2022-cbl-work/)
- [August 2022](/posts/august-2022-cbl-work/)
- [September 2022](/posts/september-2022-cbl-work/)
- [October 2022](/posts/october-2022-cbl-work/)
- [November 2022](/posts/november-2022-cbl-work/)
- [December 2022](/posts/december-2022-cbl-work/)
