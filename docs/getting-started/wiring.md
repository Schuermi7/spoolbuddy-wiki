# Wiring Guide

Detailed wiring diagrams and pin connections for all SpoolBuddy components.

---

## Safety First

- :material-power-plug-off: **Always disconnect power** before making wiring changes
- :material-flash-alert: Use **3.3V only** for the PN5180 (5V will damage it!)
- :material-check-all: Double-check connections before powering on

---

## CrowPanel Connector Layout

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     CrowPanel Advance 7.0" (Back)                       │
│                                                                         │
│   ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌─────────┐  ┌─────────┐   │
│   │UART0-OUT │  │UART1-OUT │  │ I2C-OUT  │  │   J9    │  │   J11   │   │
│   │  4-pin   │  │  4-pin   │  │  4-pin   │  │  1x7    │  │  1x7    │   │
│   │ SPI CLK  │  │  Scale   │  │          │  │  N/A!   │  │ Control │   │
│   │ SPI MISO │  │  I2C     │  │          │  │CONFLICT │  │  Pins   │   │
│   └──────────┘  └──────────┘  └──────────┘  └─────────┘  └─────────┘   │
│                                                                         │
│   [BOOT]  [RESET]                                           [USB-C]    │
└─────────────────────────────────────────────────────────────────────────┘
```

!!! danger "J9 Header Conflict"
    J9 header pins (IO4, IO5, IO6) are used internally by the RGB LCD display! These pins appear shorted to GND when the display is powered on. **DO NOT use J9 for SPI** - use UART0-OUT header instead.

---

## Wiring Diagram

```
                                    ┌─────────────────────────────────────────┐
                                    │     ELECROW CrowPanel Advance 7.0"      │
                                    │                                         │
     PN5180 NFC Module              │   UART0-OUT Header                      │
    ┌──────────────────┐            │   ┌───────────────────┐                 │
    │   ┌──────────┐   │            │   │  IO43 ●──────────┼─────SCK         │
    │   │PN5180    │   │            │   │  IO44 ●──────────┼─────MISO        │
    │   │  Chip    │   │            │   │  3V3  ●──────────┼─────VCC         │
    │   └──────────┘   │            │   │  GND  ●──────────┼─────GND         │
    │   ┌──────────┐   │            │   └───────────────────┘                 │
    │   │ Antenna  │   │            │                                         │
    │   └──────────┘   │            │   J11 Header                            │
    └──────────────────┘            │   ┌───────────────────┐                 │
            │                       │   │  IO16 ●──────────┼─────MOSI        │
            │                       │   │  IO15 ●──────────┼─────RST         │
            └───────────────────────┼───│  IO2  ●──────────┼─────BUSY        │
                                    │   │  IO8  ●──────────┼─────NSS (CS)    │
                                    │   └───────────────────┘                 │
                                    │                                         │
     NAU7802 + Load Cell            │   UART1-OUT                             │
    ┌──────────────────┐            │   ┌───────────────────┐                 │
    │  ┌────────────┐  │            │   │  IO19 ●──────────┼─────SDA         │
    │  │ SparkFun   │  │            │   │  IO20 ●──────────┼─────SCL         │
    │  │ Qwiic      │  │            │   │  3V3  ●──────────┼─────VCC         │
    │  │ Scale      │  │            │   │  GND  ●──────────┼─────GND         │
    │  └────────────┘  │            │   └───────────────────┘                 │
    │   ┌────┴────┐    │            │                                         │
    │   │Load Cell│    │            │   USB-C (Power)                         │
    │   └─────────┘    │            │   ┌───────────────────┐                 │
    └──────────────────┘            │   │    ○ USB-C        │                 │
                                    │   └───────────────────┘                 │
                                    └─────────────────────────────────────────┘
```

---

## PN5180 NFC Reader (SPI)

### UART0-OUT Header (4-pin)

```
┌──────┬──────┬──────┬──────┐
│ Pin1 │ Pin2 │ Pin3 │ Pin4 │
│IO44  │IO43  │ 3V3  │ GND  │
│MISO  │ SCK  │ VCC  │ GND  │
└──────┴──────┴──────┴──────┘

BLUE   ───┤ IO44 : ← MISO (SPI data IN)
YELLOW ───┤ IO43 : ← SCK  (SPI clock)
RED    ───┤ 3V3  : ← VCC  (⚠️ 3.3V ONLY!)
BLACK  ───┤ GND  : ← GND
```

### J11 Header Control Pins

```
        J11 (Right Side)
        ┌────────┐
Pin 2   │  IO16  │   ← MOSI (SPI data OUT)
Pin 3   │  IO15  │   ← RST
Pin 5   │  IO2   │   ← BUSY
Pin 6   │  IO8   │   ← NSS (CS)
        └────────┘

