# Variant 20 — accessibility-alone: Skills Actually Read

## Skills read in full (in order)

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-accessibility/SKILL.md`

That is the only skill file read in full. No other skill file was read, and no
other variant folder was read. The supporting files linked from that SKILL.md
(`semantics-and-aria.md`, `focus-and-keyboard.md`, `hit-areas.md`, `forms.md`,
`motion-and-zoom.md`, `screen-readers.md`) were NOT read; only the principles
stated in SKILL.md itself were applied.

## Assigned skill not used

None. The one assigned skill, Better Accessibility (`better-accessibility`),
was read in full before building and was used.

## What the skill changed in this build

- Skip link as the first focusable element; landmarks (`header`, `nav`
  `aria-label="Primary"`, one `main`, `footer`); one `<h1>`; coherent
  `<h2>` outline; `lang="en"`; viewport with no `maximum-scale` /
  `user-scalable=no` (WCAG 1.4.4 / 1.4.10).
- Native elements only: real `<button>`, `<a href>`, `<form role="search">`,
  real `<table>` with `<caption>` and `scope`; no `div onClick`, no positive
  `tabindex`, no `role="button"`.
- Visible `:focus-visible` ring (3px solid, 6.70:1 on white; yellow ring on the
  dark header/nav); `scroll-padding-top: 76px` so focused content is not
  hidden under the sticky header (2.4.11).
- Every control labelled: search `<label for>`, icon-only bell button with
  `aria-label="Notifications, 3 unread"`, decorative icons/bars
  `aria-hidden="true"`, status pills carry text + symbol + colour, never
  colour alone (1.4.1).
- Keyboard paths for every pointer interaction: collapsible mobile nav
  (`aria-expanded` / `aria-controls`), notifications popover (opens, moves
  focus inside, Escape closes and returns focus to the trigger), search
  filter (submit-validated, never a disabled submit).
- Stable `role="status"` live region rendered empty before any update;
  search announces "N of 12 students shown…". Chart is `role="img"` with a
  full text summary plus a real (visually hidden) data table for AT.
- Table scroll region is keyboard-focusable (`tabindex="0"`,
  `role="region"`, labelled) so it can be scrolled without a mouse (2.1.1).
- All interactive targets are at least 44×44px (WCAG 2.5.8 baseline is 24px).
- Motion is opt-in under `@media (prefers-reduced-motion: no-preference)`;
  under `reduce`, all transitions/animations are removed.
- Text contrast (computed, WCAG formula): ink/white 17.85:1, muted/white
  10.35:1, brand/white 8.72:1, white/header 14.69:1, focus/white 6.70:1,
  good pill 8.30:1, warn pill 7.79:1 — all above the 4.5:1 text threshold
  (1.4.3) and the 3:1 UI threshold (1.4.11).

## Self-check

### Method

Headless Chromium at `/opt/meta-chromium/chrome` would NOT start in this VM:
every invocation (including `--disable-webgl`) exits immediately with
`ERROR: chrome/app/chrome_main_delegate.cc:1283] Hatch WebGL domain policy is
required for browser startup` and produces no screenshot. No live render or
screenshot was obtained, and none is claimed. Self-check was therefore done
by code inspection plus scripted checks (Python content/contrast checks,
`node --check` on the extracted inline JS).

### Verified by inspection / scripted checks

- Data: 12/12 students, 12/12 rolls (8A-01–8A-12), 4/4 stats, Mon–Fri trend
  94/96/93/97/95, 3/3 notices, footer text exact, 0 external URLs.
- JS syntax: `node --check` PASS.
- No `maximum-scale` / `user-scalable=no`; no `outline: none` in code (the
  only occurrence is inside a comment); one `<h1>`; no `<img>` (no `alt`
  needed); `role="status"` present; `:focus-visible` present;
  `prefers-reduced-motion` present.
- 1280px: 4-column stats grid, sidebar nav, table `min-width: 560px` sits
  inside the scrollable `.table-wrap`, so no page-level horizontal scroll is
  expected.
- 390px: layout collapses to one column, nav becomes a toggle button,
  stats become 2 columns with smaller values, search form wraps full-width,
  notification panel is capped at `calc(100vw - 32px)`, table scrolls inside
  its card. No fixed-width element wider than the viewport outside a
  scroll container (1200px values are `max-width`, not `width`).

### Defects found and fixed

1. `main:focus { outline: none; }` was present in the first draft. Removed:
   the skill forbids removing the outline without a verified replacement,
   and the skip-link target should show its focus ring anyway.
2. First draft had the chart bars as the only trend representation. Fixed:
   added `role="img"` + full text summary on the chart and a real data
   table (visually hidden) so the trend is available to assistive
   technology, not only visually.
3. First draft had the student table scrolling only by pointer. Fixed: the
   `.table-wrap` is now `tabindex="0"` with `role="region"` and an
   accessible label, so keyboard users can reach and scroll it.

### Defects left

None known in code. See UNVERIFIED below: the render itself was never seen,
so a visual/layout defect at either width could still exist unnoticed.

## UNVERIFIED

- No live render or screenshot at 1280px or 390px: headless Chromium in
  this VM refuses to start (WebGL domain policy error, see Method above).
  No-horizontal-scroll, no-clipping and no-overlap at both widths are
  therefore argued from the CSS, not observed.
- No keyboard tab-through or screen-reader pass was run (no browser, no
  screen reader in this VM); focus order, accessible names and live-region
  announcements are as coded, not as observed in an accessibility tree.
- No automated accessibility audit (axe/Lighthouse) was run.
