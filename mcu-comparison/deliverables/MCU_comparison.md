# STM32 三款精确料号参数对比

数据依据用户上传PDF及官方GitHub补证；不是硬件实测。功耗均须连同条件阅读；三款工艺未在所查手册披露。前两列由用户完成，本报告不复核。

## 封装与范围

- **STM32U073CCU6**：UFQFPN48，7×7 mm，-40~85°C（6）；48 引脚、39 GPIO；DS §7.5 pp.120–121，§8 p.135
- **STM32L431RCT6**：LQFP64 (10×10mm)，−40～85°C（105°C结温）；R=64pin，C=256KB，T=LQFP，6=工业温度级
- **STM32U599VIT6Q**：LQFP100 (14×14mm)，Q=内置SMPS专用引脚版本；6=TA−40…85°C；I=2MB Flash

## 完整对比表（34项）

| 类别 / 项目 | STM32U073CCU6 | STM32L431RCT6 | STM32U599VIT6Q |
|---|---|---|---|
| Core / 内核架构 (Architecture) | Arm Cortex-M0+，32 位<br>【来源】U073-DS p.1、§2 p.4 | Arm Cortex-M4，单精度FPU、DSP、MPU<br>【来源】[L-DS] DS11453 Rev 3 p1、§3.1 p17 | Arm Cortex-M33，TrustZone、DSP、单精度 FPU、MPU<br>【来源】DS13633 Rev 3 (May 2025) pp.1,23 §3.1<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Core / 最高频率 (Frequency) | 最高 56 MHz<br>【来源】U073-DS p.1、Table 1 p.5 | 最高80 MHz<br>【来源】[L-DS] DS11453 Rev 3 表2 p14 | 最高 160 MHz<br>【来源】DS13633 Rev 3 (May 2025) p.20 Table 2<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Core / GPIO 数量 (GPIO Count) | 39 GPIO（48 引脚封装）<br>【来源】U073-DS Table 1 p.5；U073-PIN IONb=39<br>【条件】与电源脚、晶振、调试、外设复用有关；并非 48 个 GPIO | 52个（LQFP64）<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、图8 p56<br>【条件】GPIO复用、晶振和调试接口占用需在实际设计中扣除 | 79 GPIO（100引脚、带SMPS的Q专用引脚版本）；无SMPS的对应封装为82<br>【来源】DS13633 Rev 3 (May 2025) p.20 Table 2; p.98 Fig.9; p.381 §7<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Core / 制造工艺 (Manufacturing process) | 未知（数据手册未披露）<br>【来源】U073-DS 全文；U073-PIN IP Version 不作为芯片制造工艺依据 | 未知：本次原始资料未披露工艺节点<br>【来源】[L-DS] DS11453 Rev 3 全文检索；未检出 | 未知（本数据手册未披露）<br>【来源】未知；DS13633 未披露，不从U5系列推断<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Core / 内部振荡器精度 (Internal oscillator accuracy) | HSI16：16 MHz，首页标称 ±1%；详细频漂为 0~85°C ±1%，-40~125°C -2%/+1.5%。MSI：100 kHz~48 MHz，可用 LSE 自动校准，首页称优于 ±0.25%；HSI48 未经 CRS 同步时，工厂校准精度：±3%（-15~85°C、3.0~3.6V）或 ±4.5%（-40~125°C、1.65~3.6V）<br>【来源】U073-DS p.2；Table 52 p.73；Table 53 pp.74–76；Table 54 p.77<br>【条件】HSI16 在 VDD=3.0V、TA=30°C 为 15.88~16.08MHz；温漂和电压漂移为独立参数，不能把 ±1% 当全温总误差；具体料号 -40~85°C。MSI 精度须满足 LSE 校准条件。 HSI16/MSI 表值按表声明来自特性测试而非逐颗生产测试；USB免晶振依赖CRS/USB SOF同步条件，不能用未同步HSI48误差代表同步后的USB时钟误差。 | HSI16温漂：0～85°C为±1%，−40～125°C为−2%/+1.5%；MSI需LSE校准才可引用优于±0.25%；HSI48工厂校准精度：−40～125°C、1.65～3.6V为±4.5%<br>【来源】[L-DS] DS11453 Rev 3 表48 p119；表49 pp121–123；表50 p124；p1<br>【条件】HSI16：16MHz；VDD=3V、TA=30°C时15.88～16.08MHz；电压漂移−0.1/+0.05%（1.62～3.6V）；温漂不是含初始误差及电压漂移的总误差。MSI未LSE校准时温漂为0～85°C −3.5%/+3%，−40～125°C −8%/+6%，还需考虑初始校准及电压漂移。HSI48在3.0～3.6V、−15～85°C为±3%。料号6额定环境温度为−40～85°C。 | HSI16：16 MHz；工厂校准±0.5% @VDD=3.0V,TJ=30°C；±1% @TJ=−10…100°C；全结温−40…130°C为15.65…16.25MHz（−2.19%/+1.56%）；另有MSI 100kHz–48MHz，LSE自动校准时首页标称优于±0.25%<br>【来源】DS13633 Rev 3 (May 2025) p.245 Table 73；p.2 Clock management；pp.247–250 Table 74<br>【条件】宽温两行VDD=1.58…3.6V；该型号6温度级TA=−40…85°C、TJ上限105°C；这是结温参数，不能把首页±1%套到整个温区。Table 73脚注1：特性表征，非量产逐颗测试，未计封装/焊接影响。 MSI标称精度要求LSE自动校准；不能套用于未经校准的自由运行MSI（其温漂/压漂另见Table74）。 |
| Memory / Flash / RAM (Flash / RAM) | 256 KB Flash / 40 KB SRAM（32 KB SRAM1 + 8 KB SRAM2）<br>【来源】U073-DS §3.4 pp.8–9，§8 p.135；U073-PIN Flash/Ram<br>【条件】料号第二个 C=256KB；SRAM2 可在 Standby 保持 | 256 KB Flash / 64 KB SRAM（48 KB SRAM1 + 16 KB SRAM2）<br>【来源】[L-DS] DS11453 Rev 3 表2 p13，§3.5 p19，表105 p206 | 2 MB Flash；2512 KB系统SRAM + 2 KB backup SRAM = 2514 KB（SRAM3 ECC关闭）；ECC开启后总2450 KB<br>【来源】DS13633 Rev 3 (May 2025) pp.1,18 Table 2; p.29 §3.5; p.381 §7<br>【条件】I=2MB，J才是4MB；SRAM3 ECC开启减少64KB。缓存不额外计入RAM。 |
| Memory / EEPROM (EEPROM) | 无独立数据 EEPROM；Flash 仿真实现与容量未知<br>【来源】U073-DS §3.4 pp.8–9、存储映射 §5<br>【条件】不能将主 Flash 或 OTP 直接计为 EEPROM | 无独立EEPROM；数据存储可使用内部Flash，EEPROM仿真需软件<br>【来源】[L-DS] DS11453 Rev 3 §3.4～3.5 pp18–19，图15 p80<br>【条件】没有为EEPROM预留的独立容量；寿命、掉电一致性需另作软件设计 | 无独立EEPROM；Flash可用于软件仿真；最多512 KB Flash区域支持100 kcycles（并非独立EEPROM容量）<br>【来源】DS13633 Rev 3 (May 2025) p.25 §3.4; p.254 Table 79<br>【条件】具体EEPROM仿真软件/磨损均衡方案未核验；100k为Flash耐久能力，不能写成512KB EEPROM。 |
| Memory / 外部 XIP (XIP) | 无专用外部存储器映射/XIP 接口<br>【来源】U073-DS Table 1 pp.4–5、Figure 1 p.6、§3.29 p.25<br>【条件】SPI 传输不等于外部 Flash XIP | 支持外部Quad-SPI存储器映射，可用于外部代码执行<br>【来源】[L-DS] DS11453 Rev 3 §3.33 p53、图15 p80<br>【条件】1个QUADSPI；需兼容外部Flash及初始化，不代表上电直接从外部Flash安全启动 | 支持外部映射执行：2×OCTOSPI（此封装仅muxed模式）；本LQFP100无HSPI<br>【来源】DS13633 Rev 3 (May 2025) pp.18,21 Table 2 notes 1–2; pp.23,62–63 §§3.2.1,3.27<br>【条件】OCTOSPI memory-mapped；共享引脚/时分复用，不是两条独立全宽总线。FSMC该封装仅bank1复用NOR/PSRAM。 |
| Security / Boot ROM / 安全启动 (Boot ROM / Secure Boot) | 有 System Memory ROM 下载器；支持用户 Flash 安全启动隔离区（HDP/securable area），不等同于 ROM 内置验签安全启动<br>【来源】U073-DS §3.4.1 p.8、§3.5 p.9；p.2 Secure boot<br>【条件】ROM 下载支持 USART/I2C/SPI/USB；安全启动认证算法/签名实现未在手册明确 | ROM系统存储器下载器：有；原生ROM认证安全启动：未知/未在所查DS说明<br>【来源】[L-DS] DS11453 Rev 3 §3.7 p20<br>【条件】ROM下载接口USART/I2C/SPI/CAN；不能将其视为认证安全启动 | ST出厂系统存储器bootloader；提供RSS/SFI、TrustZone+HDP启动信任根与TF-M固件升级支持；应用须实现/配置认证启动，不能将ROM下载器等同验签安全启动<br>【来源】DS13633 Rev 3 (May 2025) p.2 Security; p.31 §3.7<br>【条件】是否满足具体产品的安全启动/密钥生命周期需求需按TF-M/RSS方案评估。 |
| Security / 安全密钥存储 (Secure Key Store) | 有 HDP 可隐藏代码/数据区；专用硬件密钥库未知<br>【来源】U073-DS p.2、§3.4.1 p.8<br>【条件】HDP 不能直接等同于独立硬件密钥存储 | 专用硬件密钥库：未知/未声明；有Firewall保护代码/Flash数据/SRAM1数据<br>【来源】[L-DS] DS11453 Rev 3 §3.6 p19<br>【条件】Firewall不是专用密钥存储单元；文档要求RDP Level2以达到预期保护等级 | 专用硬件密钥库未知；有TrustZone、HDP和512B OTP保护资源，未找到HUK/SAES式专用密钥存储证据<br>【来源】DS13633 Rev 3 (May 2025) pp.2,25–29 §§3.4–3.6<br>【条件】不能把HDP/普通OTP直接等同不可导出密钥库。 |
| Security / TrustZone (TrustZone) | 不支持（Cortex-M0+）<br>【来源】U073-DS §3.1 p.7；U073-CMSIS core_cm0plus.h include<br>【条件】Armv6-M 无 Armv8-M TrustZone | 不支持Arm TrustZone<br>【来源】[L-DS] DS11453 Rev 3 §3.1 p17；[L-CMSIS] Configuration_section<br>【条件】Cortex-M4；Firewall/MPU不等同TrustZone | 支持 Arm TrustZone（安全/非安全隔离）<br>【来源】DS13633 Rev 3 (May 2025) p.29 §3.6<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Security / OTP / eFuse (OTP / eFuse) | 1 KB 用户 OTP；eFuse 未明确披露<br>【来源】U073-CMSIS FLASH_OTP_BASE=0x1FFF6800、FLASH_OTP_SIZE=0x400（L804–805）；U073-ZEPHYR stm32u0.dtsi st_nvm_user_otp<br>【条件】数据手册未给出独立 OTP 章节，容量由 ST 官方 CMSIS 与 Zephyr 交叉核实 | 1 KB OTP区（地址0x1FFF7000～0x1FFF73FF）；通用eFuse阵列未知<br>【来源】[L-DS] DS11453 Rev 3 图15 p80<br>【条件】容量由原始地址映射区间计算；RDP Level2不可逆调试禁用不等同通用eFuse容量 | 512 B 用户OTP；eFuse独立容量未知<br>【来源】DS13633 Rev 3 (May 2025) p.25 §3.4<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Security / 加密加速器 (Crypto Accelerator) | 无 AES/PKA 硬件加速器<br>【来源】U073-DS Figure 1 p.6、功能概览；U073-PIN IP 清单<br>【条件】不可因 CMSIS 中共用 IRQ 名 RNG_CRYP 就认定存在 AES | 无AES/PKA等硬件加速器<br>【来源】[L-DS] DS11453 Rev 3 图1 p16、完整功能章节；[L-CMSIS] peripheral declarations<br>【条件】软件加密另行实现；CRC不算密码加速器 | 无AES/PKA硬件加速器；HASH与TRNG另列<br>【来源】DS13633 Rev 3 (May 2025) p.2 Features; pp.18–22 Table 2/框图；功能章节列表<br>【条件】STM32U599不能套用带AES/PKA的STM32U5A9等相邻产品；软件密码算法另行实现。 |
| Security / Hash 加速器 (Hash Accelerator) | 无专用 SHA/HASH 硬件加速器；有 CRC<br>【来源】U073-DS p.2 CRC calculation unit、Figure 1 p.6、功能概览；U073-PIN IP 清单<br>【条件】CRC 不等于密码学 Hash | 无SHA等硬件Hash加速器；有CRC<br>【来源】[L-DS] DS11453 Rev 3 图1 p16、§3.8 p20；[L-CMSIS] peripheral declarations<br>【条件】CRC不是安全Hash | 有：SHA-1、SHA-224、SHA-256、MD5、HMAC<br>【来源】DS13633 Rev 3 (May 2025) p.78 §3.43<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Security / TRNG (TRNG) | 支持，32 位真随机输出<br>【来源】U073-DS §3.23 p.21；p.2<br>【条件】Rev 2 首页为 NIST SP800-90B candidate，不声称已认证 | 支持硬件真随机数发生器<br>【来源】[L-DS] DS11453 Rev 3 §3.21 p42 | 支持；NIST SP800-90B熵源，32-bit输出<br>【来源】DS13633 Rev 3 (May 2025) p.78 §3.42<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Security / RDP (RDP) | 支持 Level 0/1/2，128 位密码回退机制；Level 2 不可逆<br>【来源】U073-DS §3.4.1/Table 2 p.8<br>【条件】量产前必须单独验证回退配置和选项字节策略 | 支持Level0/1/2；Level2不可逆<br>【来源】[L-DS] DS11453 Rev 3 §3.4 p18 | 支持，含RDP 0 / 0.5 / 1 / 2及密码保护调试生命周期<br>【来源】DS13633 Rev 3 (May 2025) pp.26–28 §3.4.1<br>【条件】RDP0.5仅在TZEN=1时可用。Level2默认（未配置回退密钥）不可逆；如在更低RDP级预配置安全选项中的密钥，可经JTAG/SWD密码认证回退至Level1（DS p.26）。不能统一写成Level2必然不可逆。 |
| Comm. / USART (USART) | 4 × USART（USART1~4）<br>【来源】U073-DS Table 1 p.4、§3.27/Table 9 pp.24–25<br>【条件】含同步 SPI 模式；不计入下行 LPUART | 3：USART1/2/3<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、§3.25与表12 p48；[L-CMSIS] IRQn<br>【条件】首页写4x USARTs，与表2/正文冲突；按详细正文及芯片寄存器定义填写，不重复计入UART | 4：USART1/2/3/6<br>【来源】DS13633 Rev 3 (May 2025) p.19 Table 2; p.86 Table 21<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Comm. / UART / LPUART (UART) | 3 × LPUART；无另列普通 UART IP<br>【来源】U073-DS Table 1 p.4、§3.28 p.25<br>【条件】加上 4 个 USART 共 7 个异步串口接口 | 独立普通UART：0；另有1×LPUART1；3个USART也可异步通信<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、§3.25 p48、§3.26 p49；[L-CMSIS] IRQn | 2：UART4/5；另1×LPUART1（串口总计7，不重复计数）<br>【来源】DS13633 Rev 3 (May 2025) p.19 Table 2; p.86 Table 21<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Comm. / I2C (I2C) | 4 × I2C，最高 1 Mbit/s Fast-mode Plus<br>【来源】U073-DS p.2、Table 1 p.4、§3.26 pp.23–24 | 3×I2C，最高Fm+ 1 Mbit/s<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、§3.24/表11 p47 | 6×I2C（FM+最高1 Mbit/s）<br>【来源】DS13633 Rev 3 (May 2025) pp.2,19 Table 2<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Comm. / I3C (I3C) | 不支持<br>【来源】U073-DS Table 1 pp.4–5、Figure 1 p.6 | 不支持；仅列出I2C<br>【来源】[L-DS] DS11453 Rev 3 图1 p16、§3.24 p47；[L-CMSIS] peripheral declarations | 不支持（本型号无I3C控制器）<br>【来源】DS13633 Rev 3 (May 2025) p.2接口清单；pp.18–22 Table 2/框图<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Comm. / SPI / I2S (SPI / I2S) | 3 × SPI；I2S 不支持；另有 4 个 USART 可工作于同步 SPI 模式<br>【来源】U073-DS p.2、Table 10 p.25<br>【条件】SPI1/2/3 均无 I2S；不能把 USART SPI 模式当作额外独立 SPI 控制器 | 3×SPI；另1×SAI可承载I2S；1×QUADSPI另计<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、§3.27～3.28 p50、表13 p51<br>【条件】不能把SAI和SPI当成同一实例；DS未声明SPI带I2S模式 | 3×SPI；I2S由2×SAI支持（不是把SPI另算作I2S单元）<br>【来源】DS13633 Rev 3 (May 2025) p.19 Table 2; pp.89–91 §§3.48–3.49, Table 23<br>【条件】OCTOSPI专用存储器接口另列，不计为普通SPI。 |
| Comm. / CAN FD (CANFD) | 不支持；亦无经典 CAN<br>【来源】U073-DS Table 1 pp.4–5、Figure 1 p.6 | 不支持CAN FD；有1×经典CAN 2.0B Active<br>【来源】[L-DS] DS11453 Rev 3 §3.30 p51；[L-ZEPHYR-SOC] can1 compatible st,stm32-bxcan | 1×FDCAN<br>【来源】DS13633 Rev 3 (May 2025) p.19 Table 2<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Comm. / Ethernet (Ethernet) | 不支持<br>【来源】U073-DS Table 1 pp.4–5、Figure 1 p.6 | 不支持片上Ethernet MAC<br>【来源】[L-DS] DS11453 Rev 3 图1 p16、表2 pp13–15；[L-CMSIS] peripheral declarations | 不支持（无以太网MAC）<br>【来源】DS13633 Rev 3 (May 2025) p.2接口清单；p.22 Fig.1<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Comm. / SDIO / SDMMC (SDIO) | 不支持（无 SDIO/SDMMC）<br>【来源】U073-DS Table 1 pp.4–5、Figure 1 p.6 | 1×SDMMC（支持SD/SDIO/MMC）<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、§3.31 p52<br>【条件】R封装有SDMMC；不应套用C/K封装无SDMMC结论 | 2×SDMMC，支持SD/SDIO/eMMC；同时使用有限制<br>【来源】DS13633 Rev 3 (May 2025) p.19 Table 2; p.21 note3; pp.91–92 §3.50<br>【条件】Table2脚注3概括：同时使用时其中一路仅支持SDIO。§3.50进一步规定：仅一路可用8-bit eMMC；SDMMC1使用SDIO UHS-I电压切换与SDMMC2的8-bit eMMC互斥；SDMMC2使用8-bit eMMC时，SDMMC1只支持SDIO默认/高速模式。最终仍须按本封装引脚复用核验。 |
| Comm. / USB (USB) | USB 2.0 Full Speed Device，12 Mbit/s，支持免晶振；不支持 Host<br>【来源】U073-DS §3.30 p.26<br>【条件】内置 PHY、1KB packet RAM；USB SOF 可校准内部 48MHz 时钟 | 无USB控制器/PHY<br>【来源】[L-DS] DS11453 Rev 3 表2、图1 p16；[L-CMSIS] IRQn及外设地址<br>【条件】HSI48/CRS的存在不表示有USB；部分低功耗表出现USB_FS模板文字不能作为USB存在证据 | 1×USB 2.0 OTG High-Speed（内置HS PHY，Host/Device）；另1×UCPD Type-C/PD控制器<br>【来源】DS13633 Rev 3 (May 2025) p.2; p.19 Table 2; pp.93–95 §§3.52–3.53<br>【条件】HS最高480 Mbit/s；内置PHY PLL需精确60MHz时钟，时钟源必须使用HSE晶体振荡器（DS p.93），不能因内置PHY便认为免外部晶体。Type-C/PD控制器不应计为第二个USB数据接口。 |
| Analog / ADC (ADC) | 1 × 12 位 ADC；UFQFPN48 可引出 10 个外部模拟输入（PA0~PA7、PB0、PB1）；硬件过采样可到 16 位<br>【来源】U073-DS §3.16 pp.18–19；引脚表 Table 12 pp.32–33；U073-PIN ADC1_IN 引脚清单<br>【条件】单 ADC 单元，非原生 16 位。Table 1 p.5 将 Cx 归入16通道栏，但精确 UFQFPN48 未引出 PC0~PC5，实际只有10路；不能把系列/ADC内部通道数直接当封装可用输入数。 引出对应：PA0(pin10)=IN4、PA1(11)=IN5、PA2(12)=IN6、PA3(13)=IN7、PA4(14)=IN8、PA5(15)=IN9、PA6(16)=IN10、PA7(17)=IN14、PB0(18)=IN17、PB1(19)=IN18；PC0~PC5 在 UFQFPN48 栏均为“—”。 | 1×12-bit ADC，LQFP64提供16个外部通道，最高5.33 Msps（首页概括5 Msps）；硬件过采样可至16-bit<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、§3.15 p38<br>【条件】内部温度/VREFINT/VBAT测量另计；过采样不等于原生16-bit ADC | 2×14-bit ADC + 1×12-bit ADC（各最高2.5 MSPS）；本SMPS LQFP100有18个外部模拟输入引脚<br>【来源】DS13633 Rev 3 (May 2025) pp.2,20 Table 2; pp.65–68 §3.31/Table 15<br>【条件】18为本SMPS LQFP100封装可用外部ADC输入数量，非每颗ADC各18路；ADC1/2为14位，ADC4为12位，ADC4可自主运行至Stop2；内部通道/过采样不另算原生分辨率。 |
| Analog / DAC (DAC) | 1 × 12 位 DAC 输出通道<br>【来源】U073-DS p.1、Table 1 p.5、§3.17 p.19<br>【条件】ST 通用 DAC 结构中出现双通道寄存器不代表该型号双通道 | 2路12-bit DAC输出（1个双通道DAC块）<br>【来源】[L-DS] DS11453 Rev 3 表2 p14、§3.16 p39 | 1×12-bit DAC模块，2个输出通道<br>【来源】DS13633 Rev 3 (May 2025) p.20 Table 2<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Analog / 比较器 (Comparator) | 2 × 超低功耗比较器<br>【来源】U073-DS p.1、Table 1 p.5、§3.19 p.20 | 2个超低功耗比较器<br>【来源】[L-DS] DS11453 Rev 3 §3.18 p41 | 2×超低功耗比较器<br>【来源】DS13633 Rev 3 (May 2025) pp.2,20 Table 2<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| Analog / 运放 (Op-Amp) | 1 × 运放，内置 PGA，最高 16 倍增益<br>【来源】U073-DS p.1、Table 1 p.5、§3.20 p.20 | 1个，带PGA<br>【来源】[L-DS] DS11453 Rev 3 §3.19 p41 | 2×运放，内置PGA<br>【来源】DS13633 Rev 3 (May 2025) pp.2,20 Table 2<br>【条件】精确型号 STM32U599VIT6Q；外设复用须核对引脚表，数量不代表可同时使用。 |
| DMA / DMA (DMA) | 2 个 DMA 控制器：DMA1 7 通道 + DMA2 5 通道，共 12 通道，带 DMAMUX<br>【来源】U073-DS p.2、Figure 1 p.6；U073-CMSIS DMA IRQ 与 DMA1_Channel1~7、DMA2_Channel1~5；U073-ZEPHYR stm32u073.dtsi dma2<br>【条件】DS §3.12 p.17 仅称 seven channels，属于概述不完整；总数以首页、框图和官方实现交叉核验。 | 2×DMA控制器，每个7通道，共14通道<br>【来源】[L-DS] DS11453 Rev 3 §3.13 p36；[L-CMSIS] DMA1/2 Channel1～7 IRQn | 1×GPDMA（16通道）+1×LPDMA（4通道）；另有图形DMA2D<br>【来源】DS13633 Rev 3 (May 2025) pp.53–58 §§3.17–3.20, Tables11–14<br>【条件】GPDMA自主支持Sleep/Stop0/Stop1；LPDMA支持到Stop2；不将DMA2D换算为普通DMA通道。 |
| RTOS / Zephyr (Zephyr) | 有上游 SoC 支持（STM32U073 / Xc）；精确 CCU6 自定义板仍需引脚、时钟、外设适配与构建验证<br>【来源】U073-ZEPHYR stm32u073.dtsi、stm32u073Xc.dtsi<br>【条件】核验提交 bc5d59d1b6657cfeef30124b818c8fbd69da68ac；未宣称已对该封装板完成编译/硬件运行。 | 有STM32L431 SoC及256KB/64KB器件描述支持；本精确封装需自定义板/引脚及驱动验证<br>【来源】[L-ZEPHYR-XC] 与 [L-ZEPHYR-SOC]，提交 bc5d59d1b6657cfeef30124b818c8fbd69da68ac<br>【条件】仅核实源码层SoC支持，未进行板级编译或硬件运行；不能声称存在该精确料号的现成开发板支持 | 支持STM32U599 SoC及2MB变体；自定义VIT6Q板仍需板级定义与引脚配置<br>【来源】Zephyr v4.2.0: soc/st/stm32/stm32u5x/Kconfig.soc (SOC_STM32U599XX); dts/arm/st/u5/stm32u599Xi.dtsi; stm32u599.dtsi<br>【条件】官方源文件已下载核验；此结论不是精确用户板卡已验证启动，也不是全外设驱动完备承诺。 |
| Power efficient / CoreMark (CoreMark) | 2.4 CoreMark/MHz；134 CoreMark @56 MHz（官方标称）<br>【来源】U073-DS p.1 Benchmarks<br>【条件】编译器/优化参数未披露；非本次实测；134 为首页取整数 | 273.55 CoreMark；3.42 CoreMark/MHz @80MHz（官方公布值）<br>【来源】[L-DS] DS11453 Rev 3 p1，§3.2 p17<br>【条件】DS未列完整编译器/优化选项，亦未明确该跑分的执行位置；§3.2的ART说明不能替代跑分条件；非本次实测 | 4.09 CoreMark/MHz；官方655 CoreMark（对应160MHz）<br>【来源】DS13633 Rev 3 (May 2025) p.1 Benchmarks<br>【条件】官方数据手册摘要，非本次实测；具体编译器/选项/执行位置未在摘要披露，因此不可视为同条件实测排名。 |
| Power efficient / 运行电流 / MHz (Runtime Efficiency) | 典型 52 μA/MHz（While(1)）；CoreMark 典型 76 μA/MHz @48MHz、1.8V，或 78 μA/MHz @3.0V<br>【来源】U073-DS p.1、Table 32 pp.54–55<br>【条件】LDO；25°C，Range1，MSI=48MHz，Flash 执行，ART/cache ON、prefetch OFF，全部外设关闭；52 为 1.8V 空循环条件，不能当真实工作负载电流 | 84 µA/MHz（SRAM运行、Flash关、26MHz、1.8V、25°C、Range2）；121 µA/MHz（Flash跑CoreMark、80MHz、3V、25°C、Range1、ART Cache开/Prefetch关）<br>【来源】[L-DS] DS11453 Rev 3 表4 pp24–26及注3；表29 p97；§6.1.2 p84；§6.3.5 p93<br>【条件】均为典型值；第二工况电流9.68mA。全部外设禁用、IO模拟输入；84并非80MHz Flash CoreMark功耗 | SMPS典型45 µA/MHz（7.15mA/160MHz，3.3V，CoreMark）；同条件LDO典型91 µA/MHz（14.5mA）。首页18.5µA/MHz为24MHz精简while循环低耗配置，非160MHz CoreMark<br>【来源】DS13633 Rev 3 (May 2025) p.177 §5.1.2; p.193 Table40; p.195 Table41; p.197 Table43<br>【条件】典型值；160MHz PLL由HSE16MHz旁路，Range1，外设关闭，Flash执行、ICACHE 1-way与prefetch ON、bank2 power-down、全部SRAM ON。18.5条件：3.3V,MSI24MHz,Range4,Flash bank1低功耗/bank2关闭,仅SRAM2开启。TA=25°C（p.177 §5.1.2典型值默认），所有I/O为模拟输入。 |
| Power efficient / 深睡电流 (Deep Sleep Current) | Stop 2 保持 SRAM：典型 0.695 μA（无 RTC）/0.825 μA（带 RTC）；3.0V 无 RTC 为 0.750 μA。另列：Standby 无 SRAM2 保持、无 RTC 30.5 nA；Shutdown 无 RTC 10 nA（均 1.8V/25°C，按详细表）<br>【来源】U073-DS Table 40 pp.60–62、Table 41 pp.62–64、Table 42 p.64；§3.7 p.12<br>【条件】25°C，前两值 VDD=1.8V、LCD 关闭；无 RTC EN_ULP=0，带 RTC LSE low-drive、EN_ULP=1、LPCAL=1；SRAM 保持。无 RTC 的最大值在30°C、1.8V为2.45μA，非25°C最大值。Standby 和 Shutdown 值 EN_ULP=0、无 IWDG（Standby）；Standby 保留 SRAM2 额外加89nA@1.8V/25°C。首页 Shutdown 为16nA，与详细 Table42 的10nA不同；采用可追溯完整条件的表值，不能用于保RAM深睡比较。 | 保64KB SRAM Stop2：1.06 µA典型/2.1 µA最大（3V、25°C、RTC关）；Standby：90.2 nA典型/225 nA最大（3V、25°C、RTC/IWDG关，不保SRAM）；Shutdown：44.3 nA典型/70.1 nA最大（3V、25°C、RTC关，不保SRAM）<br>【来源】[L-DS] DS11453 Rev 3 §3.9.4 pp27–28；表34 p100；表37 p104；表38 p105<br>【条件】Stop2中主时钟/PLL关，低功耗调压器工作；Stop2+RTC LSE低驱动典型1.59µA（3V、25°C）。Standby可选保16KB SRAM2但需另加电流；此处未启用。保留Backup registers，不能与保全RAM等同。 | 保RAM：Stop3+SMPS，全部SRAM保留，8.2 µA typ @3.3V/25°C、RTC关闭；最少8KB SRAM2+32KB ICACHE保留，2.0 µA typ @3.0V/25°C。Standby：0.195 µA typ @1.8V/25°C、RTC/IWDG关闭、ULPMEN=1；Shutdown：0.15 µA typ @1.8V/25°C、RTC关闭<br>【来源】DS13633 Rev 3 (May 2025) p.221 Table59; p.224 Table61; p.228 Table62<br>【条件】不同电压不能直接排名；此处Standby/Shutdown数值对应仅备份寄存器保持的表格配置，不代表Standby不能另选SRAM2/backup SRAM保持；额外保持须另计电流。Stop3全RAM在3.3V最大29µA@30°C/260µA@85°C；最少RAM 3V最大7µA@30°C/61µA@85°C（特性表征，非量产逐颗测试）。此处为芯片电流，不含板级外设；Stop2自主外设功耗不能套用Stop3。 |

