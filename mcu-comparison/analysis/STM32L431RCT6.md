# STM32L431RCT6 独立分析

LQFP64 (10×10mm)，−40～85°C（105°C结温）；R=64pin，C=256KB，T=LQFP，6=工业温度级（[L-DS] 表105 p206）。

|数据项|结论|条件/备注|来源|
|---|---|---|---|
|Architecture|Arm Cortex-M4，单精度FPU、DSP、MPU||[L-DS] DS11453 Rev 3 p1、§3.1 p17|
|Frequency|最高80 MHz||[L-DS] DS11453 Rev 3 表2 p14|
|GPIO Count|52个（LQFP64）|GPIO复用、晶振和调试接口占用需在实际设计中扣除|[L-DS] DS11453 Rev 3 表2 p14、图8 p56|
|Manufacturing process|未知：本次原始资料未披露工艺节点||[L-DS] DS11453 Rev 3 全文检索；未检出|
|Internal oscillator accuracy|HSI16温漂：0～85°C为±1%，−40～125°C为−2%/+1.5%；MSI需LSE校准才可引用优于±0.25%；HSI48工厂校准精度：−40～125°C、1.65～3.6V为±4.5%|HSI16：16MHz；VDD=3V、TA=30°C时15.88～16.08MHz；电压漂移−0.1/+0.05%（1.62～3.6V）；温漂不是含初始误差及电压漂移的总误差。MSI未LSE校准时温漂为0～85°C −3.5%/+3%，−40～125°C −8%/+6%，还需考虑初始校准及电压漂移。HSI48在3.0～3.6V、−15～85°C为±3%。料号6额定环境温度为−40～85°C。|[L-DS] DS11453 Rev 3 表48 p119；表49 pp121–123；表50 p124；p1|
|Flash / RAM|256 KB Flash / 64 KB SRAM（48 KB SRAM1 + 16 KB SRAM2）||[L-DS] DS11453 Rev 3 表2 p13，§3.5 p19，表105 p206|
|EEPROM|无独立EEPROM；数据存储可使用内部Flash，EEPROM仿真需软件|没有为EEPROM预留的独立容量；寿命、掉电一致性需另作软件设计|[L-DS] DS11453 Rev 3 §3.4～3.5 pp18–19，图15 p80|
|XIP|支持外部Quad-SPI存储器映射，可用于外部代码执行|1个QUADSPI；需兼容外部Flash及初始化，不代表上电直接从外部Flash安全启动|[L-DS] DS11453 Rev 3 §3.33 p53、图15 p80|
|Boot ROM / Secure Boot|ROM系统存储器下载器：有；原生ROM认证安全启动：未知/未在所查DS说明|ROM下载接口USART/I2C/SPI/CAN；不能将其视为认证安全启动|[L-DS] DS11453 Rev 3 §3.7 p20|
|Secure Key Store|专用硬件密钥库：未知/未声明；有Firewall保护代码/Flash数据/SRAM1数据|Firewall不是专用密钥存储单元；文档要求RDP Level2以达到预期保护等级|[L-DS] DS11453 Rev 3 §3.6 p19|
|TrustZone|不支持Arm TrustZone|Cortex-M4；Firewall/MPU不等同TrustZone|[L-DS] DS11453 Rev 3 §3.1 p17；[L-CMSIS] Configuration_section|
|OTP / eFuse|1 KB OTP区（地址0x1FFF7000～0x1FFF73FF）；通用eFuse阵列未知|容量由原始地址映射区间计算；RDP Level2不可逆调试禁用不等同通用eFuse容量|[L-DS] DS11453 Rev 3 图15 p80|
|Crypto Accelerator|无AES/PKA等硬件加速器|软件加密另行实现；CRC不算密码加速器|[L-DS] DS11453 Rev 3 图1 p16、完整功能章节；[L-CMSIS] peripheral declarations|
|Hash Accelerator|无SHA等硬件Hash加速器；有CRC|CRC不是安全Hash|[L-DS] DS11453 Rev 3 图1 p16、§3.8 p20；[L-CMSIS] peripheral declarations|
|TRNG|支持硬件真随机数发生器||[L-DS] DS11453 Rev 3 §3.21 p42|
|RDP|支持Level0/1/2；Level2不可逆||[L-DS] DS11453 Rev 3 §3.4 p18|
|USART|3：USART1/2/3|首页写4x USARTs，与表2/正文冲突；按详细正文及芯片寄存器定义填写，不重复计入UART|[L-DS] DS11453 Rev 3 表2 p14、§3.25与表12 p48；[L-CMSIS] IRQn|
|UART|独立普通UART：0；另有1×LPUART1；3个USART也可异步通信||[L-DS] DS11453 Rev 3 表2 p14、§3.25 p48、§3.26 p49；[L-CMSIS] IRQn|
|I2C|3×I2C，最高Fm+ 1 Mbit/s||[L-DS] DS11453 Rev 3 表2 p14、§3.24/表11 p47|
|I3C|不支持；仅列出I2C||[L-DS] DS11453 Rev 3 图1 p16、§3.24 p47；[L-CMSIS] peripheral declarations|
|SPI / I2S|3×SPI；另1×SAI可承载I2S；1×QUADSPI另计|不能把SAI和SPI当成同一实例；DS未声明SPI带I2S模式|[L-DS] DS11453 Rev 3 表2 p14、§3.27～3.28 p50、表13 p51|
|CANFD|不支持CAN FD；有1×经典CAN 2.0B Active||[L-DS] DS11453 Rev 3 §3.30 p51；[L-ZEPHYR-SOC] can1 compatible st,stm32-bxcan|
|Ethernet|不支持片上Ethernet MAC||[L-DS] DS11453 Rev 3 图1 p16、表2 pp13–15；[L-CMSIS] peripheral declarations|
|SDIO|1×SDMMC（支持SD/SDIO/MMC）|R封装有SDMMC；不应套用C/K封装无SDMMC结论|[L-DS] DS11453 Rev 3 表2 p14、§3.31 p52|
|USB|无USB控制器/PHY|HSI48/CRS的存在不表示有USB；部分低功耗表出现USB_FS模板文字不能作为USB存在证据|[L-DS] DS11453 Rev 3 表2、图1 p16；[L-CMSIS] IRQn及外设地址|
|ADC|1×12-bit ADC，LQFP64提供16个外部通道，最高5.33 Msps（首页概括5 Msps）；硬件过采样可至16-bit|内部温度/VREFINT/VBAT测量另计；过采样不等于原生16-bit ADC|[L-DS] DS11453 Rev 3 表2 p14、§3.15 p38|
|DAC|2路12-bit DAC输出（1个双通道DAC块）||[L-DS] DS11453 Rev 3 表2 p14、§3.16 p39|
|Comparator|2个超低功耗比较器||[L-DS] DS11453 Rev 3 §3.18 p41|
|Op-Amp|1个，带PGA||[L-DS] DS11453 Rev 3 §3.19 p41|
|DMA|2×DMA控制器，每个7通道，共14通道||[L-DS] DS11453 Rev 3 §3.13 p36；[L-CMSIS] DMA1/2 Channel1～7 IRQn|
|Zephyr|有STM32L431 SoC及256KB/64KB器件描述支持；本精确封装需自定义板/引脚及驱动验证|仅核实源码层SoC支持，未进行板级编译或硬件运行；不能声称存在该精确料号的现成开发板支持|[L-ZEPHYR-XC] 与 [L-ZEPHYR-SOC]，提交 bc5d59d1b6657cfeef30124b818c8fbd69da68ac|
|CoreMark|273.55 CoreMark；3.42 CoreMark/MHz @80MHz（官方公布值）|DS未列完整编译器/优化选项，亦未明确该跑分的执行位置；§3.2的ART说明不能替代跑分条件；非本次实测|[L-DS] DS11453 Rev 3 p1，§3.2 p17|
|Runtime Efficiency|84 µA/MHz（SRAM运行、Flash关、26MHz、1.8V、25°C、Range2）；121 µA/MHz（Flash跑CoreMark、80MHz、3V、25°C、Range1、ART Cache开/Prefetch关）|均为典型值；第二工况电流9.68mA。全部外设禁用、IO模拟输入；84并非80MHz Flash CoreMark功耗|[L-DS] DS11453 Rev 3 表4 pp24–26及注3；表29 p97；§6.1.2 p84；§6.3.5 p93|
|Deep Sleep Current|保64KB SRAM Stop2：1.06 µA典型/2.1 µA最大（3V、25°C、RTC关）；Standby：90.2 nA典型/225 nA最大（3V、25°C、RTC/IWDG关，不保SRAM）；Shutdown：44.3 nA典型/70.1 nA最大（3V、25°C、RTC关，不保SRAM）|Stop2中主时钟/PLL关，低功耗调压器工作；Stop2+RTC LSE低驱动典型1.59µA（3V、25°C）。Standby可选保16KB SRAM2但需另加电流；此处未启用。保留Backup registers，不能与保全RAM等同。|[L-DS] DS11453 Rev 3 §3.9.4 pp27–28；表34 p100；表37 p104；表38 p105|

