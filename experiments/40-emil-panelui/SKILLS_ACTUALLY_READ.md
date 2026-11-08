# Skills Actually Read — Experiment 40 (emil-panelui)

Variant: Emil design-eng + PanelUI web. Single self-contained `index.html`
(PanelUI web components adapted to plain HTML/CSS/JS; no package installed,
no npm, no build step, no external images or fonts).

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
   — ECC first, then pstack. Routing used: UI/frontend → read the assigned
   UI skills in full before building; verify before reporting; headless
   verification proves correctness, never feel.
2. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md`
   — Emil Kowalski design engineering (read in full).
3. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md`
   — PanelUI Muse adapter (read in full). Platform choice per adapter:
   PanelUI **web**, not native/Expo.
4. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md`
   — panelui-web 0.15.0 (read in full).
5. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md`
   — web component index, 79 components (read in full).

Component files read in full (before using each component, per
panelui-web rule 2):

6. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md`
7. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md`
8. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md`
9. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md`
10. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md`
11. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/stat.md`

## Assigned skills NOT used

- None. Every assigned skill file (global-harness, emil, local-panelui,
  panelui-web SKILL.md + components.md) was read in full and applied.
- Not read / not applicable: the PanelUI **native** skill
  (`skills/panelui/SKILL.md`), `apparatus`, and `logo-maker` — the task
  specifies PanelUI web. No pstack playbook file was read; the harness
  routing was followed at the level of this one-file build (plan → build →
  verify), and no ECC skill file was needed beyond the harness entry.

## How each skill was applied

- **PanelUI web** — structure follows the components, adapted to HTML:
  `NavRail` (Header/Brand/Trigger, Group, Item, Badge 3 on Notices,
  Footer; `expandedWidth` 15rem, `collapsedWidth` 3.5rem,
  `variant="tonal"` quiet tint + accent bar, `aria-current="page"` on the
  active row); `Kpi`/`Kpi.Group` for the four stat cards (number first,
  `Kpi.Progress` for Attendance 95.2% and Fees 84%, `Kpi.Footer` quiet
  lines carrying the given sub-figures); `Frame` (`variant="band"` for the
  chart, default tray for the table and notices; muted band + title strip
  + bordered inner card); `Table` (comfortable density, `align="end"` on
  the Attendance/Marks columns, pinned header, scrolls inside its own
  box); `BarChart` (CSS bars only, per task); `Badge` (role-tinted:
  success/info/warning/primary); `Stat` informed the value/label
  stacking inside the KPI cards. Colour is a role, never a value:
  Students `primary`, Teachers `info`, Attendance `success`,
  Fees `warning` (see Conflicts). No trend numbers were invented — the
  KPI cards carry no `Kpi.Trend`, because no change figures were given.
  The weekly average 95.0% shown under the chart is arithmetic on the
  given Mon–Fri values (94+96+93+97+95)/5.
