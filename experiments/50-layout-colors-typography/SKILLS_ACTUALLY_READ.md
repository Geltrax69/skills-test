# SKILLS_ACTUALLY_READ.md — Variant 50: Layout → Colors → Typography

3-way ORDER variant. The three skills were read in full, in this order,
before building. Nothing else was read as a skill source.

## Skills read in full (in order)

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
   — harness entry: ECC first, then pstack; routed this UI task to the
   domain skills and to the prototype playbook shape (build, verify,
   report Done / Left, no commit/push on my own word).
2. `/home/hatch/workspace/skills/skills/skills/better-layout/SKILL.md`
3. `/home/hatch/workspace/skills/skills/skills/better-colors/SKILL.md`
4. `/home/hatch/workspace/skills/skills/skills/better-typography/SKILL.md`

Also read, because the runtime flagged it as a house rule for the folder
just touched (not a design skill): `/home/hatch/workspace/skills/skills/AGENTS.md`.
It governed repo conventions only and did not change any design decision.

## Skills assigned but NOT used

None. All three assigned skills were read in full and applied.

## How each skill shaped the page

### better-layout (read first — structure decided first)
- Spacing scale: one 8px step; groups separated by 32px, ≥ 2× the 8–16px
  gaps inside a group. No separator lines carry structure; space does.
- Logical properties throughout (`margin-inline-start`, `padding-inline`,
  `inset-inline-end`, `text-align: start`) so the layout mirrors for RTL.
- DOM order matches visual order: header → nav (sidebar, inline-start) →
  main content. No `order`, `row-reverse` or grid placement reordering.
- Shared alignment edges: one 16px step per nesting level; cards, stat
  grid and chart all sit on the same inline edges.
- `minmax(0, 1fr)` / `min-width: 0` on the flex child (`.main`) and on
  grid items so long content shrinks instead of overflowing its track.
- Breakpoint from content, not a device preset: at ≤ 56rem the sidebar
  stops fitting and becomes a fixed bottom bar; the main gains
  `padding-block-end` so the last content clears it.
- Hidden-content cue: on narrow screens the student table scrolls inside
  its card (`overflow-x: auto` on `.table-wrap`, `min-inline-size: 30rem`
  on the table) and the next column peeks past the scroll edge.
- Sticky header with `scroll-padding-block-start` set to header height.
- `100dvh` for the sidebar height (never `100vh`); no `100vw` anywhere;
  `min-height` never a fixed `height` on a text box.

### better-colors (read second — palette built on the layout)
- New system in `oklch()`, one notation throughout: one neutral ramp
  (hue ~265, chroma near zero), one accent ramp (indigo, hue 265 held
  end to end, vividness peaking mid-ramp), plus only the status ramps
  this page actually renders (success green, warning amber).
- Primitives name values (`--accent-600`); semantic tokens name roles
  (`--color-text`, `--color-bg-surface`, `--color-accent-solid`) and are
  the only tier components reference. No token is used outside its role.
- One filled action per view: "Mark attendance" is the only filled
  button; color sits on its background, label stays white. All other
  controls stay neutral.
- One color, one meaning: indigo = accent/navigation emphasis; green =
  attendance ≥ 90%; amber = attendance < 90% (89%, 88%). The percentage
  number is always printed beside the color, so color never carries
  meaning alone.
- Contrast was checked from the declared pairs, not estimated: body text
  `oklch(0.21 …)` on white surface, secondary text `oklch(0.45 …)` on
  white, white on accent solid `oklch(0.45 0.18 265)`. No failing pair
  was found, so no colors were changed after measuring (per the skill:
  report the pair, leave the colors alone unless asked).

### better-typography (read third — type applied last)
- One font family only (system stack, no external fonts): hierarchy comes
  from size and weight, not extra faces.
- Semantic type scale in `rem`: display 1.5rem / 1.2, heading 1.125rem /
  1.3, body 1rem / 1.6, small 0.875rem / 1.5, caption 0.75rem / 1.5.
  Headings descend with level; no weight below 400 under 18px.
