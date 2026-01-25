# Wiring Guide

This guide provides detailed wiring instructions for connecting all SpoolBuddy components. Follow each section carefully and use the checklists to verify your connections.

## Safety First

- **Always disconnect power** before making wiring changes
- Use **3.3V only** for the PN5180 (5V will damage it!)
- Double-check connections before powering on
- Have a multimeter ready for verification

---

## CrowPanel Advance 7.0" Connector Layout

The CrowPanel has several headers on the back. Here's the layout:

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
│                        [DIP SWITCHES]                       [UART0-IN] │
│                           S1  S0                                       │
└─────────────────────────────────────────────────────────────────────────┘
```

### Important Warning: J9 Header

> **WARNING: J9 header pins (IO4, IO5, IO6) are used internally by the RGB LCD display!**
>
> These pins appear shorted to GND when the display is powered on.
> **DO NOT use J9 for SPI** - use UART0-OUT header instead.

---

## Component Overview

| Component | Model | Interface | Connection Point |
|-----------|-------|-----------|------------------|
| Display | CrowPanel Advance 7.0" | ESP32-S3 built-in | - |
| NFC Reader | PN5180 | SPI | UART0-OUT + J11 |
| Scale ADC | NAU7802 | I2C | UART1-OUT |
| Load Cell | 5kg Single-Point | 4-wire | NAU7802 terminals |

---

## Wiring Diagram Overview

```
                                    ┌─────────────────────────────────────────┐
                                    │     ELECROW CrowPanel Advance 7.0"      │
                                    │                                         │
                                    │   ┌─────────────────────────────────┐   │
                                    │   │                                 │   │
                                    │   │      7.0" Touch Display         │   │
                                    │   │         (800 x 480)             │   │
                                    │   │                                 │   │
                                    │   │      [Built-in - no wiring]     │   │
                                    │   │                                 │   │
                                    │   └─────────────────────────────────┘   │
                                    │                                         │
     PN5180 NFC Module              │   UART0-OUT Header                      │
    ┌──────────────────┐            │   ┌───────────────────┐                 │
    │                  │            │   │                   │                 │
    │   ┌──────────┐   │            │   │  IO43 ●──────────┼─────SCK         │
    │   │PN5180    │   │            │   │  IO44 ●──────────┼─────MISO        │
    │   │  Chip    │   │            │   │  3V3  ●──────────┼─────VCC         │
    │   └──────────┘   │            │   │  GND  ●──────────┼─────GND         │
    │                  │            │   │                   │                 │
    │   ┌──────────┐   │            │   └───────────────────┘                 │
    │   │ Antenna  │   │            │                                         │
    │   │  Coil    │   │            │   J11 Header                            │
    │   └──────────┘   │            │   ┌───────────────────┐                 │
    │                  │            │   │                   │                 │
    └──────────────────┘            │   │  IO16 ●──────────┼─────MOSI        │
            │                       │   │  IO15 ●──────────┼─────RST         │
            │                       │   │  IO2  ●──────────┼─────BUSY        │
            └───────────────────────┼───│  IO8  ●──────────┼─────NSS (CS)    │
                                    │   │                   │                 │
                                    │   └───────────────────┘                 │
                                    │                                         │
     NAU7802 + Load Cell            │   UART1-OUT (or I2C-OUT)                │
    ┌──────────────────┐            │   ┌───────────────────┐                 │
    │  ┌────────────┐  │            │   │                   │                 │
    │  │ SparkFun   │  │            │   │  IO19 ●──────────┼─────SDA         │
    │  │ Qwiic      │  │            │   │  IO20 ●──────────┼─────SCL         │
    │  │ Scale      │  │            │   │  3V3  ●──────────┼─────VCC         │
    │  └────────────┘  │            │   │  GND  ●──────────┼─────GND         │
    │        │         │            │   │                   │                 │
    │   ┌────┴────┐    │            │   └───────────────────┘                 │
    │   │Load Cell│    │            │                                         │
    │   │(4-wire) │    │            │   USB-C (Power & Debug)                 │
    │   └─────────┘    │            │   ┌───────────────────┐                 │
    │                  │            │   │    ○ USB-C        │                 │
    └──────────────────┘            │   └───────────────────┘                 │
                                    │                                         │
                                    └─────────────────────────────────────────┘
```

---

## Step 1: PN5180 NFC Reader (SPI)

The PN5180 uses SPI communication and requires 8 wires.

### Header Connections

**UART0-OUT Header (4-pin)** - Power and SPI clock/data:

```
┌──────┬──────┬──────┬──────┐
│ Pin1 │ Pin2 │ Pin3 │ Pin4 │
│IO44  │IO43  │ 3V3  │ GND  │
│MISO  │ SCK  │ VCC  │ GND  │
└──────┴──────┴──────┴──────┘

BLUE   ───┤ IO44 : ← MISO (SPI data IN from PN5180)
YELLOW ───┤ IO43 : ← SCK  (SPI clock)
RED    ───┤ 3V3  : ← VCC  (power) ⚠️ 3.3V ONLY!
BLACK  ───┤ GND  : ← GND  (ground)
```

**J11 Header (7-pin)** - Control signals:

```
        J11 (Right Side)
        ┌────────┐
Pin 1   │  IO19  │   ← (Used by Scale I2C - do not use)
Pin 2   │  IO16  │   ← MOSI (SPI data OUT to PN5180)
Pin 3   │  IO15  │   ← RST  (PN5180 reset)
Pin 4   │   NC   │
Pin 5   │  IO2   │   ← BUSY (PN5180 busy signal)
Pin 6   │  IO8   │   ← NSS  (SPI chip select)
Pin 7   │   NC   │
        └────────┘

