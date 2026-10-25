# STM32U073CCU6 独立分析（已审核）

UFQFPN48，7×7 mm，-40~85°C（6）；48 引脚、39 GPIO；DS §7.5 pp.120–121，§8 p.135

| 数据项 | 结论 | 来源 | 条件/说明 |
|---|---|---|---|
| Architecture | Arm Cortex-M0+，32 位 | U073-DS p.1、§2 p.4 |  |
| Frequency | 最高 56 MHz | U073-DS p.1、Table 1 p.5 |  |
| GPIO Count | 39 GPIO（48 引脚封装） | U073-DS Table 1 p.5；U073-PIN IONb=39 | 与电源脚、晶振、调试、外设复用有关；并非 48 个 GPIO |
| Manufacturing process | 未知（数据手册未披露） | U073-DS 全文；U073-PIN IP Version 不作为芯片制造工艺依据 |  |
| Internal oscillator accuracy | HSI16：16 MHz，首页标称 ±1%；详细频漂为 0~85°C ±1%，-40~125°C -2%/+1.5%。MSI：100 kHz~48 MHz，可用 LSE 自动校准，首页称优于 ±0.25%；HSI48 未经 CRS 同步时，工厂校准精度：±3%（-15~85°C、3.0~3.6V）或 ±4.5%（-40~125°C、1.65~3.6V） | U073-DS p.2；Table 52 p.73；Table 53 pp.74–76；Table 54 p.77 | HSI16 在 VDD=3.0V、TA=30°C 为 15.88~16.08MHz；温漂和电压漂移为独立参数，不能把 ±1% 当全温总误差；具体料号 -40~85°C。MSI 精度须满足 LSE 校准条件。 HSI16/MSI 表值按表声明来自特性测试而非逐颗生产测试；USB免晶振依赖CRS/USB SOF同步条件，不能用未同步HSI48误差代表同步后的USB时钟误差。 |
| Flash / RAM | 256 KB Flash / 40 KB SRAM（32 KB SRAM1 + 8 KB SRAM2） | U073-DS §3.4 pp.8–9，§8 p.135；U073-PIN Flash/Ram | 料号第二个 C=256KB；SRAM2 可在 Standby 保持 |
| EEPROM | 无独立数据 EEPROM；Flash 仿真实现与容量未知 | U073-DS §3.4 pp.8–9、存储映射 §5 | 不能将主 Flash 或 OTP 直接计为 EEPROM |
| XIP | 无专用外部存储器映射/XIP 接口 | U073-DS Table 1 pp.4–5、Figure 1 p.6、§3.29 p.25 | SPI 传输不等于外部 Flash XIP |
| Boot ROM / Secure Boot | 有 System Memory ROM 下载器；支持用户 Flash 安全启动隔离区（HDP/securable area），不等同于 ROM 内置验签安全启动 | U073-DS §3.4.1 p.8、§3.5 p.9；p.2 Secure boot | ROM 下载支持 USART/I2C/SPI/USB；安全启动认证算法/签名实现未在手册明确 |
| Secure Key Store | 有 HDP 可隐藏代码/数据区；专用硬件密钥库未知 | U073-DS p.2、§3.4.1 p.8 | HDP 不能直接等同于独立硬件密钥存储 |
| TrustZone | 不支持（Cortex-M0+） | U073-DS §3.1 p.7；U073-CMSIS core_cm0plus.h include | Armv6-M 无 Armv8-M TrustZone |
| OTP / eFuse | 1 KB 用户 OTP；eFuse 未明确披露 | U073-CMSIS FLASH_OTP_BASE=0x1FFF6800、FLASH_OTP_SIZE=0x400（L804–805）；U073-ZEPHYR stm32u0.dtsi st_nvm_user_otp | 数据手册未给出独立 OTP 章节，容量由 ST 官方 CMSIS 与 Zephyr 交叉核实 |
| Crypto Accelerator | 无 AES/PKA 硬件加速器 | U073-DS Figure 1 p.6、功能概览；U073-PIN IP 清单 | 不可因 CMSIS 中共用 IRQ 名 RNG_CRYP 就认定存在 AES |
| Hash Accelerator | 无专用 SHA/HASH 硬件加速器；有 CRC | U073-DS p.2 CRC calculation unit、Figure 1 p.6、功能概览；U073-PIN IP 清单 | CRC 不等于密码学 Hash |
| TRNG | 支持，32 位真随机输出 | U073-DS §3.23 p.21；p.2 | Rev 2 首页为 NIST SP800-90B candidate，不声称已认证 |
| RDP | 支持 Level 0/1/2，128 位密码回退机制；Level 2 不可逆 | U073-DS §3.4.1/Table 2 p.8 | 量产前必须单独验证回退配置和选项字节策略 |
| USART | 4 × USART（USART1~4） | U073-DS Table 1 p.4、§3.27/Table 9 pp.24–25 | 含同步 SPI 模式；不计入下行 LPUART |
| UART | 3 × LPUART；无另列普通 UART IP | U073-DS Table 1 p.4、§3.28 p.25 | 加上 4 个 USART 共 7 个异步串口接口 |
| I2C | 4 × I2C，最高 1 Mbit/s Fast-mode Plus | U073-DS p.2、Table 1 p.4、§3.26 pp.23–24 |  |
| I3C | 不支持 | U073-DS Table 1 pp.4–5、Figure 1 p.6 |  |
| SPI / I2S | 3 × SPI；I2S 不支持；另有 4 个 USART 可工作于同步 SPI 模式 | U073-DS p.2、Table 10 p.25 | SPI1/2/3 均无 I2S；不能把 USART SPI 模式当作额外独立 SPI 控制器 |
| CANFD | 不支持；亦无经典 CAN | U073-DS Table 1 pp.4–5、Figure 1 p.6 |  |
| Ethernet | 不支持 | U073-DS Table 1 pp.4–5、Figure 1 p.6 |  |
| SDIO | 不支持（无 SDIO/SDMMC） | U073-DS Table 1 pp.4–5、Figure 1 p.6 |  |
| USB | USB 2.0 Full Speed Device，12 Mbit/s，支持免晶振；不支持 Host | U073-DS §3.30 p.26 | 内置 PHY、1KB packet RAM；USB SOF 可校准内部 48MHz 时钟 |
| ADC | 1 × 12 位 ADC；UFQFPN48 可引出 10 个外部模拟输入（PA0~PA7、PB0、PB1）；硬件过采样可到 16 位 | U073-DS §3.16 pp.18–19；引脚表 Table 12 pp.32–33；U073-PIN ADC1_IN 引脚清单 | 单 ADC 单元，非原生 16 位。Table 1 p.5 将 Cx 归入16通道栏，但精确 UFQFPN48 未引出 PC0~PC5，实际只有10路；不能把系列/ADC内部通道数直接当封装可用输入数。 引出对应：PA0(pin10)=IN4、PA1(11)=IN5、PA2(12)=IN6、PA3(13)=IN7、PA4(14)=IN8、PA5(15)=IN9、PA6(16)=IN10、PA7(17)=IN14、PB0(18)=IN17、PB1(19)=IN18；PC0~PC5 在 UFQFPN48 栏均为“—”。 |
| DAC | 1 × 12 位 DAC 输出通道 | U073-DS p.1、Table 1 p.5、§3.17 p.19 | ST 通用 DAC 结构中出现双通道寄存器不代表该型号双通道 |
| Comparator | 2 × 超低功耗比较器 | U073-DS p.1、Table 1 p.5、§3.19 p.20 |  |
| Op-Amp | 1 × 运放，内置 PGA，最高 16 倍增益 | U073-DS p.1、Table 1 p.5、§3.20 p.20 |  |
| DMA | 2 个 DMA 控制器：DMA1 7 通道 + DMA2 5 通道，共 12 通道，带 DMAMUX | U073-DS p.2、Figure 1 p.6；U073-CMSIS DMA IRQ 与 DMA1_Channel1~7、DMA2_Channel1~5；U073-ZEPHYR stm32u073.dtsi dma2 | DS §3.12 p.17 仅称 seven channels，属于概述不完整；总数以首页、框图和官方实现交叉核验。 |
| Zephyr | 有上游 SoC 支持（STM32U073 / Xc）；精确 CCU6 自定义板仍需引脚、时钟、外设适配与构建验证 | U073-ZEPHYR stm32u073.dtsi、stm32u073Xc.dtsi | 核验提交 bc5d59d1b6657cfeef30124b818c8fbd69da68ac；未宣称已对该封装板完成编译/硬件运行。 |
| CoreMark | 2.4 CoreMark/MHz；134 CoreMark @56 MHz（官方标称） | U073-DS p.1 Benchmarks | 编译器/优化参数未披露；非本次实测；134 为首页取整数 |
| Runtime Efficiency | 典型 52 μA/MHz（While(1)）；CoreMark 典型 76 μA/MHz @48MHz、1.8V，或 78 μA/MHz @3.0V | U073-DS p.1、Table 32 pp.54–55 | LDO；25°C，Range1，MSI=48MHz，Flash 执行，ART/cache ON、prefetch OFF，全部外设关闭；52 为 1.8V 空循环条件，不能当真实工作负载电流 |
| Deep Sleep Current | Stop 2 保持 SRAM：典型 0.695 μA（无 RTC）/0.825 μA（带 RTC）；3.0V 无 RTC 为 0.750 μA。另列：Standby 无 SRAM2 保持、无 RTC 30.5 nA；Shutdown 无 RTC 10 nA（均 1.8V/25°C，按详细表） | U073-DS Table 40 pp.60–62、Table 41 pp.62–64、Table 42 p.64；§3.7 p.12 | 25°C，前两值 VDD=1.8V、LCD 关闭；无 RTC EN_ULP=0，带 RTC LSE low-drive、EN_ULP=1、LPCAL=1；SRAM 保持。无 RTC 的最大值在30°C、1.8V为2.45μA，非25°C最大值。Standby 和 Shutdown 值 EN_ULP=0、无 IWDG（Standby）；Standby 保留 SRAM2 额外加89nA@1.8V/25°C。首页 Shutdown 为16nA，与详细 Table42 的10nA不同；采用可追溯完整条件的表值，不能用于保RAM深睡比较。 |

