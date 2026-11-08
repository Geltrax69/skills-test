# Skills Actually Read — experiments/55-full-ui-stack-r2

Second run of the 30 "full UI stack" combination. Built fresh from the skills below.
The 30 variant folder was NOT read and no variant file was copied.

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md`
3. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md`
4. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md`
5. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md`
6. `/home/hatch/workspace/custom-skills/local-skills/SKILL.md`
7. `/home/hatch/workspace/skills/skills/skills/better-accessibility/SKILL.md`
8. `/home/hatch/workspace/skills/skills/skills/better-layout/SKILL.md`
9. `/home/hatch/workspace/skills/skills/skills/better-writing/SKILL.md`
10. `/home/hatch/workspace/skills/skills/skills/better-typography/SKILL.md`
11. `/home/hatch/workspace/skills/skills/skills/better-colors/SKILL.md`
12. `/home/hatch/workspace/skills/skills/skills/better-ui/SKILL.md`
13. `/home/hatch/workspace/custom-skills/local-astryx/SKILL.md`
14. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md`
15. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md`
16. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md`

Also read in full (house rules, and PanelUI's "read the component's own file" rule):

- `/home/hatch/workspace/build-anything/ECC/AGENTS.md` (ECC house rules)
- `/home/hatch/workspace/custom-skills/local-skills/AGENTS.md`
- `/home/hatch/workspace/skills/skills/AGENTS.md`
- `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md`
- `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/stat.md`
- `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md`
- `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md`
- `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md`
- `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md`

## Assigned skills NOT used

- None of the 16 assigned skill files went unread. One referenced-but-not-assigned file
  was NOT read in full: `~/workspace/custom-skills/local-pstack/skills/poteto-mode/SKILL.md`
  (the local-pstack adapter names it as the router; the task's ordered list named only
  the feature playbook, which was read and followed instead).
- No Astryx component was rendered: Astryx contributed theme/anatomy/a11y thinking only
  (see Conflict 7/8). No npm install, no package, no external font/image anywhere.

## pstack feature playbook — how it was applied

- Steps 1–3: subsystem walk (single static file, no subsystem) and throughput checkpoint
  recorded here, not in a todo tool: blocking first step was reading all skills;
  independent workstreams: none (one file, shared writes serialize); shared mutable
  state: n/a, single file; smallest safe decomposition: one worker (me).
- Step 4 (delegate implementation): NOT done — this agent runs at depth 2/2 and cannot
  spawn; the work was done directly with the same review separation (read skills,
  build, verify, record).
- Step 5: verified on the matching surface (headless Chromium, see Defects).
- Step 6 (rebase/commits): NOT done — task forbids commit/push.
- Step 7 (interrogate): design not contested, skipped.
- Step 8 (opening a PR): not applicable, no commit/push allowed.

## Conflicts — 19 total, every one named with a winner, applied consistently

1. **Press scale** — Emil `scale(0.97)`, 160ms vs better-ui `scale(0.96)`, 150ms.
   Winner: **better-ui** (exact value, more specific polish skill). All pressable
   elements use 0.96 / 150ms.
2. **Easing** — Emil `cubic-bezier(0.23, 1, 0.32, 1)` vs better-ui
   `cubic-bezier(0.2, 0, 0, 1)`. Winner: **better-ui**, used for every transition.
3. **Stagger** — Emil 30–80ms between items vs better-ui 100ms with 4px blur and
   12px translateY over 300ms. Winner: **better-ui** values (but see Conflict 4).
4. **First-render entrance** — Emil staggers entrances vs better-ui "skip state
   animations on first render". Winner: **better-ui**. No entrance stagger on load;
   cards render static. Emil stagger is not used.
5. **Enter/exit durations** — Emil modals/drawers 200–500ms vs better-ui enter 300ms,
   exit 150ms. Winner: **better-ui** (300ms enter / 150ms exit, e.g. chart bars 300ms).
6. **Colour notation** — better-colors: oklch() is the best default for a new system,
   vs PanelUI/shadcn hex semantic tokens. Winner: **PanelUI hex** (component-system
   consistency; better-colors' own rule that a consistent hex system beats mixed
   notation). No oklch() anywhere in the page.
7. **Accent source** — Astryx theme-neutral accent vs PanelUI `primary`.
   Winner: **PanelUI primary** `#1d4ed8` (white text on it). Astryx supplied no
   rendered component or token value.
8. **Component system** — Astryx (174 components) vs PanelUI Web (79).
   Winner: **PanelUI Web**, per the task. Page anatomy follows PanelUI: NavRail
   (tonal active pill, 15rem / 3.5rem widths), Kpi (Title/Value/Progress/Footer),
   Stat.Value lg, Table (comfortable, striped, align end), Frame band, BarChart.
9. **Info hue** — PanelUI `info` is a blue-family role, within 15° of the blue
   primary accent; better-colors forbids a status hue within 15° of the accent.
   Winner: **better-colors**. Info is rendered teal `#0f766e` (hue ~175° vs ~220°).
