# STM32U599VIT6Q 独立分析（独立审核修订版）

LQFP100 (14×14mm)，Q=内置SMPS专用引脚版本；6=TA−40…85°C；I=2MB Flash

| 项目 | 结论 | 来源 | 条件/限制 |
|---|---|---|---|
| Architecture | Arm Cortex-M33，TrustZone、DSP、单精度 FPU、MPU | DS13633 Rev 3 (May 2025) pp.1,23 §3.1 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Frequency | 最高 160 MHz | DS13633 Rev 3 (May 2025) p.20 Table 2 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| GPIO Count | 79 GPIO（100引脚、带SMPS的Q专用引脚版本）；无SMPS的对应封装为82 | DS13633 Rev 3 (May 2025) p.20 Table 2; p.98 Fig.9; p.381 §7 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Manufacturing process | 未知（本数据手册未披露） | 未知；DS13633 未披露，不从U5系列推断 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Internal oscillator accuracy | HSI16：16 MHz；工厂校准±0.5% @VDD=3.0V,TJ=30°C；±1% @TJ=−10…100°C；全结温−40…130°C为15.65…16.25MHz（−2.19%/+1.56%）；另有MSI 100kHz–48MHz，LSE自动校准时首页标称优于±0.25% | DS13633 Rev 3 (May 2025) p.245 Table 73；p.2 Clock management；pp.247–250 Table 74 | 宽温两行VDD=1.58…3.6V；该型号6温度级TA=−40…85°C、TJ上限105°C；这是结温参数，不能把首页±1%套到整个温区。Table 73脚注1：特性表征，非量产逐颗测试，未计封装/焊接影响。 MSI标称精度要求LSE自动校准；不能套用于未经校准的自由运行MSI（其温漂/压漂另见Table74）。 |
| Flash / RAM | 2 MB Flash；2512 KB系统SRAM + 2 KB backup SRAM = 2514 KB（SRAM3 ECC关闭）；ECC开启后总2450 KB | DS13633 Rev 3 (May 2025) pp.1,18 Table 2; p.29 §3.5; p.381 §7 | I=2MB，J才是4MB；SRAM3 ECC开启减少64KB。缓存不额外计入RAM。 |
| EEPROM | 无独立EEPROM；Flash可用于软件仿真；最多512 KB Flash区域支持100 kcycles（并非独立EEPROM容量） | DS13633 Rev 3 (May 2025) p.25 §3.4; p.254 Table 79 | 具体EEPROM仿真软件/磨损均衡方案未核验；100k为Flash耐久能力，不能写成512KB EEPROM。 |
| XIP | 支持外部映射执行：2×OCTOSPI（此封装仅muxed模式）；本LQFP100无HSPI | DS13633 Rev 3 (May 2025) pp.18,21 Table 2 notes 1–2; pp.23,62–63 §§3.2.1,3.27 | OCTOSPI memory-mapped；共享引脚/时分复用，不是两条独立全宽总线。FSMC该封装仅bank1复用NOR/PSRAM。 |
| Boot ROM / Secure Boot | ST出厂系统存储器bootloader；提供RSS/SFI、TrustZone+HDP启动信任根与TF-M固件升级支持；应用须实现/配置认证启动，不能将ROM下载器等同验签安全启动 | DS13633 Rev 3 (May 2025) p.2 Security; p.31 §3.7 | 是否满足具体产品的安全启动/密钥生命周期需求需按TF-M/RSS方案评估。 |
| Secure Key Store | 专用硬件密钥库未知；有TrustZone、HDP和512B OTP保护资源，未找到HUK/SAES式专用密钥存储证据 | DS13633 Rev 3 (May 2025) pp.2,25–29 §§3.4–3.6 | 不能把HDP/普通OTP直接等同不可导出密钥库。 |
| TrustZone | 支持 Arm TrustZone（安全/非安全隔离） | DS13633 Rev 3 (May 2025) p.29 §3.6 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| OTP / eFuse | 512 B 用户OTP；eFuse独立容量未知 | DS13633 Rev 3 (May 2025) p.25 §3.4 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Crypto Accelerator | 无AES/PKA硬件加速器；HASH与TRNG另列 | DS13633 Rev 3 (May 2025) p.2 Features; pp.18–22 Table 2/框图；功能章节列表 | STM32U599不能套用带AES/PKA的STM32U5A9等相邻产品；软件密码算法另行实现。 |
| Hash Accelerator | 有：SHA-1、SHA-224、SHA-256、MD5、HMAC | DS13633 Rev 3 (May 2025) p.78 §3.43 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| TRNG | 支持；NIST SP800-90B熵源，32-bit输出 | DS13633 Rev 3 (May 2025) p.78 §3.42 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| RDP | 支持，含RDP 0 / 0.5 / 1 / 2及密码保护调试生命周期 | DS13633 Rev 3 (May 2025) pp.26–28 §3.4.1 | RDP0.5仅在TZEN=1时可用。Level2默认（未配置回退密钥）不可逆；如在更低RDP级预配置安全选项中的密钥，可经JTAG/SWD密码认证回退至Level1（DS p.26）。不能统一写成Level2必然不可逆。 |
| USART | 4：USART1/2/3/6 | DS13633 Rev 3 (May 2025) p.19 Table 2; p.86 Table 21 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| UART | 2：UART4/5；另1×LPUART1（串口总计7，不重复计数） | DS13633 Rev 3 (May 2025) p.19 Table 2; p.86 Table 21 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| I2C | 6×I2C（FM+最高1 Mbit/s） | DS13633 Rev 3 (May 2025) pp.2,19 Table 2 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| I3C | 不支持（本型号无I3C控制器） | DS13633 Rev 3 (May 2025) p.2接口清单；pp.18–22 Table 2/框图 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| SPI / I2S | 3×SPI；I2S由2×SAI支持（不是把SPI另算作I2S单元） | DS13633 Rev 3 (May 2025) p.19 Table 2; pp.89–91 §§3.48–3.49, Table 23 | OCTOSPI专用存储器接口另列，不计为普通SPI。 |
| CANFD | 1×FDCAN | DS13633 Rev 3 (May 2025) p.19 Table 2 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Ethernet | 不支持（无以太网MAC） | DS13633 Rev 3 (May 2025) p.2接口清单；p.22 Fig.1 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| SDIO | 2×SDMMC，支持SD/SDIO/eMMC；同时使用有限制 | DS13633 Rev 3 (May 2025) p.19 Table 2; p.21 note3; pp.91–92 §3.50 | Table2脚注3概括：同时使用时其中一路仅支持SDIO。§3.50进一步规定：仅一路可用8-bit eMMC；SDMMC1使用SDIO UHS-I电压切换与SDMMC2的8-bit eMMC互斥；SDMMC2使用8-bit eMMC时，SDMMC1只支持SDIO默认/高速模式。最终仍须按本封装引脚复用核验。 |
| USB | 1×USB 2.0 OTG High-Speed（内置HS PHY，Host/Device）；另1×UCPD Type-C/PD控制器 | DS13633 Rev 3 (May 2025) p.2; p.19 Table 2; pp.93–95 §§3.52–3.53 | HS最高480 Mbit/s；内置PHY PLL需精确60MHz时钟，时钟源必须使用HSE晶体振荡器（DS p.93），不能因内置PHY便认为免外部晶体。Type-C/PD控制器不应计为第二个USB数据接口。 |
| ADC | 2×14-bit ADC + 1×12-bit ADC（各最高2.5 MSPS）；本SMPS LQFP100有18个外部模拟输入引脚 | DS13633 Rev 3 (May 2025) pp.2,20 Table 2; pp.65–68 §3.31/Table 15 | 18为本SMPS LQFP100封装可用外部ADC输入数量，非每颗ADC各18路；ADC1/2为14位，ADC4为12位，ADC4可自主运行至Stop2；内部通道/过采样不另算原生分辨率。 |
| DAC | 1×12-bit DAC模块，2个输出通道 | DS13633 Rev 3 (May 2025) p.20 Table 2 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Comparator | 2×超低功耗比较器 | DS13633 Rev 3 (May 2025) pp.2,20 Table 2 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Op-Amp | 2×运放，内置PGA | DS13633 Rev 3 (May 2025) pp.2,20 Table 2 | 精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| DMA | 1×GPDMA（16通道）+1×LPDMA（4通道）；另有图形DMA2D | DS13633 Rev 3 (May 2025) pp.53–58 §§3.17–3.20, Tables11–14 | GPDMA自主支持Sleep/Stop0/Stop1；LPDMA支持到Stop2；不将DMA2D换算为普通DMA通道。 |
| Zephyr | 支持STM32U599 SoC及2MB变体；自定义VIT6Q板仍需板级定义与引脚配置 | Zephyr v4.2.0: soc/st/stm32/stm32u5x/Kconfig.soc (SOC_STM32U599XX); dts/arm/st/u5/stm32u599Xi.dtsi; stm32u599.dtsi | 官方源文件已下载核验；此结论不是精确用户板卡已验证启动，也不是全外设驱动完备承诺。 |
| CoreMark | 4.09 CoreMark/MHz；官方655 CoreMark（对应160MHz） | DS13633 Rev 3 (May 2025) p.1 Benchmarks | 官方数据手册摘要，非本次实测；具体编译器/选项/执行位置未在摘要披露，因此不可视为同条件实测排名。 |
| Runtime Efficiency | SMPS典型45 µA/MHz（7.15mA/160MHz，3.3V，CoreMark）；同条件LDO典型91 µA/MHz（14.5mA）。首页18.5µA/MHz为24MHz精简while循环低耗配置，非160MHz CoreMark | DS13633 Rev 3 (May 2025) p.177 §5.1.2; p.193 Table40; p.195 Table41; p.197 Table43 | 典型值；160MHz PLL由HSE16MHz旁路，Range1，外设关闭，Flash执行、ICACHE 1-way与prefetch ON、bank2 power-down、全部SRAM ON。18.5条件：3.3V,MSI24MHz,Range4,Flash bank1低功耗/bank2关闭,仅SRAM2开启。TA=25°C（p.177 §5.1.2典型值默认），所有I/O为模拟输入。 |
| Deep Sleep Current | 保RAM：Stop3+SMPS，全部SRAM保留，8.2 µA typ @3.3V/25°C、RTC关闭；最少8KB SRAM2+32KB ICACHE保留，2.0 µA typ @3.0V/25°C。Standby：0.195 µA typ @1.8V/25°C、RTC/IWDG关闭、ULPMEN=1；Shutdown：0.15 µA typ @1.8V/25°C、RTC关闭 | DS13633 Rev 3 (May 2025) p.221 Table59; p.224 Table61; p.228 Table62 | 不同电压不能直接排名；此处Standby/Shutdown数值对应仅备份寄存器保持的表格配置，不代表Standby不能另选SRAM2/backup SRAM保持；额外保持须另计电流。Stop3全RAM在3.3V最大29µA@30°C/260µA@85°C；最少RAM 3V最大7µA@30°C/61µA@85°C（特性表征，非量产逐颗测试）。此处为芯片电流，不含板级外设；Stop2自主外设功耗不能套用Stop3。 |

