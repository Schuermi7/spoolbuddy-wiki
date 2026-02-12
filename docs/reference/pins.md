# Pin Reference

Quick reference for all SpoolBuddy hardware connections.

---

## System Overview

| Controller | Components | Interface |
|------------|------------|-----------|
| CrowPanel (ESP32-S3) | Display, NAU7802 Scale | I2C |
| Raspberry Pi Pico | PN5180 NFC Reader | SPI |

For Pin-Connections refer to [SpoolBuddy Pin Wiring](../getting-started/wiring.md)

**SPI Configuration:**

- Mode: SPI Mode 0 (CPOL=0, CPHA=0)
- Speed: 1-7 MHz
- Bit Order: MSB First

**I2C Configuration:**

- Address: 0x2A
- Speed: 400 kHz (Fast mode)

---

## CrowPanel Reserved GPIOs

Do not use these pins on the CrowPanel:

| GPIO | Reason |
|------|--------|
| IO4, IO5, IO6 | LCD Display (J9) |
| IO0 | Boot button |
| IO3 | USB D- |
| IO45 | USB D+ |
| IO38-42 | PSRAM/Flash |

---

## Voltage Reference

| Rail | Voltage | Max Current | Components |
|------|---------|-------------|------------|
| CrowPanel USB | 5.0V | 2A | CrowPanel input |
| CrowPanel 3.3V | 3.3V | 500mA | NAU7802 |
| Pico USB | 5.0V | 500mA | Pico input |
| Pico 3V3 | 3.3V | 300mA | PN5180 |

!!! danger
    PN5180 is **3.3V ONLY**. 5V will damage it!
