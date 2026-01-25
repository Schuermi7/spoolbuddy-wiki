# Troubleshooting

Common issues and solutions for SpoolBuddy.

## Hardware Issues

### PN5180 Not Responding

**Symptoms:**
- No tag detection
- SPI timeout errors
- "PN5180 not found" in logs

**Solutions:**

1. **Verify wiring (most common issue)**
   - [ ] Check all 8 connections against [[Wiring Guide]]
   - [ ] MISO/MOSI are NOT connected to J9 (conflicts with LCD!)
   - [ ] Use UART0-OUT for SCK/MISO, J11 for MOSI/control

2. **Check voltage**
   - [ ] Measure 3.3V at PN5180 VCC pin (with multimeter)
   - [ ] Must be 3.3V, NOT 5V (5V will damage the chip)
   - [ ] Check GND connection is solid

3. **Verify RST pin**
   - [ ] RST should be HIGH (3.3V) during operation
   - [ ] Briefly pull LOW to reset if needed

4. **Reduce SPI speed**
   - Try 500kHz for testing
   - Increase gradually once working

5. **Check BUSY pin behavior**
   - Should go LOW when PN5180 is processing
   - If stuck LOW, may indicate hardware fault

### NAU7802 Erratic Readings

**Symptoms:**
- Weight jumps around
- Negative values
- Drifting readings

**Solutions:**

1. **Check load cell wiring**
   - [ ] If readings inverted: swap A+ and A- wires
   - [ ] Verify all 4 wires securely connected

2. **Ensure stable power**
   - [ ] Use quality USB-C cable
   - [ ] Use 5V/2A power supply
   - [ ] Add 100nF capacitor near NAU7802 if noise persists

3. **Mechanical issues**
   - [ ] Platform only connected via load cell (no touching sides)
   - [ ] Load cell level and secure
   - [ ] Shield from drafts/air currents

4. **Allow warm-up**
   - NAU7802 needs ~1 minute to stabilize
   - Tare after warm-up

5. **Recalibrate**
   ```bash
   curl -X POST http://localhost:3000/api/scale/tare
   curl -X POST http://localhost:3000/api/scale/calibrate \
     -d '{"known_weight": 500.0}'
   ```

### Display Not Working

**Symptoms:**
- Blank screen
- Backlight off
- Touch not responding

**Solutions:**

1. **Power issues**
   - [ ] Try different USB-C cable (data capable)
   - [ ] Try different USB port/power supply
   - [ ] Check 5V/2A minimum

2. **Firmware issues**
   - [ ] Flash firmware with correct board settings
   - [ ] Verify PSRAM enabled in build config
   - [ ] Check serial output for errors

3. **Touch not working**
   - GT911 touch controller is internal
   - May need I2C address configuration
   - Check LVGL touch driver init

---

## Firmware Issues

### Upload Fails

**Solutions:**

1. **DIP switch settings**
   - Set S0=ON, S1=OFF for upload mode
   - Reset after changing switches

2. **USB issues**
   - Try different USB-C cable (some are charge-only)
   - Try different USB port
   - Use USB 2.0 port if USB 3.0 fails

3. **Boot mode**
   - Hold BOOT button while connecting USB
   - Release after connection established

4. **Reduce upload speed**
   - Change from 921600 to 460800 or 115200
   - In platformio.ini: `upload_speed = 460800`

### No Serial Output

**Solutions:**

1. **Check USB mode**
   - Build with `ARDUINO_USB_CDC_ON_BOOT=1`
   - Select correct USB mode in Arduino IDE

2. **Check baud rate**
   - Must match firmware (usually 115200)
   - Try 9600 if 115200 doesn't work

3. **Reset device**
   - Press RST button after upload
   - Check if output appears briefly on boot

### WiFi Connection Fails

**Solutions:**

1. **Credentials**
   - Verify SSID and password exactly correct
   - Check for special characters

2. **Network compatibility**
   - ESP32 only supports 2.4GHz WiFi
   - Check router settings

3. **WiFiManager**
   - If enabled, look for "SpoolBuddy-Setup" network
   - Connect and configure via 192.168.4.1

---

## Backend Issues

### Server Won't Start

**Check logs:**
```bash
journalctl -u spoolbuddy -n 50
```

**Common causes:**

