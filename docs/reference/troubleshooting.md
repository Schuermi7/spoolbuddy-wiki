# Troubleshooting

Common issues and solutions for SpoolBuddy.

---

## PN5180 NFC Issues

### PN5180 Not Responding

**Symptoms:** No tag detection, SPI timeout errors, "PN5180 not found"

**Solutions:**

1. **Verify wiring** - Check all 8 connections against [Wiring Guide](../getting-started/wiring.md)
2. **Don't use J9!** - MISO/MOSI must NOT be connected to J9 (conflicts with LCD)
3. **Check voltage** - Measure 3.3V at PN5180 VCC pin (NOT 5V!)
4. **Verify RST pin** - Should be HIGH (3.3V) during operation
5. **Reduce SPI speed** - Try 500kHz for testing

### Tags Not Detected

- **Tag compatibility:** Bambu Lab tags are ISO 15693. RC522 does NOT work!
- **Antenna position:** Center under spool core, <20cm from tag
- **Interference:** Move away from WiFi router, metal surfaces

---

## Scale Issues

### NAU7802 Erratic Readings

**Symptoms:** Weight jumps around, negative values, drifting

**Solutions:**

1. **Check load cell wiring** - If readings inverted, swap A+ and A- wires
2. **Stable power** - Use quality USB-C cable and 5V/2A power supply
3. **Mechanical issues** - Platform must only connect via load cell (no touching sides)
4. **Allow warm-up** - NAU7802 needs ~1 minute to stabilize
5. **Shield from drafts** - Air currents affect readings

### Negative Weight

- Swap A+ and A- wires
- Or configure offset in software
- Tare after fixing

---

## Display Issues

### Blank Screen

- **Power:** Try different USB-C cable (must be data capable)
- **Firmware:** Flash with correct board settings, PSRAM enabled
- **Reset:** Press RST button

### Touch Not Working

- GT911 touch controller is internal
- Check LVGL touch driver initialization
- May need I2C address configuration

---

## Firmware Issues

### Upload Fails

| Issue | Solution |
|-------|----------|
| No port detected | Try different USB-C cable (some are charge-only) |
| Timeout errors | Set DIP switches: S0=ON, S1=OFF |
| Permission denied | Add user to dialout group (Linux) |
| Wrong board | Select ESP32S3 Dev Module |

### No Serial Output

- Build with `ARDUINO_USB_CDC_ON_BOOT=1`
- Check baud rate: 115200
- Press RST button after upload

---

## Backend Issues

### Server Won't Start

```bash
# Check logs
journalctl -u spoolbuddy -n 50

# Port in use?
sudo lsof -i :3000

# Missing dependencies?
pip install -r requirements.txt

# Database permissions?
chmod 664 spoolbuddy.db
```

### WebSocket Disconnects

- Check Pi resources: `htop`
- Check network stability
- Increase WebSocket ping interval

---

## Printer Connection Issues

### Can't Connect to Printer

**Symptoms:** "Connection refused", MQTT timeout

**Verify:**

1. **Network:** Pi and printer on same network
   ```bash
   ping 192.168.1.50  # your printer IP
   ```

2. **Credentials:** Serial number and access code correct

3. **Printer settings:** LAN mode enabled, not in sleep mode

4. **Port:** MQTT uses port 8883
   ```bash
   nc -zv 192.168.1.50 8883
   ```

---

## Diagnostic Commands

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
3. **Join Discord** - [:fontawesome-brands-discord: discord.gg/3XFdHBkF](https://discord.gg/3XFdHBkF){ target="_blank" }
4. **Open an Issue** - Include logs and hardware details
