# Pin Reference

Quick reference for all SpoolBuddy hardware connections.

---

## GPIO Usage Summary

| GPIO | Function | Component | Header |
|------|----------|-----------|--------|
| IO2 | BUSY | PN5180 | J11 Pin 5 |
| IO8 | NSS (CS) | PN5180 | J11 Pin 6 |
| IO15 | RST | PN5180 | J11 Pin 3 |
| IO16 | MOSI | PN5180 | J11 Pin 2 |
| IO19 | SDA | NAU7802 | UART1-OUT Pin 1 |
| IO20 | SCL | NAU7802 | UART1-OUT Pin 2 |
| IO43 | SCK | PN5180 | UART0-OUT Pin 2 |
| IO44 | MISO | PN5180 | UART0-OUT Pin 1 |

---

## UART0-OUT Header (4-pin)

```
┌──────┬──────┬──────┬──────┐
│ Pin1 │ Pin2 │ Pin3 │ Pin4 │
│IO44  │IO43  │ 3V3  │ GND  │
│MISO  │ SCK  │ VCC  │ GND  │
└──────┴──────┴──────┴──────┘
```

| Pin | GPIO | SpoolBuddy Use |
|-----|------|----------------|
| 1 | IO44 | PN5180 MISO |
| 2 | IO43 | PN5180 SCK |
| 3 | 3V3 | PN5180 VCC |
| 4 | GND | PN5180 GND |

---

## UART1-OUT Header (4-pin)

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

## J11 Header (7-pin)

```
        J11
        ┌────────┐
Pin 1   │  IO19  │  ← (Reserved - I2C SDA)
Pin 2   │  IO16  │  ← MOSI
Pin 3   │  IO15  │  ← RST
Pin 4   │   NC   │
Pin 5   │  IO2   │  ← BUSY
Pin 6   │  IO8   │  ← NSS
Pin 7   │   NC   │
        └────────┘
```

---

## J9 Header - DO NOT USE

!!! danger "LCD Conflict"
    J9 pins conflict with the LCD display! Do not use for SPI.

| Pin | GPIO | Conflict |
|-----|------|----------|
| 1 | IO20 | LCD |
| 2 | IO5 | LCD |
| 3 | IO4 | LCD |
| 4 | IO6 | LCD |

---

## PN5180 Complete Wiring

| PN5180 Pin | ESP32-S3 GPIO | Header | Pin # | Wire Color |
|------------|---------------|--------|-------|------------|
| VCC | 3.3V | UART0-OUT | Pin 3 | Red |
| GND | GND | UART0-OUT | Pin 4 | Black |
| SCK | IO43 | UART0-OUT | Pin 2 | Yellow |
| MISO | IO44 | UART0-OUT | Pin 1 | Blue |
| MOSI | IO16 | J11 | Pin 2 | Green |
| NSS (CS) | IO8 | J11 | Pin 6 | Orange |
| BUSY | IO2 | J11 | Pin 5 | White |
| RST | IO15 | J11 | Pin 3 | Brown |

**SPI Configuration:**

- Mode: SPI Mode 0 (CPOL=0, CPHA=0)
- Speed: 1-7 MHz
- Bit Order: MSB First

---

## NAU7802 Complete Wiring

| NAU7802 Pin | ESP32-S3 GPIO | Header | Pin # |
|-------------|---------------|--------|-------|
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

## Reserved GPIOs

Do not use these pins:

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
| 5V (USB) | 5.0V | 2A | CrowPanel input |
| 3.3V | 3.3V | 500mA | PN5180, NAU7802 |

!!! danger
    PN5180 is **3.3V ONLY**. 5V will damage it!
