# Skills actually read — variant 11-layout-typography

Order-swap test: this variant applied the skills in the opposite order to
Experiment 1 variant 02 (Typography → Layout). Here Layout ran first and
set the structure; Typography ran second and set the type system inside
that structure.

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md`
2. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`

Both assigned skills were read in full before building, and both were used.
No assigned skill was left unused.

Note: the runtime also required reading the house-rules file
`~/workspace/custom-skills/local-skills/AGENTS.md` before working in that
folder. It is repository guidance, not a skill, so it is not counted above.
No other skill file was read. No other variant folder was read.

## What each skill changed in the build

### (1) better-layout — applied first

- Grouping by space, not lines: one spacing scale (`8 / 16 / 24 / 32 / 48px`),
  8px gaps inside a group, 16px+ between groups, 32px between sections.
  No separator lines between groups; the only rules are the header border
  and the chart baseline.
- Shared alignment edges: content in one centred column (`max-width: 1120px`),
  16px nesting step, controls and text inside the layout margins.
- Logical properties throughout: `padding-inline`, `margin-inline`,
  `inset-inline-start`, `inset-block-end`, `text-align: start`, plus a
  `[dir="rtl"]` override for the off-canvas sidebar transform.
- Order by importance, DOM order = visual order: header → nav → stats →
  chart/notices → table → footer. No `order`, `row-reverse` or grid
  placement tricks anywhere in the CSS.
- One primary action per view: a single "Add student" button.
- Hidden content has a visible cue: the mobile sidebar has a menu button,
  scrim and close-on-Esc; the table wrapper scrolls inside its card with a
  "Scroll sideways to see all columns →" hint at 640px and below.
- Breakpoints from content, not device presets: sidebar becomes an overlay
  at 1020px (240px sidebar + readable content stop fitting), panels stack
  at 860px, bottom nav appears at 640px. Stats use
  `repeat(auto-fit, minmax(220px, 1fr))` and hold 4-across until they break.
- Growth/clipping defences from the skill's Before-you-finish table:
  `minmax(0, 1fr)` on the app grid and panels, `min-width: 0` on grid/flex
  children, `width: 100%` (never `100vw`), `100dvh` (never `100vh`) on the
  sidebar, `min-height` (never fixed `height`) on every text-holding box,
  `scroll-padding-block-start` set to the sticky header height,
  `viewport-fit=cover` so the `env(safe-area-inset-bottom)` on the bottom
  nav is non-zero.

### (2) better-typography — applied second, inside the layout

- One system font stack, no external font; weights 400 / 600 / 700 only,
  nothing below 400 under 18px.
- Small rem type scale with semantic names, each size paired with a
  unitless line-height and weight: display 1.75rem/1.1, h1 1.5rem/1.2,
  h2 1.25rem/1.25, h3 1rem/1.4, body 1rem/1.6, small 0.875rem/1.5,
  caption 0.8125rem/1.5, overline 0.75rem/1.4.
- Headings descend with level (h1 > h2 > h3); overline labels are
  uppercase at 0.75rem with 0.05em letter-spacing; large headings take
  −0.01em; body copy stays at 0 tracking.
- `text-wrap: balance` on headings only, `text-wrap: pretty` on
  paragraphs; `overflow-wrap: break-word` on headings/paragraphs;
  `white-space: nowrap` on overlines, pills and day labels.
- `font-variant-numeric: tabular-nums` on stat values, chart values and
  the numeric table columns.
- Measure capped at 65ch on the intro paragraph and sidebar note.
- Search inputs at `font-size: 1rem` (16px) so iOS Safari does not zoom.
- Font smoothing on `:root` only; `lang="en" dir="ltr"` on the document;
  typographic punctuation (en dash in "2025–26", em dash in the footer,
  ellipsis character in placeholders); underlines from the font
  (`text-underline-position: from-font`).

### Observable order effect

Because layout ran first, the type scale was fitted to the layout: the
65ch measure cap and the 220px stat-card floor were set by the grid, and
heading sizes were picked after the column widths were fixed. A
typography-first pass would have set the scale first and let the layout
adapt to it. The visible result is the same dashboard either way; the
difference is which decisions were treated as constraints.

## Self-check — 1280px and 390px

Method: code inspection plus static checks (Python pattern scan of the
CSS/JS and `node --check` on the extracted script). No live browser is
available in this VM (no Chromium/Playwright), so widths were verified by
arithmetic against the declared grid, not by rendering. All fixed data
verified present: 12/12 students, 4/4 stats, 5/5 trend bars
(94/96/93/97/95), 3/3 notices, footer text exact.

- 1280px: sidebar 240px + content max 1120px. Stats need 4×220 + 3×16 =
  928px against 976px available → 4-across, no overflow. Panels 2fr/1fr.
  Table (min-width 560px) fits its card without scrolling. No horizontal
  scroll: no `100vw`, app grid is `240px minmax(0, 1fr)`, all children
  `min-width: 0`.
- 390px: sidebar becomes an overlay (menu button + scrim); bottom nav
  shown with safe-area padding; content padding 16px and bottom padding
  clears the 56px bottom nav. Stats need 456px for 2-across against 358px
  available → 1 column. Panels stack. Table scrolls inside its card with
  the visible scroll hint. Primary button is full-width but inside the
  16px margins. Header search is replaced by the in-flow mobile search.

### Defects found and fixed

1. **Mobile search never displayed (HIGH).** The base rule
   `.search-mobile { display: none; }` sat after the 640px media query,
   so it overrode the query's `display: flex`. Moved the base rule before
   the media queries. Verified by rule order in the final file.
2. **Roll number duplicated (MEDIUM).** Each student row showed the roll
   twice: once as a sub-line under the name and once in its own Roll
   column. Removed the sub-line spans and the now-unused `.roll` CSS.
3. **Fixed `line-height: 18px` on the bell badge (MEDIUM, typography
   Before-you-finish table).** Replaced with `display: inline-flex`,
   centred alignment and unitless `line-height: 1.5`.
4. **Brand subtitle could overflow the header (LOW).** `.brand-sub` was
   `white-space: nowrap` with no clipping. Added `overflow: hidden` and
   `text-overflow: ellipsis` (name already had it).
5. **Dead CSS (LOW).** Removed a stray `.chart ul, .chart` rule and an
   empty `.table-scroll::after` rule.

### Defects left

None known in the code as written. The following could not be checked
without a browser and are listed as UNVERIFIED rather than as defects.

## UNVERIFIED (could not be checked in this VM)

- No live-browser render at 1280px or 390px: no horizontal scroll,
  clipping, overlap or stat-card overflow confirmed visually, only by
  code inspection and width arithmetic.
- No 320px width or 200% zoom check.
- No RTL mirror check (logical properties are in place; the mirror was
  not rendered).
- No real-device check: bottom-nav safe-area behaviour, iOS search-input
  zoom behaviour and sticky-header scroll padding not observed live.
- Search filtering, sidebar toggle and bell scroll behaviour verified
  only by `node --check` on the script, not by interaction.
