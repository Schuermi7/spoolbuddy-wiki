# Firmware Installation

This guide covers flashing the SpoolBuddy firmware to the CrowPanel Advance 7.0" display.

## Prerequisites

- SpoolBuddy fully assembled (see [[Assembly Guide]])
- USB-C cable (data capable, not charge-only)
- Computer with USB port
- PlatformIO or Arduino IDE installed

---

## Development Environment Setup

### Option A: PlatformIO (Recommended)

PlatformIO provides the best development experience with automatic library management.

1. **Install VS Code**
   - Download from [code.visualstudio.com](https://code.visualstudio.com/)

2. **Install PlatformIO Extension**
   - Open VS Code
   - Go to Extensions (Ctrl+Shift+X)
   - Search "PlatformIO"
   - Install "PlatformIO IDE"

3. **Clone the Repository**
   ```bash
   git clone https://github.com/maziggy/spoolbuddy.git
   cd spoolbuddy/firmware
   ```

4. **Open Project**
   - In VS Code: File → Open Folder → select `firmware/`
   - PlatformIO will detect the project and install dependencies

### Option B: Arduino IDE

1. **Install Arduino IDE 2.x**
   - Download from [arduino.cc](https://www.arduino.cc/en/software)

2. **Add ESP32 Board Support**
   - File → Preferences
   - Add to "Additional Board Manager URLs":
     ```
     https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
     ```
   - Tools → Board → Boards Manager
   - Search "esp32" and install "esp32 by Espressif Systems"

3. **Install Required Libraries**
   - Sketch → Include Library → Manage Libraries
   - Install:
     - `lvgl` (v8.3.x)
     - `TFT_eSPI`
     - `SparkFun_Qwiic_Scale_NAU7802`
     - `ArduinoJson`
     - `WiFiManager`

---

## Board Configuration

### For CrowPanel Advance 7.0"

**PlatformIO (platformio.ini):**
```ini
[env:crowpanel-7]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino
board_build.mcu = esp32s3
board_build.f_cpu = 240000000L
board_upload.flash_size = 16MB
board_upload.maximum_size = 16777216
board_build.partitions = default_16MB.csv

; Display configuration
build_flags =
    -D LV_CONF_INCLUDE_SIMPLE
    -D BOARD_HAS_PSRAM
    -D ARDUINO_USB_CDC_ON_BOOT=1

monitor_speed = 115200
upload_speed = 921600
```

**Arduino IDE:**
- Board: "ESP32S3 Dev Module"
- Flash Size: "16MB"
- PSRAM: "OPI PSRAM"
- USB Mode: "Hardware CDC and JTAG"
- Upload Speed: "921600"

---

## Connecting to Computer

### DIP Switch Settings

Before connecting, check the DIP switches on the back of the CrowPanel:

| Mode | S1 | S0 | Use |
|------|----|----|-----|
| Normal | OFF | OFF | Running firmware |
| **Upload** | **OFF** | **ON** | Flash new firmware |
| Debug | ON | OFF | Serial debugging |

**Set S0=ON, S1=OFF for uploading.**

### USB Connection

1. Connect USB-C cable to CrowPanel
2. Connect other end to computer
3. Display may show blank or old firmware

### Finding the Port

**Windows:**
- Device Manager → Ports (COM & LPT)
- Look for "USB Serial" or "ESP32-S3"
- Note the COM port (e.g., COM3)

**Linux:**
```bash
ls /dev/ttyACM* /dev/ttyUSB*
# Usually /dev/ttyACM0 for ESP32-S3
```

**macOS:**
```bash
ls /dev/cu.usb*
# Usually /dev/cu.usbmodem*
```

---

## Configuration

Before flashing, configure your settings:

### WiFi Configuration

Edit `src/config.h`:

```cpp
// WiFi credentials (or use WiFiManager portal)
#define WIFI_SSID "YourNetworkName"
#define WIFI_PASSWORD "YourPassword"

// Or enable WiFiManager for runtime configuration
#define USE_WIFI_MANAGER true
```

### Backend Server

```cpp
// SpoolBuddy backend server address
#define BACKEND_HOST "192.168.1.100"  // IP of your Raspberry Pi
#define BACKEND_PORT 3000

// WebSocket endpoint
#define WS_PATH "/ws/ui"
```

### Pin Definitions

Verify pin definitions match your wiring:

```cpp
// PN5180 NFC Reader (SPI)
#define PN5180_SCK   43
#define PN5180_MISO  44
#define PN5180_MOSI  16
#define PN5180_NSS   8
#define PN5180_BUSY  2
#define PN5180_RST   15

// NAU7802 Scale (I2C)
#define NAU7802_SDA  19
#define NAU7802_SCL  20
```

---

## Flashing the Firmware

### Using PlatformIO

1. **Build the project**
   - Click the checkmark (✓) in the bottom toolbar
   - Or: Terminal → `pio run`

2. **Upload to device**
   - Click the right arrow (→) in the bottom toolbar
   - Or: Terminal → `pio run -t upload`

3. **Monitor serial output**
   - Click the plug icon in the bottom toolbar
   - Or: Terminal → `pio device monitor`

### Using Arduino IDE

1. **Select board and port**
   - Tools → Board → ESP32S3 Dev Module
   - Tools → Port → (your port)

2. **Compile**
   - Click Verify (checkmark)
   - Wait for compilation

3. **Upload**
   - Click Upload (right arrow)
   - Wait for "Hard resetting via RTS pin..."

4. **Open Serial Monitor**
   - Tools → Serial Monitor
   - Set baud rate to 115200

---

## First Boot

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

### If WiFiManager is Enabled

1. Display shows "WiFi Setup" screen
2. Connect to "SpoolBuddy-Setup" WiFi network
3. Open browser to 192.168.4.1
4. Enter your WiFi credentials
5. Device restarts and connects

---

## Verification Tests

### Test 1: Display

- [ ] Backlight on
- [ ] LVGL UI renders
- [ ] Touch responds

### Test 2: Scale

Place known weight on scale:
```
[SCALE] Weight: 523.4g
```

### Test 3: NFC Reader

Place Bambu Lab spool on scale:
```
[NFC] Tag detected: ISO15693
[NFC] UID: 04:A1:B2:C3:D4:E5:F6:07
[NFC] Reading Bambu data...
[NFC] Material: PLA, Color: Jade White
```

### Test 4: WebSocket

```
[WS] → {"type":"weight","value":523.4}
[WS] ← {"type":"ack"}
```

---

## Troubleshooting

### Upload Fails

1. **Check DIP switches** - S0=ON, S1=OFF for upload
2. **Try different USB cable** - Some cables are charge-only
3. **Hold BOOT button** - Press BOOT while connecting USB
4. **Reduce upload speed** - Change to 460800 or 115200

### No Serial Output

1. **Check USB mode** - Set "USB CDC On Boot: Enabled"
2. **Check baud rate** - Must be 115200
3. **Reset device** - Press RST button

### Display Blank

1. **Check build flags** - PSRAM must be enabled
2. **Verify display driver** - Check TFT_eSPI config
3. **Try factory reset** - Erase flash and re-upload

### PN5180 Not Detected

1. **Verify wiring** - Check all 8 connections
2. **Check voltage** - Must be 3.3V (measure with multimeter)
3. **Don't use J9!** - Use UART0-OUT + J11 headers

### NAU7802 Not Found

1. **Check I2C address** - Should be 0x2A
2. **Verify SDA/SCL** - Not swapped
3. **Run I2C scan** - Confirm device responds

---

## OTA Updates

Once initial firmware is installed, you can update over-the-air:

1. Build new firmware
2. Export binary: `pio run -t export`
3. Upload via web interface (if enabled)

Or re-flash via USB using the same process.

---

## Next Steps

→ **[[Software Setup]]** - Configure the backend server on Raspberry Pi
