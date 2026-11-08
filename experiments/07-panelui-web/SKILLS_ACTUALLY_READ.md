# Skills actually read — variant 07-panelui-web (PanelUI web)

Task file read first (not a skill): `~/workspace/skills-test/TASK.md`.

## Skill files read in full, in order

1. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md` — PanelUI Muse adapter (name: `panelui`). Read in full before building. Sets the rules used: pick web vs native first, colour is a role never a value, look before you build, lead with a Frame, label what a screen reader needs, semantic tokens.
2. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md` — PanelUI web skill (name: `panelui-web`, v0.15.0). Read in full. Rules applied: colour is a role, pick the role by meaning, give data its own text, lead with a Frame, pass `label` / `valueText`.
3. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md` — Component catalogue (79 web components). Read in full. Components chosen from it: Frame, Card, Stat, Kpi, Table, NavRail, Badge, Button, Input, BarChart, Avatar.
4. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md` — Frame (`Frame.Header/Title/Action/Panel/Row/Media/Content/Actions`, variants `default/band/inset/plain`). Used: muted band + title strip round an inner bordered card for the Attendance chart and the Notices panel. Notices use `Frame.Row` with `Frame.Media` / `Frame.Content` / `Frame.Actions`.
5. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/card.md` — Card (`Card.Header/Title/Description/Action/Content/Footer`, `Footer variant="panel"`). Used: one surface per stat card and for the student table card.
6. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/stat.md` — Stat (`Stat.Label/Value/Unit/Badge`, value size `lg`). Used: the four stat cards — label over a large value, unit after it, badge trailing.
7. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md` — Kpi (`Kpi.Title/Value/Trend/Progress`, `Kpi.Progress value/maxValue/color`). Used: `Kpi.Progress` bars under Attendance 95.2% and Fees 84% (progress to a target of 100). No `Kpi.Trend` was used — the task data gives no month-on-month change, and the skill says do not invent data.
8. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md` — Table (`Table.Header/Row/Head/Cell`, `align="end"` for numbers, `striped`, `framed`, `maxHeight` with pinned header, "a table wider than its container scrolls sideways inside its own box"). Used: student table, numbers right-aligned, sticky header, `max-height:340px`, scrolls inside its card.
9. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md` — NavRail (`NavRail.Header/Brand/Trigger/Group/Item/Badge/Separator/Footer`, `variant="tonal"`, `expandedWidth 15rem`, `collapsedWidth 3.5rem`, `aria-current="page"` on the active item, "folding is always the app's decision"). Used: desktop sidebar with Brand, Workspace group, active pill on Overview, badge counts (12 students, 3 notices), collapse trigger. On mobile the rail is hidden and a bottom nav is shown instead — the app's decision via media query, as the skill requires.
10. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/badge.md` — Badge (`variant` = state role, `dot` for a changing state). Used: role by meaning — success for good (attendance 95.2%, "On track"), info for neutral (Teachers, "Steady"), warning for attention (Fees 84%, "Watch", fee deadline), destructive for the bell dot only.
11. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/button.md` — Button (one `default` button per view, `outline`/`ghost` beside it, `type="button"` by default). Used: one default button ("Add student") plus an outline icon bell button.
12. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/input.md` — Input (`Input.Group` with `Input.Addon`, focus ring round the whole group). Used: header search as an `Input.Group` with a search addon and a `⌘K` addon; it live-filters the student table.
13. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md` — BarChart (`BarChart data/xDataKey`, `BarChart.Grid`, `BarChart.Bar dataKey/color`, `XAxis`, `YAxis`, `Header title/value/caption`, `Tooltip`; "the value axis always includes zero"). Used: Mon–Fri attendance (94/96/93/97/95) as CSS bars from a zero baseline, with grid, Y axis, X axis, header value (Friday 95%), and a hover/click readout as the Tooltip. Today (Fri, highest day) uses `color="success"`; the rest use `color="primary"`.

Also used, via the component docs above: Avatar (initials in a colour of their own, from `components.md` + its description — its own file was NOT read in full, so it is not claimed as a read skill file; the avatar is a plain initials circle, data from the task).

## Assigned skill NOT used

- **PanelUI native / Expo** (`~/workspace/ui-skills/PanelUI/skills/panelui/SKILL.md`) — NOT read, NOT used. Task assigns web only; the adapter and the web skill both forbid mixing native and web.
- **apparatus** and **logo-maker** (sibling PanelUI skills) — NOT read, NOT used.
- **panelui-web `setup.md` and `theming.md`** — NOT read in full. No React build step / no npm in this variant, so the shadcn-registry setup did not apply; theming was taken from the SKILL.md rules (semantic tokens, colour-as-role) instead.
- No other UI skill was read. No other variant folder was read.

## Adaptation note (PanelUI web → single-file HTML)

PanelUI web is React + Tailwind v4 on shadcn/ui tokens, with no build step here by task constraint. Each component was re-expressed in plain HTML/CSS keeping its part structure (`Frame.Header/Panel/Row`, `Card.Header/Content/Footer`, `Stat.Label/Value/Badge`, `Kpi.Progress`, `Table.Header/Body`, `NavRail.*`, `Badge`, `Button`, `Input.Group`) and its semantic token names (`--background`, `--foreground`, `--muted`, `--border`, `--primary`, plus the `success/info/warning/destructive` roles). No hex colour is used for a component's own colour — roles only. The BarChart is CSS bars (no external chart library, per TASK.md); the NavRail collapse is driven by the app (trigger + media query), as the skill requires.

## Self-check (1280px and 390px)

No Chromium/Chrome was available in this VM (`which chromium/google-chrome` found none), so no headless screenshot was possible. Check was: code-read against the layout rules + a structural Python check (all checks PASS) + `node --check` on the extracted JS (OK).

Structural check results (all PASS): no external http/font/image references; all 12 students, rolls, and fixed numbers present (1,248 / 72 / 95.2% / 84% / 1,188 present / 60 absent / ₹25.2L of ₹30L); all three notices; footer text; 12 table rows; chart values 94/96/93/97/95; table wrapper `overflow:auto` with `max-height:340px`; stats 1 column at ≤420px; bottom nav hidden on desktop, shown ≤768px; no fixed 1280px width anywhere.

### Defects found and fixed

1. **Chart bars overflowed the plot.** First version set bar height as a % of the flex column, which also holds the value label and day label, so the tallest bar (97%) would have exceeded the 180px plot. Fixed: bar height is now set in JS as `value × 1.4px` against the plot area (max 135.8px), so value + bar + day ≈ 172px < 180px.
2. **Stat cards at 390px.** A 2-column grid at 390px would have squeezed the 36px Stat values. Fixed: stats go to 1 column at ≤420px (2 columns ≤1020px, 4 columns at desktop).
3. **Table at 390px.** The 5-column table is wider than a 390px card. Per the Table skill ("a table wider than its container scrolls sideways inside its own box") it is kept at `min-width:560px` inside `.table-scroll` (`overflow:auto`), so it scrolls inside its card instead of pushing the page wide.
4. **Header at 390px.** Search was in the same row as the title and buttons. Fixed: the search `Input.Group` wraps to its own full-width row (`order:3; width:100%`) at ≤768px.

### Defects left

- None known from the code-read + structural check. Live rendering at 1280px and 390px is **UNVERIFIED** — no browser was available here, so horizontal scroll, clipping, and overlap were verified by code inspection only, not by a rendered screenshot.

## UNVERIFIED

- No live-browser look at 1280px or 390px (no Chromium in this VM).
- No real-device / mobile check; the bottom nav and single-column stats are code-only.
- No PanelUI React build was run (deliberately: single-file HTML, no npm, per TASK.md) — the adaptation is faithful to the component docs, but it is not the real `panelui-web` package rendering.
