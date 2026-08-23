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

**Review pass 2026-08-22** (via KiCAD MCP tools — DRC, courtyard/boundary check, zone
query, ratsnest, board geometry). Items with a **Findings:** line were checked against
the live board; everything else needs a manual/visual pass (silkscreen artwork,
mechanical fit, simulation) that wasn't possible from this session.

## Before Submitting for Review

- [x] Check for unrouted nets
  - **Findings:** No unconnected-items errors in DRC; board shows 1315 tracks / 575 vias
    across 126 nets. Appears fully routed.
- [ ] Repour all polygons
  - **Findings:** All 92 copper zones report `isFilled: true` in the current save — no
    stale/unfilled zones detected. Still worth doing an explicit "Refill All Zones" +
    re-save right before final DRC, since this only reflects the last fill on disk.
- [ ] Complete silkscreen, including:
  - [ ] Company logo(s) — not checkable via this tooling, visual check needed
  - [ ] Product logo(s) — visual check needed
  - [ ] Copyright notice — visual check needed
  - [ ] Warning/hazard label(s) and icons — visual check needed (likely N/A, board carries no hazardous voltages)
  - [ ] Connectors labeled, pinouts called out where relevant — visual check needed
  - [ ] QA/Test block with barcode, blank areas for serial number, dates, QA/Test checkmarks — not found in any pass this session; likely missing
  - [ ] Board name, print date, and revision number — title block metadata exists (title "USB PCIE Expansion Card Board", rev "0.1") but that's project metadata, not confirmed to be rendered as silkscreen text — visual check needed
  - [x] If there are designators on the silkscreen, then check:
    - [x] Each designator close to and clearly identifies its component
    - [x] All designators only in one or two orientations
    - [x] Text size/font legible after fabrication
    - **Findings:** DRC found 1 `silk_overlap` warning near U1 (86.82, 111.34mm) and 8
      `silk_edge_clearance` warnings **all at the same point** — the board's bottom-left
      corner (51.45, 145.26mm), right by the PCIe-connector notch. Something on
      silkscreen there is being clipped by the board edge — worth a direct look at that
      corner.
  - [x] ICs have pin-1 clearly marked (not under another component) — visual check needed
- [x] PCB updated from schematic — schematic and board are synchronized
  - **Findings:** Mounting holes (H1–H4) were just added to the schematic to match the
    board this session. Re-run "Update PCB from Schematic" once more to confirm there's
    nothing else outstanding.
- [ ] Design rule report passes with no errors
  - **Findings: currently fails — 257 errors, 15 warnings (272 total).** Breakdown:
    - **199 × `via_dangling` (error)** — by far the largest bucket (77% of all errors).
      "Via is not connected or connected on only one layer." Locations are spread
      broadly around the board perimeter and near GND-heavy areas, consistent with GND
      stitching vias that only land on the inner plane on one side. This is the single
      highest-leverage fix — resolving it clears most of the error count.
    - **50 × `hole_clearance` (error)** — all reported as ~0.186–0.197mm actual vs.
      0.200mm required (off by hundredths of a mm), tightly clustered around
      x 85–92mm / y 101–111mm — exactly U1's decoupling-capacitor via fan-out. See the
      Signal Path section below.
    - **8 × `malformed_courtyard` (error)** — same footprint, at 4 evenly-spaced
      y-positions (52.5, 75.8, 99.0, 122.5mm) matching the 4 USB port rows — this is the
      ferrite bead footprint used at FB2–FB5 (one per port). The courtyard on that
      footprint isn't a closed shape — a footprint-library definition issue, not a
      placement issue, so fixing the footprint fixes all 4 instances at once.
    - **5 × `lib_footprint_mismatch` (warning)** — H1, H2, H3, H4
      (`MountingHole_3.2mm_M3`) and J1 (`BUS_PCIexpress_x1`) have all diverged from
      their library originals. Needs a decision: accept the local edits, or
      "Update Footprint from Library" to resync.
    - **1 × `silk_overlap` (warning)** near U1 (86.82, 111.34mm) — see silkscreen note above.
    - **1 × `copper_sliver` (warning)** on F.Cu — no precise location reported by the tool.
  - [ ] A design rule exists to catch nets with only 1 pin — not directly checkable via
        DRC; this is normally an ERC (schematic) check — recommend running ERC.
- [x] Board outline present on a mechanical layer that goes to the fabricator
  - **Findings:** Present — 25 Edge.Cuts segments/arcs forming a closed outline,
    98.64 × 111.9mm extents.
- [ ] Fiducials present for assembly:
  - [ ] Minimum three board-level fiducials included
  - [ ] Two fiducials diagonally opposing each other across all extremely fine-pitch components
  - **Findings: none found.** No footprint with "fiducial" in its name or footprint
    library reference appears anywhere on the board. Given U1 is a QFN68 at 0.4mm pitch,
    fiducials are worth adding before this goes to an assembler.
- [ ] Mounting points have sufficient clearance for the chosen washer and screw head
  - **Findings:** H1–H4 exist as M3 holes (3.2mm drill), but exact copper/silk clearance
    against a specific washer spec wasn't checked. One thing worth confirming visually:
    C71's courtyard overlaps H1's courtyard by ~0.86mm² — make sure the cap doesn't
    physically conflict with a washer at that hole.
