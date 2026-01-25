# Software Setup

This guide covers setting up the SpoolBuddy backend server on a Raspberry Pi (or other Linux system) and connecting it to your Bambu Lab printer.

## Prerequisites

- Raspberry Pi Zero 2 W (or Pi 3/4/5)
- MicroSD card (16GB+) with Raspberry Pi OS
- Network connection (WiFi or Ethernet)
- SSH access to the Pi
- Your Bambu Lab printer's:
  - Serial number
  - Access code
  - Local IP address

---

## Raspberry Pi Setup

### Initial OS Setup

1. **Flash Raspberry Pi OS Lite**
   - Use Raspberry Pi Imager
   - Choose "Raspberry Pi OS Lite (64-bit)"
   - Configure WiFi and SSH in imager settings

2. **Boot and connect**
   ```bash
   ssh pi@raspberrypi.local
   # or use IP address
   ```

3. **Update system**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

### Install Dependencies

```bash
# Python 3.10+ (usually pre-installed)
python3 --version

# Install pip and venv
sudo apt install -y python3-pip python3-venv

# Install git
sudo apt install -y git
```

---

## Backend Installation

### Clone Repository

```bash
cd ~
git clone https://github.com/maziggy/spoolbuddy.git
cd spoolbuddy/backend
```

### Create Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### Install Python Dependencies

```bash
pip install -r requirements.txt
```

This installs:
- FastAPI (web framework)
- Uvicorn (ASGI server)
- SQLAlchemy (database ORM)
- Paho-MQTT (Bambu printer connection)
- Pydantic (data validation)
- Websockets (real-time updates)

---

## Configuration

### Environment Variables

Create a `.env` file or set environment variables:

```bash
# Create config file
nano .env
```

```env
# Server settings
HOST=0.0.0.0
PORT=3000

# Database
DATABASE_URL=sqlite:///./spoolbuddy.db

# Bambu Cloud (optional, for filament presets)
BAMBU_CLOUD_EMAIL=your@email.com
BAMBU_CLOUD_PASSWORD=yourpassword

# Or use token directly
BAMBU_CLOUD_TOKEN=your_jwt_token
```

### Printer Configuration

Printers are configured via the web UI or API:

```bash
# Example: Add printer via API
curl -X POST http://localhost:3000/api/printers \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My X1C",
    "serial": "PRINTER_SERIAL",
    "access_code": "12345678",
    "ip_address": "192.168.1.50"
  }'
```

---

## Running the Server

### Manual Start

```bash
cd ~/spoolbuddy/backend
source venv/bin/activate
python main.py
```

Expected output:
```
INFO:     SpoolBuddy Backend v1.0.0
INFO:     Database initialized
INFO:     Starting server on 0.0.0.0:3000
INFO:     Uvicorn running on http://0.0.0.0:3000
```

### Systemd Service (Auto-start)

Create a service file:

```bash
sudo nano /etc/systemd/system/spoolbuddy.service
```

```ini
[Unit]
Description=SpoolBuddy Backend Server
After=network.target

[Service]
Type=simple
User=pi
WorkingDirectory=/home/pi/spoolbuddy/backend
Environment=PATH=/home/pi/spoolbuddy/backend/venv/bin
ExecStart=/home/pi/spoolbuddy/backend/venv/bin/python main.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Enable and start:

```bash
sudo systemctl daemon-reload
sudo systemctl enable spoolbuddy
sudo systemctl start spoolbuddy

# Check status
sudo systemctl status spoolbuddy

# View logs
journalctl -u spoolbuddy -f
```

---

## Frontend Setup (Development)

For development, you can run the frontend separately:

### Install Node.js

```bash
# On Pi (using nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 18
nvm use 18
```

### Build Frontend

```bash
cd ~/spoolbuddy/frontend
npm install
npm run build
```

The built files go to `dist/` and are served by the backend.

### Development Mode

```bash
npm run dev
# Frontend at http://localhost:5173
# Proxies API calls to backend at :3000
```

---

## Connecting to Bambu Printer

### Finding Printer Information

1. **Serial Number**
   - Printer Settings → Device → Device Info
   - Format: `01S00A0B0012345`

2. **Access Code**
   - Printer Settings → Network → Access Code
   - 8-digit code like `12345678`

3. **IP Address**
   - Printer Settings → Network → IP Address
   - Or check your router's DHCP leases

### Adding Printer in SpoolBuddy

Via web interface:
1. Open http://[pi-ip]:3000
2. Go to Printers page
3. Click "Add Printer"
4. Enter serial, access code, and IP
5. Click Connect

Via API:
```bash
curl -X POST http://localhost:3000/api/printers \
  -H "Content-Type: application/json" \
  -d '{
    "name": "X1 Carbon",
    "serial": "01S00A0B0012345",
    "access_code": "12345678",
    "ip_address": "192.168.1.50"
  }'
