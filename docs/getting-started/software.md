# Software Setup

Install the SpoolBuddy backend server to connect your station to Bambu printers.

---

## Installation Options

| Method | Best For | Difficulty |
|--------|----------|------------|
| [Docker](#docker-recommended) | Most users, Raspberry Pi, NAS | Easy |
| [Manual](#manual-installation) | Development, customization | Intermediate |

---

## Docker (Recommended)

Docker is the easiest way to run SpoolBuddy. Pre-built images support both x86_64 (Intel/AMD) and ARM64 (Raspberry Pi 4/5).

### Prerequisites

Install Docker and Docker Compose:

=== "Raspberry Pi / Debian / Ubuntu"

    ```bash
    # Install Docker
    curl -fsSL https://get.docker.com | sh

    # Add your user to docker group
    sudo usermod -aG docker $USER

    # Log out and back in, then verify
    docker --version
    ```

=== "Synology NAS"

    Install "Container Manager" from Package Center.

=== "UNRAID"

    Docker is built-in. Use the Community Applications plugin.

=== "macOS / Windows"

    Install [Docker Desktop](https://www.docker.com/products/docker-desktop/){ target="_blank" }

### Quick Start

```bash
# Create a directory for SpoolBuddy
mkdir ~/spoolbuddy && cd ~/spoolbuddy

# Download docker-compose.yml
curl -O https://raw.githubusercontent.com/maziggy/spoolbuddy/main/docker-compose.yml

# Start SpoolBuddy
docker compose up -d

# Check logs
docker compose logs -f
```

SpoolBuddy is now running at `http://localhost:3000`

### Docker Compose Configuration

```yaml title="docker-compose.yml"
name: spoolbuddy

services:
  spoolbuddy:
    image: ghcr.io/maziggy/spoolbuddy:latest
    # Host network required for SSDP printer discovery
    network_mode: host
    volumes:
      - spoolbuddy-data:/app/data
    restart: unless-stopped

volumes:
  spoolbuddy-data:
```

!!! warning "Host Network Mode Required"
    SpoolBuddy uses `network_mode: host` instead of port mapping. This is **required** for automatic printer discovery via SSDP (Simple Service Discovery Protocol).

    SSDP uses UDP multicast on port 1900, which doesn't work through Docker's default bridge network. With host mode:

    - SpoolBuddy can discover Bambu printers on your local network
    - The container shares the host's network stack directly
    - Port 3000 is exposed automatically (no `-p 3000:3000` needed)

    **If you use bridge networking**, printer discovery won't work and you'll need to add printers manually by IP address.

### Environment Variables

Customize SpoolBuddy with environment variables:

```yaml title="docker-compose.yml"
services:
  spoolbuddy:
    image: ghcr.io/maziggy/spoolbuddy:latest
    network_mode: host
    volumes:
      - spoolbuddy-data:/app/data
    environment:
      # Database location (inside container)
      SPOOLBUDDY_DATABASE_PATH: /app/data/spoolbuddy.db
      # Log level: DEBUG, INFO, WARNING, ERROR
      LOG_LEVEL: INFO
    restart: unless-stopped

volumes:
  spoolbuddy-data:
```

### Updating

```bash
cd ~/spoolbuddy

# Pull latest image
docker compose pull

# Restart with new image
docker compose up -d

# Clean up old images
docker image prune -f
```

### Using a Specific Version

Pin to a specific version instead of `latest`:

```yaml
image: ghcr.io/maziggy/spoolbuddy:0.1.1b2
```

View all available versions on [GitHub Packages](https://github.com/maziggy/spoolbuddy/pkgs/container/spoolbuddy){ target="_blank" }.

### Viewing Logs

```bash
# Follow logs in real-time
docker compose logs -f

# Last 100 lines
docker compose logs --tail 100

# Logs from specific time
docker compose logs --since 1h
```

### Backup & Restore

**Backup:**
```bash
# Stop container
docker compose down

# Copy database
docker run --rm -v spoolbuddy_spoolbuddy-data:/data -v $(pwd):/backup alpine \
    cp /data/spoolbuddy.db /backup/spoolbuddy-backup.db

# Restart
docker compose up -d
```

**Restore:**
```bash
# Stop container
docker compose down

# Restore database
docker run --rm -v spoolbuddy_spoolbuddy-data:/data -v $(pwd):/backup alpine \
    cp /backup/spoolbuddy-backup.db /data/spoolbuddy.db

# Restart
docker compose up -d
```

### Troubleshooting Docker

| Issue | Solution |
|-------|----------|
| Port 3000 already in use | Stop other services or change port |
| Can't discover printers | Ensure `network_mode: host` is set |
| Permission denied | Add user to docker group: `sudo usermod -aG docker $USER` |
| Container keeps restarting | Check logs: `docker compose logs` |
| ARM64 image slow on Pi | Ensure you're using 64-bit Raspberry Pi OS |

---

## Manual Installation

For development or when Docker isn't available.

### Raspberry Pi Setup

1. **Flash Raspberry Pi OS Lite** (64-bit) using Raspberry Pi Imager. Configure WiFi and SSH in imager settings.

2. **Connect via SSH:**
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

# Install pip, venv, and git
sudo apt install -y python3-pip python3-venv git

# Node.js 18+ (for frontend development)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

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

### Build Frontend (Optional)

If you want to modify the frontend:

```bash
cd ~/spoolbuddy/frontend
npm install
npm run build
```

### Running the Server

**Manual start:**
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

| Setting | Location on Printer |
|---------|---------------------|
| Serial Number | Settings → Device → Device Info |
| Access Code | Settings → Network → Access Code |
| IP Address | Settings → Network → IP Address |

!!! tip "Serial Number Format"
    The serial number looks like `01S00A0B0012345` (15 characters)

### Adding Printer via Web UI

1. Open SpoolBuddy at `http://[your-ip]:3000`
2. Click **Printers** in the navigation
3. Click **Add Printer**
4. Enter the printer details and click **Save**

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

### Auto-Discovery

SpoolBuddy can automatically discover Bambu printers on your network using SSDP:

1. Go to **Printers** → **Discover**
2. Found printers will appear in the list
3. Click a printer to add it (you'll still need the access code)

!!! note "Network Requirements"
    Auto-discovery requires the backend to be on the same network/VLAN as your printers. Docker must use `network_mode: host`.

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

Connect to `ws://[ip]:3000/ws/ui` for real-time updates:

- Printer status changes
- AMS slot updates
- Scale weight changes
- NFC tag detection

---

## You're Done!

Your SpoolBuddy station is now fully set up. Place a spool on the scale and watch it automatically identify the filament!

[Troubleshooting :material-arrow-right:](../reference/troubleshooting.md){ .btn .btn-secondary }
