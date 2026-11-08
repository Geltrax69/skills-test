# Skills Actually Read — Variant 23-uiuxpromax-alone

Assigned skill: **ui-ux-pro-max** (used). No other skill was assigned, read, or used.
No other variant folder was read.

## Files read in full, in order

1. `/home/hatch/workspace/ui-skills/ui-ux-pro-max-skill/.claude/skills/ui-ux-pro-max/SKILL.md`
2. `/home/hatch/workspace/ui-skills/ui-ux-pro-max-skill/.claude/skills/ui-ux-pro-max/references/quick-reference.md`
   (part of the assigned skill; read on demand per SKILL.md § "Rule Categories by Priority")
3. `/home/hatch/workspace/ui-skills/ui-ux-pro-max-skill/.claude/skills/ui-ux-pro-max/references/pro-rules.md`
   (part of the assigned skill; read for the pre-delivery checklist per SKILL.md § "Before Delivering App UI")

## Skill-tool searches run (same skill's own search script, `scripts/search.py`)

- `"school student dashboard analytics" --design-system -p "Greenwood High"` →
  Style **Data-Dense Dashboard**; palette Primary `#1E40AF`, Secondary `#3B82F6`,
  Accent `#D97706`, Background `#F8FAFC`, Foreground `#1E3A8A`, Card `#FFFFFF`,
  Muted `#E9EEF6`, Muted-fg `#475569`, Border `#DBEAFE`; typography Fira Sans / Fira Code.
  Applied as semantic CSS tokens; Fira fonts NOT loaded (TASK forbids external fonts —
  system stack + `ui-monospace` for tabular figures instead).
- `"attendance trend bar chart dashboard" --domain chart` → bar/line guidance; direct value
  labels, legend, text summary, keyboard-focusable bars (TASK requires CSS bars, so the
  chart is CSS bars with the chart-search accessibility guidance applied).
- `"sortable data table keyboard" --domain ux` → table overflow guidance: horizontal-scroll
  wrapper on mobile, sortable columns with `aria-sort`.

## Assigned skill not used

None. ui-ux-pro-max was read in full before building and applied throughout.

## What the skill changed in this build

- Priority order from the skill's table: accessibility first (skip link, visible focus,
  `aria-label` on icon-only bell/avatar, `aria-sort` on sortable headers, status pills
  carry text + dot, never colour alone, chart has `role="img"` summary + per-bar labels,
  `prefers-reduced-motion` respected), then touch (44px targets, 8px+ gaps, bottom nav
  ≤5 items with icon + label), then layout (mobile-first, sidebar ≥1024px / bottom nav
  below, no horizontal scroll, 4/8px spacing scale, semantic colour tokens, tabular
  numerals for data, transform-only bar/progress animation, 150–300ms transitions).
- No emojis as icons (inline SVG only, one consistent 2px-stroke set); no external
  images/fonts; single primary action per view.

## Self-check (1280px and 390px)

Headless Chromium at `/opt/meta-chromium/chrome` **could not run in this sandbox**:
every invocation exits with
`ERROR:chrome/app/chrome_main_delegate.cc:1283] Hatch WebGL domain policy is required
for browser startup`, and its README states normal startup requires the image-owned
WebGL policy supplied by browser-daemon. No screenshot and no live `scrollWidth`
measurement were possible — **the render is UNVERIFIED**. The check below is a
code-level audit only, plus a JS syntax check (`node --check` on the extracted
inline script: PASS) and a data sweep (all 12 students, all 4 stats, all 5 trend
values, all 3 notices, footer text present; zero `http`/external references).

Defects found and fixed during the audit:

1. **Chart bars could overflow the 220px chart area.** `.bar-col` was a flex column
   whose children (value label + bar at 93–97% of column height + day label) summed
   to more than the column height. Fixed: bars now sit in a `.bar-track` flex child
   (`flex:1; min-height:0`) so the percentage height resolves against the remaining
   track space only.
2. **Invalid CSS variable in skip link.** `border-radius` used undefined
   `--space-sm`. Fixed to `var(--space-2)`.
3. **`aria-sort` was on the sort `<button>`, not the `<th>`.** Moved to the `<th>`
   elements and updated the JS to set it there.

Checked by code inspection (not live-rendered):

- 1280px: sidebar 232px + content max-width 1120px; stats 4-across via
  `repeat(4,minmax(0,1fr))`; table `min-width:560px` inside `.table-scroll`
  (`overflow-x:auto`) so the table scrolls inside its card, never the page.
- 390px: sidebar hidden, bottom nav (4 items, 56px tall, safe-area padding);
  stats 2-across (`minmax(0,1fr)`); header wraps with full-width search;
  `box-sizing:border-box` everywhere, `min-width:0` on flex/grid children,
  `overflow-wrap:anywhere` on stat values — no page-level horizontal scroll expected.
- No clipped text expected: names wrap in the student cell, pills/terms use
  `white-space:nowrap` only on short tokens.

## Left / UNVERIFIED

- **Live render at 1280px and 390px NOT verified** — headless Chromium blocked by
  the Hatch WebGL domain policy (see above). No screenshot exists; the
  no-horizontal-scroll / no-overflow claims rest on code inspection only.
- Live behaviour not exercised in a browser: search filter, column sorting,
  bell scroll-to-notices, nav active-state sync (JS syntax-checked only).
- Contrast ratios computed from the design-system tokens (`#475569` on white,
  `#1E3A8A` on `#F8FAFC`, white on `#1E40AF`) but not measured with a tool.
- Colour-blind / screen-reader pass not run; only the markup hooks are in place.