GREEN  ───┤ IO16 : ← MOSI
BROWN  ───┤ IO15 : ← RST
WHITE  ───┤ IO2  : ← BUSY
ORANGE ───┤ IO8  : ← NSS
```

### Complete PN5180 Pin Table

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

---

## NAU7802 Scale ADC (I2C)

### UART1-OUT Header (4-pin)

```
┌──────┬──────┬──────┬──────┐
│ Pin1 │ Pin2 │ Pin3 │ Pin4 │
│IO19  │IO20  │ 3V3  │ GND  │
│ SDA  │ SCL  │ VCC  │ GND  │
└──────┴──────┴──────┴──────┘

YELLOW ───┤ IO19 : ← SDA (I2C data)
WHITE  ───┤ IO20 : ← SCL (I2C clock)
RED    ───┤ 3V3  : ← VCC
BLACK  ───┤ GND  : ← GND
```

| NAU7802 Pin | ESP32-S3 GPIO | Header | Pin # |
|-------------|---------------|--------|-------|
| VCC | 3.3V | UART1-OUT | Pin 3 |
| GND | GND | UART1-OUT | Pin 4 |
| SDA | IO19 | UART1-OUT | Pin 1 |
| SCL | IO20 | UART1-OUT | Pin 2 |

---

## Load Cell to NAU7802

```
   Load Cell (5kg)                SparkFun Qwiic Scale
  ┌─────────────────┐            ┌─────────────────┐
  │  Red ───────────┼────────────┤► E+ (Red)       │
  │  Black ─────────┼────────────┤► E- (Black)     │
  │  White ─────────┼────────────┤► A- (White)     │
  │  Green ─────────┼────────────┤► A+ (Green)     │
  └─────────────────┘            └─────────────────┘
```

| Load Cell Wire | NAU7802 Terminal | Function |
|----------------|------------------|----------|
| Red | E+ | Excitation + |
| Black | E- | Excitation - |
| White | A- | Signal - |
| Green | A+ | Signal + |

!!! info "Wire Colors Vary"
    Wire colors vary by manufacturer. If readings are negative or erratic, try swapping A+ and A-.

---

## Quick Reference Card

```
┌────────────────────────────────────────────────────────────┐
│           SPOOLBUDDY QUICK WIRING (CrowPanel 7.0")         │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  *** DO NOT USE J9 HEADER - CONFLICTS WITH LCD ***         │
│                                                            │
│  PN5180 (NFC)              NAU7802 (Scale)                 │
│  ───────────               ──────────────                  │
│  VCC  → UART0 Pin3 (3V3)   VCC → UART1 Pin3 (3V3)         │
│  GND  → UART0 Pin4 (GND)   GND → UART1 Pin4 (GND)         │
│  SCK  → UART0 Pin2 (IO43)  SDA → UART1 Pin1 (IO19)        │
│  MISO → UART0 Pin1 (IO44)  SCL → UART1 Pin2 (IO20)        │
│  MOSI → J11 Pin2 (IO16)                                    │
│  CS   → J11 Pin6 (IO8)     Load Cell → Qwiic terminal     │
│  BUSY → J11 Pin5 (IO2)       Red   → E+                   │
│  RST  → J11 Pin3 (IO15)      Black → E-                   │
│                              White → A-                    │
│  Power: USB-C 5V/2A          Green → A+                   │
└────────────────────────────────────────────────────────────┘
```

---

## Power Requirements

| Component | Voltage | Current (typical) | Current (peak) |
|-----------|---------|-------------------|----------------|
| CrowPanel 7.0" | 5V (via USB) | 300mA | 600mA |
| PN5180 | 3.3V | 80mA | 150mA |
| NAU7802 | 3.3V | 1mA | 2mA |
| **Total** | **5V USB** | **~400mA** | **~750mA** |

!!! success "Recommendation"
    Use a quality USB-C cable and 5V/2A power adapter.

---

## Wiring Checklist

### PN5180

- [ ] VCC connected to 3.3V (NOT 5V!)
- [ ] GND connected to GND
- [ ] SCK → UART0-OUT Pin 2 (IO43)
- [ ] MISO → UART0-OUT Pin 1 (IO44)
- [ ] MOSI → J11 Pin 2 (IO16)
- [ ] NSS → J11 Pin 6 (IO8)
- [ ] BUSY → J11 Pin 5 (IO2)
- [ ] RST → J11 Pin 3 (IO15)

### NAU7802

- [ ] VCC connected to 3.3V
- [ ] GND connected to GND
- [ ] SDA → UART1-OUT Pin 1 (IO19)
- [ ] SCL → UART1-OUT Pin 2 (IO20)
- [ ] Load cell wired to E+/E-/A+/A-

---

[Next: Assembly Guide :material-arrow-right:](assembly.md){ .btn .btn-primary }
