# USB PCIe Expansion Card

A PCI Express x1 to USB 3.0 expansion card, designed in KiCad. Plugs into a PCIe x1
slot and breaks it out to four USB 3.0 Type-A ports.

## Overview

The design is built around **U1**, a Renesas `µPD720201` xHCI USB 3.0 host
controller, which bridges the PCIe link to four independent USB 3.0 ports.

| Function | Reference(s) | Part |
|---|---|---|
| PCIe x1 edge connector | J1 | `Bus_PCI_Express_x1` |
| USB host controller | U1 | Renesas `UPD720201K8-711-BAC-A` |
| Controller firmware/config flash | U2 | `MX25L4006EM1I-12G` (SPI NOR) |
| Controller clock | X1 | 24MHz crystal |
| USB 3.0 Type-A ports | J2–J5 | Molex `48406-0001` |
| Per-port VBUS power switches | U8–U11 | `AP2191SG-13` |
| Per-port ESD protection | U12–U15 | `SP3011-06UTG` |
| Power regulation | U3, U4, U5, U7 | Buck regulators + 3.3V LDO |

Power for the board and its 5V VBUS rails is derived from the PCIe slot's 12V/3.3V
supply through an on-board regulation chain.

## Board

- 4-layer, ~98.6 x 111.9mm
- ~175 components, ~126 nets

## Repository layout

- `USB_PCIE_Board.kicad_pro` / `.kicad_pcb` / `.kicad_sch` — main project, board, and schematic
- `USB_PCIE_BOARD_POWER.kicad_sch` — power supply schematic sheet
- `Doc/` — documentation, including a PCB design review checklist
- `production/` — generated fabrication outputs (gerbers, BOM, position files)

## Status

Actively being worked through a design review pass (DRC cleanup, courtyard/footprint
fixes, silkscreen check) — see `Doc/PCB_Design_Review_Checklist.md` for the current
punch list.
