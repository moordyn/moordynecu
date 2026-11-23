# Rev C: assemble-vs-source decision sheet (23 Nov 2026)

**Aim:** Fully populated Rev C ECU electronics PCB, no customer-consigned parts, optional J1 omitted in favor of J40 USB-B.

## Important scope and evidence

- Current GitHub quote BOM/CPL: **418 unique placements** with no designator mismatches, per `README.md`. No Gerber changes.
- Stock statuses below are **from the customer's previous JLCPCB component-matching report** (`Pasted text(7).txt`) for an **earlier, 2-PCBA profile**, not a live stock check of the current 418-position files. The earlier profile also included U300/U303 and J1 which are now omitted, and used ZXMS6005 on Q305–Q312 instead of the documented NCV8440 substitution. **Do not treat old stock availability as a current commitment.**
- **JLCPCB only assembles items actually checked/selected in its component-matching screen.** Unmatched or shortfall positions can be left empty if the order is advanced.
- JLCPCB can do SMT and some through-hole/wave soldering; **a catalog entry is not proof the particular connector is accepted for placement**.
- Shortages should be pre-ordered through JLCPCB Parts Manager (or Global Sourcing / New Parts Request). **No customer shipping** is required if JLCPCB can source and accept the part. Do not order/ship parts to yourself unnecessarily.

## First-pass order status and what to do

| Refs | Qty/ECU | Exact part / LCSC | Previous matching status | Next step | Potential user install |
|---|---:|---|---|---|---|
| **P1** | 1 | TE `284617-1` / **C499121** | Unmatched previously; new code not rechecked | Search code; ask JLCPCB whether it will source and **solder the 154-pin THT connector**, confirm clearance and placement. If it won't, leave P1 unassembled | **Yes, if JLCPCB refuses**; requires careful high-pin-count soldering |
| **F3–F6** | 4 | Littelfuse `36911600000` **1.6 A**, C3165187 | Unmatched previously due erroneous 16A text and 10A auto-match; new code not rechecked | Search/pre-order exact 1.6A part; verify wave/manual THT assembly | **Possible**, relatively manageable THT soldering |
| Q1–Q4 | 4 | `ISL9V3040D3ST-F085C` / C603080 | Matched in old 2-PCBA report (8 total) | Keep selected; confirm 0° CPL versus physical transistor tab/pad | Only if JLCPCB later refuses, not planned |
| Q5/Q6/Q10 | 3 | `VND14NV04TR-E` / C2649436 | Matched in old 2-PCBA report (6 total) | Retain exact part; do not let auto-match weaker VND7NV04 | No expected |
| **U490/U491** | 2 | `74HC2G17GW/C4125` / C3754951 | **6 shortfall** | Pre-order through JLCPCB; populate, don't bridge | Not recommended manually (very small SOT-363) |
| **U4** | 1 | `LPS22HBTR` / C94049 | **2 shortfall** | Pre-order, preserve correct HLGA footprint | Not recommended manually (tiny LGA pressure sensor) |
| **Q300–Q304, Q313/Q314** | 7 | `PJM2319PSA` / C411720 | **18 shortfall** | Pre-order exact P-channel MOSFETs; choose alternates only after ratings/pinout review | Not planned |
| **Q305–Q312** | 8 | `NCV8440ASTT3G` / C604063 | No current match checked. Previous report had *different* ZXMS6005SGTA | Check current match; pre-order if needed; follow documented rusEFI `bom_replace` | Not planned |
| **L420** | 1 | `LBC3225T2R2MR` / C223413 | **3 shortfall** | Pre-order exact source inductor | Not planned |
| **R7/R16/R76** | 3 | 20k four-resistor isolated array / C507417 | **3 shortfall** | Pre-order exact array; alternative requires matching topology/footprint | Not planned |
| D3/D28/D29/D30/D105 | 5 | blue 0603 LEDs / C72041 | **20 shortfall** | Pre-order or choose electrically/footprint compatible same-color 0603 LED in JLC matching UI | Possible but not desirable |
| D106 | 1 | yellow 0603 LED / C72038 | **20 shortfall** | Pre-order or verified equivalent in matching UI | Possible but not desirable |
| **BT1** | 1 | CR2032 holder / C70377 | **0 quantity selected** | Check if **placed**, not just matched; user may need to insert CR2032 cell separately | Possible holder install |
| **S1/S2** | 2 | tact switches / C115357 | **0 quantity selected** | Check if placed; otherwise match/sourcing | Possible small SMD install |
| **J2** | 1 | 12-pin Molex Mini-Fit Jr style / C2764798 | Matched previously | Check actual assembly selection / THT acceptance | No expected |
| **J40** | 1 | USB-B receptacle / C46392 | Matched previously | Keep selected for USB interface / THT acceptance | No expected |
| U105/U6/U7 and most other parts | many | Source improved BOM | Matched previously | Keep selected, check package/orientation | No expected |
| **J1** | 0 | optional USB header | Deliberately removed | Leave footprint empty; J40 is used instead | **No** |
| U300/U303/U103/U480/D40–D44/Q7–Q9/R72, etc. | 0 | omitted by improved BOM | Not required by current quote configuration | Do not re-add from raw CPL only; use upstream improved BOM | No planned |

