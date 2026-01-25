# Software Setup

Configure the backend server on Raspberry Pi and connect to your Bambu printer.

---

## Raspberry Pi Setup

### Initial OS Setup

1. **Flash Raspberry Pi OS Lite** - Use Raspberry Pi Imager, choose "Raspberry Pi OS Lite (64-bit)", configure WiFi and SSH in imager settings

2. **Boot and connect:**
   ```bash
   ssh pi@raspberrypi.local
   ```

3. **Update system:**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

### Install Dependencies

```bash
# Python 3.10+ (usually pre-installed)
python3 --version

# Install pip and venv
sudo apt install -y python3-pip python3-venv git
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

This installs FastAPI, Uvicorn, SQLAlchemy, Paho-MQTT, Pydantic, and Websockets.

---

## Configuration

### Environment Variables

Create a `.env` file:

```bash title=".env"
# Server settings
HOST=0.0.0.0
PORT=3000

# Database
DATABASE_URL=sqlite:///./spoolbuddy.db

# Bambu Cloud (optional)
BAMBU_CLOUD_EMAIL=your@email.com
BAMBU_CLOUD_PASSWORD=yourpassword
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

Create service file:

```bash
sudo nano /etc/systemd/system/spoolbuddy.service
```

```ini title="/etc/systemd/system/spoolbuddy.service"
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
```

---

## Connecting to Bambu Printer

### Finding Printer Information

1. **Serial Number:** Printer Settings → Device → Device Info (format: `01S00A0B0012345`)

2. **Access Code:** Printer Settings → Network → Access Code (8-digit code)

3. **IP Address:** Printer Settings → Network → IP Address

### Adding Printer via API

```bash
curl -X POST http://localhost:3000/api/printers \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My X1C",
    "serial": "01S00A0B0012345",
    "access_code": "12345678",
    "ip_address": "192.168.1.50"
  }'
```

### Verify Connection

```bash
curl http://localhost:3000/api/printers
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

### WebSocket

Connect to `ws://[pi-ip]:3000/ws/ui` for real-time updates.

---

## You're Done! :material-party-popper:

Your SpoolBuddy station is now fully set up. Place a spool on the scale and watch it automatically identify the filament!

[Troubleshooting :material-arrow-right:](../reference/troubleshooting.md){ .btn .btn-secondary }
