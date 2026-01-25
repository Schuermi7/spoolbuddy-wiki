# SpoolBuddy Wiki

Welcome to the SpoolBuddy documentation! This wiki covers everything you need to build and set up your own SpoolBuddy filament management station.

## What is SpoolBuddy?

SpoolBuddy is an open-source filament management system for Bambu Lab 3D printers. It combines:

- **NFC Tag Reading** - Automatically identify spools using Bambu Lab's RFID tags
- **Weight Scale Integration** - Track remaining filament with precision weighing
- **Touchscreen Interface** - 7" display for easy spool management
- **MQTT Integration** - Seamless connection to your Bambu Lab printer's AMS

## Quick Navigation

| Section | Description |
|---------|-------------|
| [[Hardware Required]] | Complete bill of materials with purchase links |
| [[Wiring Guide]] | Detailed wiring diagrams and pin connections |
| [[Assembly Guide]] | Physical assembly and mounting instructions |
| [[Firmware Installation]] | Flashing the ESP32 firmware |
| [[Software Setup]] | Backend and frontend server configuration |
| [[Troubleshooting]] | Common issues and solutions |

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                       SpoolBuddy Station                        │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              7" Touchscreen Display                      │   │
│  │         (Elecrow CrowPanel Advance 7.0")                 │   │
│  │                                                          │   │
│  │   ┌─────────────┐    ┌─────────────────────────────┐    │   │
│  │   │   Spool     │    │  Dashboard / Inventory UI    │    │   │
│  │   │   Weight    │    │                              │    │   │
│  │   │   1234g     │    │  Material: PLA Matte        │    │   │
│  │   │             │    │  Color: Jade White           │    │   │
│  │   └─────────────┘    │  Remaining: 847g             │    │   │
│  │                       └─────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                   │
│  ┌───────────────────────────┼───────────────────────────────┐  │
│  │                           │                               │  │
│  │   ┌─────────┐    ┌───────┴───────┐    ┌─────────────┐   │  │
│  │   │PN5180   │    │  Scale        │    │ Load Cell   │   │  │
│  │   │NFC      │    │  NAU7802      │    │ 5kg         │   │  │
│  │   │Reader   │    │  ADC          │    │             │   │  │
│  │   └─────────┘    └───────────────┘    └─────────────┘   │  │
│  │                                                          │  │
│  │                    Sensor Module                         │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ WiFi
                              ▼
              ┌───────────────────────────────┐
              │      Bambu Lab Printer        │
              │         (via MQTT)            │
              └───────────────────────────────┘
```

## Features

### Automatic Spool Identification
- Reads Bambu Lab's built-in RFID tags (ISO 15693)
- Works with OEM and third-party NFC-tagged spools
- Instant spool recognition when placed on scale

### Precise Weight Tracking
- 5kg load cell with 24-bit ADC precision
- Real-time weight updates
- Track filament usage across projects

### Printer Integration
- Direct MQTT connection to Bambu Lab printers
- Update AMS slot information automatically
- Sync spool data with printer

### Inventory Management
- Full spool database with materials, colors, brands
- Track empty spool weights by vendor
- Custom vendor color database

## Requirements

Before you begin, you'll need:

1. **Hardware Components** (~$100-150 total)
   - See [[Hardware Required]] for the complete BOM

2. **Basic Tools**
   - Soldering iron and solder
   - Wire strippers
   - Small screwdrivers
   - Multimeter (recommended)

3. **Software**
   - Python 3.10+ for the backend server
   - Node.js 18+ for frontend development
   - PlatformIO or Arduino IDE for firmware

4. **Network**
   - WiFi network accessible by both SpoolBuddy and your Bambu printer
   - Printer's access code and serial number

## Getting Started

1. **Order Components** - Follow the [[Hardware Required]] guide
2. **Wire Everything** - Use the [[Wiring Guide]] for connections
3. **Assemble the Station** - See [[Assembly Guide]]
4. **Flash Firmware** - Follow [[Firmware Installation]]
5. **Set Up Server** - Complete [[Software Setup]]
6. **Configure & Use** - You're ready to go!

## Community

- **GitHub**: [github.com/maziggy/spoolbuddy](https://github.com/maziggy/spoolbuddy)
- **Discord**: [Join our Discord server](https://discord.gg/3XFdHBkF)

## License

SpoolBuddy is open source under the MIT License.