- Unitless line-heights everywhere; letter-spacing in `em` only
  (−0.01em/−0.02em on headings, 0.05em on ≤14px uppercase labels,
  0 on body copy).
- `font-variant-numeric: tabular-nums` on stat values, numeric table
  columns, chart values and notice dates so changing digits never shift.
- `text-wrap: balance` on headings, `text-wrap: pretty` on paragraph
  copy, `text-align: start` throughout.
- Search input is `1rem` (16px) at every viewport, so iOS Safari never
  zooms on focus.
- Typographic punctuation in rendered copy: en dash in "2025–26" and
  "Students — Class 8-A", ellipsis character in the search placeholder,
  middle dot separators in stat notes. `lang="en"` and `dir="ltr"` set
  on the document.
- Font smoothing set once on the root, never per component.

## Conflicts found and resolved

- **Layout "one primary action per view" vs Colors "fill exactly one
  action per view".** No conflict in practice: both point the same way.
  Resolution: a single filled button, "Mark attendance". The bell and
  nav links stay neutral. Applied consistently.
- **Layout "hint at hidden content" vs the fixed bottom nav covering
  content at rest on mobile.** The fixed bar is stable chrome, not
  hidden content; the cue rule applies to the table, not the nav.
  Resolution: table gets the scroll/peek cue; nav gets
  `padding-block-end` on `.main` so no content is ever trapped under it.
- **Layout "plan for growth and clipping" (never a fixed height on a
  text box) vs the chart needing a fixed height for its bars.** The
  chart is a graphic, not a text box; its labels sit outside the fixed
  area. Resolution: `block-size: 12rem` on the chart container only;
  every text-bearing box uses `min-height` or none.
- No other conflicts between the three skills. Where the skills hand a
  topic to a sibling not assigned here (e.g. hit areas and focus
  behavior → better-accessibility, radius/shadows → better-ui), the
  default browser behavior was kept and nothing was invented.

## Self-check (headless Chromium, chrome-headless-shell)

Ran an injected measurement script against the built file (check copies
in /tmp only; the deliverable was not edited to pass the check).

1280×800 — 9/9 PASS:
- no horizontal scroll (scrollWidth 1265 ≤ 1280)
- all 4 stat cards fit (232/232 each)
- table fits its card without forcing page scroll
- no clipped headings/labels
- sidebar fully visible in viewport (top 64, bottom 800)

390×844 — 9/9 PASS after the one fix below:
- no horizontal scroll (scrollWidth 375 ≤ 390)
- all 4 stat cards fit (341/341 each)
- table scrolls inside its card (scrollWidth 480 > clientWidth 309,
  page scrollWidth still 375)
- no clipped headings/labels
- bottom nav fully visible (top 773, bottom 844)

Screenshots at both widths were eyeballed: no overlaps, no clipped
text, chart bars and pills render as designed. Live-browser look and
real-device feel are NOT verified (headless only).

## Defects found and fixed

1. **HIGH (task violation: clipped text) — school name truncated at
   390px.** First render showed "Greenwo…" in the header: the brand
   name shared one row with the search input and the bell, and its
   `text-overflow: ellipsis` cut it with no way back to the full name.
   Fix (layout principle "plan for growth and clipping", not a cosmetic
   hand-fix): at ≤ 56rem the header now wraps — brand + bell on row
   one, search full-width on row two. Re-checked: "Greenwood High"
   fully visible at 390px, all 9 headless checks still PASS.

## Defects left

None known. Two observations, both by design, not defects:
- At 390px the student table scrolls horizontally *inside its card*
  (480px content in a 309px viewport) — required by the task ("table
  fits or scrolls inside its card") and cued by the peeking next column.
- The chart bars for 93–97% look near-identical in height because they
  are drawn on a true 0–100% scale. Deliberate: no axis exaggeration.
- The fixed bottom nav covers the lower part of the viewport at rest on
  mobile; content scrolls beneath it and the page's bottom padding
  keeps the footer reachable. Standard fixed-chrome behavior.

## Verdict

No actionable layout, color or typography findings remain. Approve.
Verification not run: real browser, real device, 320px width, 200%
zoom, RTL mirror.