## 来源

[
  {
    "id": "DS13633",
    "title": "STM32U59xxx datasheet",
    "revision": "DS13633 Rev 3, May 2025",
    "local": "sources/DS_stm32u595ai.pdf",
    "origin": "用户上传原件；文件名stm32u595ai，Table1明确覆盖STM32U599VI",
    "official_url": "https://www.st.com/resource/en/datasheet/stm32u595ai.pdf",
    "access": "ST网站当前CONNECT403，未声称联网下载成功；内容从用户上传原件核验。"
  },
  {
    "id": "Zephyr",
    "version": "v4.2.0",
    "urls": [
      "https://github.com/zephyrproject-rtos/zephyr/blob/v4.2.0/soc/st/stm32/stm32u5x/Kconfig.soc",
      "https://github.com/zephyrproject-rtos/zephyr/blob/v4.2.0/dts/arm/st/u5/stm32u599Xi.dtsi",
      "https://github.com/zephyrproject-rtos/zephyr/blob/v4.2.0/dts/arm/st/u5/stm32u599.dtsi"
    ],
    "access": "已通过官方raw.githubusercontent.com下载本地核验。"
  }
]

未访问用户E盘；未知项保留未知。Zephyr仅文档/源码支持核验，未编译或硬件测试。独立审核记录见 reviews/STM32U599VIT6Q.md。
