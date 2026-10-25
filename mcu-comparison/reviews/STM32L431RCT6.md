# STM32L431RCT6 独立审核记录

审核角色：独立于初稿分析 agent 的第二阶段审核 agent。核对对象为 analysis/STM32L431RCT6.md/.json 的34项；已直接读取用户原始 DS11453 Rev 3（2018年5月）相关页及已下载官方 CMSIS / Zephyr 文件。

## 修订

- 内部振荡器精度：原稿 HSI16 温漂有依据，但没有给 HSI48，且 MSI 自动校准指标容易脱离条件。补充表50 p124 的 HSI48 ±4.5%（1.65～3.6V，−40～125°C）和 ±3%（3～3.6V，−15～85°C）。补充表49 pp121–123 的未校准 MSI 温漂，明确不能将 LSE 校准后的 ±0.25% 套用于自由运行 MSI。明确表48 HSI16 的温漂与初始频率误差、电压漂移是分别给定参数。
- 修改已经直接写入该型号的 MD、JSON；未运行初稿生成脚本覆盖文件。

## 逐组核验结果

|核验项目|原始依据与结论|
|---|---|
|Architecture / Frequency / GPIO Count / Flash-RAM|DS pp13–14 表2、p19、p206：Cortex-M4+FPU/DSP/MPU，80MHz，R封装52GPIO，256KB Flash/64KB SRAM（48+16）。LQFP64尺寸及料号温度范围确认。|
|Manufacturing process|所查原始资料未披露，保留未知。|
|Internal oscillator accuracy|表48–50，按上述修订。无全温±1%的总精度声明。|
|EEPROM / XIP / OTP-eFuse|DS pp18–19、p53 §3.33、p80存储图：无专用EEPROM；QUADSPI有memory-mapped模式，外部映射代码执行需初始化；OTP范围0x1FFF7000至0x1FFF73FF为1KB；通用eFuse阵列未知。|
|Boot ROM / Secure Boot / Secure Key Store / TrustZone / RDP|DS pp18–20：ROM下载器有USART/I2C/SPI/CAN；文档未证明ROM验签启动，保留未知；Firewall有代码/Flash数据/SRAM1数据保护，但不是专用硬件密钥库；Cortex-M4无TrustZone；RDP0/1/2且2级不可逆。|
|Crypto / Hash / TRNG|DS图1 p16、§3.21 p42及CMSIS：无AES/PKA/Hash硬件外设，CRC不是密码学Hash；有硬件RNG。|
|USART / UART / I2C / I3C / SPI-I2S|表2 p14、§3.24–3.28 pp47–51及CMSIS：3USART+1LPUART，无独立普通UART；3I2C，无I3C；3SPI无I2S声明，但1SAI支持I2S。首页“4USART”与详细正文冲突，初稿正确揭示并采用正文。|
|CANFD / Ethernet / SDIO / USB|DS pp14–16、pp51–52及CMSIS：CAN2.0B不是CANFD；无Ethernet MAC；R封装有1SDMMC支持SDIO；无USB，HSI48/CRS不能证明USB存在。|
|ADC / DAC / Comparator / Op-Amp|表2 p14、§3.15–3.19 pp38–41：1个12位ADC、R封装16外部通道，正文5.33Msps；2路12位DAC（一个双通道块）、2COMP、1OPAMP。|
|DMA|p36 表7：DMA1与DMA2各7通道，共14。|
|Zephyr|已下载官方固定提交的stm32l431.dtsi和stm32l431Xc.dtsi：有SoC和容量描述；尚未构建精确自定义板或执行硬件测试，保留限制。|
|CoreMark|首页273.55，3.42/MHz@80MHz；编译器/优化选项及该跑分执行位置未明确，不能当本次实测。|
|Runtime Efficiency|表4 pp24–26注3：84µA/MHz为1.8V/25°C/26MHz Range2/SRAM执行/Flash关；表29 p97：121µA/MHz、9.68mA为80MHz Range1/Flash CoreMark/cache开/prefetch关，3V默认条件来自§6.1.2 p84。不是同一测试负载。|
|Deep Sleep Current|p27确认Stop保全部SRAM，表34 p100确认3V25°C RTC关1.06µA典型/2.1µA最大；表37 p104与表38 p105确认Standby90.2nA/225nA、Shutdown44.3nA/70.1nA。两种后者不保SRAM，Standby可另加电流保SRAM2；Stop+LSE低驱动RTC典型1.59µA，不能以首页1.28µA（1.8V）代替。|

## 未决项与限制

- 尚未独立取得 RM0394 或在线最新版勘误，不能宣称消除了当前Rev3之外的所有文档问题。
- 制造工艺、专用密钥库、ROM内置认证安全启动未获证据，保持未知，不从其他L4系列型号推断。
- 不涉及硬件实测、引脚复用可同时使用的验证、Zephyr板级构建或量产选项字节操作。

结论：34项已审，修订集中在内部时钟精度的完整条件；其余精确料号、外设数量、安全和功耗结论与所引原始资料一致，可供主agent汇总，但需保留上述限制。
