# SKILLS_ACTUALLY_READ — Variant 44 (colors + layout)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` (entry router; ECC first, then pstack)
2. `/home/hatch/workspace/skills/skills/skills/better-colors/SKILL.md`
3. `/home/hatch/workspace/skills/skills/skills/better-layout/SKILL.md`

Also read (house rules, not a skill): `/home/hatch/workspace/skills/skills/AGENTS.md`
(brought in by a runtime note while reading the skills repo; it governs skill
authoring in that repo, not this dashboard, and changed nothing in the build.)

No assigned skill was left unused. No other variant folder was read. Nothing
was copied from another variant.

## How better-colors was applied

- **A system is ramps, not colors.** One neutral ramp (hue 250, near-zero
  chroma), one accent ramp (hue 255 held end to end), and only the status
  ramps this page actually renders (success 150, warning 80, danger 25).
- **Primitives vs semantics.** Primitives (`--blue-600`, `--neutral-900`) name
  values in `oklch()` throughout — one notation, the skill's default for a new
  system. Semantic tokens (`--color-text`, `--color-accent-solid`,
  `--color-bg-surface`) name jobs and are the only tier components reference.
  No hex, `rgb()` or `hsl()` literal appears anywhere in the file.
- **Every step has a job.** Each ramp step maps to a role (page bg, surface,
  subtle bg, border, solid fill, text). No unused step was generated.
- **One color, one meaning.** Accent marks only the current nav item and the
  single primary action. Status hues are all more than 15° from the accent
  hue (255) and from each other. Meaning is never carried by colour alone:
  status pills carry text labels ("On track", "Watch", "Needs support"), the
  bell badge is a number (3), every chart bar carries its numeric label.
- **One primary action per view.** Exactly one filled accent button exists:
  "Mark attendance". Every other control (menu, bell, "View all 12") is
  neutral: surface background + border + text colour.
- **Contrast, computed from the declared tokens** (OKLCH → sRGB → WCAG ratio,
  computed in Python, not browser-measured): body text on page 17.9:1,
  secondary text on page 12.4:1, secondary on surface 14.2:1, white on
  accent-solid 13.8:1, accent text on accent-soft 11.2:1. All pass 4.5:1 by a
  wide margin. Browser-measured rendered pairs were not taken (see
  "Not verified" below).

## How better-layout was applied

- **Group with space, not lines.** 8px gaps inside a group, 24px+ between
  groups, 32px between sections. No separator line carries structure.
- **Controls distinct from content.** Search input, buttons and nav links all
  have a background shape and/or border; none is styled like static text.
- **Shared edges / one spacing step.** 16px base step; main padding 24px,
  cards padded 16px.