## 来源

- [L-DS] STM32L431xx datasheet DS11453 Rev 3 (May 2018)：https://www.st.com/resource/en/datasheet/stm32l431rc.pdf；本地 sources/DS_stm32l431rc.pdf。
- [L-CMSIS] ST 官方 CMSIS STM32L431xx Header：https://raw.githubusercontent.com/STMicroelectronics/cmsis-device-l4/ca0bfa2b8b68dc2994b27fba0a10dfd28d086ee1/Include/stm32l431xx.h；本地 sources/stm32l431xx.h。
- [L-ZEPHYR-XC] Zephyr STM32L431Xc device tree：https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/bc5d59d1b6657cfeef30124b818c8fbd69da68ac/dts/arm/st/l4/stm32l431Xc.dtsi；本地 sources/zephyr-stm32l431Xc.dtsi。
- [L-ZEPHYR-SOC] Zephyr STM32L431 SoC device tree：https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/bc5d59d1b6657cfeef30124b818c8fbd69da68ac/dts/arm/st/l4/stm32l431.dtsi；本地 sources/zephyr-stm32l431.dtsi。

## 限制与冲突

- DS首页4×USART与表2及§3.25的3个USART冲突，采用详细正文/CMSIS一致的3USART+1LPUART。
- 来源为用户上传2018年Rev3 DS，未认定是ST当前最新修订；未获得RM0394。
- 工艺节点与专用安全密钥库未知；安全启动不与ROM下载器混淆。

## 独立审核

已由独立审核 agent 逐项核验34行。补全 HSI48 原始精度及 MSI 未校准漂移，明确 HSI16 温漂不是总误差。其他数值与所引 Rev 3 原始资料一致；详见 reviews/STM32L431RCT6.md。
