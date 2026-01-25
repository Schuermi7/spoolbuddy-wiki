# Assembly Guide

Physical assembly and mounting instructions for your SpoolBuddy station.

---

## Tools Required

- :material-screwdriver: Phillips screwdriver (small)
- :material-soldering-iron: Soldering iron + solder
- :material-content-cut: Wire strippers
- :material-tape-measure: Double-sided tape or mounting adhesive
- :material-printer-3d: 3D printer (optional, for enclosure)

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

## Step 2: Position the NFC Antenna

The PN5180's antenna should be positioned under the scale platform, centered where the spool's core will sit.

```
            Top View (Scale Platform)
     ┌────────────────────────────────────┐
     │        ┌──────────────┐            │
     │        │    Spool     │            │
     │        │    Core      │            │
     │        │   ╭────╮     │            │
     │        │   │ ○  │◄────┼── RFID tag location
     │        │   ╰────╯     │            │
     │        └──────────────┘            │
     │              │                     │
     │        ┌─────┴─────┐               │
     │        │  Antenna  │◄── PN5180 here
     │        └───────────┘               │
     └────────────────────────────────────┘
```

### Mounting the PN5180

1. **Position antenna** - Center under spool core position, ~10-20mm below platform surface. Antenna coil facing up.

2. **Secure module** - Use double-sided tape or 3D printed mount. Keep flat and parallel to platform.

3. **Route wires** - Route away from load cell. Avoid wire strain on connections.

### Read Range

- Bambu Lab tags are inside the spool core
- PN5180 has ~20cm read range
- Closer = faster reads
- Through 3mm wood/plastic: no problem

---

## Step 3: Mount the Electronics

### CrowPanel Display

The display can be mounted:

- **Upright** - Behind the scale platform
- **Angled** - Using a stand for better viewing
- **Integrated** - In a 3D printed enclosure

### NAU7802 Placement

- Mount near load cell terminals
- Keep I2C wires short (~10-15cm)
- Protect from vibration

### Raspberry Pi Pico + PN5180

- Mount Pico near PN5180 to keep SPI wires short
- Can be secured with double-sided tape or 3D printed holder
- Position to allow USB access for firmware updates

---

## Step 4: Enclosure Options

### Option A: 3D Printed Enclosure

STL files available in the repository under `/hardware/enclosure/`.

- Base plate with load cell mount
- Scale platform
- Electronics bay
- Display frame

### Option B: DIY Enclosure

Materials:

- Plywood or MDF (6-10mm)
- Acrylic (for display window)
- Rubber feet

### Option C: Bare Assembly

For testing or temporary use:

- Mount load cell to rigid surface
- Place PN5180 under test platform
- Display propped up nearby

---

## Final Checklist

### Mechanical

- [ ] Load cell securely mounted at fixed end
- [ ] Platform attached to load cell free end only
- [ ] Platform has clearance (doesn't touch base/sides)
- [ ] NFC antenna centered under spool position
- [ ] Display mounted stably

### Electrical

- [ ] All wiring complete per [Wiring Guide](wiring.md)
- [ ] No loose connections
- [ ] Wire routing away from moving parts
- [ ] Power cable accessible

### Verification

- [ ] Platform moves freely when pressed
- [ ] No binding or rubbing
- [ ] NFC antenna unobstructed

---

[Next: Firmware Installation :material-arrow-right:](firmware.md){ .btn .btn-primary }
