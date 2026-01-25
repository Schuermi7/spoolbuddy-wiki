# Assembly Guide

This guide covers the physical assembly of your SpoolBuddy station, including mounting components and positioning for optimal operation.

## Tools Required

- Phillips screwdriver (small)
- Soldering iron + solder
- Wire strippers
- Heat shrink tubing (optional but recommended)
- Double-sided tape or mounting adhesive
- 3D printer (for enclosure, optional)

---

## Assembly Overview

```
┌─────────────────────────────────────────────────────────────┐
│                   SpoolBuddy Station                        │
│                                                             │
│   ┌─────────────────────────────────────────────────────┐   │
│   │                                                     │   │
│   │             CrowPanel 7.0" Display                  │   │
│   │               (Mounted Upright)                     │   │
│   │                                                     │   │
│   └─────────────────────────────────────────────────────┘   │
│                          │                                   │
│   ┌──────────────────────┴──────────────────────────────┐   │
│   │                                                      │   │
│   │    ┌────────────────────────────────────────┐       │   │
│   │    │          Scale Platform               │       │   │
│   │    │                                        │       │   │
│   │    │    ┌──────────────────────────────┐   │       │   │
│   │    │    │      Spool Placement         │   │       │   │
│   │    │    │          Area                │   │       │   │
│   │    │    │                              │   │       │   │
│   │    │    │    ╭──────────╮              │   │       │   │
│   │    │    │    │  NFC     │  ← Antenna   │   │       │   │
│   │    │    │    │  Reader  │    Below     │   │       │   │
│   │    │    │    ╰──────────╯              │   │       │   │
│   │    │    │                              │   │       │   │
│   │    │    └──────────────────────────────┘   │       │   │
│   │    │                                        │       │   │
│   │    └────────────────────────────────────────┘       │   │
│   │                      │                               │   │
│   │    ┌─────────────────┴─────────────────┐            │   │
│   │    │         Load Cell (hidden)        │            │   │
│   │    └───────────────────────────────────┘            │   │
│   │                                                      │   │
│   │    ┌──────────┐           ┌──────────────┐          │   │
│   │    │NAU7802   │           │ Pi Zero 2 W  │          │   │
│   │    │Scale ADC │           │ (Backend)    │          │   │
│   │    └──────────┘           └──────────────┘          │   │
│   │                                                      │   │
│   └──────────────────────────────────────────────────────┘   │
│                            Base                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Step 1: Prepare the Load Cell

### Mounting the Load Cell

The 5kg load cell is a bar-type single-point sensor. One end is fixed to the base, the other supports the scale platform.

```
                 Fixed End                    Free End
                    │                            │
                    ▼                            ▼
            ┌───────────────────────────────────────────┐
            │  ●                                    ●   │
            │  ○                                    ○   │
            │     ┌─────────────────────────────┐       │
            │     │    Strain Gauge Element     │       │
            │     └─────────────────────────────┘       │
            │  ○                                    ○   │
            │  ●                                    ●   │
            └───────────────────────────────────────────┘
                 │                                │
                 │                                │
            ┌────┴────┐                    ┌──────┴──────┐
            │  Base   │                    │   Platform  │
            │ (fixed) │                    │  (floats)   │
            └─────────┘                    └─────────────┘
```

### Mounting Steps

1. **Attach to base**
   - Use 2x M4x25 screws on the fixed end
   - Ensure load cell is level
   - Tighten firmly

2. **Attach platform**
   - Use 2x M4x25 screws on the free end
   - Platform should "float" and only connect via load cell

3. **Check clearance**
   - Platform should not touch base or sides
   - ~2-3mm gap all around

> **Important:** The load cell must be the ONLY connection between base and platform for accurate readings.

---

## Step 2: Position the NFC Antenna

### Optimal Placement

The PN5180's antenna should be positioned under the scale platform, centered where the spool's core will sit.

```
            Top View (Scale Platform)
     ┌────────────────────────────────────┐
     │                                    │
     │        ┌──────────────┐            │
     │        │              │            │
     │        │    Spool     │            │
     │        │    Core      │            │
     │        │   ╭────╮     │            │
     │        │   │ ○  │◄────┼── RFID tag location
     │        │   ╰────╯     │            │
     │        │              │            │
     │        └──────────────┘            │
     │              │                     │
     │              │                     │
     │        ┌─────┴─────┐               │
     │        │  Antenna  │◄── PN5180 antenna
     │        │  (below)  │    positioned here
     │        └───────────┘               │
     │                                    │
     └────────────────────────────────────┘
