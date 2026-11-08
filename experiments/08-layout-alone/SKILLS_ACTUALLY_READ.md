# Skills actually read — variant 08-layout-alone

## Skills read in full (in order)

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md` — Better Layout, read in full before building.

## Assigned skills NOT used

- None. The only assigned skill was Better Layout, and it was used.
- Note: the skill's `SKILL.md` links to supporting files (`grouping-and-alignment.md`,
  `spacing-and-adaptivity.md`). Those were NOT read; only the entry `SKILL.md` itself
  counts as read here. Principles were applied from the `SKILL.md` text alone.

## What Better Layout changed in this build

- Grouping by space, not lines: 8px inside a group, 16px between groups, 32px
  between sections; separator lines only on cards/table rows.
- One 16px spacing step per nesting level; all blocks align to the same inline
  margin (16px mobile / 24px desktop) and the same content column.
- Logical properties throughout (`padding-inline`, `margin-inline-start`,
  `inset-block-start`, `text-align: start`) so the layout mirrors for RTL.
- DOM order matches visual order; no `order`, `row-reverse` or grid placement
  moves content (grid areas only place the shell regions header/sidebar/main/footer).
- `minmax(0, 1fr)` on every grid track holding text or the table, `min-width: 0`
  on flex children; `width: 100%`, never `100vw`; `min-height`, never fixed
  `height`, on text boxes; `100dvh` for the full-height sidebar.
- `scroll-padding-block-start` equals the sticky header height (60px).
- Breakpoints from content: stats 2-col → 4-col at 640px, sidebar appears at
  900px, attendance/notices go 2-col at 900px. Table scrolls inside its card
  with the last column peeking past the scroll edge as the hidden-content cue.
- One primary action per view ("Take attendance"), inset from the edges.
- Borderless header controls (bell, menu) get 24px clearance between glyphs.

## Self-check (1280px and 390px)

Method: code-read verification only. No Chromium/Playwright is installed in this
VM (`which chromium/google-chrome` found nothing, no Playwright module), so no
live render or screenshot was possible. Verified by reading the code and by
`grep` counts against the built file.

Checks that pass by construction/count:

- Data: 12 student rows (12 unique rolls 8A-01–8A-12), 4 stat cards, 5 trend
  bars (94/96/93/97/95), 3 notices, footer text "Fictional demo data — UI Forge
  Experiment 1" present once.
- No `100vw` in CSS (only a comment), no fixed `height` on any text box
  (fixed heights exist only on the decorative notification dot and chart bars).
- Stat cards: `repeat(2, minmax(0, 1fr))` at 390px, `repeat(4, minmax(0, 1fr))`
  from 640px; `min-height: 96px` with wrapping sub-text, so they cannot overflow.
- Table: `min-width: 560px` inside `.table-wrap { overflow-x: auto }`, so at
  390px it scrolls inside its card and never forces page-level horizontal scroll.
- Header search wraps to a full-width second row when it stops fitting
  (`flex-wrap: wrap`, `flex: 1 1 160px`).

## Defects found and fixed

1. HIGH — App shell grid had no explicit areas: on desktop the header, sidebar,
   main and footer auto-placed into the wrong columns (header in column 1 only).
   Fixed with `grid-template-areas` ("header header" / "sidebar main" /
   "footer footer") and `grid-area` assignments.
2. MEDIUM — Header at 390px: brand + search + bell + menu button in one row
   squeezed the search input to ~30px wide. Fixed by letting the header wrap
   and giving search `flex-basis: 160px`, so it drops to a full-width second row
   when it stops fitting. (Trade-off, disclosed: the wrapped row reads
   brand/actions then search, while DOM order is brand/search/actions; this is
   a flex-wrap collapse, not an `order`/placement reorder.)

## Defects left

- None known from the code read. All layout findings above are fixed.

## UNVERIFIED

- No live-browser render at 1280px or 390px (no Chromium/Playwright in the VM):
  no-horizontal-scroll, no-clipped-text and no-overlap are established by code
  reading only, not by a rendered screenshot.
- 200% zoom and the RTL mirror were not tested (no browser); logical properties
  are in place, but the mirror is unconfirmed.
- The Menu button's scroll-into-view behaviour and the bell button were not
  exercised in a browser.
