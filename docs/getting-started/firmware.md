# Firmware Installation

Flash the SpoolBuddy firmware to the CrowPanel Advance 7.0" display.

---

## Prerequisites

- SpoolBuddy fully assembled (see [Assembly Guide](assembly.md))
- USB-C cable (data capable, not charge-only)
- Computer with USB port
- PlatformIO or Arduino IDE installed

---

## Option A: PlatformIO (Recommended)

PlatformIO provides the best development experience with automatic library management.

### 1. Install VS Code

Download from [code.visualstudio.com](https://code.visualstudio.com/){ target="_blank" }

### 2. Install PlatformIO Extension

1. Open VS Code
2. Go to Extensions (++ctrl+shift+x++)
3. Search "PlatformIO"
4. Install "PlatformIO IDE"

### 3. Clone the Repository

```bash
git clone https://github.com/maziggy/spoolbuddy.git
cd spoolbuddy/firmware
```

### 4. Open Project

In VS Code: File → Open Folder → select `firmware/`

PlatformIO will detect the project and install dependencies.

---

## Option B: Arduino IDE

### 1. Install Arduino IDE 2.x

Download from [arduino.cc](https://www.arduino.cc/en/software){ target="_blank" }

### 2. Add ESP32 Board Support

1. File → Preferences
2. Add to "Additional Board Manager URLs":
   ```
   https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
   ```
3. Tools → Board → Boards Manager
4. Search "esp32" and install "esp32 by Espressif Systems"

### 3. Install Required Libraries

Sketch → Include Library → Manage Libraries. Install:

- `lvgl` (v8.3.x)
- `TFT_eSPI`
- `SparkFun_Qwiic_Scale_NAU7802`
- `ArduinoJson`
- `WiFiManager`

---

## Board Configuration

=== "PlatformIO"

    ```ini title="platformio.ini"
    [env:crowpanel-7]
    platform = espressif32
    board = esp32-s3-devkitc-1
    framework = arduino
    board_build.mcu = esp32s3
    board_build.f_cpu = 240000000L
    board_upload.flash_size = 16MB
    board_build.partitions = default_16MB.csv

    build_flags =
        -D LV_CONF_INCLUDE_SIMPLE
        -D BOARD_HAS_PSRAM
        -D ARDUINO_USB_CDC_ON_BOOT=1

    monitor_speed = 115200
    upload_speed = 921600
    ```

=== "Arduino IDE"

    | Setting | Value |
    |---------|-------|
    | Board | ESP32S3 Dev Module |
    | Flash Size | 16MB |
    | PSRAM | OPI PSRAM |
    | USB Mode | Hardware CDC and JTAG |
    | Upload Speed | 921600 |

---

## Connecting to Computer

### DIP Switch Settings

Before connecting, check the DIP switches on the back of the CrowPanel:

| Mode | S1 | S0 | Use |
|------|----|----|-----|
| Normal | OFF | OFF | Running firmware |
| **Upload** | **OFF** | **ON** | Flash new firmware |
| Debug | ON | OFF | Serial debugging |

!!! warning "Set S0=ON, S1=OFF for uploading"

### USB Connection

1. Connect USB-C cable to CrowPanel
2. Connect other end to computer
3. Display may show blank or old firmware

### Finding the Port

=== "Windows"
    Device Manager → Ports (COM & LPT) → Look for "USB Serial" (e.g., COM3)

=== "Linux"
    ```bash
    ls /dev/ttyACM* /dev/ttyUSB*
    # Usually /dev/ttyACM0 for ESP32-S3
    ```

=== "macOS"
    ```bash
    ls /dev/cu.usb*
    # Usually /dev/cu.usbmodem*
    ```

---

## Flashing the Firmware

=== "PlatformIO"

    1. **Build:** Click the checkmark (✓) or run `pio run`
    2. **Upload:** Click the arrow (→) or run `pio run -t upload`
    3. **Monitor:** Click the plug icon or run `pio device monitor`

=== "Arduino IDE"

    1. Select board and port in Tools menu
    2. Click Verify (checkmark) to compile
    3. Click Upload (right arrow)
    4. Open Serial Monitor at 115200 baud

---

## Verification

### Expected Serial Output

```
SpoolBuddy Firmware v1.0.0
==========================

[INIT] Starting display...
[INIT] Display initialized: 800x480
[INIT] Starting I2C...
[INIT] NAU7802 found at 0x2A
[INIT] Starting SPI...
[INIT] PN5180 detected
[INIT] Connecting to WiFi...
[WIFI] Connected: 192.168.1.45
[WS] Connecting to ws://192.168.1.100:3000/ws/ui
[WS] Connected!
[READY] SpoolBuddy ready
```

### Verification Tests

- [ ] Display backlight on and LVGL UI renders
- [ ] Touch responds to input
- [ ] Scale shows weight readings
- [ ] NFC detects tags when spool placed
- [ ] WebSocket connects to backend

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Upload fails | Check DIP switches (S0=ON, S1=OFF) |
| No port detected | Try different USB cable (must be data cable) |
| Timeout errors | Hold BOOT button while connecting USB |
| Slow upload | Reduce upload_speed to 460800 or 115200 |
| No serial output | Enable USB CDC On Boot, check baud rate |

---

[Next: Software Setup :material-arrow-right:](software.md){ .btn .btn-primary }