## 来源索引

### STM32U073CCU6

- **U073-DS**：STM32U073x8/B/C Datasheet DS14548 Rev 2 (March 2024)，用户上传原件；https://www.st.com/resource/en/datasheet/stm32u073cc.pdf；本地：sources/DS_stm32u073cc.pdf
- **U073-PIN**：ST 官方 STM32_open_pin_data，精确 STM32U073CCUx.xml；https://github.com/STMicroelectronics/STM32_open_pin_data/blob/7d1f1514ed5583ec5007ad91236b4e1d377295b1/mcu/STM32U073CCUx.xml；本地：sources/u073cc_alt.xml
- **U073-CMSIS**：ST 官方 CMSIS STM32U073xx 头文件；https://github.com/STMicroelectronics/cmsis-device-u0/blob/5fca202fc2c5c5273238cfd27943dd95759a44a6/Include/stm32u073xx.h；本地：sources/u073_header.h
- **U073-ZEPHYR**：Zephyr 官方 STM32U073 device tree，main 提交 bc5d59d1b6657cfeef30124b818c8fbd69da68ac；https://github.com/zephyrproject-rtos/zephyr/blob/bc5d59d1b6657cfeef30124b818c8fbd69da68ac/dts/arm/st/u0/stm32u073.dtsi；本地：sources/zephyr_u073.dtsi

