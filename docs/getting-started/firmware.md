# Firmware Installation

SpoolBuddy requires firmware on two devices: the **Raspberry Pi Pico** (NFC bridge) and the **CrowPanel Display** (ESP32-S3).

---

## Device Overview

| Device | Purpose | Firmware Type |
|--------|---------|---------------|
| Raspberry Pi Pico | NFC reader controller | Arduino (.uf2) |
| CrowPanel 7.0" | Display, scale, WiFi | Rust/ESP-IDF (.bin) |

---

## Part 1: Pico NFC Bridge

The Pico runs the NFC bridge firmware that communicates with the PN5180 reader.

### Option A: Pre-built UF2 (Recommended)

1. **Download** the pre-built firmware:

    [:material-download: **pico-nfc-bridge.uf2**](https://github.com/maziggy/spoolbuddy/releases){ .btn .btn-primary target="_blank" }

2. **Enter BOOTSEL mode**:
   - Hold the **BOOTSEL** button on the Pico
   - While holding, plug in the USB cable
   - Release the button
   - The Pico appears as a USB drive named "RPI-RP2"

3. **Flash the firmware**:
   - Drag and drop the `.uf2` file onto the RPI-RP2 drive
   - The Pico automatically reboots and starts running

4. **Verify**:
   - The onboard LED should blink every 500ms
   - Connect a serial monitor at 115200 baud to see:
     ```
     Pico NFC Bridge v2.0 starting...
     Features: MIFARE Classic + NTAG + Bambu HKDF
     SPI OK
     Reset OK
     PN5180 version: 3.5
     RF ON (ISO14443A)
     I2C ready at 0x55
     Ready!
     ```

### Option B: Build from Source

Requires [PlatformIO](https://platformio.org/){ target="_blank" } installed.

```bash
# Clone the repository
git clone https://github.com/maziggy/spoolbuddy.git
cd spoolbuddy/pico-nfc-bridge

# Build
pio run

# Upload (Pico in BOOTSEL mode)
pio run -t upload

# Monitor serial output
pio device monitor
```

---

## Part 2: CrowPanel Display

The CrowPanel runs the main SpoolBuddy firmware (Rust/ESP-IDF).

### Option A: Pre-built Binary (Recommended)

1. **Download** the pre-built firmware:

    [:material-download: **spoolbuddy-firmware.bin**](https://github.com/maziggy/spoolbuddy/releases){ .btn .btn-primary target="_blank" }

2. **Install espflash**:
   ```bash
   cargo install espflash
   ```

   Or download from [espflash releases](https://github.com/esp-rs/espflash/releases){ target="_blank" }

3. **Connect the display**:
   - Use a USB-C data cable (not charge-only)
   - Connect to your computer
   - The display may show a blank screen or old firmware

4. **Find the port**:

    === "Windows"
        Device Manager → Ports → Look for "USB Serial" (e.g., COM3)

    === "Linux"
        ```bash
        ls /dev/ttyACM* /dev/ttyUSB*
        # Usually /dev/ttyACM0
        ```

    === "macOS"
        ```bash
        ls /dev/cu.usb*
        # Usually /dev/cu.usbmodem*
        ```

5. **Flash the firmware**:
   ```bash
   espflash flash --monitor spoolbuddy-firmware.bin
   ```

### Option B: Build from Source

Requires Rust and the ESP toolchain.

#### Prerequisites

1. **Install Rust**:
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   source $HOME/.cargo/env
   ```

2. **Install ESP toolchain via espup**:
   ```bash
   # Install espup
   cargo install espup --locked

   # Install the ESP toolchain (takes several minutes)
   espup install

   # Source the environment (add to your shell profile)
   . $HOME/export-esp.sh
   ```

3. **Install espflash**:
   ```bash
   cargo install espflash
   ```

4. **(Linux only) Add USB permissions**:
   ```bash
   sudo usermod -a -G dialout $USER
   # Log out and back in
   ```

#### Build and Flash

```bash
# Clone the repository
git clone https://github.com/maziggy/spoolbuddy.git
cd spoolbuddy/firmware

# Source ESP environment
. $HOME/export-esp.sh

# Build and flash (with serial monitor)
cargo run --release
```

Or build separately:

```bash
# Build only
cargo build --release

# Flash manually
espflash flash --monitor target/xtensa-esp32s3-espidf/release/spoolbuddy-firmware
```

---

## Verification

After flashing both devices, verify the system is working:

### Pico NFC Bridge

- [ ] LED blinks every 500ms
- [ ] Serial output shows "Ready!"
- [ ] PN5180 version displayed (e.g., 3.5)

### CrowPanel Display

- [ ] Display backlight turns on
- [ ] UI renders on screen
- [ ] Touch responds to input
- [ ] Serial output shows initialization

### Integration Test

1. Power on the complete system
2. Place a Bambu Lab spool on the scale
3. The NFC tag should be detected
4. Weight should display on screen

---

## Troubleshooting

### Pico Issues

| Issue | Solution |
|-------|----------|
| RPI-RP2 drive doesn't appear | Try a different USB cable, ensure BOOTSEL held during plug-in |
| "PN5180 ERROR!" in serial | Check SPI wiring to PN5180 |
| I2C not working | Verify SDA/SCL connections to CrowPanel |

### Display Issues

| Issue | Solution |
|-------|----------|
| "Permission denied" (Linux) | Run `sudo usermod -a -G dialout $USER` and re-login |
| Port not detected | Try different USB cable (must be data cable) |
| espflash timeout | Hold BOOT button while pressing RESET, then flash |
| Build fails with "rust-src not found" | Run `espup install` and source `export-esp.sh` |

---

[Next: Software Setup :material-arrow-right:](software.md){ .btn .btn-primary }
