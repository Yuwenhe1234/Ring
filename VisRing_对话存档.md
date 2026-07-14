# VisRing 对话存档 — 代码与硬件交叉验证

## 仓库对应关系

| 仓库 | 内容 |
|------|------|
| ChristianKrauter/VisRing (VisRing_Firmware/) | 固件核心库 + 13 种可视化示例 + Demo |
| TaitingLu/VisRing (Ring/) | PCB + 外壳 + Gmail 通知固件 |

## 交叉验证结论：代码完全对应硬件

### 逐项校验通过清单

| 硬件芯片 | BOM 编号 | 代码引用 | 接口 | 结果 |
|----------|---------|----------|------|------|
| nRF52832 | U2 | Bluefruit52Lib, S132 SoftDevice | - | ✅ |
| SSD1320 OLED | DS3 | SSD1320_OLED.h, CS=15,RST=16,SCLK=12,SDIN=13 | SPI 9-bit | ✅ |
| ICM-20948 | IC1 | ICM_20948.h, CS_PIN=2, SPI | SPI | ✅ |
| MAX30101 | U1 | MAX30105.h (兼容), I²C 0x57 | I²C | ✅ |
| TMP117 | U4 | Adafruit_TMP117.h | I²C | ✅ |
| MIC5370 | U$1 | 无代码（纯硬件 LDO） | - | ✅ |
| AP3012KTR | U5 | 无代码（纯硬件升压） | - | ✅ |
| DW01A+FS8205 | IC2+Q3 | 无代码（纯硬件保护） | - | ✅ |
| TXS0102 | U3 | 无代码（纯硬件电平转换） | - | ✅ |
| 陶瓷天线 | E1 | Bluefruit.setTxPower(4) | - | ✅ |
| 32MHz 晶振 | Y2 | nRF52 BSP 默认 | - | ✅ |
| 32.768kHz | Y1 | nRF52 BSP 默认 | - | ✅ |

### 注意项

- BOM 写 MAX30101，代码用 MAX30105 库 — 两者寄存器完全兼容，无需修改

## PCB 文件说明

- VisRing_PCB_Schematic.png — 原理图
- VisRing_PCB_Gerber.zip — 生产文件（发工厂）
- VisRing_PCB_BOM.csv — 总物料清单（40 种）
- SMT Document/top_bom + top_cpl — 顶层贴片清单和坐标
- SMT Document/bottom_bom + bottom_cpl — 底层贴片清单和坐标

## 环境：纯 Arduino 生态，无 ESP-IDF

- IDE: Arduino
- BSP: Adafruit nRF52 (开发板管理器安装)
- 编译器: arm-none-eabi-gcc
- 烧录: J-Link/DAPLink → SWCLK/SWIO

## 依赖库

Arduino 库管理器:
- Adafruit Bluefruit nRF52
- SparkFun MAX3010x
- SparkFun ICM-20948
- Adafruit TMP117 + Adafruit Sensor
- TimeLib

手动:
- movingAvg (JChristensen/movingAvg)
