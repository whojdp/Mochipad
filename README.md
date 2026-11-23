# Mochipad

Mochipad is a goated macropad made by a goated person, built from scratch in EasyEDA around the Seeed XIAO nRF52840.

## Basic Outline

Build arounf the SEEED XIAO, it has 9 tactile keys, a clickable roatary knob(tuff), and a screen on a custom 2 layed PCB. The top of the pcb is dedicated to inputs and the display, while the diodes and the XIAO is mounted on the back 

## Why I Built It

I really wanted to make my first project and felt that macropad is the perfect palce to start, it took me around 12-13 hours to complete this and was really fun to make, even though it was a bit complicated a lol here n there but I managed. I have no regrets starting this

On the practical side, I now have a dedicated desk controller for everyday shortcuts, layer switching, media playback, and volume control via the rotary knob, with the OLED showing active layers or status info.

## Hardware & Technical Specs

* **Microcontroller:** Seeed XIAO nRF52840 (`102010448` / LCSC `C17209540`), bottom-mounted with USB-C flush at the top edge
* **Key Matrix:** 3x3 direct-solder Cherry MX switches (`MX1A-11NW`, `SW1` to `SW9`) with `1N4148W` SOD-123 switching diodes (`C2099`, `D1` to `D9`) wired `col2row` for full N-key rollover
* **Rotary Encoder:** ALPS Alpine EC11 (`EC11E18244AU` / LCSC `C202365`) with integrated push-button switch
* **Display:** 0.91" I2C OLED module (`HS91L02W2C01` / LCSC `C5248081`)
* **PCB:** 2-layer FR-4, `66 x 90 mm` rounded portrait outline, 0 DRC errors
* **Power & Connectivity:** Powered over USB-C (BLE-capable MCU with a cleared bottom antenna keepout strip; dedicated battery pads were intentionally omitted in Rev 1 to keep the build clean and reliable)

### Pinout

| XIAO Pin | MCU Pin | Net | Function |
| :--- | :--- | :--- | :--- |
| `D0` | `P0.02` | `ROW0` | Matrix Row 0 (`SW1`, `SW4`, `SW7`) |
| `D1` | `P0.03` | `ROW1` | Matrix Row 1 (`SW2`, `SW5`, `SW8`) |
| `D2` | `P0.28` | `ROW2` | Matrix Row 2 (`SW3`, `SW6`, `SW9`) |
| `D3` | `P0.29` | `COL0` | Matrix Column 0 (`SW1` to `SW3`) |
| `D4` | `P0.04` | `SDA` | OLED I2C Data |
| `D5` | `P0.05` | `SCL` | OLED I2C Clock |
| `D6` | `P1.11` | `COL1` | Matrix Column 1 (`SW4` to `SW6`) |
| `D7` | `P1.12` | `COL2` | Matrix Column 2 (`SW7` to `SW9`) |
| `D8` | `P1.13` | `SCK` | Rotary Encoder Pin A |
| `D9` | `P1.14` | `MISO` | Rotary Encoder Push Switch (Pin E) |
| `D10` | `P1.15` | `MOSI` | Rotary Encoder Pin B |
| `3V3` / `GND` | Power | `3V3` / `GND` | OLED Power & Encoder Common (`C`, `D`) |

## PCB Design Highlights

* **Bottom-mounted XIAO:** Tucking `U1` underneath the right side of the OLED kept the top edge clean and cut down overall board height without overlapping the OLED header pins.
* **RF Keepout Clearance:** Tracks were manually routed outside the bottom strip of the XIAO (around the NFC/antenna area) to avoid detuning the 2.4 GHz ceramic antenna.
* **Compact Diode Placement:** All nine SMD `1N4148W` diodes sit on the bottom layer directly beneath each switch footprint to keep routing short and tidy.
