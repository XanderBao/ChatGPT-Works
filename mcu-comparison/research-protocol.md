# MCU 数据搜索与审核口径

目标：补齐用户截图中的 STM32U073CCU6、STM32L431RCT6、STM32U599VIT6Q 三列，不改动前两列。分析和审核分别由三个独立 agent 执行。

## 来源与检索顺序
1. 用户本地路径 E:\Pyronix\11、资料存档\7、供货商\5、ST\Low power MCU 当前不可从云机器直接访问；如用户上传，保存原件并核对版本。不可声称已查阅本地文件。
2. 优先 ST 官方精确型号产品页、数据手册、参考手册、勘误、应用笔记。下载至 sources/，用 pdftotext -layout 提取。来源写明 URL、文档编号/修订版、印刷页或章节/表号。
3. Zephyr 支持只使用 Zephyr 官方代码或文档，区分 SoC 支持、板卡支持与自定义板适配，记录版本/提交。
4. 官方无法访问时可用可信镜像原版手册，明确镜像来源和版本；不能凭产品系列、搜索摘要或相邻型号猜测。

已获用户上传的三份原始 PDF，存于 sources/DS_stm32u073cc.pdf、DS_stm32l431rc.pdf、DS_stm32u595ai.pdf，文本为同名 .txt。优先查这三份文件，并核对首页覆盖型号和修订版本。www.st.com 当前代理 CONNECT 403；无需要求用户重复提供手册，也不为了这次研究修改云环境配置。raw.githubusercontent.com 可访问，用于 Zephyr 官方核验。引用采用 PDF 文件、印刷页码/表号；ST 对应原始 URL 可作为文档定位，不声称在线下载成功。

## 所有分析均覆盖以下行
Architecture; Frequency; GPIO Count; Manufacturing process; Internal oscillator accuracy; Flash / RAM; EEPROM; XIP; Boot ROM / Secure Boot; Secure Key Store; TrustZone; OTP / eFuse; Crypto Accelerator; Hash Accelerator; TRNG; RDP; USART; UART; I2C; I3C; SPI / I2S; CANFD; Ethernet; SDIO; USB; ADC; DAC; Comparator; Op-Amp; DMA; Zephyr; CoreMark; Runtime Efficiency; Deep Sleep Current.

## 统一解释
- 仅填写精确料号的容量、封装和外设；封装 GPIO 数与系列最大值分开。用户 HQ 字样不作封装事实依据，先解码完整料号。
- USART、UART、LPUART 分列说明，避免重复计数。SPI/I2S 的复用计数、ADC/DAC 单元与通道数分开。
- Boot ROM 下载器不等于安全启动；软件加密不等于硬件加速；支持 Flash EEPROM 仿真不等于具有独立 EEPROM。
- 内部振荡器标明 HSI/MSI/HSI48、精度和温度/电压条件，不混用典型/最大保证值。
- CoreMark 注明频率、执行位置、编译或官方报告条件；未有可信原始结果写未知。
- 运行电流和深睡电流必须附模式、电压、温度、时钟、RAM/RTC/稳压器条件，且典型/最大值分开。不同条件不作直接能效排名。
- 未披露工艺、无足够证据的安全密钥库、测试成绩等直接标未知。明确区分“不支持”“未知”和“需软件实现”。

## 交付格式与审核
分析 agent 写 analysis/<料号>.md 和 analysis/<料号>.json。JSON 包含 part、package、rows（每项 item/value/source/conditions）、sources。逐行有来源或明确未知。
审核 agent 读原始文档独立核验，修订各自分析文件并写 reviews/<料号>.md，记录错误、修正、未决项。不得改其他芯片文件。
主 agent 汇总为中文 Markdown、CSV 和可供表格软件打开的文件，并保留过程文档与原始证据。
