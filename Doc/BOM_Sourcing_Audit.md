# BOM Sourcing Audit (JLCPCB/LCSC)

Checked 2026-08-23 against a local snapshot of the JLCPCB parts catalog (633,703
in-stock parts, downloaded via the KiCad MCP JLCPCB integration). Source BOM:
`production/USB_PCIE_Board.csv`.

## Headline finding

Every part present in the snapshot has a first price-break tier of **"1+"** — none
carry a JLCPCB SMT-assembly MOQ above 1 unit. The high-MOQ warning seen on LCSC's site
is most likely their separate loose-part/reel retail MOQ, not the JLCPCB assembly MOQ
this database tracks. Worth confirming which flow (assembly vs. loose-part purchase)
produced that warning before resourcing anything below.

Real issues found instead: one part with insufficient stock for the quantity needed,
three parts not present in the local snapshot, and near-universal Extended library
type (a cost issue, not a MOQ/stock issue).

## Full part table

| Ref(s) | Qty | Original LCSC# | MOQ (1st tier) | Stock | Library | Status | Suggested Alternative |
|---|---|---|---|---|---|---|---|
| C1-C17 | 17 | C60133 | 1 | 3,311 | Extended | OK | — |
| C18-C27,C29,C37-44 | 19 | C60474 | 1 | 10,000 | Extended | OK | — |
| C28,C62,C63,C66,C69,C74,C75 | 7 | C29936 | 1 | 178,930 | Extended | OK | — |
| C30,C31 | 2 | C2191663 | 1 | 29,868 | Extended | OK | — |
| C32,C34,C64,C67,C70,C71 | 6 | C326595 | 1 | 3,754 | Extended | OK | — |
| C33,C36,C50,C57,C59 | 5 | C14663 | 1 | 12,618,106 | Basic | OK | — |
| C35,C51,C56 | 3 | C162437 | 1 | 4,162 | Extended | OK | — |
| C45,C46 | 2 | C342646 | 1 | 1,810 | Extended | OK | — |
| C47 | 1 | C128458 | 1 | 3,852 | Extended | OK | — |
| C48 | 1 | C494858 | 1 | 126 | Extended | OK (thin stock) | — |
| C49 | 1 | C106239 | 1 | 51,862 | Extended | OK | — |
| C52 | 1 | C107024 | 1 | 123,326 | Extended | OK | — |
| C53 | 1 | C60137 | 1 | 2,128,208 | Extended | OK | — |
| C54,C55 | 2 | C23742 | 1 | 4,743 | Extended | OK | — |
| C58,C60,C61 | 3 | C77042 | 1 | 421,317 | Extended | OK | — |
| C65,C68,C72,C73 | 4 | C190722 | 1 | 725 | Extended | OK | — |
| D1 | 1 | C261914 | 1 | 26,448 | Extended | OK | — |
| D2 | 1 | C85100 | 1 | 212,196 | Extended | OK | — |
| D3 | 1 | C253526 | 1 | 3,022 | Extended | OK | — |
| D4,D5 | 2 | C2845139 | 1 | 3,031 | Extended | OK | — |
| D6 | 1 | C110484 | 1 | 6,374 | Extended | OK | — |
| D7-D10 | 4 | C2992528 | 1 | 2,680 | Extended | OK | — |
| D11-D13 | 3 | C2837824 | 1 | 28,334 | Extended | OK | — |
| D14,D15 | 2 | C5588998 | 1 | 3,060 | Extended | OK | — |
| F1 | 1 | C1972746 | 1 | 53 | Extended | OK (thin stock) | — |
| FB1-FB5 | 5 | C18305 | 1 | 132,019 | Extended | OK | — |
| J2-J5 | 4 | C6376748 | 1 | 123 | Extended | OK (thin stock) | none — connector, no safe swap |
| L1 | 1 | C2045037 | 1 | 100 | Extended | OK | — |
| L2 | 1 | C87572 | 1 | 21,781 | Extended | OK | — |
| L3 | 1 | C909806 | 1 | 750 | Extended | OK | — |
| Q1 | 1 | C111698 | 1 | 10,686 | Extended | OK | — |
| Q2 | 1 | C111700 | 1 | 20,055 | Extended | OK | — |
| Q4 | 1 | C151586 | 1 | 2,554 | Extended | OK | none — specific MOSFET, no safe swap |
| Q5 | 1 | C142600 | 1 | 12,902 | Extended | OK | none — specific MOSFET, no safe swap |
| R1,R12-R15 | 5 | C95177 | 1 | 477,935 | Extended | OK | — |
| R2,R3,R5,R6,R7,R16,R23,R27,R28,R29,R30,R40 | 12 | C159891 | — | — | — | Not found in DB | C1511834 CRCW060310K0JNEBC — 10K 0603 5%, MOQ 1, 9,377 stock, Extended (best same-package match; no Basic 0603 10K in this snapshot) |
| R4,R24 | 2 | C14675 | 1 | 5,999 | Extended | OK | — |
| R8 | 1 | C413085 | 1 | 967 | Extended | OK | — |
| R9 | 1 | C413078 | 1 | 15,078 | Extended | OK | — |
| R10,R11,R17,R26,R31 | 5 | *(none given)* | — | — | — | No LCSC# given | C408932 CR-06JA7----0R — 0Ω 1206 jumper, MOQ 1, 5,051 stock, Extended (no Basic 0Ω 1206 found) |
| R18 | 1 | C127447 | 1 | 91,974 | Extended | OK | — |
| R19 | 1 | C1883521 | 1 | 303 | Extended | OK | — |
| R20 | 1 | C481960 | — | — | — | Not found in DB | C2933077 FRC0402F1742TS — 17.4K 0402 1%, MOQ 1, 129,307 stock, Extended |
| R21 | 1 | C2076772 | 1 | 35,568 | Extended | OK | — |
| R22 | 1 | C844922 | 1 | 19,789 | Extended | OK | — |
| R25 | 1 | C2076703 | 1 | 1,801 | Extended | OK | — |
| R32-R39 | 8 | C22548 | 1 | 5 | Extended | **Stock too low (5 in stock, 8 needed)** | C21190 0603WAF1001T5E — 1K 0603 1%, MOQ 1, 8,013,731 stock, Basic |
| U1 | 1 | C355375 | 1 | 1,089 | Extended | OK | none — host controller, no safe swap |
| U2 | 1 | C56001 | 1 | 34 | Extended | OK (thin stock) | none — specific flash, no safe swap |
| U3 | 1 | C527397 | 1 | 1,180 | Extended | OK | — |
| U4 | 1 | C459876 | 1 | 40 | Extended | OK (thin stock) | none — specific controller, no safe swap |
| U5 | 1 | C5896722 | 1 | 594 | Extended | OK | — |
| U6 | 1 | C706976 | 1 | 8,341 | Extended | OK | — |
| U7 | 1 | C460367 | 1 | 1,655 | Extended | OK | — |
| U8-U11 | 4 | C2150698 | 1 | 701 | Extended | OK | — |
| U12-U15 | 4 | C207281 | 1 | 2,555 | Extended | OK | — |
| X1 | 1 | C20010545 | — | — | — | Not found in DB | see notes — no verified pin/load-cap match confirmed |

