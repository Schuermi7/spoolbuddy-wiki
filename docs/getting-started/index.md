# Getting Started

Welcome to SpoolBuddy! This guide will walk you through building your own filament management station.

## Build Process

<ol class="steps">
<li>

**Order Components** - Review the [Hardware Required](hardware.md) page and order all components. Allow 1-4 weeks for shipping.

</li>
<li>

**Wire Components** - Follow the [Wiring Guide](wiring.md) to connect all components correctly.

</li>
<li>

**Assemble Station** - Use the [Assembly Guide](assembly.md) for physical mounting.

</li>
<li>

**Flash Firmware** - Follow [Firmware Installation](firmware.md) to program the ESP32.

</li>
<li>

**Setup Backend** - Configure the server using [Software Setup](software.md).

</li>
<li>

**Start Using** - Connect to your Bambu printer and start managing your filament!

</li>
</ol>

---

## What You'll Need

### Hardware (~$100-150)

| Component | Purpose |
|-----------|---------|
| Elecrow CrowPanel 7.0" Advance | Display + ESP32-S3 |
| Raspberry Pi Zero 2 W | Backend server |
| SparkFun Qwiic NAU7802 | Scale ADC |
| 5kg Load Cell | Weight sensor |
| PN5180 NFC Reader | RFID tag reading |
| 22AWG Silicone Wire | Connections |
| M4x25 Screws | Mounting |

[View Full BOM :material-arrow-right:](hardware.md){ .btn .btn-primary }

### Tools

- Soldering iron and solder
- Wire strippers
- Small screwdrivers
- Multimeter (recommended)

### Software

- Python 3.10+ for the backend server
- Node.js 18+ for frontend development
- PlatformIO or Arduino IDE for firmware

### Network

- WiFi network accessible by both SpoolBuddy and your Bambu printer
- Printer's access code and serial number

---

## Quick Reference

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### :material-alert-circle: Important Warnings
- PN5180 is **3.3V only** - 5V will damage it!
- **Don't use J9 header** - conflicts with LCD
- RC522 won't work - need PN5180 for ISO 15693
</div>

<div class="feature-card" markdown>
### :material-information: Key Specs
- Display: 800x480 IPS touch
- Scale: 5kg capacity, 24-bit ADC
- NFC: ISO 15693 (Bambu Lab tags)
- Power: USB-C 5V/2A
</div>

</div>

---

## Next Steps

Ready to start building?

[View Hardware List :material-arrow-right:](hardware.md){ .btn .btn-primary }
