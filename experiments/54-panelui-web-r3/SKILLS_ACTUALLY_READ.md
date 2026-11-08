# Skills actually read — 54-panelui-web-r3 (third run of the 07 combination: local-panelui + panelui-web)

Built fresh from the skills. The 07 and 14 variant folders were NOT read and no variant file was copied.

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — global harness entry (ECC first, then pstack; routing table: React Native/web components → local-panelui).
2. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md` — PanelUI Muse adapter (pick native vs web first; read owning sub-skill in full; colour is a role, never a value; lead with a frame; label what a screen reader needs; semantic tokens).
3. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md` — panelui-web 0.15.0 (React web on shadcn/ui tokens, Tailwind v4; colour is a role; pick the role by meaning; give data its own text; lead with a frame; compound parts; label what a screen reader needs).
4. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md` — full 79-component index; components chosen from it before writing anything custom.

Component files read in full before use (per panelui-web SKILL.md rule 2):

5. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md`
6. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/stat.md`
7. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md`
8. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/card.md`
9. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md`
10. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md`
11. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md`
12. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/badge.md`
13. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/input.md`

## Assigned skills NOT used

- None. Every assigned skill was read and used. (The native `panelui` skill was deliberately NOT read/used: the task specifies PanelUI WEB, and the adapter forbids mixing native and web.)
- Not read (not assigned, not needed for a single-file adaptation): `panelui-web/setup.md`, `theming.md`, and the remaining ~70 component files — no package was installed and no npm/shadcn CLI was run, per the task constraints.

## How the PanelUI web components were adapted (single-file HTML, no React/npm)

- **Frame (variant="band")** — every section leads with a frame: a muted band with an uppercase title + trailing action, round an inner bordered card. Used for the KPI overview, the attendance trend, and the notices panel.
- **Kpi / Kpi.Group** — the four stat cards are a `Kpi.Group` (rules between metrics): `Kpi.Title`, `Kpi.Value`, `Kpi.Trend` (signed, colour by good direction: success for attendance, warning for fees), `Kpi.Chart` (sparkline under Attendance), `Kpi.Progress` (84% fees bar with `role="progressbar"`). Trend captions and footers use the task's own data (1,188 present · 60 absent; ₹25.2L of ₹30L).
- **Stat** — the attendance strip in the notices frame (1,188 present / 60 absent) follows the Stat shape: label over a large figure.
- **NavRail (variant="tonal", expandedWidth 15rem, collapsedWidth 3.5rem)** — desktop sidebar with Brand, Group label "School", active item carrying `aria-current="page"` plus the tonal tint and short accent bar, `NavRail.Badge` counts (12 students, 84% fees, 3 notices), `NavRail.Trigger` with "Collapse/Expand navigation" labels and `aria-expanded`. Folding is app-driven (button), never viewport-driven, per the skill. On mobile the rail is replaced by a fixed bottom nav (same destinations, same active item).
- **BarChart** — the Mon–Fri trend is CSS bars only, per the task, but follows the BarChart rule: the value axis always includes zero, so bar heights are value/100 of the plot (94/96/93/97/95%). Clicking a band updates the readout below the chart (the Tooltip behaviour). Friday is highlighted in the success role as "today".
- **Table (density comfortable, striped)** — student table with `Table.Caption`, pinned header (`position:sticky`), numbers `align="end"` with tabular numerals, attendance meter bars coloured by role (success ≥90%, warning below). Per the Table doc, a table wider than its container scrolls sideways inside its own box (`.table-wrap { overflow-x:auto }`).
- **Card** — the students section is a Card (Header with Title + Description + Action badge, Content holding the table, Footer `variant="panel"` with the "Showing 12 of 12" line).
- **Badge** — variants picked by meaning only: `success` (On track, 95.2% present), `warning` (Watch, Due soon, fees), `info` (Event, Students count), `destructive` (bell count, 3 notices), `default`/`outline` for neutral kinds.
- **Input.Group** — header search is one box round the search addon, the input, and the ⌘K addon, with the focus ring round the whole group. It live-filters the student table by name or roll.
- **Colour is a role, never a value** — hex appears only in the `:root` semantic-token definitions (the shadcn token layer: `--primary`, `--success`, `--warning`, `--destructive`, `--info`, `--muted`, …). Every component colours itself with `var(--role)`, chosen by meaning.

## Conflicts resolved

1. **PanelUI is a React/shadcn system; the task requires one self-contained HTML file with no npm.** Resolution: adapted the components' structure, parts, props and rules into plain HTML/CSS/JS (same part names in class names and comments, e.g. `kpi__title`, `frame__panel`, `nav-rail__item`) instead of importing `panelui-web`. No package installed, no CLI run.
2. **BarChart's zero-based axis vs. making 93–97% differences visible.** The BarChart doc states the value axis always includes zero because a bar cropped at the bottom misstates its value. Resolution: kept the zero baseline (bars 93–97% tall, visually near-equal) and put the exact percentages as labels above each bar plus a click-to-read readout, rather than exaggerating the differences with a truncated axis.
3. **NavRail never picks a width from the viewport vs. the task's mobile bottom nav.** Resolution: the rail itself is only folded by its Trigger (app decision); the mobile switch to a bottom nav is a separate layout rule at ≤760px, not the rail choosing its own width.
4. **Frame.Title truncates long row titles to one line vs. the task's "no clipped text".** Resolution: notice row titles are allowed to wrap (no truncation on `.row-content p`); only the deliberately short KPI footers were truncated — see Defects.

## Defects found and fixed (objective task violations only)

1. **KPI footers clipped at 390px (found by headless render + screenshot).** "Across Greenwoo…" and "1,188 present · 60 a…" were ellipsised by `.kpi__foot { white-space:nowrap; overflow:hidden }` in the 2-column mobile grid. Fixed by letting `.kpi__foot` and `.kpi__trend` wrap below 760px. Re-verified by screenshot: both footers now read in full.
2. **No other data defects.** Scripted check confirmed all fixed data present: school/Class/year, 1,248 / 72 / 95.2% / 1,188 / 60 / 84% / ₹25.2L of ₹30L, all 12 students with correct rolls/attendance/marks, trend 94/96/93/97/95, all three notices, footer text. Zero external URLs, images, fonts, or imports.

## Defects left

- **Table caption at 390px is only fully readable after horizontal scrolling inside its card.** The caption lives inside the scrolling `.table-wrap`, so at 390px its right-hand words sit beyond the visible edge until the user scrolls the table sideways. This is PanelUI Table's documented behaviour ("a table wider than its container scrolls sideways inside its own box"), not a page-level overflow — the page itself has no horizontal scroll — so it was left as-is rather than hand-fixing the layout beyond what the skill produces. The caption text itself is not lost.
- **Bar heights look near-identical (93–97%).** Correct per the BarChart zero-baseline rule above; left intentionally.

## Self-check (headless Chromium, Playwright browser binaries)

- 1280px: `documentElement.scrollWidth = 1265` < 1280 — no horizontal scroll. Screenshot eyeballed: sidebar, header, 4 stat cards, chart + notices, table header all clean, no overlap.
- 390px (headless-shell binary; full Chromium enforces a 500px minimum window): `scrollWidth = 375` < 390 — no horizontal scroll. Screenshot eyeballed at top/middle/bottom: 2-column stat cards, chart, notices, table scrolling inside its card, bottom nav fixed, footer visible. Stat cards do not overflow; table fits by scrolling inside its card; no clipped text except the caption note above.
- Live-browser look / real-device feel: UNVERIFIED (headless only), as with all variants.
