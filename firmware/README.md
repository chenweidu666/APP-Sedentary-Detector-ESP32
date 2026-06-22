# 固件联调指南

**平台**：ESP32-WROOM-32 + IO 扩展板  
**传感器**：**GY-87**（固件仅使用板载 MPU6050 @ `0x68`）

## 接线（已验证）

| GY-87 丝印 | 扩展板 | GPIO |
| ---------- | ------ | ---- |
| VCC_IN | **3V3** | — |
| GND | **GND** | — |
| SCL | **D5** | GPIO5 |
| SDA | **D4** | GPIO4 |
| INTA | **D2** | GPIO2（可选） |
| 3.3V / FSYNC / DRDY | 不接 | — |

载板 J2 引脚顺序见 [hardware/HARDWARE.md](../hardware/HARDWARE.md)。

## 安装姿态

**竖直立放**（台架）：基准角约 **±90°**（实测 ~97°），看 **delta** 判人。  
**水平安装**（坐垫下）：基准约 0°，装好后串口 `c` 重校准。

## 编译烧录

```bash
cd Projects/02-APP-Sedentary-Detector-ESP32/firmware
pio run -t upload
pio device monitor
```

`MOCK_MPU6050=0`（已接 GY-87）；无传感器时改为 `1`。

## 串口示例

```
[I2C] scan SDA=GPIO4 SCL=GPIO5
  device 0x68
  device 0x77
[MPU6050] connected
[CAL] baseline=97.17 deg (mock=0)
```

`0x77` = GY-87 上 BMP180，可忽略。

## 命令

| 键 | 作用 |
|----|------|
| `r` | I2C 重扫 |
| `c` | 重校准 |
| `m` | 切换 mock（无 MPU 时） |

详细架构见 [FIRMWARE.md](FIRMWARE.md)。
