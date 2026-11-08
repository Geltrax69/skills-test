# Experiment 41 — PanelUI Web vs OpenSourceUI (conflict test)

## Skills actually read (in full, in order)

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — ECC first, then pstack; routing table (PanelUI web / OpenSourceUI rows).
2. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md` — PanelUI Muse adapter; pick web vs native first; colour-is-a-role; lead with a frame; semantic tokens.
3. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md` — panelui-web 0.15.0; colour roles; compound parts; lead with a frame; `label`/`valueText`.
4. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md` — 79 web component docs index.
5. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md` — Kpi parts (Title/Value/Trend/Chart/Progress/Group), Trend sign/colour rules, Progress props.
6. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md` — NavRail parts, tonal vs solid, expandedWidth `15rem` / collapsedWidth `3.5rem`, collapse is the app's decision, `aria-current="page"`.
7. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md` — Table density/striped/framed, `align="end"` numbers, "a table wider than its container scrolls sideways inside its own box".
8. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md` — BarChart parts, "the value axis always includes zero", `cornerRadius` 4, `color` names a theme role.
9. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md` — Frame variants (`band`/`inset`/`plain`), muted band + title strip + bordered inner card.
10. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/theming.md` — five roles; `--success: oklch(0.696 0.17 162.48)`, `--warning: oklch(0.769 0.188 70.08)`, `--info: oklch(0.623 0.214 259.815)`; "Do not override a component's colour with a palette class".
11. `/home/hatch/workspace/custom-skills/local-opensourceui/SKILL.md` — OpenSourceUI Muse adapter; no npm package, copy-paste; read upstream SKILL.md + design.md + implementation.md.
12. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/SKILL.md` — upstream workflow; neutral stage, Instrument Serif + Geist, base + `md:` only, border-only focus.
13. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/design.md` — full design system (tokens, radii, elevation, component rules, don'ts).
14. `/home/hatch/workspace/ui-skills/opensourceui/AGENTS.md` — agent-kit layout (house rules for the opensourceui folder; read before further work there).
15. `/home/hatch/workspace/ui-skills/opensourceui/components/table/team-members-table.tsx` — TeamMembersTable source (read in full): rounded-xl border, header row, avatar+name cell, status dot, hover `neutral-50`, `md:`-hidden Role column, `overflow-x-auto` table.
16. `/home/hatch/workspace/ui-skills/opensourceui/components/notifications/calendar-reminder-notification-banner.tsx` — CalendarReminder banner source (read in full): dismiss with 260 ms exit (`EXIT_MS`), red app tile, app name + time label, restore trigger button.

Also consulted (not counted as full reads): `~/workspace/ui-skills/opensourceui/skills/opensource-ui/references/catalog.md` (grep for table/widget/notification slugs only).

## Assigned skills NOT used

- None. Both assigned libraries were used: PanelUI Web (NavRail, Kpi, Table, BarChart, Frame structure) and OpenSourceUI (design tokens, TeamMembersTable pattern, CalendarReminder banner pattern, input/button rules).
- Not used, by design: PanelUI native/Expo skill (task says web only), OpenSourceUI `implementation.md` (no React/Next.js target — single-file HTML adaptation instead; no `cn.ts`/`clsx` copy possible or needed).

## Components used, by library

PanelUI Web: NavRail (sidebar, tonal variant, collapse trigger), Kpi + Kpi.Group (4 stat cards, Kpi.Trend, Kpi.Progress for fees), Table (align="end" numbers, pinned header semantics, in-card horizontal scroll), BarChart (CSS bars, axis includes zero, cornerRadius 4, peak day in success role).
OpenSourceUI: neutral stage tokens, Instrument Serif display + Geist UI type pairing, TeamMembersTable pattern (avatar initials, name+roll, status dot, hover tint, rounded-xl border), CalendarReminder banner pattern (dismissible notices with coloured app tile + time label, 260 ms exit, restore), input (rounded-lg, h-10, border-only focus, `ring-0`), primary button (ink-soft fill, rounded-md 6px).

## Conflicts recorded — 14

| # | Conflict | PanelUI says | OpenSourceUI says | Winner | Applied |
|---|---|---|---|---|---|
| 1 | Page surface tokens | shadcn tokens `--background` / `--card` / `--muted` | paper `#ffffff`, paper-tint `#fafafa`, hairline `#e5e5e5` | **OpenSourceUI** | All surfaces paper white; tint only for recessed fills/hover |
| 2 | Primary colour | `--primary` theme role | ink-soft `#262626` (ink `#171717` for text) | **OpenSourceUI** | Buttons, chart bars, active states in ink-soft; no separate primary hue |
| 3 | Status colours | 5 semantic roles, oklch tokens | sparse rose / cyan / amber / teal / emerald, only for state | **PanelUI** (roles), with one remap | Attendance/status use success/warning/destructive roles; PanelUI `--info` oklch hue 259.8 sits in the indigo family OpenSourceUI bans, so info is remapped to OpenSourceUI cyan `#0891b2` |
| 4 | Corner radii | shadcn default ~10px radius | `rounded-md` 6px buttons, `rounded-lg` 8px fields/small cards, `rounded-2xl` 16px large surfaces | **OpenSourceUI** | 6/8/16px scale throughout; only exception is BarChart `cornerRadius: 4` (PanelUI chart rule, conflict 13) |
| 5 | Card container | Frame `band`: muted band + title strip + bordered inner card | flat paper card, `p-4`, hairline border, no shadow | **OpenSourceUI** | Every panel is a flat paper card; PanelUI Frame band rejected everywhere (Frame structure — header title + action — survives only as layout) |
| 6 | Typography | default sans only (shadcn) | Instrument Serif display + Geist UI; serif for brand/headlines only, never dense controls | **OpenSourceUI** | School name + section titles in Georgia/Instrument Serif fallback; all controls/body in system sans (Geist fallback) |
| 7 | Focus style | `--ring` token ring | border colour only, `ring-0`, never a coloured ring | **OpenSourceUI** | Inputs: border darkens to ink on focus, no ring; a visible 2px ink keyboard outline is kept for accessibility (not a coloured ring) |
| 8 | Shadows | shadcn subtle card/border shadows | flat by default; shadow only on a primary marketing CTA | **OpenSourceUI** | No shadows anywhere on the page |
| 9 | Internal padding | Frame.Panel examples use `p-5` | `p-4` (16px) common internal padding | **OpenSourceUI** | All cards/panels `p-4` |
| 10 | Navigation pattern | NavRail: sliding active pill, tonal/solid, collapse trigger, badge becomes dot when folded | docs nav: `text-sm` sans, idle `#737373`, active ink, 2px ink bar as active indicator | **PanelUI** | NavRail structure wins (task requires collapsible sidebar); active row is a tonal pill + short ink accent bar, not OpenSourceUI's 2px bar alone |
| 11 | Table pattern | Table: comfortable density, `align="end"` numbers, pinned header, optional striped | TeamMembersTable: rounded-xl border, avatar cell, status dot, hover `#fafafa`, hide a column below `md:` | **OpenSourceUI** (chrome) + **PanelUI** (numbers) | OSUI chrome wins (avatar initials, status dot, hover tint, rounded border); PanelUI `align="end"` numbers kept — the two do not contradict, so both apply |
| 12 | Buttons | Button weights per action importance | primary = ink-soft `#262626` fill, white text, `rounded-md` 6px, hover deepens to ink | **OpenSourceUI** | Export button is ink-soft, 6px radius |
| 13 | Breakpoints | no breakpoint rule | base + `md:` only — never `sm:` | **OpenSourceUI** | Only one breakpoint used (`md:` 768px, plus a 1024px step for the 4-up KPI grid — no `sm:` anywhere) |
| 14 | Notices pattern | Alert (in-page card) / Toast (auto-dismiss) | CalendarReminder banner: coloured app tile, app name + time label, dismiss with 260 ms exit, restore trigger | **OpenSourceUI** | Notices are three dismissible banner rows with rose/amber/cyan tiles and time labels; dismiss animates 260 ms and a restore link appears |

