# Pin Reference

Quick reference for all SpoolBuddy hardware connections.

## CrowPanel Advance 7.0" Pin Map

### UART0-OUT Header (4-pin)

| Pin | GPIO | Function | SpoolBuddy Use |
|-----|------|----------|----------------|
| 1 | IO44 | UART0 RX | PN5180 MISO |
| 2 | IO43 | UART0 TX | PN5180 SCK |
| 3 | 3V3 | Power | PN5180 VCC |
| 4 | GND | Ground | PN5180 GND |

### UART1-OUT Header (4-pin)

| Pin | GPIO | Function | SpoolBuddy Use |
|-----|------|----------|----------------|
| 1 | IO19 | UART1 RX | NAU7802 SDA |
| 2 | IO20 | UART1 TX | NAU7802 SCL |
| 3 | 3V3 | Power | NAU7802 VCC |
| 4 | GND | Ground | NAU7802 GND |

### J11 Header (7-pin)

| Pin | GPIO | Function | SpoolBuddy Use |
|-----|------|----------|----------------|
| 1 | IO19 | GPIO | (Reserved - I2C SDA) |
| 2 | IO16 | GPIO | PN5180 MOSI |
| 3 | IO15 | GPIO | PN5180 RST |
| 4 | NC | - | Not Connected |
| 5 | IO2 | GPIO | PN5180 BUSY |
| 6 | IO8 | GPIO | PN5180 NSS (CS) |
| 7 | NC | - | Not Connected |

### J9 Header - DO NOT USE

> **WARNING:** J9 pins conflict with the LCD display!

| Pin | GPIO | Conflict |
|-----|------|----------|
| 1 | IO20 | LCD |
| 2 | IO5 | LCD |
| 3 | IO4 | LCD |
| 4 | IO6 | LCD |
| 5 | 3V3 | - |
| 6 | GND | - |
| 7 | 5V | - |

---

## PN5180 NFC Reader Pinout

```
     PN5180 Module
    ┌─────────────┐
    │ VCC  ●──────┼──→ 3.3V (UART0-OUT Pin 3)
    │ GND  ●──────┼──→ GND  (UART0-OUT Pin 4)
    │ SCK  ●──────┼──→ IO43 (UART0-OUT Pin 2)
    │ MISO ●──────┼──→ IO44 (UART0-OUT Pin 1)
    │ MOSI ●──────┼──→ IO16 (J11 Pin 2)
    │ NSS  ●──────┼──→ IO8  (J11 Pin 6)
    │ BUSY ●──────┼──→ IO2  (J11 Pin 5)
    │ RST  ●──────┼──→ IO15 (J11 Pin 3)
    │ IRQ  ●      │    (Not used)
    │ AUX  ●      │    (Not used)
    └─────────────┘
```

### SPI Configuration

| Parameter | Value |
|-----------|-------|
| Mode | 0 (CPOL=0, CPHA=0) |
| Speed | 1-7 MHz |
| Bit Order | MSB First |

---

## NAU7802 Scale ADC Pinout

```
    SparkFun Qwiic Scale
    ┌─────────────┐
    │ VCC  ●──────┼──→ 3.3V (UART1-OUT Pin 3)
    │ GND  ●──────┼──→ GND  (UART1-OUT Pin 4)
    │ SDA  ●──────┼──→ IO19 (UART1-OUT Pin 1)
    │ SCL  ●──────┼──→ IO20 (UART1-OUT Pin 2)
    │             │
    │ E+   ●──────┼──→ Load Cell Red
    │ E-   ●──────┼──→ Load Cell Black
    │ A+   ●──────┼──→ Load Cell Green
    │ A-   ●──────┼──→ Load Cell White
    └─────────────┘
```

### I2C Configuration

| Parameter | Value |
|-----------|-------|
| Address | 0x2A |
| Speed | 400 kHz (Fast mode) |

---

## Load Cell Wiring

### Standard 4-Wire Color Code

| Wire Color | Function | NAU7802 Terminal |
|------------|----------|------------------|
| Red | E+ (Excitation+) | E+ |
| Black | E- (Excitation-) | E- |
| Green | A+ (Signal+) | A+ |
| White | A- (Signal-) | A- |

> **Note:** Colors vary by manufacturer. If readings are negative, swap A+ and A-.

---

## Complete GPIO Usage Summary

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

### Reserved GPIOs (Do Not Use)

| GPIO | Reason |
|------|--------|
| IO4, IO5, IO6 | LCD Display (J9) |
| IO0 | Boot button |
| IO3 | USB D- |
| IO45 | USB D+ |
| IO38-42 | PSRAM/Flash |

---

## Wire Color Reference

### Recommended Color Coding

| Color | Signal |
|-------|--------|
| Red | VCC (3.3V) |
| Black | GND |
| Yellow | Clock (SCK/SCL) |
| Blue | Data In (MISO) |
| Green | Data Out (MOSI/SDA) |
| Orange | Chip Select (NSS) |
| White | Status (BUSY) |
| Brown | Reset (RST) |

---

## Physical Connector Diagrams

### UART0-OUT (Looking at back of display)

```
     ┌─────────────────────────────────┐
     │  ●     ●     ●     ●           │
     │ IO44  IO43  3V3   GND          │
     │ Pin1  Pin2  Pin3  Pin4         │
     └─────────────────────────────────┘
```

### UART1-OUT (Looking at back of display)

```
     ┌─────────────────────────────────┐
     │  ●     ●     ●     ●           │
     │ IO19  IO20  3V3   GND          │
     │ Pin1  Pin2  Pin3  Pin4         │
     └─────────────────────────────────┘
```

### J11 (Looking at back of display)

```
             ┌─────┐
      Pin 1  │ IO19│  ← Don't use (I2C SDA)
      Pin 2  │ IO16│  ← MOSI
      Pin 3  │ IO15│  ← RST
      Pin 4  │  NC │
      Pin 5  │ IO2 │  ← BUSY
      Pin 6  │ IO8 │  ← NSS
      Pin 7  │  NC │
             └─────┘
```

---

## Voltage Reference

| Rail | Voltage | Max Current | Components |
|------|---------|-------------|------------|
| 5V (USB) | 5.0V | 2A | CrowPanel input |
| 3.3V | 3.3V | 500mA | PN5180, NAU7802 |

> **IMPORTANT:** PN5180 is 3.3V ONLY. 5V will damage it!

---

## Quick Troubleshooting by Pin

| Symptom | Check |
|---------|-------|
| NFC not working | IO43 (SCK), IO44 (MISO), IO16 (MOSI) |
| Scale not reading | IO19 (SDA), IO20 (SCL) |
| NFC intermittent | IO8 (NSS), IO2 (BUSY), IO15 (RST) |
| Display issues | Don't use J9! |