1. **Port in use**
   ```bash
   sudo lsof -i :3000
   # Kill conflicting process or change port
   ```

2. **Missing dependencies**
   ```bash
   cd ~/spoolbuddy/backend
   source venv/bin/activate
   pip install -r requirements.txt
   ```

3. **Database issues**
   ```bash
   # Check permissions
   ls -la spoolbuddy.db
   # Should be readable/writable by pi user
   chmod 664 spoolbuddy.db
   ```

### Can't Connect to Printer

**Symptoms:**
- "Connection refused" errors
- MQTT timeout
- Printer shows disconnected

**Solutions:**

1. **Network verification**
   ```bash
   # Ping printer
   ping 192.168.1.50  # your printer IP

   # Check MQTT port
   nc -zv 192.168.1.50 8883
   ```

2. **Credentials**
   - [ ] Serial number correct (check printer settings)
   - [ ] Access code correct (8 digits)
   - [ ] IP address current (may change with DHCP)

3. **Printer settings**
   - [ ] LAN mode enabled on printer
   - [ ] Printer not in sleep mode

4. **Firewall**
   ```bash
   # On Pi
   sudo ufw status
   # Allow if needed
   sudo ufw allow 8883/tcp
   ```

### WebSocket Disconnects

**Solutions:**

1. **Resource issues**
   ```bash
   htop  # Check CPU/memory
   free -h  # Check available RAM
   ```

2. **Network stability**
   ```bash
   ping -c 100 192.168.1.1  # Check for packet loss
   ```

3. **Timeout settings**
   - Increase WebSocket ping interval in config
   - Check client reconnection logic

---

## NFC Tag Issues

### Tags Not Detected

**Solutions:**

1. **Tag compatibility**
   - Bambu Lab tags are ISO 15693
   - PN5180 supports this (RC522 does NOT)
   - Third-party tags must be ISO 15693

2. **Antenna position**
   - Center under spool core
   - Distance < 20cm from tag
   - No metal between antenna and tag

3. **Interference**
   - Move away from WiFi router
   - Keep away from other NFC devices
   - Metal scale platform may need window/cutout

### Tag Read Errors

**Solutions:**

1. **Slow down**
   - Don't move spool while reading
   - Wait for read confirmation

2. **Spool position**
   - Center spool over antenna
   - Tag is inside core, not on side

3. **Multiple tags**
   - Remove other NFC-tagged spools from area
   - PN5180 can get confused with multiple tags

---

## Scale Issues

### Scale Shows Wrong Weight

**Solutions:**

1. **Re-tare**
   - Remove all weight
   - Tare the scale

2. **Recalibrate**
   - Use known weight (e.g., 500g calibration weight)
   - Run calibration procedure

3. **Check mounting**
   - Load cell level
   - Platform not touching anything

### Negative Weight

**Solutions:**

1. **Swap A+/A- wires**
   - Most common cause
   - Or configure offset in software

2. **Zero offset**
   - May need software zero adjustment
   - Tare should fix this

---

## Quick Diagnostic Commands

### Check Hardware

```bash
# I2C scan (should show NAU7802 at 0x2A)
i2cdetect -y 1

# Check USB devices
lsusb

# Check serial ports
ls /dev/tty*
```

### Check Services

```bash
# SpoolBuddy service
sudo systemctl status spoolbuddy

# View logs
journalctl -u spoolbuddy -f

# Restart service
sudo systemctl restart spoolbuddy
```

### Check Network

```bash
# Pi IP address
ip addr

# Ping printer
ping -c 5 192.168.1.50

# Check ports
netstat -tlnp | grep 3000
```

### Check API

```bash
# Health check
curl http://localhost:3000/api/health

# List printers
curl http://localhost:3000/api/printers

# Current weight
curl http://localhost:3000/api/scale/weight
```

---

## Getting Help

If you're still stuck:

1. **Check the logs** - Usually contains the answer
2. **Search GitHub Issues** - Someone may have had the same problem
3. **Join Discord** - [discord.gg/3XFdHBkF](https://discord.gg/3XFdHBkF)
4. **Open an Issue** - Include logs and hardware details

When reporting issues, include:
- Hardware versions (display, PN5180, etc.)
- Firmware version
- Backend version
- Relevant log output
- Steps to reproduce