```

### Mounting the PN5180

1. **Position antenna**
   - Center under spool core position
   - ~10-20mm below platform surface
   - Antenna coil facing up

2. **Secure module**
   - Use double-sided tape or 3D printed mount
   - Keep flat and parallel to platform

3. **Route wires**
   - Route away from load cell
   - Avoid wire strain on connections

### Read Range Considerations

- Bambu Lab tags are inside the spool core
- PN5180 has ~20cm read range
- Closer = faster reads
- Through 3mm wood/plastic: no problem

---

## Step 3: Wire the Components

Follow the [[Wiring Guide]] for detailed connections. Summary:

1. **PN5180 → CrowPanel**
   - 4 wires to UART0-OUT header
   - 4 wires to J11 header

2. **NAU7802 → CrowPanel**
   - 4 wires to UART1-OUT header

3. **Load Cell → NAU7802**
   - 4 wires to screw terminals

### Wire Management Tips

- Use color-coded wires (as specified in Wiring Guide)
- Keep wires neat and organized
- Use zip ties or cable clips
- Leave some slack for maintenance

---

## Step 4: Mount the Electronics

### CrowPanel Display

The display can be mounted:
- **Upright** - Behind the scale platform
- **Angled** - Using a stand for better viewing
- **Integrated** - In a 3D printed enclosure

```
           Upright Mount              Angled Mount

              ┌───┐                     ╱───╲
              │   │                    ╱     ╲
              │   │                   ╱       ╲
              │   │                  ╱    ○    ╲
              │ ○ │                 ╱           ╲
              │   │                ╱             ╲
              └───┘               ╱───────────────╲
                │                       │
           ─────┴─────            ──────┴──────
```

### NAU7802 Placement

- Mount near load cell terminals
- Keep I2C wires short (~10-15cm)
- Protect from vibration

### Raspberry Pi Zero 2 W

- Mount in enclosure or separate case
- Ensure good ventilation
- Keep accessible for SD card

---

## Step 5: Enclosure Options

### Option A: 3D Printed Enclosure

STL files available in the repository under `/hardware/enclosure/`.

Components:
- Base plate with load cell mount
- Scale platform
- Electronics bay
- Display frame

### Option B: DIY Enclosure

Materials:
- Plywood or MDF (6-10mm)
- Acrylic (for display window)
- Rubber feet

Basic structure:
1. Base with load cell mounting holes
2. Floating platform connected only via load cell
3. Display bracket or stand
4. Electronics compartment

### Option C: Bare Assembly

For testing or temporary use:
- Mount load cell to rigid surface
- Place PN5180 under test platform
- Display propped up nearby

---

## Step 6: Final Assembly Checklist

### Mechanical

- [ ] Load cell securely mounted at fixed end
- [ ] Platform attached to load cell free end only
- [ ] Platform has clearance (doesn't touch base/sides)
- [ ] NFC antenna centered under spool position
- [ ] Display mounted stably

### Electrical

- [ ] All wiring complete per [[Wiring Guide]]
- [ ] No loose connections
- [ ] Wire routing away from moving parts
- [ ] Power cable accessible

### Verification

- [ ] Platform moves freely when pressed
- [ ] No binding or rubbing
- [ ] NFC antenna unobstructed

---

## Physical Assembly Notes

### NFC Antenna Positioning
- Position PN5180 antenna coil **under** the scale platform
- Center the antenna with the spool's core hole
- PN5180 has ~20cm read range (suitable for Bambu Lab tags inside spool core)
- Keep antenna flat and parallel to scale surface

### Scale Platform
- Load cell mounting: single-point (bar type)
- Ensure stable, level mounting surface
- Protect load cell from overload (add mechanical stops if needed)
- Shield from drafts for stable readings

---

## Testing Before First Use

### Mechanical Test
1. Place known weight on platform
2. Platform should depress slightly
3. Remove weight, platform returns

### Visual Inspection
1. No wires pinched
2. All connectors secure
3. Antenna visible from bottom (optional window)

### Power Test
1. Connect USB-C to CrowPanel
2. Display should illuminate
3. Touch should respond

---

## Next Steps

Once assembly is complete:

→ **[[Firmware Installation]]** - Flash the ESP32 firmware

→ **[[Software Setup]]** - Configure the backend server
