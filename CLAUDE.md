# VisRing 项目复现

## 项目结构

```
Ring/                  ← TaitingLu/VisRing (硬件)
├── PCB/               ← 原理图 + Gerber + BOM + SMT 坐标
├── Case/              ← 3D 打印外壳 STL
└── Firmware/          ← Gmail BLE 通知 demo (.ino)

VisRing_Firmware/      ← ChristianKrauter/VisRing (固件库)
├── src/               ← 核心驱动库 (OLED + 传感器 + BLE)
└── Examples/          ← 可视化示例 + Demo
```

## 硬件架构

- MCU: nRF52832 (QFN48, Cortex-M4, 512KB Flash, 64KB RAM)
- BLE: Nordic S132 SoftDevice
- OLED: SSD1320 控制器, 160×32, 4-bit 灰度, SPI (9-bit 3-wire)
- PPG 心率: MAX30101 (I²C 0x57)
- IMU 9轴: ICM-20948 (SPI, CS=P0.02)
- 温度: TMP117 (I²C)
- PMIC: MIC5370-SGYMT (LDO)
- 升压: AP3012KTR-G1 (OLED 驱动)
- 电池保护: DW01A + FS8205
- 天线: 2450AT18B100E 陶瓷天线

## OLED 引脚映射

| SSD1320 | nRF52832 |
|---------|----------|
| CS      | P0.15    |
| RST     | P0.16    |
| SCLK    | P0.12    |
| SDIN    | P0.13    |

## IMU 引脚

| ICM-20948 | nRF52832 |
|-----------|----------|
| CS        | P0.02    |
| SCLK/MOSI/MISO | 共用 SPI |

## 开发环境

- Arduino IDE + Adafruit nRF52 BSP
- 编译器: arm-none-eabi-gcc
- 烧录: J-Link/DAPLink → SWCLK/SWIO 测试点
- OTA: Adafruit Bluefruit APP → BLE DFU

## 依赖库

Arduino 库管理器安装:
- Adafruit Bluefruit nRF52
- SparkFun MAX3010x
- SparkFun ICM-20948
- Adafruit TMP117
- Adafruit Sensor
- TimeLib

手动安装:
- movingAvg (https://github.com/JChristensen/movingAvg)

## PCB 制造

- 发 Gerber: PCB/VisRing_PCB_Gerber.zip
- 按 BOM 采购: PCB/VisRing_PCB_BOM.csv (40 种物料)
- SMT 坐标: PCB/SMT Document/ (top + bottom 面)
- 3D 打印: Case/inner_case.STL + Case/outer_case.STL