## Assembly orientation checkpoints

- Original rusEFI Rev C issue [#105](https://github.com/rusefi/hellen154hyundai/issues/105) describes rotations affecting TLE9201SG, ISL9V3040D3S, LMV321, MIC5205, and SRV05-4-P-T7. It is not proof the current JLC preview is wrong because the original issue uses different references/rotation conventions. **Verify against actual pin-1 and tab pads / silkscreen, and ask PCBA engineering to review**; never claim the 2D preview validates every IC.
- The custom CPL changes Q1–Q4 to 0° versus original 180°. Check tab alignment with pad *before* authorizing build.
- If any component gets a different MPN in the final JLC matching results, verify the exact pinout, voltage/current, thermal parameters and package before approval.

## Steps for a fast, no-consignment order

1. Upload the unchanged `hellen154hyundai-c-gerber.zip` from the Rev C source and this folder's current BOM/CPL; choose the intended **PCB fabrication count** and **PCBA assembled count**.
2. In matching, **select every intended component**. Run through all designators; inspect **unmatched, shortfall, 0-placed, and alternate MPN** statuses.
3. Ask JLC PCB assembly engineering specifically about P1 C499121 (154-pin connector), F3–F6 C3165187 (radial/THT fuses), J2 and J40 (through-hole). Get agreement on through-hole assembly.
4. Pre-order all actual shortages through JLCPCB Parts Manager. Global Sourcing / New Parts Request if unavailable in normal library. Check quantities/minimum attrition shown by JLC rather than ordering just 2x each part. **Do not purchase customer consignment inventory**.
5. Once sourcing inventory has **arrived** in JLC warehouse and is selectable, return to component matching, allocate each part, inspect 3D/2D polarity, and pay for the fully populated PCBA.
6. If you choose to order before part arrivals, expect a **partial assembly**; JLCPCB's official guidance says unmatched/unselected parts are not populated. Don't assume a missing MCU/sensor/output component can be installed later without manual work.

Official references:
- https://jlcpcb.com/help/article/component-matching-guidelines-for-pcba-orders
- https://jlcpcb.com/help/article/pcb-assembly-faqs
- https://jlcpcb.com/help/article/how-to-use-jlcpcb-global-sourcing-parts-service
- https://jlcpcb.com/help/article/what-is-jlcpcb-parts-pre-order-service

**Do not connect an untested ECU to a running engine.** Bench power-up, rails, firmware, crank/cam inputs, injectors/ignition, throttle and harness pinout should be validated first.
