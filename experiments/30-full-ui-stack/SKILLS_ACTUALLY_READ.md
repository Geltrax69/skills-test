# Variant 30-full-ui-stack — Skills Actually Read

"Everything" combo, applied in the assigned order. Every file below was read
in full before building. No other skill and no other variant folder was read.

## Skills read, in order

1. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/SKILL.md`
2. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md`
   (the Feature playbook named by poteto-mode; found at
   `skills/poteto-mode/playbooks/feature.md`, not `playbooks/feature.md`)
3. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md`
4. `/home/hatch/workspace/build-anything/ECC/AGENTS.md`
   (house rules for that folder; read on the runtime's instruction, not a skill)
5. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md`
6. `/home/hatch/workspace/custom-skills/local-skills/skills/better-interface/SKILL.md`
7. `/home/hatch/workspace/custom-skills/local-skills/AGENTS.md`
   (house rules for that folder; read on the runtime's instruction, not a skill)
8. `/home/hatch/workspace/custom-skills/local-skills/skills/better-accessibility/SKILL.md`
9. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md`
10. `/home/hatch/workspace/custom-skills/local-skills/skills/better-writing/SKILL.md`
11. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`
12. `/home/hatch/workspace/custom-skills/local-skills/skills/better-colors/SKILL.md`
13. `/home/hatch/workspace/custom-skills/local-skills/skills/better-ui/SKILL.md`
14. `/home/hatch/workspace/custom-skills/local-astryx/SKILL.md`
15. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md`
16. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md`
17. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md`
    (read because panelui-web's own SKILL.md requires it before using a component)
18. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md`
19. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/stat.md`
20. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md`
21. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md`
22. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md`
23. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md`
24. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/badge.md`

## Assigned skills not used

None. All six assigned skills were read in full and applied. Notes on scope:

- **Astryx CLI not run.** The adapter says to start every Astryx task with the
  CLI bootstrap (`astryx component --list`, `astryx docs tokens`, templates).
  No React build is allowed here and no npm install was run, so there was no
  consumer project to run it in. Astryx's contribution is therefore its
  theme-neutral ramp values, its component anatomy (Button 4.5:1 text, Card
  composition, KPI/stat structure) and its theming model, adapted to plain
  HTML/CSS — not a verified Astryx component render. The Astryx CLI docs were
  not consulted; no component props were taken from the CLI.
- **PanelUI native skill not read.** The task assigned PanelUI web only, and
  the adapter forbids mixing native and web. Only `panelui-web` was used.
- **pstack subagent delegation not done.** The Feature playbook step 4 requires
  delegating code-writing to a subagent. This run is itself a subagent at depth
  2 with no spawn ability, so the playbook's own escape applies: the delegate
  role was satisfied by owning the diff directly with the same review
  separation (build, then an independent headless verification pass).
- **better-interface `review-format.md` not read.** The orchestrator skill
  points at it for its report format; this variant's deliverable is the page,
  not a review report, so the format file was not needed.

## How each skill shaped the page

- **pstack (poteto-mode + Feature playbook).** Data shape named first
  (`DASHBOARD` object: school, totals, attendance trend, fees, 12 students,
  3 notices) and rendered by small component functions. Throughput
  checkpoint: blocking first steps were the skill reads; independent
  workstreams n/a (single file, one writer); shared mutable state n/a
  (single file); smallest safe decomposition is one worker because the
  artifact is one file. Principles applied: model-the-domain, prove-it-works
  (headless CDP check, not a proxy), laziness (no abstraction beyond the
  render functions), subtract-before-you-add (no chart library, no framework).
- **ECC frontend-patterns.** Composition and compound-component anatomy
  translated to vanilla JS: `renderStats` / `renderChart` / `renderNotices` /
  `renderStudents` as the component boundaries, one state object, a debounced
  search (200 ms) that filters into a new array and never mutates the source.
  Error/empty state is a real branch (empty table state with a clear action),
  not a silent failure. ECC's React hooks, Context, Framer Motion and
  virtualization were not applicable to a single static file and were not used.
- **emil-design-eng.** Custom easings, press feedback on every button,
  transitions (never keyframes) for interactive state, staged entrance,
  `prefers-reduced-motion` handling, hover gated behind
  `@media (hover: hover) and (pointer: fine)`, 16 px search input (no iOS zoom).
- **better-interface + six domain skills.** Drove the audit order
  (accessibility first) and the fixes: skip link, native `<button>`/`<table>`/
  `<nav>` landmarks, visible `:focus-visible` ring, accessible names on the
  icon-only bell and nav items, `aria-current="page"` on the single active
  pill, `role="progressbar"` on the fees bar, `role="img"` + `aria-label` on
  the chart, live count text on search, logical properties (`margin-inline-*`,
  `inset-inline-*`) for mirroring, semantic type scale in `rem` with unitless
  line-heights and `tabular-nums` on changing numbers, sentence case
  everywhere, verb-first button labels ("Clear search", "Collapse
  navigation"), empty state that names the query and offers an exit.
- **Astryx.** Theme-neutral ramp values mapped onto PanelUI's shadcn semantic
  token names (one token system, hex notation). Component anatomy followed:
  KPI (Title / Value / Trend / sparkline / Progress / Footer), Stat
  (Label / Value+Unit / Badge), Card/Frame composition, Button accessibility
  (4.5:1 text in every state).
- **PanelUI web.** Direct anatomy of the components actually used, from their
  own doc files: `Kpi` (with `Kpi.Trend` sign/colour rules and `Kpi.Progress`
  towards a target), `Stat`, `Table` (native table, `align="end"` numbers,
  striped, scrolls inside its own box), `BarChart` (value axis includes zero,
  bars grow from the baseline, corner radius 4, best day in the success role),
  `NavRail` (tonal variant, active pill + accent bar, collapse trigger with
  `aria-expanded`, folded to a bottom nav on mobile), `Frame` (muted band with
  title strip round an inner bordered card), `Badge` (colour by meaning:
  info / warning / success). Colour is a role, never a value: components take
  `primary | info | success | warning | destructive` only.

## Conflicts and overhead, and how each was resolved

1. **Press scale: emil 0.97 vs better-ui 0.96.** better-ui wins: its values are
   declared exact, and 0.97 already sits inside emil's own 0.95–0.98 range.
   All pressable elements scale to **0.96 over 150 ms, ease-out**, disabled
   elements never scale.
2. **Stagger: emil 30–80 ms vs better-ui 100 ms with opacity + 4 px blur +
   12 px translateY over 300 ms.** better-ui wins for the one staged entrance
   (the four stat cards on first load, 100 ms apart). Emil's shorter stagger
   was not used anywhere; routine content (table rows, notices) is not
   staggered at all, per better-ui's "never stagger routine interactions".
3. **Easing curve: emil `cubic-bezier(0.23, 1, 0.32, 1)` vs better-ui
   `cubic-bezier(0.2, 0, 0, 1)`.** Split by use: better-ui's curve is the
   single interactive-transition easing; emil's drawer curve
   `(0.32, 0.72, 0, 1)` is kept only where emil names it, for drawer-style
   motion. In practice the sidebar collapse uses the shared 200 ms ease-out;
   the drawer curve is defined as a token and documented, not silently dropped.
4. **Token systems: Astryx theme-neutral vs PanelUI shadcn semantic tokens.**
   Both are CSS custom-property systems. PanelUI's semantic names
   (`background`, `primary`, `muted`, `destructive`, `border`) are the base
   because the rendered components are PanelUI's; Astryx's neutral ramp
   supplies the values. One system, one notation (hex throughout, per
   better-colors: a consistent hex system beats mixed notation).
5. **Colour notation: better-colors prefers `oklch()` for a new system, but
   also says to reuse the project's established notation.** This file has no
   prior system, so hex was chosen deliberately: it keeps every contrast pair
   computable with a one-line script, and no second notation is mixed in.
6. **BarChart value axis vs the 93–97 % data range.** PanelUI requires the
   value axis to include zero ("a bar cropped at the bottom misstates its
   value"), so bar heights are the values themselves (94 % of plot height,
   etc.), not a zoomed 90–100 % scale. The bars look nearly equal in height;
   that is the honest reading of the data, not a defect.
7. **Astryx "use components the Astryx way" vs the single-file constraint.**
   Real Astryx components need React 19 + npm. Resolved by adapting anatomy
   and tokens to plain HTML/CSS and recording the adaptation honestly above,
   rather than pretending a React render happened.
8. **ECC "test-driven, 80%+ coverage" vs a static page.** No test runner fits
   a single HTML file. Resolved with the pstack "prove it works" rule
   instead: a real headless-browser check (CDP, below) that asserts the
   rendered DOM, plus a static data audit. Coverage is not claimed.
9. **better-ui concentric radii vs PanelUI Frame band.** Frame outer radius
   16 px, inner panel 12 px, band inset 4 px: 12 + 4 = 16, so the rule holds
   exactly.
10. **Overhead.** Six skills plus seven component docs is a lot of reading
    for one page; the cost showed up as real conflicts (1–5 above) that each
    needed a recorded decision. The benefit is that every contested value
    (press scale, stagger, easing, tokens, radii) has one named owner instead
    of a guess.

## Self-check (headless Chrome via CDP, Chromium 1248)

Rendered the page in headless Chrome and measured the live DOM at 1280 px
and 390 px, plus a static audit of the file. Screenshots were eyeballed at
both widths.

| Check | 1280 px | 390 px |
| --- | --- | --- |
| Document horizontal scroll (`scrollWidth > innerWidth`) | No (1265 ≤ 1280) | No (390 = 390) |
| Stat cards rendered | 4 | 4, stacked, 358 px wide |
| Student rows rendered | 12 | 12 |
| Trend bars rendered | 5 (94/96/93/97/95) | 5 |
| Notices rendered | 3 | 3 |
| Elements overflowing the viewport | None | None at document level |
| Console errors | 0 | 0 |
| Search "priya" | 1 row, "1 of 12 students" | 1 row |
| Search "zzz" (no match) | 0 rows, empty state shown | 0 rows, empty state shown |
| Clear search | Back to 12 rows | Back to 12 rows |

Static audit: 0 external URLs, 0 `<link>`/`<img>`/`@import`/font references,
12 students in the data model, 3 notices, footer text exact, viewport meta
present, skip link, `:focus-visible`, `prefers-reduced-motion` guard present.
Contrast measured with a script before writing: text #17181c on white 17.74:1,
muted #5b606b on white 6.31:1, accent #2b4fd8 on white 6.54:1 and on its tint
5.59:1, success #146c43 on white 6.45:1, warning #7a5200 on white 6.92:1,
destructive #b3261e on white 6.54:1, white on accent 6.54:1. Every pair passes
4.5:1.

## Defects found and fixed

1. **390 px: search field squeezed to ~150 px** between the title and the
   bell, truncating its placeholder ("Search stude…"). Found in the 390 px
   screenshot. Fixed by letting the topbar wrap at ≤480 px so the search
   takes a full second row; re-screenshot confirmed the full placeholder is
   visible.
2. **Sparkline bars in the attendance KPI rendered as near-identical blocks.**
   Not a defect: values are 93–97 %, so near-equal heights are correct.
   Left as-is (see conflict 6).

## Defects left

- None known. The 390 px probe lists table internals (TABLE, THEAD, TR, TD)
  extending past the viewport, but that is the table scrolling inside its
  own card — explicitly allowed by TASK.md ("table fits or scrolls inside
  its card") — and the document itself does not scroll horizontally.

## UNVERIFIED

- **Real-device feel.** Headless Chrome proves layout, counts, search and
  zero console errors. It does not prove how the press scale, stagger or
  bottom nav feel on a phone, or how the page renders in Safari/Firefox.
  Needs a live-browser / real-device look.
- **Astryx component fidelity.** No real Astryx component was rendered
  (no React build); the adaptation is anatomy + tokens only, so Astryx's
  documented accessibility behaviour (e.g. Button 4.5:1 in every state) is
  met by the measured pairs above, not by an Astryx render.
- **PanelUI component fidelity.** Same: PanelUI components were adapted to
  plain HTML/CSS from their doc files, not imported from `panelui-web`.
  Keyboard behaviour claimed by the docs (e.g. BarChart arrow-key band
  reading) was not reproduced; the chart is a `role="img"` with a full
  text alternative instead.
- **Screen-reader pass.** Landmarks, names, roles and the chart alternative
  were checked in code only; no screen reader was run.
- **200 % zoom / 320 px.** Checked at 390 px and 1280 px only; 320 px and
  200 % zoom were not measured.