- **Emil** — custom easings `--ease-out: cubic-bezier(0.23,1,0.32,1)`
  (no `ease-in` anywhere on UI); all UI transitions 160–200ms
  (under the 300ms rule), transform/opacity only; buttons get
  `transform: scale(0.97)` on `:active`; stat cards stagger in at
  0/50/100/150ms (Emil allows 30–80ms gaps; 50ms used); hover gated
  behind `@media (hover:hover) and (pointer:fine)`; `prefers-reduced-motion`
  collapses all motion; bar-chart entrance uses no keyframe grow (the
  task's CSS-bars chart is static, so no animation was added to it).
- **Global harness** — plan (read skills in order) → build one file →
  verify with a static/data check (see Defects). No commit, no push,
  nothing edited outside `experiments/40-emil-panelui/`.

## Conflicts resolved

1. **PanelUI BarChart default animation 700ms vs Emil UI motion < 300ms.**
   BarChart's `animationDuration` defaults to 700ms (bars grow in).
   Emil caps UI animation at 300ms and the task's chart is CSS bars only.
   Resolution: Emil wins for this static chart — no grow-in animation at
   all; bars render at final height. The 700ms value is not used anywhere.
2. **Kpi.Chart min–max scaling vs BarChart zero-baseline.**
   `Kpi.Chart` scales between the lowest and highest value (no axis);
   `BarChart` insists the value axis always includes zero, because a bar
   cropped at the bottom misstates its value. Resolution: the trend chart
   follows BarChart (it is a bar chart): heights are value/100
   (94/96/93/97/95%), so the 93–97% differences read honestly small.
   Friday's bar is `success` (today), the others `primary`.
3. **PanelUI role colours vs Emil "match motion to mood".**
   PanelUI says pick the colour role by meaning and do not give every
   component the same colour. Emil says a professional dashboard should
   be crisp and fast. Resolution: restrained roles — near-black `primary`
   for structure/navigation/chart, one semantic role per KPI, no
   decorative colour. Fees is `warning` (84% collected, 16% outstanding
   needs attention), not `success`, even though 84% is a good figure.
4. **NavRail "never picks a width from the viewport" vs the task's
   mobile bottom nav.** NavRail folds only on app decision, never from
   the viewport. Resolution: the rail itself never changes from the
   viewport (it folds only via its Trigger button); the switch to a
   fixed bottom nav below 760px is a separate CSS layout decision for
   the mobile shell, not a rail collapse.
5. **shadcn radius vs PanelUI `cornerRadius: 4`.** BarChart's bar corner
   radius is 4px; the shadcn token radius is 0.5rem (8px) for cards.
   Resolution: bars use 4px top corners (BarChart value), cards/inputs
   use the 0.5rem token — each component keeps its own documented value.

## Defects found and fixed (objective task violations only)

1. **Invented notice sub-lines (found while re-reading my own code).**
   The first draft added "bring your project models", "Second-term fees
   due · ₹4.8L outstanding", "10:00 AM onwards, main hall" under the
   three notices. None of that is in the fixed data. Fixed: removed all
   three sub-lines; notices now carry exactly the three given strings
   (plus role Badges Event/Fees/Meeting, which are PanelUI labelling,
   not data).
2. No other task violations found. All fixed data verified present by
   script (see below).

## Self-check (static; no live browser available in this sandbox)

- Script-checked `index.html`: all 12 students with exact name/roll/
  attendance/marks; Greenwood High, Class 8-A, 2025–26; 1,248 / 72 /
  95.2% (1,188 present, 60 absent) / 84% (₹25.2L of ₹30L); trend bars
  94/96/93/97/95; all three notices; footer
  "Fictional demo data — UI Forge Experiment 1". Zero external URLs,
  zero external images/fonts, one inline `<script>` only.
- HTML parser: no unclosed or stray tags.
- Layout reasoning (not a render): stat grid is `repeat(4,minmax(0,1fr))`
  → 2 columns ≤900px with `min-width:0` and ellipsis on values, so cards
  cannot overflow at 390px; table has `min-width:560px` inside an
  `overflow-x:auto` wrapper, so it scrolls inside its card, never the
  page; sidebar is `display:none` ≤760px and replaced by a fixed bottom
  nav; header wraps with search going full-width on mobile; the only
  `100vw` use is a `max-width:calc(100vw - 32px)` on the bell popover.

## Defects left / unverified

- **No live render check.** No Chromium/Playwright is installed in this
  sandbox, so 1280px/390px rendering, real horizontal-scroll behaviour,
  and visual feel are **UNVERIFIED** — the checks above are static +
  layout reasoning only. A live-browser look is still needed.
- **Status badges are interpretation, not data.** "Excellent / Good /
  Watch" on student rows is a PanelUI `Badge` role assignment from the
  given percentages (≥95% attendance → Excellent, 90–94% → Good,
  &lt;90% → Watch); the underlying percentages are exact, the labels are
  presentation. Left as-is as the skill's intended use of Badge.
- No other known defects. No hand-polish was applied after building
  beyond the objective fix in (1) above.
