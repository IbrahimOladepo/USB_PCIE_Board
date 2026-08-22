# PCB Design & Review Checklist

Source: [Altium — PCB Design and Review Checklist](https://resources.altium.com/p/pcb-design-and-review-checklist)
(Mark Harris, updated 2026-08-10). Full article saved at
`Doc/PCB Design Review Checklist _ Blog _ Altium.PDF`.

> Per the article: treat each item as a **discussion point**, not just a box to tick —
> the goal is confirming the design meets intent, not just clearing a checklist. Not
> every item applies to every board; mark N/A with a short reason rather than deleting
> a line, so the reasoning is preserved for next time.

Check items off as `[x]` as we work through the design. Notes on findings go inline
under each item.

## Before Submitting for Review

- [ ] Check for unrouted nets
- [ ] Repour all polygons
- [ ] Complete silkscreen:
  - [ ] Company logo(s)
  - [ ] Product logo(s)
  - [ ] Copyright notice
  - [ ] Warning/hazard label(s) and icons
  - [ ] Connectors labeled, pinouts called out where relevant
  - [ ] QA/Test block with barcode, blank areas for serial number, dates, QA/Test checkmarks
  - [ ] Board name, print date, and revision number
  - [ ] If designators are on the silkscreen:
    - [ ] Each designator is close to and clearly identifies its component
    - [ ] All designators are only in one or two orientations
    - [ ] Text size/font will remain legible after fabrication
  - [ ] ICs have pin-1 clearly marked (not located under another component)
- [ ] PCB updated from schematic — schematic and board are synchronized
- [ ] Design rule report passes with no errors
  - [ ] A design rule exists to catch nets with only 1 pin
- [ ] Board outline present on a mechanical layer that goes to the fabricator
- [ ] Fiducials present for assembly:
  - [ ] Minimum three board-level fiducials included
  - [ ] Two fiducials diagonally opposing each other across all extremely fine-pitch components
- [ ] Mounting points have sufficient clearance for the chosen washer and screw head
- [ ] If an enclosure model is available, it's been tested against the board (no interference)
  - [ ] All components (including mechanical items) have accurate 3D models

## Layers

- [ ] Layer stack and substrate heights meet the fabricator's specifications
- [ ] Copper thickness on all layers matches the fabricator's spec (or callout exists on the documentation layer)
- [ ] At least one continuous, unbroken ground plane
- [ ] Controlled-impedance nets correctly set up in both the layer stack and design rules
- [ ] Any keep-out track matches the board shape
  - [ ] Board cutouts/slots have keep-out barriers to prevent nets crossing milled areas

## Signal Path

- [ ] Ground plane has sufficient current-carrying vias near connectors and voltage/return sinks
- [ ] Voltage planes/areas have sufficient connection vias for current requirements (if relevant)
- [ ] Tracks to reference planes are sufficiently wide for current requirements
- [ ] Sufficient quantity of vias for the current-carrying capacity of traces
- [ ] Minimum track widths sufficient for all current-carrying nets
- [ ] All ground pins have a via to the ground plane
- [ ] Continuous ground plane within one signal layer of any signal trace
- [ ] Controlled-impedance traces have correct net rules and impedance profile
- [ ] Differential pair tracks are as close together as possible
- [ ] Differential pair track lengths are matched
- [ ] High-speed signals are length-matched, specifically: DDR / **PCIe** / Ethernet / LVDS / HDMI / **USB3+** / MIPI
- [ ] Every signal trace has constant impedance along its length (including across layer changes)
- [ ] Traces longer than 1/6th of the rise/fall time have been simulated:
  - [ ] Termination resistors (or other termination) present to prevent ringing/overshoot
  - [ ] Termination resistors are in the relevant locations
- [ ] Long traces that hug other traces have been simulated for crosstalk
- [ ] All high-speed traces run over a continuous ground pour
- [ ] No sensitive nets run under noisy components
- [ ] Decoupling capacitor vias are not shared — each decoupling cap has its own via for VCC and for GND, direct to the reference planes

## Components

- [ ] All through-hole pads set to plated (if soldered)
- [ ] Sufficient clearance for:
  - [ ] Pick-and-place heads in production
  - [ ] Hand assembly in prototypes
  - [ ] Soldering pen tip access for rework
- [ ] Bypass capacitors placed as close to IC power pins as possible (under 15mm)
- [ ] Crystal/oscillator clock sources as close to the IC clock pins as possible
- [ ] Termination resistors as close to the signal source as possible
- [ ] EMI/RFI filtering as close to the exit point (board edge, connector, shield) as possible
- [ ] Potentiometers increase signal/voltage when turned clockwise (n/a if none on this board)
- [ ] Programmable devices have accessible programming header/pads
- [ ] No high-thermal-mass components (large transformers/inductors) located next to very small components
- [ ] Component placement prioritizes short track lengths for high-speed signals
- [ ] Sufficiently large copper area for heat-sinking high-dissipation devices, including: linear regulators, switched-mode power supplies (incl. LED drivers), high-power LEDs, high-frequency gate drivers, MOSFETs, motor drivers, chargers, high-speed microprocessors, power amplifiers

## Testing

- [ ] Test pads sufficiently far from the board edge to allow fixturing
- [ ] Test pads don't create stubs/impedance mismatch on high-speed nets
- [ ] Components don't obstruct access to test pads (manual probe or bed-of-nails)
- [ ] Test pads are clearly labeled for prototypes
- [ ] Any signal needed for testing or inspection has a test point
- [ ] Test points are all on the same side of the board — bottom for bed-of-nails fixtures, top for manual probing

## Protection / EMI / EMC

- [ ] Appropriate creepage and clearance rules set for all high-voltage nets
- [ ] Separate earth tracks/paths where necessary for ESD
- [ ] Decoupling capacitors next to connectors and vias that may require them
- [ ] TVS diodes/other ESD mitigation component pads are in series with the track (any ESD event must pass through the component pad before reaching a sensitive device)
- [ ] No track stubs/net antennas going to test points or unused connector pins
- [ ] High-speed signals routed as directly as possible — no scenic routes
- [ ] Any track carrying over 100mA has its width calculated for sufficient current
  - [ ] If enclosed with little/no airflow, width calculated/simulated for an internal layer instead of external
- [ ] RF shield footprint present, if required
- [ ] If a 2-layer board:
  - [ ] No ground loops
  - [ ] Ground track sized sufficiently for the return current of every device
  - [ ] Unbroken ground pour under every high-speed trace
- [ ] If multiple grounds: tied together at a single point only

## Panels

*Only applies if this board is being ordered/assembled as part of a multi-board panel — otherwise mark N/A.*

- [ ] Sufficient frame area for conveyors and clamping
- [ ] Silkscreen includes: blanks for QA/testing marks, print date, machine name, company name, panel barcode, board part number and revision
- [ ] Panel fiducials present
- [ ] Origin identification mark present
- [ ] Impedance/layer/other test areas present, if required
- [ ] V-score/milling/tab layers present and aligned with the board (if not already in the board file)
- [ ] Panel isn't too large for the board thickness and any milling (low flexibility/bounce)
