# Mochipad
Mochipad is a custom made Macropad. Being my first project, it had ot be something cool, so I made sure it had a rotary knob and a Oled screen.

I designed it using easy eda, and even though it is my first pcb ever, I wanted wireless support too so I went with a Seeed XIAO nRF52840, 9 mechanical keys, and a 3.7V LiPo battery.

## What's Inside# Mochipad

A compact, custom 3×3 mechanical macropad with a rotary encoder and a 0.91" OLED display, built from scratch in EasyEDA around the Seeed XIAO nRF52840.

![Mochipad 3D Front View](screenshots/pcb-3d-front.png)

## What It Is

Mochipad is a `66 × 90 mm` portrait-oriented desk macropad combining nine tactile mechanical keys, a clickable rotary knob, and an onboard status screen on a single custom 2-layer PCB. To keep the front surface clean and compact, the Seeed XIAO nRF52840 and all nine matrix diodes are mounted on the bottom layer, leaving the top dedicated purely to inputs and the display.

## Why I Built It

I wanted to move beyond software-only projects and learn real hardware design end to end—going from a blank schematic sheet to a manufacturable PCB. Building a macropad from scratch forced me to learn the fundamentals hands-on:

* Designing an anti-ghosting diode switch matrix (`col2row`)
* Choosing real LCSC components, verifying footprints, and fixing pinout mistakes
* Managing 2-layer routing, drill clearances, RF/antenna keepout zones, and DRC debugging
* Iterating through multiple layouts (from a wide `97 × 65 mm` draft to the final compact `66 × 90 mm` vertical board)

On the practical side, it gives me a dedicated desk controller for everyday shortcuts, layer switching, media playback, and volume control via the rotary knob, with the OLED showing active layers or status info.

## Hardware & Technical Specs

* **Microcontroller:** Seeed XIAO nRF52840 (`102010448` / LCSC `C17209540`) — bottom-mounted with USB-C flush at the top edge
* **Key Matrix:** 3×3 direct-solder Cherry MX switches (`MX1A-11NW`, `SW1`–`SW9`) with `1N4148W` SOD-123 switching diodes (`C2099`, `D1`–`D9`) wired `col2row` for full N-key rollover
* **Rotary Encoder:** ALPS Alpine EC11 (`EC11E18244AU` / LCSC `C202365`) with integrated push-button switch
* **Display:** 0.91" I2C OLED module (`HS91L02W2C01` / LCSC `C5248081`)
* **PCB:** 2-layer FR-4, `66 × 90 mm` rounded portrait outline, 0 DRC errors
* **Power & Connectivity:** Powered over USB-C (BLE-capable MCU with a cleared bottom antenna keepout strip; dedicated battery pads were intentionally omitted in Rev 1 to keep the build clean and reliable)

### Pinout

| XIAO Pin | MCU Pin | Net | Function |
| :--- | :--- | :--- | :--- |
| `D0` | `P0.02` | `ROW0` | Matrix Row 0 (`SW1`, `SW4`, `SW7`) |
| `D1` | `P0.03` | `ROW1` | Matrix Row 1 (`SW2`, `SW5`, `SW8`) |
| `D2` | `P0.28` | `ROW2` | Matrix Row 2 (`SW3`, `SW6`, `SW9`) |
| `D3` | `P0.29` | `COL0` | Matrix Column 0 (`SW1`–`SW3`) |
| `D4` | `P0.04` | `SDA` | OLED I2C Data |
| `D5` | `P0.05` | `SCL` | OLED I2C Clock |
| `D6` | `P1.11` | `COL1` | Matrix Column 1 (`SW4`–`SW6`) |
| `D7` | `P1.12` | `COL2` | Matrix Column 2 (`SW7`–`SW9`) |
| `D8` | `P1.13` | `SCK` | Rotary Encoder Pin A |
| `D9` | `P1.14` | `MISO` | Rotary Encoder Push Switch (Pin E) |
| `D10` | `P1.15` | `MOSI` | Rotary Encoder Pin B |
| `3V3` / `GND` | Power | `3V3` / `GND` | OLED Power & Encoder Common (`C`, `D`) |

## PCB Design Highlights