```

### Verify Connection

```bash
# Check printer status
curl http://localhost:3000/api/printers

# Should show:
{
  "printers": [{
    "name": "X1 Carbon",
    "serial": "01S00A0B0012345",
    "connected": true,
    "status": "IDLE"
  }]
}
```

---

## Bambu Cloud Integration (Optional)

To access Bambu's filament database for presets:

### Authentication Methods

**Method 1: Email/Password**
```bash
curl -X POST http://localhost:3000/api/cloud/login \
  -H "Content-Type: application/json" \
  -d '{"email": "your@email.com", "password": "yourpassword"}'
```

If 2FA is enabled, you'll receive a verification request.

**Method 2: Token**
1. Log into Bambu Handy app
2. Extract JWT token (advanced)
3. Set via API:
```bash
curl -X POST http://localhost:3000/api/cloud/token \
  -H "Content-Type: application/json" \
  -d '{"token": "your_jwt_token"}'
```

### Syncing Filament Presets

Once authenticated:
```bash
curl http://localhost:3000/api/cloud/settings
# Returns available filament presets
```

---

## WebSocket API

The display connects to the backend via WebSocket:

### Connection

```
ws://[pi-ip]:3000/ws/ui
```

### Message Types

**From Server:**
```json
{"type": "initial_state", "data": {...}}
{"type": "weight", "value": 523.4}
{"type": "tag_detected", "uid": "04:A1:B2...", "spool": {...}}
{"type": "printer_state", "serial": "01S...", "state": {...}}
```

**From Client:**
```json
{"type": "subscribe", "events": ["weight", "tag"]}
{"type": "tare"}
{"type": "calibrate", "known_weight": 500.0}
```

---

## Database Management

### Location

```
~/spoolbuddy/backend/spoolbuddy.db
```

### Backup

```bash
cp spoolbuddy.db spoolbuddy.db.backup
```

### Reset (Caution!)

```bash
rm spoolbuddy.db
# Restart server to recreate
sudo systemctl restart spoolbuddy
```

### View Data

```bash
sqlite3 spoolbuddy.db
.tables
SELECT * FROM spools LIMIT 5;
.quit
```

---

## API Reference

### Spools

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/spools` | GET | List all spools |
| `/api/spools` | POST | Create spool |
| `/api/spools/{id}` | GET | Get spool |
| `/api/spools/{id}` | PUT | Update spool |
| `/api/spools/{id}` | DELETE | Delete spool |

### Printers

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/printers` | GET | List printers |
| `/api/printers` | POST | Add printer |
| `/api/printers/{serial}/connect` | POST | Connect |
| `/api/printers/{serial}/disconnect` | POST | Disconnect |

### AMS

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/printers/{serial}/ams/{id}/tray/{tray}/filament` | POST | Set filament |
| `/api/printers/{serial}/ams/{id}/tray/{tray}/calibration` | POST | Set K-value |

---

## Firewall Configuration

If using a firewall, allow port 3000:

```bash
sudo ufw allow 3000/tcp
```

---

## Troubleshooting

### Server Won't Start

```bash
# Check logs
journalctl -u spoolbuddy -n 50

# Common issues:
# - Port already in use: sudo lsof -i :3000
# - Missing dependencies: pip install -r requirements.txt
# - Database permissions: chmod 664 spoolbuddy.db
```

### Can't Connect to Printer

1. **Verify network** - Pi and printer on same network
2. **Check firewall** - Printer allows MQTT (port 8883)
3. **Verify credentials** - Serial and access code correct
4. **Check printer** - LAN mode enabled in printer settings

### WebSocket Disconnects

1. **Check Pi resources** - `htop` for CPU/memory
2. **Check network stability** - `ping` printer and display
3. **Increase timeout** - Adjust WebSocket ping interval

---

## Updating

### Pull Latest Changes

```bash
cd ~/spoolbuddy
git pull origin main

# Backend
cd backend
source venv/bin/activate
pip install -r requirements.txt

# Frontend
cd ../frontend
npm install
npm run build

# Restart
sudo systemctl restart spoolbuddy
```

---

## Next Steps

→ **[[Troubleshooting]]** - Common issues and solutions

→ **[[Pin Reference]]** - Hardware pin reference
