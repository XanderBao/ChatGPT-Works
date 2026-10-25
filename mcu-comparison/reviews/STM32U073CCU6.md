# STM32U073CCU6 独立审核记录

第二阶段审核由不同于原分析者的 agent 执行。已逐项检查 34 项，直接核对用户上传 DS14548 Rev 2 (2024-03) 的印刷页和官方精确料号 XML/CMSIS/Zephyr 文件；已修订 analysis/STM32U073CCU6.md 和 .json。JSON/Markdown 的 34 行结论一致性检查通过。

## 实质修正

1. **ADC 外部输入数从 16 改为 10。** DS Table 1 p.5 将 Cx 归入 16 通道栏，但 Table 12 pp.32–33 中 UFQFPN48 实际只引出以下模拟输入。精确 STM32U073CCUx.xml 的 ADC1_IN 列表也与这 10 项一致。PC0~PC5 在该封装列均为“—”，不能作为外部输入使用。

| UFQFPN48 脚号 | GPIO | ADC 输入 |
|---|---|---|
| 10 | PA0 | ADC1_IN4 |
| 11 | PA1 | ADC1_IN5 |
| 12 | PA2 | ADC1_IN6 |
| 13 | PA3 | ADC1_IN7 |
| 14 | PA4 | ADC1_IN8 |
| 15 | PA5 | ADC1_IN9 |
| 16 | PA6 | ADC1_IN10 |
| 17 | PA7 | ADC1_IN14 |
| 18 | PB0 | ADC1_IN17 |
| 19 | PB1 | ADC1_IN18 |

2. 修正容易误导回查的页码：DMA §3.12 在 p.17，ADC §3.16 在 pp.18–19，DAC §3.17 在 p.19，比较器 §3.19 和运放 §3.20 在 p.20，USART Table 9 跨 pp.24–25，LPUART §3.28 在 p.25。原 HASH 行误引 §3.21（实际是 LCD），已换为 p.2 CRC 功能和 p.6 框图。
3. 补充 HSI48 未经 CRS 同步的工厂精度与电压/温度条件（Table 54 p.77）；HSI16/MSI 标为特性测试值而不是逐颗生产测试。保留 HSI16 温漂与电压漂移分别列项的解释，避免把首页 ±1% 视作所有温度/电压下总误差。
4. 运行电流明确为 LDO。52 μA/MHz 是 While(1) 而非 CoreMark 工作负载；Table 32 p.54 的 76 μA/MHz（48 MHz、1.8 V）与 78 μA/MHz（3.0 V）核对无误，均典型 25°C、Flash、cache ON/prefetch OFF、外设关闭。

## 已核实的重点

- 精确订货号：UFQFPN48 7×7 mm，256 KB Flash / 40 KB SRAM，39 GPIO，温度等级 6 为 -40~85°C（§8 p.135、§7.5 pp.120–121、Table 1、官方 XML）。
- 4 USART + 3 LPUART、3 SPI 且无 I2S、4 I2C；USB 仅 FS Device。DAC 单通道、2 比较器、1 运放。外设数量不代表所有复用功能可同时引出。
- OTP 1 KB 来源是官方 CMSIS FLASH_OTP_BASE/FLASH_OTP_SIZE（L804–805），Zephyr 用户 OTP 节点交叉核对；不捏造手册未列出的独立 OTP 章节。
- RDP2 不可逆是该版 DS §3.4.1 p.8 原文，并非由其他系列推断。密码回退只作存在机制说明，不断言 RDP2 可以解锁。
- DMA 首页 p.2 写 12 channels，框图 p.6 写 DMA1&2；正文 §3.12 p.17 写 seven channels。官方 CMSIS DMA1_Channel1~7 + DMA2_Channel1~5、Zephyr dma2 的 5 请求和 DMAMUX 12 通道交叉支持 7+5=12；保留 DS 文字冲突说明。
- Stop 2 保持全部 SRAM（§3.7 p.12）。Table 40 的 695 nA 是 1.8 V/25°C、RTC/LCD 关闭、EN_ULP=0；825 nA 是 RTC=LSE low-drive、EN_ULP=1/LPCAL=1，不能把两行仅作为启用RTC的增量。3.0 V 无 RTC 为 750 nA。
- Standby 30.5 nA 与 Shutdown 10 nA 均按 1.8 V/25°C 详细表；Standby 若保持 SRAM2 需额外加 89 nA。首页 Shutdown 16 nA 与 Table 42 p.64 的 10 nA 不一致，保留差异，不把 Shutdown 当保 RAM 深睡。
- Boot ROM 下载器与基于 HDP/securable area 的应用安全启动隔离分别表述；没有把它写成 ROM 内置签名验证启动。HDP 也未冒充专用密钥库。
- CoreMark 134/2.4 per MHz 为数据手册官方结果，未声称本次实测。

## 联网证据独立复取

审核时从原分析记录的固定提交 raw.githubusercontent.com URL 再次下载 ST pin XML、ST CMSIS 和 Zephyr U073 dtsi，分别为 22719、861269、2295 字节，与 sources/ 已存文件逐字节一致。链接和固定提交见分析文件来源列表。

## 未决与限制

- 工艺未知；IP Version 字串不能当晶圆制造工艺证明。
- 独立硬件密钥库未知；未将 HDP/OTP 推断为硬件密钥管理模块。
- Flash EEPROM 仿真实现/容量没有本次可核实的目标软件方案，标未知；无独立 EEPROM。
- 未进行目标 PCB 引脚整体分配、Zephyr 构建、板级电流测量或量产安全配置验证。
- 结论基于用户提供修订版，未能通过被代理阻断的 ST 官网确认是否为最新修订。