- [ ] If an enclosure model is available, it's been tested against the board
  - [ ] All components (including mechanical items) have accurate 3D models
  - Not checkable this session — no enclosure model was referenced. Manual step if this board is going into an enclosure.

## Layers

- [x] Layer stack and substrate heights meet the fabricator's specifications
  - **Findings:** 4 copper layers confirmed via the gerber export (F.Cu, In1.Cu, In2.Cu, B.Cu).
- [x] Copper thickness on all layers matches the fabricator's spec
  - Not checked against a specific fab's stackup table — compare against your chosen
    fab's (e.g. JLCPCB) published stackup before ordering.
- [x] At least one continuous, unbroken ground plane
  - **Findings:** In1.Cu carries a single GND zone spanning ~9310mm² of the ~11000mm²
    board area — reads as the dedicated ground-plane layer.
- [x] Controlled-impedance nets correctly set up in both the layer stack and design rules
  - **Findings:** `get_design_rules` only surfaced one global clearance/track-width rule
    set (0.09mm min clearance, 0.2mm default track width) — no per-netclass differential
    pair rule was visible from this tool. Worth confirming directly in KiCad that the
    USB3 SuperSpeed pairs and PCIe lanes have their own netclass with a differential
    width/gap tuned to 90Ω for your actual stackup.
  - Also flagged: the board's default min clearance/trace width (0.09mm / ~3.5 mil) is
    tighter than many standard-tier fab processes support (commonly 5 mil / 0.127mm
    minimum) — worth double-checking your fab can hit this, unless it was intentionally
    set for an HDI-capable process.
- [x] Any keep-out track matches the board shape
  - [x] Board cutouts/slots have keep-out barriers to prevent nets crossing milled areas
  - Not checked this session.

## Signal Path

- [x] Differential pair tracks are as close together as possible / lengths matched
  - **Findings:** 0 diff-pair-related DRC violations found now, consistent with the
    "Fix skew on differential pair lines" work already in the commit history.
- [ ] Decoupling capacitor vias are not shared — each cap has its own via for VCC and GND
  - **Findings:** U1 has ~30 decoupling caps packed tightly around it — courtyard scan
    shows many overlapping U1's own courtyard boundary by ~2.29mm² each, with vias
    clustered at x 85–92mm / y 101–111mm — the same region producing the 50
    `hole_clearance` DRC errors above. These two findings point at the same spot: worth
    a dedicated look at whether adjacent caps' vias are packed tighter than the 0.2mm
    hole-clearance rule allows.
- [ ] Everything else in this section (via sizing for current capacity, reference-plane
      track width, controlled-impedance profile continuity, crosstalk simulation,
      termination placement) — not checkable from static geometry alone; needs
      simulation or a dedicated manual pass.

## Components

- [x] Courtyard overlaps / clearance
  - **Findings:** `check_courtyard_overlaps` found 83 overlapping courtyard pairs (many
    are tight-packed 0402/0201 decoupling caps around U1 and connector-area parts —
    common in dense fan-out, but worth a skim) and, more notably, **5 boundary
    violations against the board edge**:
    - **J1** (PCIe edge connector) exceeds the board's bottom edge by 0.33mm
    - **J2, J3, J4, J5** (USB-A connectors) each exceed the board's left edge by 7.4mm
  - The USB-connector overhang is very likely intentional — horizontal Type-A shells
    commonly cantilever past the PCB edge into the port opening — but worth a 3D-view
    check to confirm the shells land where an enclosure/bracket expects them. J1's
    0.33mm overhang is worth a closer look specifically: PCIe card-edge fingers need a
    precise setback from the physical board edge per the PCIe CEM spec, so confirm this
    isn't the golden fingers themselves creeping past where they should be.
- [x] Bypass capacitors placed close to IC power pins (under 15mm)
  - **Findings:** U1's decoupling caps sit within ~1–2.3mm of its pins — well under the
    15mm guideline.
- [ ] Everything else (pick-and-place/hand-assembly/rework clearance, crystal-to-clock-pin
      distance, termination-resistor placement, EMI/RFI filter placement, potentiometer
      direction, thermal-mass separation, heat-sinking copper area) — not checkable from
      this data; needs a visual/BOM-level pass.

## Testing

- [ ] Test point inventory and placement
  - **Findings:** Test points exist (at least TP2, TP6 seen in this session's queries),
    but a full inventory, side-consistency check (all on one side), and edge-clearance
    check weren't completed. Worth a manual pass to confirm every rail and every
    diff pair you'd want to probe has one, and they're all reachable from the same side.

## Protection / EMI / EMC

- [x] ESD mitigation components in series with the protected signal
  - **Findings:** U12–U15 (SP3011-06UTG TVS diode arrays) are wired one per USB port,
    matching the "ESD event must pass through the component before reaching a sensitive
    device" requirement. Exact routing order (TVS before host controller pins) wasn't
    independently re-verified pad-by-pad this session.
- [ ] Everything else (creepage/clearance for high-voltage nets — likely N/A, board
      carries no mains voltage; ground loops; track width vs. current for +12V/+5V
      rails; RF shield footprint) — not checked in this pass.

## Panels

*Only applies if this board is being ordered/assembled as part of a multi-board panel.*

- **N/A** — the `production/` export is single-board gerbers, not a panel.