Not checked: J1 (PCIe edge connector — card-edge finger pattern on the PCB itself, not
a purchasable part) and TP1-TP7 (generic through-hole test points, not LCSC-sourced).

## Notes / needs your judgment

1. **R32–R39 (C22548) is the one genuine stock risk**: 5 in stock, 8 needed. Suggested
   drop-in: C21190 (0603WAF1001T5E), same 1kΩ/0603/1% spec, Basic library (saves the $3
   setup fee too), 8M+ stock.
2. **Three parts aren't in the local snapshot**: R2-group (C159891), R20 (C481960), and
   X1 (C20010545). Could be delisted/renumbered since the snapshot was taken, or a
   transcription issue in the BOM — verify directly on LCSC.com before ordering. For X1
   specifically, do **not** treat the suggested alternates as verified drop-ins — load
   capacitance and pinout weren't confirmed against the NDK CS14522-24M, and clock
   accuracy matters for U1 (the USB3 host controller). Check the datasheet before
   substituting.
3. **55 of 57 line items are Extended library**, each carrying JLCPCB's $3/unique-part
   setup fee regardless of quantity — roughly $165+ in setup fees baked into this BOM.
   Only C14663 (0.1uF 0603 X7R 50V) is Basic. Worth a look if assembly cost matters more
   than exact spec match, though no Basic equivalents turned up for the other passives
   in this pass.
4. **No substitutions were suggested for any IC, MOSFET, connector, or crystal**
   (U1-U15, Q4, Q5, J2-J5, X1) unless a confirmed pin/function-identical part was found —
   where nothing safe turned up, the table says so rather than guessing.
