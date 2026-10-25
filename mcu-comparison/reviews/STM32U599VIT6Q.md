# STM32U599VIT6Q 独立审核记录

审核者：第二阶段独立 agent `review_u599`，与原分析者不同。

状态：完成。逐项核对 34 行；直接阅读用户上传 DS13633 Rev 3（May 2025）的逐页提取内容，交叉核对本地保存的 Zephyr v4.2.0 官方 DTS/Kconfig。没有声称访问 E 盘、ST 官网在线原件或实际芯片测试。

## 已修订

1. ADC 来源章节由错误的 §3.32–3.33 改为 §3.31 / Table 15（pp.65–68）。数值本身正确：ADC1/2 各14位，ADC4为12位，均最高2.5MSPS；SMPS LQFP100封装18个可用外部输入（p.20 Table2），不是每ADC各18个。
2. USB 增加关键硬件先决条件：内置HS PHY的PLL需要精确60MHz，源必须为 HSE 晶体振荡器（p.93 §3.52）。内置PHY不能推论免外部晶体。
3. RDP 增加 Level0.5 仅在 TrustZone 启用时可用；Level2 默认不可逆，但低级别预配置安全选项中的密钥可允许认证回退至Level1（p.26）。避免照搬其他系列的“Level2一律不可逆”。
4. SDMMC 并用条件保留 Table2 脚注3，补充 p.92 §3.50 的具体互斥关系。最终封装引脚并用仍需板级核验。
5. Standby/Shutdown 电流所附“仅备份寄存器保持”限定为该表的测量配置，补充 Standby 可另选 SRAM2/backup SRAM 保持但应另计电流（p.29）。未把典型低电流外推至全RAM保持。
6. 内部时钟补充 MSI 100kHz–48MHz 及首页精度“优于±0.25%”必须配合 LSE 自动校准。HSI16 的结温与电压条件保留。

所有修订已同步到 analysis/STM32U599VIT6Q.json 与 .md；未运行原生成脚本覆盖审核结果。

## 逐组核验依据（覆盖全部34项）

| 覆盖项目 | 独立核验依据及结论 |
|---|---|
| Architecture、Frequency | pp.1,20,23：M33 + TrustZone/FPU/DSP，最高160MHz。 |
| GPIO Count、精确封装 | pp.20,98,381：V=100脚，T=LQFP，Q=SMPS专用引脚；82/79行取with SMPS一侧79，不取系列156。 |
| Manufacturing process | 原数据手册未披露，保留未知。没有由IP版本或系列名推断工艺。 |
| Internal oscillator accuracy | pp.245–246 Table73：30°C/3V为15.92–16.08MHz；−10…100°C和1.58–3.6V为±1%；−40…130°C为15.65–16.25MHz；型号6的TA/TJ范围另列。p.2、Table74补充MSI校准条件。 |
| Flash / RAM | pp.18,29,381：I=2MB而非4MB；系统SRAM2512KB + backup2KB；SRAM3 ECC使总可用RAM减少64KB至2450KB，不额外计缓存。 |
| EEPROM、OTP / eFuse | p.25：无独立EEPROM；512B用户OTP；512KB Flash高耐久区不能写成512KB EEPROM。eFuse未披露保留未知。 |
| XIP | pp.18,20–21,23,62–63：本封装2×OCTOSPI仅muxed；无HSPI；NOR/PSRAM仅FSMC bank1复用。 |
| Boot ROM / Secure Boot、Secure Key Store、TrustZone、RDP | pp.2,25–31：ST系统下载器与RSS/SFI存在；不等于自动完成用户应用验签。TrustZone/HDP/OTP可保护资源，但未证明存在专用不可导出密钥库。RDP2密码回退已详述。 |
| Crypto Accelerator、Hash Accelerator、TRNG | p.2功能清单、pp.18–22总表/框图、p.78：本型号无AES/PKA，不能从U5A9套用；HASH支持SHA1/224/256、MD5、HMAC；TRNG为SP800-90B compliant熵源。 |
| USART、UART、I2C、I3C | pp.19,85–86：4 USART + 2 UART + 1 LPUART；6 I2C；无I3C。实例数不证明全部引脚能同时布线。 |
| SPI / I2S | pp.19,89–91：3 SPI；I2S协议通过2 SAI支持，不把OCTOSPI/HSPI计入普通SPI。 |
| CANFD、Ethernet、SDIO、USB | pp.19,21,91–95：1 FDCAN，无Ethernet MAC；2 SDMMC有并用限制；USB2 OTG_HS内置PHY支持Host/Device，UCPD另列且HSE前提已补。 |
| ADC、DAC、Comparator、Op-Amp | p.20、pp.65–72：3 ADC/2输出通道DAC/2比较器/2运放；数量及封装条件正确。 |
| DMA | pp.53–58，Tables11–14：GPDMA16通道 + LPDMA4通道；另有DMA2D，不换算普通DMA通道。GPDMA至Stop1、LPDMA至Stop2。 |
| Zephyr | 本地保存的官方v4.2.0 Kconfig存在 SOC_STM32U599XX；stm32u599Xi.dtsi明确2MB；支持SoC不等于精确自定义板已验证。 |
| CoreMark | p.1：655、4.09/MHz为官方摘要；未声称编译器、同条件实测或板测。 |
| Runtime Efficiency | pp.177,193,195,197：CoreMark160MHz时SMPS7.15mA/45µA/MHz@3.3V；LDO14.5mA/91µA/MHz。18.5是24MHz精简While低耗配置。 |
| Deep Sleep Current | pp.221–222 Table59：Stop3/SMPS全RAM8.2µA@3.3V25°C，最少8KB SRAM2+ICACHE为2µA@3V25°C；pp.224/228分别为Standby0.195µA与Shutdown0.15µA@1.8V25°C。典型值和最大值分开；未偷换电压或RAM保持范围。 |

## 未决项与限制

- 制造工艺、专用不可导出硬件密钥库、独立eFuse容量：仍未知。
- 未获取ST官网最新版勘误/手册；报告依据用户上传版本，不保证量产前无需复核最新文档。
- 未做目标板引脚分配、所有外设并用、Zephyr编译、CoreMark/功耗实测或安全启动实现验证。
- 现有审查可支持文献参数对比，不能替代PCB与电源设计审查。