GREEN  ───┤ IO16 : ← MOSI (SPI data OUT to PN5180)
BROWN  ───┤ IO15 : ← RST
WHITE  ───┤ IO2  : ← BUSY
ORANGE ───┤ IO8  : ← NSS (chip select)
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

### SPI Configuration

- **Mode:** SPI Mode 0 (CPOL=0, CPHA=0)
- **Speed:** 1 MHz (can increase up to 7 MHz once working)
- **Bit order:** MSB first

### PN5180 Wiring Checklist

- [ ] VCC connected to 3.3V (NOT 5V!)
- [ ] GND connected to GND
- [ ] SCK → UART0-OUT Pin 2 (IO43)
- [ ] MISO → UART0-OUT Pin 1 (IO44)
- [ ] MOSI → J11 Pin 2 (IO16)
- [ ] NSS → J11 Pin 6 (IO8)
- [ ] BUSY → J11 Pin 5 (IO2)
- [ ] RST → J11 Pin 3 (IO15)

---

## Step 2: NAU7802 Scale ADC (I2C)

The NAU7802 uses I2C communication and requires only 4 wires.

### Header Connection

**UART1-OUT Header (4-pin)**:

```
┌──────┬──────┬──────┬──────┐
│ Pin1 │ Pin2 │ Pin3 │ Pin4 │
│IO19  │IO20  │ 3V3  │ GND  │
│ SDA  │ SCL  │ VCC  │ GND  │
└──────┴──────┴──────┴──────┘

YELLOW ───┤ IO19 : ← SDA (I2C data)
WHITE  ───┤ IO20 : ← SCL (I2C clock)
RED    ───┤ 3V3  : ← VCC (power)
BLACK  ───┤ GND  : ← GND (ground)
```

### Complete NAU7802 Pin Table

| NAU7802 Pin | ESP32-S3 GPIO | Header | Pin # | Wire Color |
|-------------|---------------|--------|-------|------------|
| VCC | 3.3V | UART1-OUT | Pin 3 | Red |
| GND | GND | UART1-OUT | Pin 4 | Black |
| SDA | IO19 | UART1-OUT | Pin 1 | Yellow |
| SCL | IO20 | UART1-OUT | Pin 2 | White |

### I2C Configuration

- **Address:** 0x2A
- **Speed:** 400 kHz (Fast mode)

### NAU7802 Wiring Checklist

- [ ] VCC connected to 3.3V
- [ ] GND connected to GND
- [ ] SDA → UART1-OUT Pin 1 (IO19)
- [ ] SCL → UART1-OUT Pin 2 (IO20)

---

## Step 3: Load Cell to NAU7802

Connect the load cell's 4 wires to the NAU7802's screw terminals.

### Wiring Diagram

```
   Load Cell (5kg)                SparkFun Qwiic Scale
  ┌─────────────────┐            ┌─────────────────┐
  │                 │            │                 │
  │  Red ───────────┼────────────┤► E+ (Red)       │
  │  Black ─────────┼────────────┤► E- (Black)     │
  │  White ─────────┼────────────┤► A- (White)     │
  │  Green ─────────┼────────────┤► A+ (Green)     │
  │                 │            │                 │
  │   ┌─────────┐   │            │                 │
  │   │ Strain  │   │            │                 │
  │   │ Gauge   │   │            │                 │
  │   └─────────┘   │            │                 │
  │                 │            │                 │
  └─────────────────┘            └─────────────────┘
```

### Load Cell Pin Mapping

| Load Cell Wire | NAU7802 Terminal | Function |
|----------------|------------------|----------|
| Red | E+ | Excitation + |
| Black | E- | Excitation - |
| White | A- | Signal - |
| Green | A+ | Signal + |

> **Note:** Wire colors vary by manufacturer. If readings are negative or erratic, try swapping A+ and A-.

### Load Cell Wiring Checklist

- [ ] Red → E+ terminal
- [ ] Black → E- terminal
- [ ] White → A- terminal
- [ ] Green → A+ terminal
- [ ] All terminals tightened securely

---

## Quick Reference Card

Print this for easy reference during assembly:

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
│                                                            │
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

**Recommendation:** Use a quality USB-C cable and 5V/2A power adapter.

---

## Pre-Power Verification

Before applying power, verify:

1. **Visual Inspection**
   - [ ] All wires securely connected
   - [ ] No loose strands bridging pins
   - [ ] No solder bridges
   - [ ] Correct color coding

2. **Multimeter Checks**
   - [ ] No continuity between VCC and GND
   - [ ] Continuity from each wire end to its destination

3. **Component Orientation**
   - [ ] PN5180 module orientation correct (check silkscreen)
   - [ ] NAU7802 not reversed

---

## Migration from Old Wiring (J9)

If you previously wired PN5180 to J9 header, move these 3 wires:

| Signal | Old Location | New Location |
|--------|--------------|--------------|
| SCK | J9 Pin 2 (IO5) | UART0-OUT Pin 2 (IO43) |
| MISO | J9 Pin 3 (IO4) | UART0-OUT Pin 1 (IO44) |
| MOSI | J9 Pin 4 (IO6) | J11 Pin 2 (IO16) |

Control pins (NSS, BUSY, RST) on J11 stay the same.
VCC/GND can stay at UART0-OUT header.

---

## Next Steps

Once wiring is complete:

→ **[[Assembly Guide]]** - Physical mounting and enclosure

→ **[[Firmware Installation]]** - Flash the ESP32 firmware
