# Rev C — JLCPCB maximum PCB assembly profile

**Scope:** Dedicated manufacturing quote profile for the Rev C ECU electronics PCB. The original rusEFI Gerbers, source BOM, CPL, frame and schematic are unchanged.

## Upload to JLCPCB

Use these three files (download the original Gerber archive from the linked source path):

1. [Original Rev C Gerbers](../../../boards/hellen154hyundai-c.fab/board/hellen154hyundai-c-gerber.zip) — upload without extracting
2. `JLC_BOM_FULL_ASSEMBLY.csv` — edited assembly BOM
3. `JLC_CPL_FULL_ASSEMBLY.csv` — edited matching placements

**Validation:** 418 unique BOM designators = 418 unique CPL designators. 122 original CPL positions omitted because they are not populated in this manufacturing profile. See `UNPOPULATED_POSITION_AUDIT.csv`. **No PCB copper/drill changes.**

## Decisions

- Base: original `boards/hellen154hyundai-c.fab/board/hellen154hyundai-c-BOM.improved.csv` (SHA `bd4d813834417bd3e7ab4f35aef3d6c2db09cbec`) and original CPL (SHA `4b1ae97b5f368d8e9dc4ea066020a166b43dce63`).
- Removed logos, PCB pads, non-component test marks and DNPs from the assembly files. Original PCB test pads stay physically on Gerbers.
- Follow upstream `bom_replace_hellen154hyundai-c.csv`: omit documented unused R103, R105, R131, R309, U1, J3, J25, J39, P480; use `NCV8440ASTT3G` on Q305–Q312, and exact `LBC3225T2R2MR` for L420.
- Other circuits omitted by the improved BOM (including U103/U480/U300/U303/D40/D41/D44/Q7–Q9/R72) remain unpopulated. Do **not** restore indiscriminately.
- F3–F6: correct original text typo `Fuse-16A` to **1.6 A**, preserving exact manufacturer part `36911600000`; use LCSC C3165187 (not the wrong 10 A auto-match).
- Q5/Q6/Q10: enforce exact original ST `VND14NV04TR-E` rather than automatically matched VND7NV04.
- **J1 deliberately unpopulated:** its optional USB pin header is unnecessary when using the installed **J40 USB Type-B** connector. The J1 copper footprint is left in the Gerbers; do not select a component for it.
- Add **P1** TE 284617-1 C499121 for an attempt at full JLCPCB through-hole assembly; P1 is verified in the Rev C frame BOM. JLCPCB must individually approve 154-pin connector soldering and placement.
- Add missing **U490/U491** Nexperia `74HC2G17GW/C4125` C3754951 (digital input buffer issue [#131](https://github.com/rusefi/hellen154hyundai/issues/131)).
- Q1–Q4: onsemi `ISL9V3040D3ST-F085C` C603080, same-package automotive variant of source design. Change CPL orientation **180° → 0°** per documented [Rev C rotation issue #105](https://github.com/rusefi/hellen154hyundai/issues/105). Confirm against tab/pinout and JLC placement preview before ordering. Other polarities not certified.

## Sourcing (no customer-shipped parts required)

See [SOURCING.csv](SOURCING.csv) for designators, quantities per ECU, and catalog codes. For each shortage, use **JLCPCB Pre-order**; for part numbers not supported in the JLCPCB assembly library, request **Global Sourcing / New Parts**. LCSC catalog listings alone do not establish that JLCPCB can source or mount the component. JLCPCB sets attrition/minimum order quantities.

- [JLCPCB sourcing instructions](https://jlcpcb.com/help/article/pcba-parts-sourcing-instruction)
- [JLCPCB global sourcing](https://jlcpcb.com/help/article/how-to-use-jlcpcb-global-sourcing-parts-service)
- [Rev C reorder history #123](https://github.com/rusefi/hellen154hyundai/issues/123)
- [TE 284617-1 connector specifications](https://www.te.com/en/product-284617-1.html)

**Manufacturing status:** Suitable for quote/pre-order review, **not unconditional production approval**. Confirm part matching, THT assembly acceptance and polarized-component rotations. Bench-test power, outputs, sensors, firmware and vehicle harness configuration before connecting to an engine.