10. **Stat value line-height** — better-typography headings 1.2–1.3 vs PanelUI Stat
    "value is set with no leading". Winner: **PanelUI, for Stat.Value only**
    (line-height 1, so the number's foot aligns with the progress bar/footer).
    All other text uses unitless 1.3 headings / 1.5 body.
11. **Search input size** — PanelUI Input default 14px vs better-typography "inputs
    at 16px on mobile" (iOS zoom). Winner: **better-typography**, input is 16px.
12. **Nav collapse driver** — PanelUI NavRail: "the rail never picks a width from
    the viewport itself" vs task (sidebar desktop, bottom nav mobile) and
    better-layout (breakpoints from content). Winner: **task**. A CSS media query
    at 860px drives sidebar → bottom nav + overlay drawer; PanelUI widths
    (15rem open) are kept as values.
13. **Chart animation** — PanelUI BarChart 700ms bar grow vs better-ui 300ms and
    Emil "UI under 300ms". Winner: **better-ui**, 300ms. Bars are CSS only per task,
    zero baseline per PanelUI BarChart, corner radius 4 on the growing end.
14. **Spacing step** — better-layout 16px default step vs PanelUI Frame `p-5` (20px).
    Winner: **better-layout**. Page grid, gaps and card padding are 16px throughout.
15. **Radius** — PanelUI/shadcn default radii vs better-ui concentric rule
    (outer = inner + padding). Winner: **better-ui**. Cards 16px outer / 12px inner,
    controls 10px; PanelUI BarChart corner radius 4 kept on bar tops.
16. **Focus ring** — PanelUI `ring-1` vs better-accessibility 2px solid ring at 3:1
    against adjacent colours. Winner: **better-accessibility** (`:focus-visible`,
    2px solid accent, 2px offset). No `outline: none` anywhere.
17. **Group label case** — PanelUI NavRail group label is small uppercase vs
    better-writing sentence-case default. Winner: **PanelUI uppercase**, justified
    by better-typography's uppercase-label rule (≤14px, tracked; used 11px / 0.09em).
18. **Coverage / TDD** — ECC 80% coverage mandatory + TDD-first vs pstack
    test-behaviour-not-implementation and the global harness ("80% is not a gate
    for a small UI demo"). Winner: **harness**. Behaviour checks only: static data
    audit + headless render at 1280/390. No test suite was written.
19. **File structure** — ECC "many small files, 200–400 lines" vs pstack
    subtract-before-you-add and the task's one-file constraint. Winner: **task**.
    One self-contained `index.html` (~25 KB), inline CSS+JS only.

## What was deliberately NOT invented

- No Kpi.Trend values: the task supplies no trend/change figures, so no trend
  percentages were rendered (PanelUI Trend needs a real `value`).
- No attendance categories: table dots are colour + the numeric % itself (the number
  is the redundant cue, per better-accessibility 1.4.1); no "Good/At risk" labels
  were invented.
- Notice badges (Event / Fees / Meeting) describe the notice's own subject only.

## Defects found and fixed (objective task violations only)

- **D1 — Search placeholder clipped at 390px.** First render showed "Try Aar…".
  Fix: below 860px the header wraps to two rows and the search takes the full
  second row; `--header-h` set to 113px in that breakpoint so the sticky nav and
  scroll-padding stay aligned. Re-verified: placeholder reads "Try Aarav Sharma"
  in full at 390px.
- **D2 — Header-height token 1px off.** Measured header height was 113px, token
  was 112px. Fix: token corrected to 113px.
- **D3 — System Chromium unusable.** `/opt/meta-chromium/chrome` refuses to start
  ("Hatch WebGL domain policy is required for browser startup"). Not a page defect.
  Verification used the Playwright headless shell instead
  (`~/.cache/ms-playwright/chromium_headless_shell-1248/.../chrome-headless-shell`).

## Self-check results (measured)

- Static audit (Python): all fixed data present (0 missing strings, incl. all 12
  students, all rolls, all % values, all 3 notices, footer verbatim); 13 table
  rows (1 head + 12 body); 5 chart bars at 94/96/93/97/95%; HTML tags balanced,
  no unclosed/stray tags; zero external URLs, zero external fonts/images/scripts.
- Headless render: `documentElement.scrollWidth` 1265 at 1280px viewport and 375
  at 390px viewport — **no horizontal scroll at either width**. Stat cards do not
  overflow (4-across → 2-across ≤1100px → 1-across ≤480px). Table scrolls inside
  its card at 390px (min-width 560px inside `overflow-x: auto`), which the task
  allows. Screenshots eyeballed at 1280px and 390px, full page: no clipped text
  (after D1 fix), no overlaps.
- Not numerically measured: rendered contrast ratios (pairs were chosen dark-on-light
  and white-on-`#1d4ed8`/`#15803d`/`#b91c1c` by construction, not measured with a tool).
- Not verified: feel on a real device / real browser, touch targets by hand,
  screen-reader announcement order, 200% zoom, RTL mirror. Headless proves
  correctness, never feel.
- Left as designed (not defects): chart bars 93–97% look near-equal in height —
  honest zero baseline per PanelUI BarChart, labelled "Bars start at 0%";
  the table's in-card horizontal scroll at 390px (task-permitted); emoji/text
  glyphs used as icons (no icon library and no external assets allowed), all
  decorative glyphs `aria-hidden="true"` with real accessible names on controls.