### STM32L431RCT6

- **L-DS**：STM32L431xx datasheet DS11453 Rev 3 (May 2018)；https://www.st.com/resource/en/datasheet/stm32l431rc.pdf；本地：sources/DS_stm32l431rc.pdf
- **L-CMSIS**：ST 官方 CMSIS STM32L431xx Header；https://raw.githubusercontent.com/STMicroelectronics/cmsis-device-l4/ca0bfa2b8b68dc2994b27fba0a10dfd28d086ee1/Include/stm32l431xx.h；本地：sources/stm32l431xx.h
- **L-ZEPHYR-XC**：Zephyr STM32L431Xc device tree；https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/bc5d59d1b6657cfeef30124b818c8fbd69da68ac/dts/arm/st/l4/stm32l431Xc.dtsi；本地：sources/zephyr-stm32l431Xc.dtsi
- **L-ZEPHYR-SOC**：Zephyr STM32L431 SoC device tree；https://raw.githubusercontent.com/zephyrproject-rtos/zephyr/bc5d59d1b6657cfeef30124b818c8fbd69da68ac/dts/arm/st/l4/stm32l431.dtsi；本地：sources/zephyr-stm32l431.dtsi

### STM32U599VIT6Q

- **DS13633**：STM32U59xxx datasheet；https://www.st.com/resource/en/datasheet/stm32u595ai.pdf；本地：sources/DS_stm32u595ai.pdf
- **Zephyr**：Zephyr v4.2.0；https://github.com/zephyrproject-rtos/zephyr/blob/v4.2.0/soc/st/stm32/stm32u5x/Kconfig.soc
https://github.com/zephyrproject-rtos/zephyr/blob/v4.2.0/dts/arm/st/u5/stm32u599Xi.dtsi
https://github.com/zephyrproject-rtos/zephyr/blob/v4.2.0/dts/arm/st/u5/stm32u599.dtsi；本地：

## 独立审核记录

### STM32U073CCU6

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


### STM32L431RCT6

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


### STM32U599VIT6Q

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

