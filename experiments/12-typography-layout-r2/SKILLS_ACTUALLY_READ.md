# SKILLS_ACTUALLY_READ — Variant 12-typography-layout-r2 (REPEAT of Experiment 1 variant 02)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`
2. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md`

## Assigned skills not used

None. Both assigned skills were read in full before building and both were applied.

Note: the `local-skills` repo's `AGENTS.md` (`/home/hatch/workspace/custom-skills/local-skills/AGENTS.md`)
was also read (runtime house-rules instruction). It is repo authoring guidance, not a build skill;
it did not change the build. No other skill file, variant folder, or reference file was opened —
variant 02 and all other variant folders were not read, copied, or opened.

## What each skill changed in the build

### better-typography
- Semantic type scale in `rem`: display 1.75rem / h1 1.5rem / h2 1.125rem / body 1rem / UI 0.875rem /
  caption 0.8125rem, each paired with a unitless line-height (display 1.1, headings 1.25, body 1.55, UI 1.45).
- Headings descend with level; letter-spacing in `em` only (headings −0.01em to −0.015em,
  uppercase labels 0.05em at ≤14px, body 0).
- `font-variant-numeric: tabular-nums` on stat values, table cells, bar values and marks.
- `text-wrap: balance` on the page heading, `text-wrap: pretty` on intro/section text, `text-align: start`.
- Body measure capped at 65ch (60–75 character range).
- Search input raised to `16px` at ≤26rem so iOS Safari does not zoom on focus.
- No weight below 400 under 18px; font smoothing set once on `html`, never per component.
- Typographic punctuation throughout: en dash in "2025–26" and the footer, curly apostrophes avoided,
  "…" in the search placeholder.
- System font stack only — the skill says never to propose a new typeface unless the task asks for one,
  and TASK.md forbids external fonts.

### better-layout
- Space groups first: `gap: 2rem` between main sections vs `0.5rem` inside groups (≥2× rule).
- Logical properties throughout: `padding-inline`, `margin-inline-start`, `inset-block-start`,
  `text-align: start`, `inline-size`/`block-size` on the notice pins.
- `minmax(0, 1fr)` on every grid track and `min-width: 0` on main/table wrappers so long text
  and the wide table cannot blow out their tracks.
- No `100vw` / `100vh`; full-height uses `min-height: 100dvh`; no fixed `height` on any box holding text
  (the only fixed heights are the chart bar tracks, which hold no text).
- `scroll-padding-block-start: 4.5rem` set to the sticky header height.
- Breakpoints from content, not device presets: stats collapse 4→2→1, content grid 2→1 col,
  sidebar becomes a sticky horizontal scroll nav below ~48rem.
- Table scrolls inside its card on narrow screens with a visible cue ("Scroll sideways to see all
  columns →" shown at ≤26rem); sidebar nav scrolls horizontally with items peeking past the edge.
- DOM order matches visual order; no `order`, `row-reverse`, or grid placement reordering.
- Controls distinct from content: bell is a bordered circular button, nav current page has a
  filled background, search has a bordered pill shape.

## Self-check (1280px and 390px)

Method: code-reading check plus static verification — no Chromium/Playwright is available in this VM,
so no live render or screenshot was possible. Verified by script/grep:
- 12 student rows in data, 5 trend entries, 3 notices, 4 stat cards, footer text exact.
- JS syntax valid (`node --check` PASS).
- Zero external URLs, zero `<link>`/`<img>`/`@import`/external fonts.
- No `100vw` / `100vh` anywhere.

### Defects found and fixed
1. Dead CSS: `.nav-toggle { display: none; }` declared twice but no such element exists — removed both.
2. Inline styles on the visually-hidden search label — moved into a `.visually-hidden` CSS class.
3. (Design review, pre-build) Initial plan used `height: 100vh` for the app shell — changed to
   `min-height: 100dvh` before writing the file.
4. (Design review, pre-build) Initial stat grid used fixed `1fr` tracks without `minmax(0, …)` —
   changed to `minmax(0, 1fr)` so long stat sub-labels cannot overflow at 390px.

### Defects / limitations left
1. **Chart bars look nearly flat.** Trend values are 93–97%, plotted on a true 0–100% scale, so the
   tallest bar is only ~4% taller than the shortest. This is honest (no truncated axis), but the
   day-to-day differences are hard to read at a glance. Left as-is deliberately; a truncated axis
   would exaggerate a 4-point swing.
2. **No live-browser verification.** No Chromium/Playwright in the VM, so 1280px and 390px rendering,
   horizontal-scroll absence, and sticky-header behaviour were verified by code reading and static
   checks only — NOT by an actual render. UNVERIFIED.
3. **320px width / 200% zoom not checked.** TASK.md requires 1280px and 390px only; the skills'
   HIGH-severity triggers at 320px/200% zoom were not exercised (no browser available).
4. **RTL mirror not checked.** Logical properties are used throughout, but no RTL render was run.
   UNVERIFIED.
