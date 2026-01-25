# SpoolBuddy

<div class="coming-soon">:material-clock-outline: Hardware Coming Soon</div>

<div class="hero" markdown>
<div markdown>

## Smart Filament Management for Bambu Lab

NFC-powered spool identification, precision weight tracking, and seamless inventory management. Scan your spool and auto-configure your AMS slots instantly.

<div class="stats-row">
  <span class="stat-badge">:material-nfc: NFC Tags</span>
  <span class="stat-badge">:material-scale: Weight Scale</span>
  <span class="stat-badge">:material-printer-3d: AMS Integration</span>
</div>

[Get Started :material-arrow-right:](getting-started/index.md){ .btn .btn-primary }
[View on GitHub :material-github:](https://github.com/maziggy/spoolbuddy){ .btn .btn-secondary }

</div>
<div markdown>

![SpoolBuddy Device](assets/preview.png){ .screenshot }

</div>
</div>

---

## Quick Start

<div class="quick-start" markdown>

[:material-shopping: **Hardware**<br><small>Bill of materials</small>](getting-started/hardware.md)

[:material-connection: **Wiring**<br><small>Connection guide</small>](getting-started/wiring.md)

[:material-tools: **Assembly**<br><small>Build instructions</small>](getting-started/assembly.md)

[:material-chip: **Firmware**<br><small>Flash ESP32</small>](getting-started/firmware.md)

[:material-server: **Software**<br><small>Backend setup</small>](getting-started/software.md)

[:material-help-circle: **Troubleshooting**<br><small>Common issues</small>](reference/troubleshooting.md)

</div>

---

## Features

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### :material-nfc: Universal NFC Support
Read any RFID/NFC spool tag including Bambu Lab, third-party brands, or write your own custom tags.
</div>

<div class="feature-card" markdown>
### :material-lightning-bolt: Auto-Configure AMS
Scan a spool and SpoolBuddy automatically updates the matching AMS slot with correct filament type, color, and K-factor.
</div>

<div class="feature-card" markdown>
### :material-scale-balance: Precision Weight Scale
Built-in 5kg load cell with 24-bit ADC tracks remaining filament down to 0.1 gram accuracy.
</div>

<div class="feature-card" markdown>
### :material-tablet: 7" Touchscreen Display
IPS touchscreen shows AMS status, spool details, and inventory at a glance.
</div>

<div class="feature-card" markdown>
### :material-bookshelf: Spool Catalog
Complete inventory management with filtering by material, color, and brand.
</div>

<div class="feature-card" markdown>
### :material-update: OTA Updates
Automatic firmware updates keep your device current over WiFi.
</div>

</div>

---

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                       SpoolBuddy Station                        │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              7" Touchscreen Display                      │   │
│  │         (Elecrow CrowPanel Advance 7.0")                 │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                   │
│  ┌───────────────────────────┼───────────────────────────────┐  │
│  │   ┌─────────┐    ┌───────┴───────┐    ┌─────────────┐    │  │
│  │   │PN5180   │    │  Scale        │    │ Load Cell   │    │  │
│  │   │NFC      │    │  NAU7802      │    │ 5kg         │    │  │
│  │   │Reader   │    │  ADC          │    │             │    │  │
│  │   └─────────┘    └───────────────┘    └─────────────┘    │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │ WiFi
                              ▼
              ┌───────────────────────────────┐
              │      Bambu Lab Printer        │
              │         (via MQTT)            │
              └───────────────────────────────┘
```

---

## Supported Printers

| Series | Models | Status |
|--------|--------|--------|
| X1 | X1, X1C, X1E | :material-check-circle:{ .success } Tested |
| P1 | P1P, P1S | :material-check-circle:{ .success } Compatible |
| P2 | P2S | :material-check-circle:{ .success } Compatible |
| A1 | A1, A1 Mini | :material-check-circle:{ .success } Compatible |
| H2 | H2D | :material-check-circle:{ .success } Tested |
| AMS | AMS, AMS Lite, AMS HT | :material-check-circle:{ .success } All Types |

---

## Requirements

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### :material-currency-usd: Cost
**$100-150** total for all components
</div>

<div class="feature-card" markdown>
### :material-clock-outline: Build Time
**2-4 hours** for assembly
</div>

<div class="feature-card" markdown>
### :material-hammer-wrench: Skill Level
**Intermediate** - soldering required
</div>

</div>

---

## Community

<div class="quick-start" markdown>

[:material-github: **GitHub**<br><small>Source code</small>](https://github.com/maziggy/spoolbuddy)

[:fontawesome-brands-discord: **Discord**<br><small>Get help</small>](https://discord.gg/3XFdHBkF)

</div>
