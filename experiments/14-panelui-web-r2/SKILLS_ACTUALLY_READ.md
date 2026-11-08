# Skills Actually Read — Variant 14-panelui-web-r2 (REPEAT of Experiment 1 variant 07)

Repeat build. No other variant folder was opened, read, or copied. Only
`TASK.md` and the PanelUI skill files listed below were used.

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md` — Muse adapter for PanelUI (adapter rules, colour-is-a-role, read the owning sub-skill + components.md + each component's own file before using it, never mix native and web).
2. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md` — panelui-web 0.15.0 (React web, shadcn tokens, lead with a Frame, label what a screen reader needs).
3. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md` — component catalogue (79 web components; picked NavRail, Kpi, BarChart, Table, Frame, Badge, Avatar, Input, Button, Alert).
4. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/theming.md` — five roles (primary/info/success/warning/destructive) and the three tokens a stock theme lacks (`--success`, `--warning`, `--info` oklch values, used verbatim).
5. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md` — NavRail: `expandedWidth` 15rem / `collapsedWidth` 3.5rem, `variant="tonal"` (default), active pill + `aria-current="page"`, folding is the app's decision, one level of hierarchy only.
6. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md` — Kpi: Title / Value / Trend (`value`, sign picks colour, `goodDirection`) / Progress (`value`, `maxValue`, `color` role) / Chart sparkline (`data` oldest first, no axis).
7. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/stat.md` — Stat: Label / Value (`size` sm/md/lg) / Unit / Badge. Read in full; **not applied** — stat.md itself says to use Kpi for a metric with a trend, sparkline, or progress target, so the four stat cards are Kpi.
8. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md` — BarChart: Grid + Bar (`dataKey`, `color` role) + XAxis + YAxis; value axis always includes zero (bars scaled 0–100, not cropped at 90); Tooltip reads a band on hover/tap.
9. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md` — Table: `density="comfortable"`, `striped`, numbers `align="end"` on head and cells, table wider than its container scrolls sideways inside its own box.
10. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/card.md` — Card: Header / Title / Description / Action / Content / Footer. Read in full; **not applied** — frame.md says to use Frame for a list of rows under a title, so sections are Frames.
11. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md` — Frame: `variant="band"` (muted band + title strip + bordered panel), Header / Title / Action / Panel / Row / Media / Content / Actions.
12. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/badge.md` — Badge: `variant` by meaning (`success`/`info`/`warning`/`destructive`), `dot` for a changing state.
13. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/avatar.md` — Avatar: initials on a tint picked from the name, `name` is what a screen reader says.
14. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/input.md` — Input: `Input.Group` (box round input + addons, focus ring round all of it), `aria-label` where no visible label.
15. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/button.md` — Button: one `default` button per view, `ghost` for toolbar/row actions, `type="button"` default.
16. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/alert.md` — Alert: `tone` (warning/destructive get `role="alert"`, others `role="status"`), `variant="plain"` for a box inside a panel.

## Assigned skills not used

- `panelui` (native/Expo) — explicitly excluded by the task; web only.
- `apparatus`, `logo-maker` — other PanelUI sub-skills, not relevant to this dashboard.
- `panelui-web/setup.md` — not read; no npm/React install in this single-file adaptation (no `panelui-web` package, no shadcn CLI), so its install steps do not apply. Components were adapted to plain HTML/CSS/JS following the component docs' props and rules.

## How the skills were applied

- Tokens: shadcn surface tokens (`--background`, `--card`, `--muted`, `--border`, `--ring`, `--primary`) + theming.md's `--success`/`--warning`/`--info` oklch values. Colour only by role: success = attendance ≥95% / Thursday-highest bar / present trend; warning = fees 84% / fee-deadline notice / attendance <90%; info = teachers / 90–94% attendance / event notices; primary = default / chart bars.
- NavRail: 15rem open / 3.5rem collapsed via trigger (`aria-expanded`, "Expand/Collapse navigation"); collapses to icons ≤1024px; drawer + bottom nav ≤640px.
- Kpi cards: Students 1,248 (footer "12 in Class 8-A"), Teachers 72, Attendance 95.2% (Trend "▲ 1,188 present · 60 absent", Progress 95.2, sparkline of the Mon–Fri trend), Fees 84% (Progress 84, "₹25.2L of ₹30L"). No invented trend percentages.
- BarChart: Mon–Fri 94/96/93/97/95, domain 0–100 with Grid + YAxis (0/25/50/75/100), Friday highlighted in success, hover/focus/tap readout ("Thursday was highest at 97%").
- Table: 12 students exactly as specified, striped, comfortable density, attendance/marks right-aligned with tabular numerals, Avatar initials, attendance meter bars coloured by role.
- Notices: Frame rows with Alert tones — fee deadline `role="alert"` (warning), the other two `role="status"` (info) — each with a Badge by meaning.
- Footer: exact text "Fictional demo data — UI Forge Experiment 1".

## Self-check (1280px and 390px)

No Chromium/Playwright in this VM, so the check was by code inspection plus
automated static verification (Python: all 12 students/rolls/names, 4 stats,
5 trend values, 3 notices, exact footer, 0 external URLs/fonts/images;
`node --check` on the extracted inline JS: OK; HTML parses OK).

Defects found and fixed during the check:

1. Chart axis: first draft considered scaling bars from 90 to make differences visible — rejected per bar-chart.md (value axis always includes zero); bars are 0–100 with the Y-axis labelled 0–100.
2. Stat vs Kpi: first plan used Stat for all four cards — changed to Kpi after reading stat.md ("for a metric with a trend, a sparkline or progress towards a target, use Kpi"); Stat was read but not applied.
3. Table at 390px: a full-width 4-column table cannot fit 390px — kept `min-width:560px` inside an `overflow-x:auto` wrapper so it scrolls inside its card, per table.md ("a table wider than its container scrolls sideways inside its own box").
4. Stat-card overflow at 390px: 4-column grid would clip — grid is 2 columns ≤1024px and stays 2 columns at 390px with the value reduced to 1.45rem; "95.2%" + unit fits.
5. Topbar at 390px: search `⌘K` kbd and the page subtitle are hidden ≤640px so hamburger + title + search + bell fit without overflow.

Defects / risks left:

- `color-mix()` is used for tints (active nav pill, badge backgrounds, striped rows). It needs a modern browser; no fallback tint is provided, so on a very old browser those surfaces fall back to un-tinted backgrounds (layout unaffected).
- No live-browser render or screenshot was possible in this VM — visual feel, exact pixel overflow, and the collapsed-rail pill position are UNVERIFIED (see below).

## UNVERIFIED

- No live-browser look at 1280px or 390px (no Chromium/Playwright in the VM): no horizontal scroll, no clipped text, no overlap confirmed only by code inspection + static checks, not by rendering.
- No real React/shadcn build: this is a hand adaptation of panelui-web components to single-file HTML, not the `panelui-web` package or shadcn CLI output.
- Animation smoothness, reduced-motion behaviour, and screen-reader announcements not tested beyond the ARIA attributes written in the markup.
