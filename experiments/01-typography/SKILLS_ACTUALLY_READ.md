# Skills actually read — variant 01-typography

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md` — read in full before building. This is the ONLY skill read for this variant.

## Assigned skills NOT used

None. The only assigned skill was Better Typography, and it was used. No other UI skill was read (no better-layout, no emil-design-eng, no Astryx, no OpenSourceUI, no PanelUI, no pstack, no ECC). No other variant folder was read.

## What Better Typography changed in this build

- Semantic type scale in `rem`: display 1.75rem, title 1.25rem, body 1rem, small 0.875rem, caption 0.8125rem; headings descend with level.
- Unitless line-heights: headings 1.2–1.25, body 1.5, wrapping text 1.4+.
- Letter-spacing in `em`: headings ≥24px −0.01em, uppercase labels ≤14px 0.05em, body 0.
- Weights ≥400 below 18px (no thin weights anywhere).
- `font-variant-numeric: tabular-nums` on stat values, bar values and numeric table columns.
- Measure capped at 65ch on paragraphs; `text-wrap: balance` on headings, `text-wrap: pretty` on paragraphs; `overflow-wrap: break-word`.
- Search input at 1rem (16px) so iOS Safari does not zoom on focus.
- Font smoothing on the root only; `text-align: start`; typographic punctuation (en dash in “2025–26”); underlines from the font.
- Truncation with a way back: stat labels and student names use ellipsis + `title` attribute.

## Self-check — 1280px and 390px

No Chromium / Playwright browser was available in this VM, so this was a code-based static check (Python, 16 checks), not a rendered screenshot check. All 16 checks PASS:

- No external http/https, no `<link>`, no `<img>`; no external fonts or images.
- All 12 students, all 4 stat values (1,248 / 72 / 95.2% / 84%), trend 94/96/93/97/95, all 3 notices, and the required footer text present with the exact TASK.md data.
- No `px` font sizes and no `px` line-heights; tabular-nums present; input at 1rem.
- Table is inside `.table-wrap` with `overflow-x: auto`, so it scrolls inside its card instead of forcing page scroll.
- `box-sizing: border-box` throughout; sidebar hidden at ≤640px, bottom nav shown; stats go 4 → 2 columns at ≤900px.

## Defects found and fixed

- None found by the static check. No defect was found and fixed during this build; the layout was written to the constraints from the start (2-column stats at 390px, table scrolling inside its card, sidebar replaced by bottom nav).

## Defects left / UNVERIFIED

- **UNVERIFIED: rendered appearance at 1280px and 390px.** No browser was available, so no horizontal scroll, clipping, overlap, or stat-card fit was confirmed visually — only by code inspection and the static checks above. A live-browser / screenshot check is still needed.
- **UNVERIFIED: real-device feel.** No iOS Safari check that the 16px search input actually prevents zoom, and no 200% zoom check.
