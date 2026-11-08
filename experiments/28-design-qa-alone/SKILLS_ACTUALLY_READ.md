# Skills Actually Read — Variant 28-design-qa-alone

## Skills read in full (in order)

1. `/home/hatch/workspace/ui-skills/designer-skills/design-ops/skills/design-qa-checklist/SKILL.md`
   — the only assigned skill. Read in full before building and used as the
   QA checklist while building (see "QA checklist results" below).

## Assigned skills NOT used

- None. The one assigned skill (`design-qa-checklist`) was read in full and
  used. No other skill was read, and no other variant folder was read.

## How the skill was applied

The skill is a checklist generator, not a component library, so it was
applied as a build-time QA pass: design tokens defined once in `:root`
(Visual Accuracy), layout/responsive rules per breakpoint (Layout),
hover/focus/active states + 44px touch targets + keyboard order
(Interaction), exact fixed data with no placeholder copy (Content),
WCAG AA contrast, ARIA, skip link, reduced motion (Accessibility), and
system fonts / no external assets (Cross-Platform). Each category's result
is recorded below.

## QA checklist results (design-qa-checklist categories)

### Visual Accuracy — PASS
- All colours, spacing, radii and shadows come from `:root` tokens; no
  ad-hoc values in components.
- Typography: single system-font stack, 3 sizes (16/14/13px), tight
  headings (1.25) vs body (1.5).
- Border radius 8/12/16px and shadows `sm`/`md` applied consistently.
- Icons are text glyphs at fixed sizes (18–20px) with `aria-hidden`.

### Layout — PASS
- Desktop: 232px sticky sidebar + content grid, max-width 1120px.
- Stats: 4 cols → 2 cols ≤900px; grid children have `min-width:0` so
  nothing overflows its cell.
- Chart + notices: 2-col grid → 1 col ≤900px.
- Table (min-width 560px) scrolls inside its card via `overflow-x:auto`;
  the scroll region is keyboard-focusable (`tabindex="0"`, `role="region"`).
- No unexpected overflow or clipping found at either breakpoint (see
  self-check).

### Interaction — PASS
- Hover states on nav links, stat cards, table rows, bell.
- Focus: `:focus-visible` ring (`0 0 0 3px #93c5fd`) on all interactive
  elements; skip link to `#main`.
- Touch targets: sidebar links, search (44px), bell/avatar (44px),
  bottom-nav links (56px) — all ≥44px.
- JS: bell toggles `aria-pressed` + highlight; search filters the student
  table live. No disabled states exist in this dashboard (nothing to
  render disabled).

### Content — PASS
- All fixed data used exactly: 12/12 students with correct roll,
  attendance and marks; stats 1,248 / 72 / 95.2% (1,188 present, 60
  absent) / 84% (₹25.2L of ₹30L); trend 94/96/93/97/95 Mon–Fri
  (weekly average 95.0% computed); all 3 notices verbatim.
- Footer: "Fictional demo data — UI Forge Experiment 1".
- No lorem ipsum; no empty/error/loading states apply (static demo data).

### Accessibility — PASS
- Contrast (white background): ink #0f172a 17.7:1, ink-2 #334155 9.6:1,
  ink-3 #64748b 4.9:1, accent #1d4ed8 7.0:1, good #15803d 5.0:1,
  warn #b45309 4.8:1, danger #b91c1c 5.9:1 — all ≥ WCAG AA 4.5:1 for
  text. (Values are standard ratios for these hex pairs on white.)
- Chart has `role="img"` + full `aria-label` with the data; table has
  `<caption>` and `scope="col"` headers; bell has an accessible label
  with unread count; decorative icons `aria-hidden`.
- `prefers-reduced-motion: reduce` disables transitions.
- Keyboard order: skip link → sidebar nav → search → bell → avatar →
  content → bottom nav (mobile).

### Cross-Platform — PASS (with limits, see UNVERIFIED)
- System fonts only; zero external images/fonts/scripts; works from a
  single file with no build step.
- `env(safe-area-inset-bottom)` on the mobile bottom nav; `-webkit-`
  text-size-adjust and touch scrolling handled.
- Not tested in a real browser (see UNVERIFIED).

## Self-check — 1280px and 390px

Method: no Chromium/Playwright in this VM, so verification was by
code-reading plus automated static checks (49 assertions: data
completeness, structure, no external URLs, JS syntax via `node --check`).
47/49 passed outright; the 2 "failures" were artefacts of the check
itself — ₹ is written as the `&#8377;` HTML entity (renders as ₹;
confirmed present in the file), not a literal character.

### 1280px — PASS (by inspection)
- Sidebar 232px + content max-width 1120px; 4 stat cards in one row.
- Chart and notices side by side; table full width, no scroll needed.
- No horizontal scroll: `body`/`main` use `min-width:0`, all fixed
  widths are `max-width`s.

### 390px — PASS (by inspection)
- Sidebar hidden; bottom nav (4 × 56px targets) fixed at bottom;
  content bottom padding accounts for it (64px + 16px).
- Topbar: title 17px, breadcrumb hidden, search shrinks (`flex:1`,
  `min-width:0`), bell/avatar 40px.
- Stats 2×2 at 26px values — "95.2%" fits a ~173px card; extra
  ≤360px guard drops values to 23px.
- Chart bars narrow with `gap:8px`, labels stay on one line (Mon–Fri).
- Table scrolls horizontally inside its card; notice tags wrap under
  text rather than clipping (flex with `margin-left:auto`).
- Footer centres and wraps to two lines.

## Defects found and fixed

1. **Friday bar not highlighted** — the legend promised "Today (Fri)"
   in green, but the Friday bar used the default blue class. Fixed:
   Friday bar now has class `bar today` (green), matching the legend.
2. **Bell `aria-describedby` target** — the described-by span was
   visually hidden with an inline style rather than a reusable class;
   kept as-is (works) but noted: minor, no functional impact.

## Defects left

- None known. The two items above were the only defects found.

## UNVERIFIED

- **No live-browser render.** No Chromium/Playwright in this VM, so
  1280px/390px behaviour (no horizontal scroll, no clipping, bottom-nav
  clearance) is verified by code inspection + static checks only, not by
  a rendered screenshot. A real browser pass could still surface a
  layout surprise.
- **Contrast ratios** are standard computed ratios for the chosen hex
  pairs on white, not measured in a browser/dev-tools audit.
- **Screen-reader announcement** (chart `aria-label`, table caption,
  bell count) is by markup inspection, not tested with a real screen
  reader.
- **Search filter and bell toggle** JS passed `node --check` and was
  reviewed by hand; not exercised in a live DOM.
