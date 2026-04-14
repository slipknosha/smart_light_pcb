# Smart Light PCB

ESP32-S3-based AC dimmer PCB for controlling a 220 V AC load with phase-angle dimming.

> [!WARNING]
> This project works with **mains voltage (220–230 VAC)**.
> Assembly, testing, and operation require proper isolation, safety precautions, and experience with high-voltage electronics.
> This board is suitable for education, prototyping, and lab demonstrations — not as a finished certified consumer product.

## Overview

This repository contains the **KiCad project** for a compact smart AC dimmer board built around the **ESP32-S3-WROOM-1**.

The board combines:
- **ESP32-S3** as the main controller
- **native USB-C** for power/programming
- **isolated AC/DC power supply** based on Mean Well **IRM-05-5**
- **zero-cross detection** using **LTV-814**
- **triac gate driver** using **MOC3052M**
- **power triac** **BTB16-600B** for AC load control

The design is intended for learning, prototyping, and coursework related to:
- embedded systems
- AC power control
- opto-isolated mains interfaces
- smart lighting / dimmer systems

## Main Features

- ESP32-S3-WROOM-1 module
- Native USB-C connection
- 3.3 V logic rail generated from onboard 5 V supply
- Isolated 5 V AC/DC converter
- Zero-cross detection for mains synchronization
- Phase-angle dimming architecture
- Push buttons for **BOOT** and **RESET**
- 2x3 programming/debug header
- Generated production outputs:
  - Gerbers
  - drill files
  - BOM
  - pick-and-place / component position file
  - IPC netlist

## Hardware Architecture

### 1. Power stage
The board is powered either from:
- **USB-C 5 V**, or
- onboard isolated **IRM-05-5** AC/DC converter

The 5 V rail is converted to **3.3 V** by **LM1117-3.3** for the ESP32-S3 and low-voltage logic.

### 2. Control stage
The main controller is **ESP32-S3-WROOM-1**.
It handles:
- zero-cross interrupt processing
- dimming timing logic
- user/application logic
- future networking features via Wi‑Fi / Bluetooth

### 3. Zero-cross detection
An **LTV-814** optocoupler is used to detect mains zero crossing and provide galvanic isolation between the mains domain and the ESP32 domain.

### 4. Triac drive
A **MOC3052M** optotriac driver controls the gate of the power triac.
The power switching element is **BTB16-600B**, which drives the AC load.

## Key Components

| Reference | Part | Function |
|---|---|---|
| U2 | ESP32-S3-WROOM-1 | Main MCU / wireless controller |
| PS1 | IRM-05-5 | Isolated AC/DC 5 V power module |
| U6 | LM1117-3.3 | 3.3 V regulator |
| U1 | LTV-814 | Zero-cross optocoupler |
| U3 | MOC3052M | Optotriac gate driver |
| Q1 | BTB16-600B | Main AC triac |
| J3 | USB-C | Power / programming |
| SW1 | RESET | Reset button |
| SW2 | BOOT | Bootloader button |
| U5 | USBLC6-2SC6 | USB ESD protection |

## USB Programming

The ESP32-S3 is connected for **native USB flashing**.
The board includes:
- USB-C connector
- CC resistors for USB device mode
- ESD protection on USB lines
- BOOT and RESET buttons

That means the board can be programmed directly from USB without an external USB-UART converter.
A fallback debug/programming header is also available on **J4**.

## Repository Structure

```text
smart_light_pcb/
├── LICENSE
├── README.md
├── sn.kicad_sch                # main schematic
├── sn.kicad_pcb                # PCB layout
├── sn.kicad_pro                # KiCad project file
├── sn.step                     # exported 3D model
├── production/
│   ├── bom.csv                 # bill of materials
│   ├── designators.csv         # designator list
│   ├── netlist.ipc             # IPC netlist
│   ├── positions.csv           # pick-and-place positions
│   └── sn.zip                  # Gerber + drill production package
├── jlcpcb/
│   └── project.db              # JLCPCB plugin database
├── Snapeda.kicad_sym           # custom symbol library
├── Snapeda.pretty/             # custom footprints
├── Snapeda.3dshapes/           # custom 3D models
└── sn-backups/                 # project backups
```

## Manufacturing Notes

The repository already contains the files usually needed for PCB fabrication and assembly:

- **Gerber archive**: `production/sn.zip`
- **BOM**: `production/bom.csv`
- **Pick-and-place file**: `production/positions.csv`
- **IPC netlist**: `production/netlist.ipc`

This makes the project ready for:
- PCB fabrication
- partial SMT assembly
- manual final assembly of through-hole and mains components

## Assembly Notes

A mix of **SMD** and **through-hole** parts is used.
Typical parts likely to require manual assembly or extra care:
- IRM-05-5 AC/DC module
- terminal blocks
- triac package / thermal handling
- fuse
- mains-side resistors / high-voltage components

## Design Notes

This board was designed as a practical embedded AC dimmer project and is especially suitable for:
- student work
- coursework / diploma demonstrations
- proof-of-concept smart lighting projects
- embedded + power electronics learning

It is a good prototype architecture, but it should still be treated as a **prototype**, especially because it operates directly from the AC mains.

## Safety

> [!CAUTION]
> The board contains both **low-voltage logic** and **mains-voltage AC sections**.
> Do not power, probe, or modify the board unless you understand the risks of working with 220–230 VAC.
>
> Recommended precautions:
> - use an isolation strategy during bring-up
> - test first with a simple resistive load
> - do not touch the board while energized
> - keep proper enclosure and mains insulation in mind

## Suggested Bring-Up Procedure

1. Inspect PCB for shorts and wrong polarity.
2. Power the board from **USB only** and verify 3.3 V.
3. Check ESP32-S3 boot/reset behavior.
4. Flash a simple test firmware over USB.
5. Verify zero-cross input logic.
6. Test AC stage with a safe resistive load.
7. Only then move to real dimming tests.

## License

This project is licensed under the **MIT License**.
See the [LICENSE](LICENSE) file for details.

## Author

**Vladyslav Bobrykov**

---

If this project helped you, feel free to fork it, improve it, and build your own revision.