* **Bottom-mounted XIAO:** Tucking `U1` underneath the right side of the OLED kept the top edge clean and cut down overall board height without overlapping the OLED header pins.
* **RF Keepout Clearance:** Tracks were manually routed outside the bottom strip of the XIAO (around the NFC/antenna area) to avoid detuning the 2.4 GHz ceramic antenna.
* **Compact Diode Placement:** All nine SMD `1N4148W` diodes sit on the bottom layer directly beneath each switch footprint to keep routing short and tidy.

![Schematic](screenshots/schematic.png)
![PCB 2D Layout](screenshots/pcb-2d.png)
![PCB 3D Back View](screenshots/pcb-3d-back.png)

## Repository Structure

```text
mochipad/
├── bom/            # Bill of Materials (.csv)
├── hardware/       # Gerber fabrication archive (.zip) & EasyEDA source files (.json)
├── screenshots/    # Schematic, 2D PCB, and 3D render captures
└── README.md

| Part | What it is | Details / Footprint |
| :--- | :--- | :--- |
| Microcontroller | Seeed XIAO nRF52840 (`U1`) | 2.4 GHz Bluetooth Low Energy, USB-C |
| Switches | 9x MX hot-swap sockets (`SW1` to `SW9`) | `CPG151101S11-16` (`C41430893`) in a 3x3 grid |
| Diodes | 9x switching diodes (`D1` to `D9`) | `1N4148W` (SOD-123, `C2099`) |
| Rotary Knob | EC11 rotary encoder (`SW10`) | `EC11E18244AU` (`C202365`) with push button |
| Display | 0.91" I2C OLED screen (`OLED1`) | `HS91L02W2C01` (`C5248081`) |
| Battery Pads | `TP1` and `TP2` | `BAT+` and `GND` / `BAT-` for a 3.7V LiPo |

## How It's Wired Up

| Signal / Group | Pins / Connections | Notes |
| :--- | :--- | :--- |
| Matrix Rows | `ROW0` to `ROW2` on XIAO `D0`, `D1`, `D2` | Goes to diode cathodes (`Pin 1`), switch `Pin 2` to anode |
| Matrix Columns | `COL0` to `COL2` on XIAO `D3`, `D6`, `D7` | 3x3 `col2row` switch matrix |
| OLED Screen | `SDA` to `D4`, `SCL` to `D5`, `3V3`, `GND` | Standard I2C wiring |
| Rotary Encoder | `A` to `D8` (`SCK`), `B` to `D10` (`MOSI`), `E` to `D9` (`MISO`), `C` & `D` to `GND` | Quadrature rotation + push click |
| Battery & Unused | `TP1` to `BAT+`, `TP2` to `GND`, unused XIAO pins marked `X` | No-Connect flags on unused pins |

## How It Went So Far

* **Schematic & ERC:** Wired up the 3x3 matrix, OLED, encoder, and battery pads in EasyEDA and checked the schematic before moving to the PCB.
* **PCB Layout & Routing:** Set up a 2 layer board in `mm` (`97.0 mm x 65.0 mm` starting outline), moved everything inside the border, arranged the 3x3 switches, ran the autorouter, fixed a `D5` to `D6` trace snag plus the last `TP1` and `SW10` airwires, and hit `Nets (23 / 23)` with `0` DRC errors.
* **Nudging & Review:** Moving a couple of things around brought back 7 DRC errors, so I am leaving that checkpoint for review before touching everything again.

## Stuff Still Left to Sort Out

| Topic | What's Next |
| :--- | :--- |
| DRC & Footprints | Sort out the 7 DRC errors, double check Cherry MX holes (`Ø 4.00 mm` center + `2x Ø 1.70 mm` 5 pin holes), and check `TP1`/`TP2` battery pad sizing and underside `BAT+` access |
| Case & Bluetooth | Full CNC aluminum can act like a Faraday cage for 2.4 GHz Bluetooth and short out if uninsulated, so I am leaning toward a 3D printable case later |
| Switches & BOM | Pick the exact MX switches, keycaps, knob, and 3.7V LiPo cell, and put together the full BOM |
| Firmware | Set up ZMK firmware for the 3x3 matrix, OLED screen, and EC11 encoder once the board is ready |
