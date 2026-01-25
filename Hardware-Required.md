# Hardware Required

This page lists all the hardware components needed to build a SpoolBuddy station. Total cost is approximately **$100-150** depending on your region and shipping.

## Bill of Materials

### 1. Display - Elecrow CrowPanel Advance 7.0"

| Specification | Value |
|--------------|-------|
| **Price** | ~$50-60 |
| **Display** | 7.0" IPS LCD, 800x480 |
| **Touch** | Capacitive (GT911) |
| **MCU** | ESP32-S3 (built-in) |
| **Power** | USB-C 5V |

**Purchase Links:**
- [Amazon DE](https://www.amazon.de/dp/B0F2TB4ZHL)
- [Elecrow Official](https://www.elecrow.com/crowpanel-advance-7-0-inch-esp32-ai-display.html)

> **IMPORTANT:** Make sure you choose the **Advance (AI)** version, not the basic HMI version!

![CrowPanel Advance 7.0"](images/crowpanel-7.png)

**Why this display?**
- Built-in ESP32-S3 eliminates separate microcontroller
- Plenty of GPIO pins for peripherals
- High-quality IPS touchscreen
- USB-C power and programming
- Good community support

---

### 2. Compute Module - Raspberry Pi Zero 2 W

| Specification | Value |
|--------------|-------|
| **Price** | ~$15-20 |
| **CPU** | Quad-core ARM Cortex-A53 @ 1GHz |
| **RAM** | 512MB |
| **WiFi** | 2.4GHz 802.11 b/g/n |
| **Purpose** | Backend server |

**Purchase Links:**
- [Raspberry Pi Official](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/)
- Check local electronics retailers

![Raspberry Pi Zero 2 W](images/pi-zero-2w.png)

**Why Pi Zero 2 W?**
- Runs the Python FastAPI backend
- Connects to Bambu printer via MQTT
- Low power consumption
- Compact size fits in enclosure

> **Note:** You can also use a Raspberry Pi 3/4/5 if you have one available.

---

### 3. Scale ADC - SparkFun Qwiic NAU7802

| Specification | Value |
|--------------|-------|
| **Price** | ~$15-25 |
| **ADC** | 24-bit |
| **Interface** | I2C (Qwiic) |
| **Address** | 0x2A |

**Purchase Links:**
- [eBay](https://www.ebay.de/itm/116887968109)
- [SparkFun Official](https://www.sparkfun.com/products/15242)
- [Mouser](https://www.mouser.com/ProductDetail/SparkFun/SEN-15242)

![SparkFun Qwiic Scale](images/nau7802.png)

**Why NAU7802?**
- 24-bit resolution for accurate weight readings
- Qwiic connector simplifies wiring (or use header pins)
- Integrated excitation voltage for load cell
- Well-documented with libraries

---

### 4. Load Cell - 5kg Single Point

| Specification | Value |
|--------------|-------|
| **Price** | ~$5-10 |
| **Capacity** | 5kg |
| **Type** | Single-point bar |
| **Output** | 4-wire (E+, E-, A+, A-) |

**Purchase Links:**
- [eBay](https://www.ebay.de/itm/175773183411)
- Search "5kg load cell" on Amazon/AliExpress

![5kg Load Cell](images/load-cell-5kg.png)

**Specifications to look for:**
- Rated capacity: 5kg (sufficient for 1kg spool + holder)
- Mounting holes at both ends
- 4-wire connection (red, black, white, green typically)

> **Tip:** Higher capacity (10kg) load cells have lower resolution per gram. 5kg is ideal for filament spools.

---

### 5. NFC Reader - PN5180

| Specification | Value |
|--------------|-------|
| **Price** | ~$10-15 |
| **Chip** | NXP PN5180 |
| **Interface** | SPI |
| **Protocols** | ISO 14443A/B, ISO 15693 |

**Purchase Links:**
- [LaskaKit](https://www.laskakit.cz/en/rfid-ctecka-s-vestavenou-antenou-nfc-rf-pn5180-iso15693-cteni-i-zapis/)
- Search "PN5180 NFC" on AliExpress

![PN5180 NFC Module](images/pn5180.png)

**Why PN5180?**
- Supports ISO 15693 (required for Bambu Lab tags)
- Built-in antenna with ~20cm range
- Full-featured NFC chipset
- Reads Bambu Lab's spool RFID tags

> **Important:** The cheaper RC522 reader does **NOT** support ISO 15693 and won't work with Bambu Lab tags!

---

### 6. Wire - 22AWG Silicone Stranded

| Specification | Value |
|--------------|-------|
| **Price** | ~$10-15 (kit) |
| **Gauge** | 22 AWG |
| **Type** | Silicone stranded |
| **Colors** | Multiple (for color-coding) |

**Purchase Links:**
- [Amazon DE](https://www.amazon.de/dp/B0BTBBV5FK)
- Search "22AWG silicone wire kit"

**Why silicone wire?**
- Flexible and easy to route
- Heat resistant (important near display)
- Easy to strip and solder
- Multiple colors for organization

---

### 7. Mounting Hardware

| Item | Quantity | Size | Purpose |
|------|----------|------|---------|
| DIN 912 Cylinder Screws | 4 | M4x25 | Load cell mounting |
| Nuts | 4 | M4 | Load cell mounting |
| Standoffs | 4 | M3x10 | Display mounting |

**Purchase Links:**
- [eBay - M4x25 Screws](https://www.ebay.de/itm/155408514898)
- Local hardware store

---

## Optional Components

### SD Card for Raspberry Pi
- 16GB+ microSD card
- Class 10 or faster
- For Raspberry Pi OS

### Enclosure
- 3D printed case (STL files in repo)
- Or custom enclosure

### USB-C Cables
- Quality USB-C cable for display power
- Micro USB for Pi Zero programming

### Power Supply
- 5V/3A USB power supply
- Powers both display and Pi (via hub)

---

## Summary Table

| # | Component | Price (approx) | Required |
|---|-----------|----------------|----------|
| 1 | Elecrow CrowPanel 7.0" Advance | $50-60 | Yes |
| 2 | Raspberry Pi Zero 2 W | $15-20 | Yes |
| 3 | SparkFun Qwiic NAU7802 | $15-25 | Yes |
| 4 | Load Cell 5kg | $5-10 | Yes |
| 5 | PN5180 NFC Reader | $10-15 | Yes |
| 6 | 22AWG Silicone Wire | $10-15 | Yes |
| 7 | M4x25 Screws (4x) | $2-5 | Yes |
| | **Total** | **$107-150** | |

---

## Before You Buy

1. **Verify the display version** - Must be CrowPanel **Advance** (AI) version
2. **Check PN5180 antenna** - Get the module with built-in antenna
3. **Load cell capacity** - 5kg is recommended, 10kg works but less precise
4. **Shipping times** - Some items from China take 2-4 weeks

---

## Next Steps

Once you have all components:

→ **[[Wiring Guide]]** - Connect everything together
