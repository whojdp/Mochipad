# Mochipad
Mochipad is a custom made Macropad. Being my first project, it had ot be something cool, so I made sure it had a rotary knob and a Oled screen.

I designed it using easy eda, and even though it is my first pcb ever, I wanted wireless support too so I went with a Seeed XIAO nRF52840, 9 mechanical keys, and a 3.7V LiPo battery.

## What's Inside

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
