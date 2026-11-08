# Skills Actually Read — Variant 02-typography-layout

Variant: 02-typography-layout (Better Typography → Better Layout)
Task: UI Forge Experiment 1 — Student Management Dashboard (see `~/workspace/skills-test/TASK.md`)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md` — Better Typography, read in full before building.
2. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md` — Better Layout, read in full before building, after Better Typography.

No other skill file was read in full. No other variant folder was read.

## Assigned skills NOT used

None. Both assigned skills were read in full and used (see below).

## How each skill was applied

**Better Typography**
- Semantic type scale in `rem`: display 1.75rem/1.2, headings 1.25rem/1.3, section 1rem/1.3, body 1rem/1.5, small 0.8125rem/1.4. Headings descend with level; weights 400+ below 18px, 650 only at 16px and up.
- Unitless line-heights everywhere; `text-wrap: balance` on headings, `text-wrap: pretty` on paragraphs; `text-align: start`.
- `font-variant-numeric: tabular-nums` on stat values, table numeric columns, and chart values.
- Intro text capped at 65ch (60–75ch measure); headings tracked −0.01em at 24px+, uppercase labels 0.05em.
- Search input at 1rem (16px) so iOS Safari does not zoom; font smoothing on the root only; typographic punctuation (en dash in "2025–26", em dash in footer); natural case with `text-transform` for labels/eyebrows.

**Better Layout**
- Grouping with space, not lines: 8px inside a group, 16px between groups, 24px between sections.
- Shared 16px alignment edges; logical properties (`padding-inline`, `margin-inline`, `inset-inline-start/end`, `text-align: start`) throughout; `viewport-fit=cover` + `env(safe-area-inset-bottom)` for the mobile bottom nav.
- DOM order = visual order; no `order`, `row-reverse`, or grid placement reordering.
- Breakpoints from content: sidebar + 4-column stats only at ≥56rem; 2-column stats below that; table never forces the page wider — it scrolls inside its card (`overflow-x: auto`, `min-width: 40rem` on the table) with a visible "Scroll sideways inside this card" cue.
- No `100vw`; `minmax(0, 1fr)` tracks and `min-width: 0` on flex/grid children; `min-height` (never fixed `height`) on text containers; `100dvh` for the app shell; `scroll-padding-block-start` set to the sticky header height.
- Controls distinct from content: bordered search input and bordered notification button; bottom nav / sidebar links have a background shape on the current page.

## Self-check at 1280px and 390px

Method: code-reading + a static script check (all data present, no external refs, no `100vw`, `minmax(0, 1fr)` present, table wrapper scrolls, DOM order ascending). **No browser was available in this VM (no Chromium/Chrome), so no live render, screenshot, or measured scrollWidth check was run — the 1280px / 390px checks below are by code inspection, not by a rendered page.**

- No horizontal scroll: page width is `100%` (no `100vw`); every grid track is `minmax(0, 1fr)`; the only wide element is the table, which is inside `overflow-x: auto` in its card.
- No clipped text: no fixed heights on text containers (only `min-height`); text wraps (`overflow-wrap: break-word` on stat values/labels); chart bar heights are percentages on bars that contain no text.
- No overlap: sticky header has `scroll-padding-block-start`; bottom nav is sticky at the block end on mobile and hidden on desktop; sidebar is hidden below 56rem.
- Stat cards fit: 4 columns ≥56rem and 2 columns below; each card is `minmax(0, 1fr)` with `min-width: 0`, values at 1.75rem ("1,248", "95.2%") fit a ~170px column at 390px.
- Table fits/scrolls inside its card: `min-width: 40rem` table inside `.table-wrap` with `overflow-x: auto` and a visible scroll hint.

## Defects found and fixed

- Two static-check flags turned out to be false positives on investigation, not defects: (1) a "fixed height" flag matched chart-bar percentage heights and `min-height` declarations — bars contain no text, and text containers use `min-height` only; (2) a "sections out of order" flag matched the word "sidebar" in the stylesheet before the body — DOM order in the body is ascending (topbar → sidebar → stats → trend → notices → students → footer), verified separately. No code change was needed for either.
- No other defects were found by the static check.

## Defects left

- None found by the static check. No defect is knowingly left in the file.

## UNVERIFIED

- Live render at 1280px and 390px in a real browser (no browser in this VM): no horizontal scroll, no clipped text, no overlap, stat-card fit, and table scrolling are all **UNVERIFIED by measurement** — they rest on the code inspection above. A real-browser check (or screenshot at both widths) is still needed before this variant can be ranked on rendered output.
- Feel / visual quality of the typography and layout choices: not verified by eye.