Non-conflict noted: charting — OpenSourceUI has no bar chart (widgets only), so PanelUI BarChart wins by default; adapted to CSS bars per the task (bars start at zero, `cornerRadius` 4, peak day Thu 97% in the success role).

## Self-check (headless Chromium, temp probe copies in /tmp — deliverable untouched)

- Data: all 12 students with exact name/roll/attendance/marks verified by script; all fixed figures present (1,248 / 72 / 95.2% / 1,188 present / 60 absent / 84% / ₹25.2L of ₹30L); trend 94/96/93/97/95; all three notices; footer text present (via `&mdash;` entity). 0 external URLs, 0 `<img>`, 0 `<link>`, no external fonts.
- 1280px: `scrollWidth 1265`, no page horizontal overflow; KPI grid 4 columns; table fits its card (`scrollWidth == clientWidth`).
- 390px: `scrollWidth 375`, no page horizontal overflow; KPI grid 1 column; table scrolls **inside** its card (`table-scroll` right edge 358 < 390, `scrollWidth 448 > clientWidth 341`) — allowed by the task and by PanelUI Table's own rule.
- No JS errors observed in either run.

## Defects found and fixed

1. First probe at 390px rendered at 500px (headless Chrome minimum window width) — test-harness artefact, not a page defect; re-ran with the headless-shell binary, which honoured 390px.
2. None in the page itself: no missing/wrong data, no broken HTML found. No hand-fixes were applied to improve the look beyond the assigned skills.

## Defects left

- At 390px the table's Status column sits outside the visible card area until the user scrolls the table horizontally (by design — table scrolls inside its card; the task permits this). No other defect left.
- Unverified (out of scope for this variant): live-browser look and feel, touch behaviour of the bottom nav, screen-reader pass.