- **Logical properties throughout.** `margin-inline`, `padding-inline`,
  `inset-inline-start/end`, `text-align: start`, `border-inline-start` for
  the active-nav and notice accents. Physical left/right are used nowhere
  except the `translateX` drawer transform, which has an explicit
  `[dir="rtl"]` override (the skill's prescribed fix).
- **DOM order = visual order.** No `order`, `row-reverse` or grid placement
  reorders content. See the conflict below for where this bit.
- **Hint at hidden content.** The student table scrolls inside its card at
  narrow widths; a visible "Scroll sideways inside this card to see marks →"
  hint is shown on mobile, and the next column peeks past the scroll edge.
- **Content bleeds, controls float.** Sticky header with
  `scroll-padding-block-start` set to the 64px header height; bottom nav
  respects `env(safe-area-inset-bottom)` with `viewport-fit=cover` in the
  viewport meta; main content gets bottom padding so the fixed bottom nav
  never covers it.
- **Hold structure until it breaks.** The sidebar collapses at 860px because
  that is where 232px sidebar + content stops fitting, not a device preset.
  `minmax(0, 1fr)` on the shell grid and `min-width: 0` on flex/grid children
  (the skill's table-overflow fix); no `100vw`, no `100vh` (drawer uses
  `100dvh`); no fixed height on any text-holding box (the chart's 220px
  height is on a non-text chart container — see below).
- **Plan for growth/clipping.** Text containers use `max-width`/`min-height`,
  never fixed width/height; rows wrap (`flex-wrap` on section heads and the
  mobile header).

## Conflicts and resolutions

1. **Task section order vs. a two-column chart+notices layout (better-layout
   "visual order matches DOM order").** My first build put chart and notices
   side by side, which placed notices visually before the student table,
   violating the task's required order (chart 4 → table 5 → notices 6).
   Fixing it with grid placement would have broken DOM/visual order, which
   better-layout forbids. Resolution: the task order and better-layout both
   win over the prettier layout. The page is now a single sequential flow —
   stats → chart → students → notices — and notices are a 3-column grid
   below the table (1 column on mobile).
2. **"One primary action per view" (both skills) vs. a dashboard full of
   controls.** Resolved by treating it strictly: one filled accent button
   ("Mark attendance"); search, bell, menu, "View all 12" and all nav links
   stay neutral. No second filled button was added anywhere.
3. **Chart bar heights as percentages vs. better-layout's "no fixed height".**
   Percentage bar heights need a definite containing-block height or they
   collapse (this actually happened — see defects). Resolution: the fixed
   220px height is on the chart container, which holds no text directly;
   text boxes keep `min-height` only. The bar heights themselves remain
   data-driven percentages (93–97%) inside a flex track.

## Defects found and fixed (all objective task/skill violations, not polish)

1. **Chart bars rendered as ~8px slivers** (headless screenshot at 1280px).
   Cause: `height: 94%` on `.bar` had no definite-height containing block.
   Fix: added a `.bar-wrap` flex track per column and a definite 220px chart
   height, so percentages resolve. Re-verified by screenshot: bars are tall
   and proportional.
2. **Mobile header search squeezed to a ~20px sliver at 390px** (placeholder
   clipped, control unusable). Fix: at ≤860px the header wraps and the
   search becomes a full-width second row. Re-verified by screenshot.
3. **Hidden empty `<h2 style="display:none">` for the stats section.**
   Fix: removed the heading; the section now uses `aria-label`.
4. **Chart note claimed Friday's bar was "labelled 'today'"** when no such
   label existed. Fix: copy now says the number above each bar carries the
   value.
5. **Notices placed before the student table** (order violation, see
   conflict 1). Fix: restructured to stats → chart → students → notices.

## Defects left

None known. Notes that are not defects:

- Chart bar height differences look small because the data range is narrow
  (93–97% of the track). That is honest to the data, not a rendering fault.
- At 390px the table intentionally scrolls inside its card (marks/status
  off-screen until scrolled, with a visible hint) — the task explicitly
  allows "table fits or scrolls inside its card".
- In the full-page `captureBeyondViewport` screenshot the fixed bottom nav
  appears mid-page; that is a capture artifact. In viewport screenshots it
  sits at the bottom correctly.

## Verification performed

- Python content check: all 12 students (name/roll/attendance/marks), all
  trend values, all 3 notices, all headline figures, exact footer text —
  all PASS. No external URLs, images or fonts; no hex/`rgb()`/`hsl()`; no
  `100vw`/`100vh`.
- Headless Chromium (Playwright chromium-1248 binary) via CDP with
  `Emulation.setDeviceMetricsOverride`: 1280px → `scrollWidth = clientWidth
  = 1265`, 0 overflowing elements, 0 console errors. 390px →
  `scrollWidth = clientWidth = 390`, 0 overflowing elements outside the
  table's own scroll container (table/thead/tbody cells extend past the
  viewport only inside `.table-scroll`, which is the intended in-card
  scroll), 0 console errors.
- Full-page screenshots eyeballed at 1280px and 390px after the fixes:
  no horizontal scroll, stat cards do not overflow, no clipped or
  overlapping text, footer present.
- Contrast values above are computed from declared tokens, not
  browser-measured.

## Not verified

- Browser-measured rendered contrast pairs (computed from tokens only).
- RTL mirror in a browser (logical properties used throughout; the drawer
  `translateX` has a `[dir="rtl"]` override, but no RTL render was run).
- 200% zoom. Real-device feel, touch targets and screen readers.
