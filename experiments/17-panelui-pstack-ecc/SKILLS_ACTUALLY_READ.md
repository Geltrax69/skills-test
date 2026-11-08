# Skills actually read — variant 17-panelui-pstack-ecc

## Skills read in full, in order

1. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/SKILL.md`
2. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md`
3. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md`
4. `/home/hatch/workspace/build-anything/ECC/AGENTS.md` — house rules for the ECC
   reference clone, read on the runtime's instruction before further work there.
   (Not a skill; listed so the read is on the record.)
5. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md`
6. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md`
7. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md`
   — the panelui-web skill itself instructs reading this before writing any UI.
8. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md`
9. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md`
10. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md`
11. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md`
12. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md`
13. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/badge.md`

No other skill was read. No other variant folder was read.

## Assigned skills not used

None. All three assigned skills were read in full and applied. The PanelUI
native/Expo skill was deliberately not read or used, per the task: web only.

## What each skill changed

- **pstack (poteto-mode + Feature playbook).** Data shape named first
  (`STUDENTS`, `TREND`, `NOTICES` as one typed model, rendered, never
  hand-written twice). Throughput checkpoint: blocking first steps were the
  skill reads above; independent workstreams n/a (single file, no fan-out);
  shared mutable state n/a (one file, no shared writes); smallest safe
  decomposition was one worker owning the single file. Principles applied:
  Model the Domain (filter/sort return new arrays, no mutation), Prove It
  Works (static verification script, see below), Laziness Protocol (no
  invented features, no dark-mode toggle, no extra pages).
  Deviations from the playbook, forced by the task: step 4 delegates
  code-writing to a subagent, but this run cannot spawn one
  (depth 2, `can_spawn=no`), so I owned the diff directly with the same
  review separation (write, then verify with an independent check script);
  steps 6 and 8 (small ordered commits, Opening a PR) do not apply because
  the task forbids pushing and editing outside the variant folder.
  The `how`/`architect` steps were folded into the design below: a single
  self-contained file admits no subsystem to explore in parallel.
- **ECC frontend-patterns.** Composition (Card/Frame/Kpi/NavRail as
  composable structures), Context+Reducer spirit (one view state derived
  from the model, never scattered booleans), debounce hook pattern for the
  search input (150 ms), accessibility patterns (keyboard navigation on the
  rail and the chart, `aria-current`, `aria-expanded`, `aria-sort`,
  `scope="col"`, table caption, focus management on the bell), error
  handling at the boundary (search input rendered via `textContent`, never
  `innerHTML` with user data; the only `innerHTML` use is fixed icon
  strings from the component data), immutability (filter/sort copy first).
  ECC's TDD/coverage and commit rules do not apply to a single static file;
  the check script is the verification substitute.
- **PanelUI web.** shadcn semantic tokens as CSS custom properties
  (`background`, `primary`, `muted`, `border`, `success`, `warning`,
  `destructive`, `info`, `chart-1`) with a `prefers-color-scheme: dark`
  token set, one colour system across themes. Colour is a role, never a
  value: attendance/marks badges use `success` (≥95/≥90), `info` (90–94),
  `warning` (<90) by meaning. Lead with a frame: every widget sits in a
  Frame (muted band, title strip, inner bordered card); the chart uses
  `band` framing. Components adapted from their own files: `Kpi` (Title,
  Value, Chart sparkline, Progress, Footer), `Kpi.Group` horizontal,
  `NavRail` (tonal variant, 15rem / 3.5rem, active pill, trigger with
  `aria-expanded`, badge count that becomes a dot when folded, arrow-key
  navigation), `BarChart` (value axis includes zero, grid, Y axis, tooltip
  readout outside the plot, keyboard bands with Arrow/Home/End/Escape),
  `Table` (numbers `align="end"`, pinned header, scrolls inside its own
  box), `Badge` (role variants with dot). Data's own text throughout: no
  sample text from the component docs was reused.

## Conflicts resolved

- **PanelUI `Kpi.Trend` vs TASK.md data.** `Kpi.Trend` needs a change value
  (e.g. "+6.4% this month"). TASK.md supplies no prior-period figure for
  any metric. Resolved by omitting `Trend` entirely and using only
  derivable parts: sparkline from the given Mon–Fri trend, progress bar
  (84% of the given ₹30L target), and footers built from given numbers
  (1,188 present · 60 absent; 12 in Class 8-A). No delta was invented.
- **PanelUI `NavRail` never picks a width from the viewport vs TASK.md
  mobile requirement.** The rail itself is viewport-agnostic (app-driven
  collapse, per the skill); the mobile bottom nav is a separate element
  shown by a media query, not the rail changing itself. Both honoured.
- **ECC "many small files, 200–400 lines" vs TASK.md single-file
  constraint.** The task wins: one self-contained `index.html`.
  Composition is kept inside the file instead.
- **pstack "delegate implementation" vs no-subagent run.** See above:
  direct ownership with a separate verification pass instead of a delegate.

## Self-check (1280px and 390px)

No Chromium or Playwright exists in this VM (`which chromium` empty,
`python3 -c "import playwright"` fails), so no live render or screenshot
was possible. Verification was a Node static check
(`/tmp/check17.mjs`, plus a scoped re-check `/tmp/check17b.mjs`) run
against the finished file, plus a code read for overflow risks. Results:

- 42/42 static checks passed: no external URLs/fonts/CDN; all 12 students,
  all rolls, all fixed numbers (1,248 / 72 / 95.2% / 84% / ₹25.2L / ₹30L /
  1,188 / 60 absent), all 3 notices, footer verbatim, sections in required
  order inside `<main>`, inline JS parses and executes against a stub DOM,
  390px breakpoints present, table scrolls inside its card, bottom nav
  present, `border-box` set, reduced-motion handled, no invented
  `+/-` trend deltas.
- One check initially failed ("sections in required order") and was a
  check-script bug, not a page bug: it matched the string "Notices" in the
  sidebar navigation, which precedes the main content in the document.
  Re-scoped to `<main>` only: PASS.

### Defects found and fixed

1. **Section-order check false positive** (above). Fixed the checker, not
   the page. No page change needed.
2. **KPI group border seam at the 2-column breakpoint.** At ≤1020px the
   grid becomes 2 columns; the third KPI kept its left border against the
   grid edge. Fixed: `.kpi:nth-child(3){border-left:0}` and top borders
   for rows 2+. Same treatment at the 1-column breakpoint (≤480px).
3. **Chart Y-axis alignment.** The axis labels sit beside a 180px plot
   plus a ~22px x-axis strip; added `padding-bottom:22px` so the 0 label
   aligns with the baseline instead of the x-axis labels.
4. **Table min-width vs 390px.** The table needs 560px for four columns;
   left unconstrained it would force horizontal page scroll. Fixed by
   design: the table sits in `.table-scroll{overflow-x:auto}` so it scrolls
   inside its card, and the meter track hides under 480px to shrink rows.

### Defects left

None known in code review. The following are unverified rather than
known-broken (see below).

## UNVERIFIED

- **Live render at 1280px and 390px.** No browser in the VM. No
  horizontal scroll, no clipped text, no overlap was observed in a real
  render; the checks above are static + code review only. A live-browser
  look is still needed before the ranking treats this variant as
  layout-verified.
- **Real keyboard/pointer feel.** Arrow-key chart navigation, rail
  collapse, search debounce and sorting were exercised only against a
  stub DOM (JS executes without error); focus order, the pill travel and
  the tooltip readout were not observed in a browser.
- **Dark mode.** Token overrides are declared under
  `prefers-color-scheme: dark` but were never rendered.
