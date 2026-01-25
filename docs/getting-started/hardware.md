# Hardware Required

Complete bill of materials for building a SpoolBuddy station. Total cost: **$100-150**.

---

## 1. Display - Elecrow CrowPanel Advance 7.0"

| Specification | Value |
|--------------|-------|
| Price | ~$50-60 |
| Display | 7.0" IPS LCD, 800x480 |
| Touch | Capacitive (GT911) |
| MCU | ESP32-S3 (built-in) |
| Power | USB-C 5V |

**Purchase:**

- [Amazon DE](https://www.amazon.de/dp/B0F2TB4ZHL){ target="_blank" }
- [Elecrow Official](https://www.elecrow.com/crowpanel-advance-7-0-inch-esp32-ai-display.html){ target="_blank" }

!!! warning "Important"
    Make sure you choose the **Advance (AI)** version, not the basic HMI version!

**Why this display?**

- Built-in ESP32-S3 eliminates separate microcontroller
- Plenty of GPIO pins for peripherals
- High-quality IPS touchscreen
- USB-C power and programming

---

## 2. Raspberry Pi Pico

| Specification | Value |
|--------------|-------|
| Price | ~$4-6 |
| CPU | Dual-core ARM Cortex-M0+ @ 133MHz |
| RAM | 264KB |
| Interface | USB, SPI, I2C, UART |
| Purpose | NFC reader controller |

**Purchase:**

- [Raspberry Pi Official](https://www.raspberrypi.com/products/raspberry-pi-pico/){ target="_blank" }
- [Amazon](https://www.amazon.com/s?k=raspberry+pi+pico){ target="_blank" }

!!! info "Pico W Alternative"
    The Pico W (with WiFi) also works if you have one available.

---

## 3. Scale ADC - SparkFun Qwiic NAU7802

| Specification | Value |
|--------------|-------|
| Price | ~$15-25 |
| ADC | 24-bit |
| Interface | I2C (Qwiic) |
| Address | 0x2A |

**Purchase:**

- [eBay](https://www.ebay.de/itm/116887968109){ target="_blank" }
- [SparkFun Official](https://www.sparkfun.com/products/15242){ target="_blank" }
- [Mouser](https://www.mouser.com/ProductDetail/SparkFun/SEN-15242){ target="_blank" }

---

## 4. Load Cell - 5kg Single Point

| Specification | Value |
|--------------|-------|
| Price | ~$5-10 |
| Capacity | 5kg |
| Type | Single-point bar |
| Output | 4-wire (E+, E-, A+, A-) |

**Purchase:**

- [eBay](https://www.ebay.de/itm/175773183411){ target="_blank" }
- Search "5kg load cell" on Amazon/AliExpress

!!! tip
    5kg is ideal for filament spools. Higher capacity (10kg) cells have lower resolution per gram.

---

## 5. NFC Reader - PN5180

| Specification | Value |
|--------------|-------|
| Price | ~$10-15 |
| Chip | NXP PN5180 |
| Interface | SPI |
| Protocols | ISO 14443A/B, ISO 15693 |

**Purchase:**

- [LaskaKit](https://www.laskakit.cz/en/rfid-ctecka-s-vestavenou-antenou-nfc-rf-pn5180-iso15693-cteni-i-zapis/){ target="_blank" }
- Search "PN5180 NFC" on AliExpress

!!! danger "RC522 Won't Work!"
    The cheaper RC522 reader does **NOT** support ISO 15693 and won't work with Bambu Lab tags!

---

## 6. Wire - 22AWG Silicone Stranded

| Specification | Value |
|--------------|-------|
| Price | ~$10-15 (kit) |
| Gauge | 22 AWG |
| Type | Silicone stranded |
| Colors | Multiple (for color-coding) |

**Purchase:**

- [Amazon DE](https://www.amazon.de/dp/B0BTBBV5FK){ target="_blank" }

---

## 7. Mounting Hardware

| Item | Quantity | Size | Purpose |
|------|----------|------|---------|
| DIN 912 Cylinder Screws | 4 | M4x25 | Load cell mounting |
| Nuts | 4 | M4 | Load cell mounting |
| Standoffs | 4 | M3x10 | Display mounting |

**Purchase:**

- [eBay - M4x25 Screws](https://www.ebay.de/itm/155408514898){ target="_blank" }

---

## Summary Table

| # | Component | Price | Required |
|---|-----------|-------|----------|
| 1 | Elecrow CrowPanel 7.0" Advance | $50-60 | :material-check: |
| 2 | Raspberry Pi Pico | $4-6 | :material-check: |
| 3 | SparkFun Qwiic NAU7802 | $15-25 | :material-check: |
| 4 | Load Cell 5kg | $5-10 | :material-check: |
| 5 | PN5180 NFC Reader | $10-15 | :material-check: |
| 6 | 22AWG Silicone Wire | $10-15 | :material-check: |
| 7 | M4x25 Screws (4x) | $2-5 | :material-check: |
| | **Total** | **$96-136** | |

---

## Before You Buy

- [x] Verify the display version - Must be CrowPanel **Advance** (AI) version
- [x] Check PN5180 antenna - Get the module with built-in antenna
- [x] Load cell capacity - 5kg recommended
- [x] Shipping times - Some items from China take 2-4 weeks

---

[Next: Wiring Guide :material-arrow-right:](wiring.md){ .btn .btn-primary }
