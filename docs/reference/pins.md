# Pin Reference

Quick reference for all SpoolBuddy hardware connections.

---

## System Overview

| Controller | Components | Interface |
|------------|------------|-----------|
| CrowPanel (ESP32-S3) | Display, NAU7802 Scale | I2C |
| Raspberry Pi Pico | PN5180 NFC Reader | SPI |

---

## CrowPanel GPIO Usage

| GPIO | Function | Component | Header |
|------|----------|-----------|--------|
| IO19 | SDA | NAU7802 | UART1-OUT Pin 1 |
| IO20 | SCL | NAU7802 | UART1-OUT Pin 2 |

### UART1-OUT Header (4-pin)

```
┌──────┬──────┬──────┬──────┐
│ Pin1 │ Pin2 │ Pin3 │ Pin4 │
│IO19  │IO20  │ 3V3  │ GND  │
│ SDA  │ SCL  │ VCC  │ GND  │
└──────┴──────┴──────┴──────┘
```

| Pin | GPIO | SpoolBuddy Use |
|-----|------|----------------|
| 1 | IO19 | NAU7802 SDA |
| 2 | IO20 | NAU7802 SCL |
| 3 | 3V3 | NAU7802 VCC |
| 4 | GND | NAU7802 GND |

---

## Raspberry Pi Pico GPIO Usage

| GPIO | Function | Component | Pico Pin # |
|------|----------|-----------|------------|
| GP16 | MISO | PN5180 | 21 |
| GP17 | NSS (CS) | PN5180 | 22 |
| GP18 | MOSI | PN5180 | 24 |
| GP19 | SCK | PN5180 | 25 |
| GP20 | BUSY | PN5180 | 26 |
| GP21 | RST | PN5180 | 27 |
| 3V3 | VCC | PN5180 | 36 |
| GND | GND | PN5180 | 33/38 |

### Pico Pinout Diagram

```
                    Raspberry Pi Pico
                  ┌───────────────────┐
            GP0  ─┤ 1              40 ├─ VBUS
            GP1  ─┤ 2              39 ├─ VSYS
            GND  ─┤ 3              38 ├─ GND
            GP2  ─┤ 4              37 ├─ 3V3_EN
            GP3  ─┤ 5              36 ├─ 3V3 (OUT) ← VCC
            GP4  ─┤ 6              35 ├─ ADC_VREF
            GP5  ─┤ 7              34 ├─ GP28
            GND  ─┤ 8              33 ├─ GND ← GND
            GP6  ─┤ 9              32 ├─ GP27
            GP7  ─┤ 10             31 ├─ GP26
            GP8  ─┤ 11             30 ├─ RUN
            GP9  ─┤ 12             29 ├─ GP22
            GND  ─┤ 13             28 ├─ GND
           GP10  ─┤ 14             27 ├─ GP21 ← RST
           GP11  ─┤ 15             26 ├─ GP20 ← BUSY
           GP12  ─┤ 16             25 ├─ GP19 ← SCK
           GP13  ─┤ 17             24 ├─ GP18 ← MOSI
            GND  ─┤ 18             23 ├─ GND
           GP14  ─┤ 19             22 ├─ GP17 ← NSS
           GP15  ─┤ 20             21 ├─ GP16 ← MISO
                  └───────────────────┘
```

---

## PN5180 Complete Wiring

| PN5180 Pin | Pico GPIO | Pico Pin # | Wire Color |
|------------|-----------|------------|------------|
| VCC | 3V3 | 36 | Red |
| GND | GND | 33 | Black |
| SCK | GP19 | 25 | Yellow |
| MISO | GP16 | 21 | Orange |
| MOSI | GP18 | 24 | Green |
| NSS (CS) | GP17 | 22 | Blue |
| BUSY | GP20 | 26 | Pink |
| RST | GP21 | 27 | Brown |

**SPI Configuration:**

- Mode: SPI Mode 0 (CPOL=0, CPHA=0)
- Speed: 1-7 MHz
- Bit Order: MSB First

---

## NAU7802 Complete Wiring

| NAU7802 Pin | CrowPanel GPIO | Header | Pin # |
|-------------|----------------|--------|-------|
| VCC | 3.3V | UART1-OUT | Pin 3 |
| GND | GND | UART1-OUT | Pin 4 |
| SDA | IO19 | UART1-OUT | Pin 1 |
| SCL | IO20 | UART1-OUT | Pin 2 |

**I2C Configuration:**

- Address: 0x2A
- Speed: 400 kHz (Fast mode)

---

## Load Cell Wiring

| Load Cell Wire | NAU7802 Terminal | Function |
|----------------|------------------|----------|
| Red | E+ | Excitation + |
| Black | E- | Excitation - |
| White | A- | Signal - |
| Green | A+ | Signal + |

!!! info
    Wire colors vary by manufacturer. If readings are negative, swap A+ and A-.

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

!!! danger "J9 Header Conflict"
    J9 pins conflict with the LCD display! Do not use for SPI.

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