## 来源

- [U073-DS] STM32U073x8/B/C Datasheet DS14548 Rev 2 (March 2024)，用户上传原件 — https://www.st.com/resource/en/datasheet/stm32u073cc.pdf
- [U073-PIN] ST 官方 STM32_open_pin_data，精确 STM32U073CCUx.xml — https://github.com/STMicroelectronics/STM32_open_pin_data/blob/7d1f1514ed5583ec5007ad91236b4e1d377295b1/mcu/STM32U073CCUx.xml
- [U073-CMSIS] ST 官方 CMSIS STM32U073xx 头文件 — https://github.com/STMicroelectronics/cmsis-device-u0/blob/5fca202fc2c5c5273238cfd27943dd95759a44a6/Include/stm32u073xx.h
- [U073-ZEPHYR] Zephyr 官方 STM32U073 device tree，main 提交 bc5d59d1b6657cfeef30124b818c8fbd69da68ac — https://github.com/zephyrproject-rtos/zephyr/blob/bc5d59d1b6657cfeef30124b818c8fbd69da68ac/dts/arm/st/u0/stm32u073.dtsi

## 限制

- 本地 E: 路径未访问；手册使用用户上传原件。
- 未进行板级硬件验证；Zephyr仅核实上游代码支持。
- 制造工艺与专用密钥库未知；EEPROM仿真容量未知。
