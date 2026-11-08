# Skills actually read — Experiment 2, variant 19-better-interface-alone

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-interface/SKILL.md`
2. `/home/hatch/workspace/custom-skills/local-skills/skills/better-accessibility/SKILL.md`
3. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md`
4. `/home/hatch/workspace/custom-skills/local-skills/skills/better-writing/SKILL.md`
5. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`
6. `/home/hatch/workspace/custom-skills/local-skills/skills/better-colors/SKILL.md`
7. `/home/hatch/workspace/custom-skills/local-skills/skills/better-ui/SKILL.md`

Also read (house rules, not a skill): `~/workspace/custom-skills/local-skills/AGENTS.md`, as prompted by the runtime when touching that folder. No other skill files were read, and no other variant folder was read.

## Assigned skills not used

None. All 7 assigned skills were read in full before building and all 7 informed the build.

## How the skills were applied

- **better-interface (orchestration):** reviewed in the mandated order accessibility → layout → writing → typography → colors → UI, so foundational failures were fixed before polish; escalation triggers treated as blockers (focus ring, color-alone status, clipping at narrow width, reduced motion).
- **better-accessibility:** skip link as first focusable element; `<header>` / `<nav aria-label>` / `<main>` / `<footer>` landmarks; `:focus-visible` ring (browser default color token, 3px); native `<button>` for menu toggle and bell; `aria-expanded` synced on the toggle; icon-only bell has `aria-label="Notifications, 3 unread"`; search input has a visually-hidden `<label>`; table uses `<th scope>` row/column headers plus `<caption>`; chart has `role="img"` with a full text `aria-label`; live search announces via a stable `role="status"` region rendered before updates; status is never color alone (text badges "On track" / "Watch" / "At risk"); motion gated behind `prefers-reduced-motion: no-preference`; viewport meta has no `maximum-scale`/`user-scalable=no`; `scroll-padding-block-start` equals the sticky header height.
- **better-layout:** logical properties (`margin-inline-start`, `inset-inline`, `text-align: start`) throughout; spacing scale with group gaps ≥ 2× inner gaps; `minmax(0, 1fr)` tracks so long content cannot overflow a grid track; breakpoints from content (900px stats 4→2, 760px sidebar collapses to toggle + bottom nav); table scrolls inside its card (`overflow-x: auto` on `.table-wrap`, never the page); full-width controls inset from edges; `env(safe-area-inset-bottom)` on the bottom nav; DOM order matches visual order.
- **better-writing:** sentence case everywhere (one case policy); verb-first/ plain labels ("Search students…", "Skip to main content"); notices use the exact fixed wording; no "Oops", no exclamation marks, no "successfully"; search hint states the outcome ("Scroll sideways on small screens").
- **better-typography:** system font stack only (no external fonts); type scale 0.75–1.75rem with semantic roles; unitless line-heights (1.2 headings, 1.5 body); `font-variant-numeric: tabular-nums` on stat values, counts, table numbers and chart values; `text-wrap: balance` on headings, `pretty` on intro paragraph; search input `16px` so iOS does not zoom; no weight below 400 under 18px; font smoothing set once on the root; en dashes in "Mon–Fri" and "2025–26".
- **better-colors:** one neutral ramp + one accent ramp + only the status ramps rendered; primitives by hue, semantic tokens by role (`--color-text-primary`, `--color-accent-solid`, …) and components reference only semantic tokens; status hues kept ≥15° from the accent hue and always paired with a text label; body text #111827 on #f9fafb / #ffffff (well above 4.5:1), muted #6b7280 on white (4.83:1, passes AA for small text), accent text #1d4ed8 on #eff6ff; filled accent used for exactly one primary emphasis (the skip link); contrast values are computed from the declared token pairs, not measured in a browser (see UNVERIFIED).
- **better-ui:** outer radius = inner radius + padding; layered transparent shadows for elevation, borders kept on inputs/table cells/selected states, `border: 1px solid transparent` under shadowed cards for forced-colors mode; press scale exactly `0.96` over `150ms` `ease-out` on enabled buttons only; transitions name exact properties (never `all`); hover styling gated behind `@media (hover: hover)`; staggered entrance 100ms apart, opacity + 4px blur + 12px translateY over 300ms, load-only, reduced-motion gated; icons use `currentColor`/inherit text color.

## Self-check (code-read; no headless browser available in this VM)

Verified by parsing the file (Python HTMLParser: 0 tag errors, 0 unclosed tags) and by string checks:

- 12/12 students present with correct rolls, attendance and marks; 12 table rows.
- All fixed data present: 1,248 / 72 / 95.2% / 1,188 present / 60 absent / 84% / ₹25.2L of ₹30L; trend 94/96/93/97/95; all 3 notices; footer text exact.
- 0 external URLs, 0 external fonts/images, 0 `@import`.
- No `100vw`, no `outline: none`, no `user-scalable=no`, no `transition: all`.
- Inline JS parses (checked with `new Function`).

### 1280px

- Sidebar 232px + content in a max-width 1280px shell; stats 4 across (`repeat(4, minmax(0, 1fr))`); table min-width 560px fits inside the content column without scrolling. No horizontal scroll expected: no fixed widths wider than the viewport, `min-width: 0` on grid children.

### 390px

- Stats collapse to 2 across at ≤900px with value size reduced to 1.5rem at ≤420px; header wraps with search going full-width; sidebar becomes a toggle-controlled panel (2-column nav links); bottom nav fixed with `main` padded 72px clear of it; table scrolls inside its card via `.table-wrap`. No horizontal scroll expected at page level.

## Defects found and fixed

1. **Stats section used `aria-labelledby` pointing at a visually-hidden heading** — the referenced element was hidden, so the label could not be announced. Fixed: section now uses `aria-label="School overview"` and the hidden heading was removed.
2. **Bell button carried two redundant unread indicators** (a red dot plus a "3" count badge). Fixed: the dot was removed; the count badge alone carries the number, and the `aria-label` carries it for assistive technology.
3. **(Design-time) Status shown by color only in early draft** — resolved before writing by adding text badges ("On track" / "Watch" / "At risk") alongside the status colors.

## Defects left

None known from the code-read check.

## UNVERIFIED

- **No live render:** no Chromium/Playwright in this VM, so the page was never opened in a browser. The 1280px and 390px no-scroll / no-clip / no-overlap claims are from code inspection only and need a real browser check (or screenshots) to confirm.
- **Contrast ratios not measured in a browser:** values above are computed from the declared token pairs (e.g. #6b7280 on #ffffff = 4.83:1); rendered pairs including any compositing were not measured.
- **Keyboard/focus walk not performed:** focus order, visible focus ring at every stop, and the menu toggle / live-region announcements were not tested with a keyboard or screen reader.
- **Status thresholds are this variant's own judgement:** "Watch" below 90% attendance or below 80% marks, "At risk" below 89% attendance / 75% marks — the fixed data does not define thresholds; the badges add interpretation the task did not specify.
