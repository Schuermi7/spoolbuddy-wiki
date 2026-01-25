# Assembly Guide

Physical assembly and mounting instructions for your SpoolBuddy station.

---

## Tools Required

- :material-screwdriver: Phillips screwdriver (small)
- :material-soldering-iron: Soldering iron + solder
- :material-content-cut: Wire strippers
- :material-tape-measure: Double-sided tape or mounting adhesive
- :material-printer-3d: 3D printer (for enclosure)

---

## Assembly Overview

```
┌─────────────────────────────────────────────────────────────┐
│                   SpoolBuddy Station                        │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │             CrowPanel 7.0" Display                  │   │
│   │               (Mounted Upright)                     │   │
│   └─────────────────────────────────────────────────────┘   │
│                          │                                   │
│   ┌──────────────────────┴──────────────────────────────┐   │
│   │    ┌────────────────────────────────────────┐       │   │
│   │    │          Scale Platform               │       │   │
│   │    │    ┌──────────────────────────────┐   │       │   │
│   │    │    │      Spool Placement         │   │       │   │
│   │    │    │    ╭──────────╮              │   │       │   │
│   │    │    │    │  NFC     │  ← Antenna   │   │       │   │
│   │    │    │    │  Reader  │    Below     │   │       │   │
│   │    │    │    ╰──────────╯              │   │       │   │
│   │    │    └──────────────────────────────┘   │       │   │
│   │    └────────────────────────────────────────┘       │   │
│   │    ┌─────────────────┴─────────────────┐            │   │
│   │    │         Load Cell (hidden)        │            │   │
│   │    └───────────────────────────────────┘            │   │
│   │    ┌──────────┐           ┌──────────────┐          │   │
│   │    │NAU7802   │           │  Pico + NFC  │          │   │
│   │    └──────────┘           └──────────────┘          │   │
│   └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## Step 1: Mount the Load Cell

The 5kg load cell is a bar-type single-point sensor. One end is fixed to the base, the other supports the scale platform.

```
                 Fixed End                    Free End
                    │                            │
                    ▼                            ▼
            ┌───────────────────────────────────────────┐
            │  ●                                    ●   │
            │     ┌─────────────────────────────┐       │
            │     │    Strain Gauge Element     │       │
            │     └─────────────────────────────┘       │
            │  ●                                    ●   │
            └───────────────────────────────────────────┘
                 │                                │
            ┌────┴────┐                    ┌──────┴──────┐
            │  Base   │                    │   Platform  │
            │ (fixed) │                    │  (floats)   │
            └─────────┘                    └─────────────┘
```

### Mounting Steps

1. **Attach to base** - Use 2x M4x25 screws on the fixed end. Ensure load cell is level. Tighten firmly.

2. **Attach platform** - Use 2x M4x25 screws on the free end. Platform should "float" and only connect via load cell.

3. **Check clearance** - Platform should not touch base or sides. Maintain ~2-3mm gap all around.

!!! warning "Critical"
    The load cell must be the **ONLY** connection between base and platform for accurate readings.

---

## Step 2: 3D Printed Enclosure

A complete 3D printable enclosure kit is available on MakerWorld:

[:material-printer-3d: **Download Enclosure from MakerWorld**](https://makerworld.com/en/models/2296982-spoolbuddy#profileId-2506740){ .btn .btn-primary target="_blank" }

The enclosure includes:

- Base plate with load cell mounting points
- Scale platform (connects to load cell)
- Electronics bay for NAU7802 and Raspberry Pi Pico
- PN5180 NFC antenna mount
- Display frame for CrowPanel 7.0"
- Cable management clips

### Print Settings

| Setting | Value |
|---------|-------|
| Material | PETG or ABS (PLA for testing) |
| Layer Height | 0.2mm |
| Infill | 20-30% |
| Wall Loops | 3-4 for structural parts |
| Supports | Required for some parts (noted in filenames) |

---

## Final Checklist

### Mechanical

- [ ] Enclosure printed and assembled
- [ ] Load cell securely mounted
- [ ] Platform attached to load cell (free-floating)
- [ ] Platform has clearance (doesn't touch sides)
- [ ] Display mounted stably

### Electrical

- [ ] All wiring complete per [Wiring Guide](wiring.md)
- [ ] No loose connections
- [ ] Wire routing away from moving parts
- [ ] Power cables accessible

### Verification

- [ ] Platform moves freely when pressed
- [ ] No binding or rubbing
- [ ] All components secured

---

[Next: Firmware Installation :material-arrow-right:](firmware.md){ .btn .btn-primary }
