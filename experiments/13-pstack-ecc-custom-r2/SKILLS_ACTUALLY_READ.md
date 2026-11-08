# Skills Actually Read — Variant 13-pstack-ecc-custom-r2 (Experiment 2, repeat of 04)

Fresh repeat of Experiment 1 variant 04. Same skills, same order. No variant folder was
opened, read, or copied. No Astryx, OpenSourceUI, or PanelUI material was read.

## Skills read in full, in order

1. `/home/hatch/workspace/skills-test/TASK.md` — fixed task, data, sections, constraints.
   Read first. Not a skill, listed for completeness.
2. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/SKILL.md` — poteto-mode.
3. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md` — Feature playbook.
4. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC frontend patterns.
5. `/home/hatch/workspace/build-anything/ECC/AGENTS.md` — ECC house rules. Read on the runtime
   instruction to read AGENTS.md files in folders touched. Not an assigned skill.
6. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md` — emil-design-eng.
7. `/home/hatch/workspace/custom-skills/local-skills/skills/better-interface/SKILL.md` — better-interface.
8. `/home/hatch/workspace/custom-skills/local-skills/AGENTS.md` — local-skills house rules.
   Read on the runtime instruction. Not an assigned skill.
9. `/home/hatch/workspace/custom-skills/local-skills/skills/better-accessibility/SKILL.md`
10. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md`
11. `/home/hatch/workspace/custom-skills/local-skills/skills/better-writing/SKILL.md`
12. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`
13. `/home/hatch/workspace/custom-skills/local-skills/skills/better-colors/SKILL.md`
14. `/home/hatch/workspace/custom-skills/local-skills/skills/better-ui/SKILL.md`

## Assigned skills not used

- pstack `how` and `architect` skills. The Feature playbook names them in steps 1–2.
  Neither SKILL.md was read. This agent cannot spawn subagents, so no `how` or `architect`
  delegate ran. Their intent was applied inline instead. The data shape was named first
  (one `DASHBOARD` model), design alternatives were weighed in the build, and the choice
  was recorded here.
- `interface-review`. better-interface routes change reviews there. This task builds a new
  file rather than reviewing a change, so it does not apply. It is user-invoked and cannot
  be started from better-interface in any case.
- Domain reference files linked from the better-* SKILL.md files (for example
  `semantics-and-aria.md`, `grouping-and-alignment.md`, `contrast.md`). Only the SKILL.md
  bodies were read in full. Principles were applied from those bodies.
- No other pstack playbook or skill was used. No `how`, `why`, `swarm`, `arena`,
  `interrogate`, `unslop`, `technical-writing`, or `deslop` SKILL.md was read.

## How the Feature playbook was followed

- Throughput checkpoint. Blocking first steps: read TASK.md and every assigned skill
  before writing code. Independent workstreams: not applicable, single file. Shared
  mutable state: not applicable, one data object owns all state. Smallest safe
  decomposition: one file built in section order (header, nav, stats, chart, table,
  notices, footer), each verified by the check script before finishing.
- Step 4 (delegate to a subagent) was not possible. This agent owns the diff directly,
  with the same review separation the playbook asks for: build first, then verify
  against the check script, then fix.
- Steps 6–8 (rebase into commits, interrogate, open a PR) do not apply. No push was
  requested and no commit was made.

## Principles applied, and the choice each changed

- pstack Model the Domain. One `DASHBOARD` object holds school, stats, trend, students,
  notices. Status is a lookup over marks, not a branch repeated per row.
- pstack Laziness Protocol. No framework, no chart library, no extra abstraction. Plain
  HTML, CSS, and JS in one file.
- pstack Prove It Works. A static verification script ran 36 assertions against the
  finished file, plus a Node parse check of the script block.
- pstack Sequence into Verifiable Units. Sections were built in TASK.md order and the
  check script ran after the build, then again after the one fix.
- ECC composition. Card, stat, bar, and row rendering are small functions that compose,
  mirroring the Card/Header/Body pattern in vanilla JS.
- ECC state. One state object (`query`, `sort`) plus a pure render, mirroring the
  Context plus reducer pattern without React.
- ECC debounce hook. Search re-renders 200ms after typing stops.
- ECC accessibility patterns. Focus-visible rings, Escape closes overlays, focus returns
  to the trigger, `aria-pressed` carries sort state, `role="status"` carries the result count.

## Conflicts resolved

| Conflict | Resolution |
| --- | --- |
| Press scale: emil says 0.97, better-ui says exactly 0.96 | Used 0.96. better-ui states its values are exact, not ranges. |
| Stagger: emil says 30–80ms between items, better-ui says 100ms with opacity, 4px blur, 12px translateY over 300ms | Used the better-ui recipe. It is the later, more prescriptive polish rule. |
| ECC is React and Next.js oriented, TASK.md requires one self-contained HTML file with no build step | Adapted ECC patterns into vanilla JS. React was not used. The constraint wins. |
| ECC AGENTS.md requires TDD and 80 percent coverage | No test runner fits a static page. Replaced with a static verification script (36 assertions) and a Node syntax check. |
| better-interface output format is a review report | This task is construction, not a review. Its rules were applied at build time instead of producing a separate review. |
| emil stagger plus better-ui entrance motion versus reduced motion | All motion runs only under `prefers-reduced-motion: no-preference`. Under reduced motion, durations collapse to near zero. |

## Design choices from the skills

- System font stack only. TASK.md allows it and better-typography forbids proposing a new
  typeface unasked.
- Hex tokens throughout, semantic tokens only in components, per better-colors.
- One accent (indigo `#1d4ed8`). Status hues (green, amber, red) sit more than 15 degrees
  from it. Status is never carried by color alone. Every chip has an icon and a label.
- Type scale in rem, unitless line-heights, `tabular-nums` on every changing number,
  headings descend with level, `text-wrap: balance` on headings, `pretty` on paragraphs.
- Layout uses logical properties (`margin-inline-start`, `inset-inline-end`), `minmax(0, 1fr)`
  tracks, `min-width: 0` on grid children, and `scroll-padding-top` for the sticky header.
- Motion uses only transform and opacity, named transition properties, the emil custom
  ease-out curve `cubic-bezier(0.23, 1, 0.32, 1)`, hover gated behind
  `@media (hover: hover) and (pointer: fine)`.
- Copy is sentence case, verb-first buttons, plain error and empty states, no "Oops".
  Links name their destination ("See fair details").

## Contrast, measured not estimated

Computed in Python from the declared token pairs before building. All pass 4.5:1.

| Pair | Ratio |
| --- | --- |
| `#171a21` on `#ffffff` | 17.41 |
| `#4b5563` on `#ffffff` | 7.56 |
| `#1e40af` on `#ffffff` | 8.72 |
| `#ffffff` on `#1d4ed8` | 6.70 |
| `#14532d` on `#dcfce7` | 8.30 |
| `#92400e` on `#fef3c7` | 6.37 |
| `#991b1b` on `#fee2e2` | 6.80 |
| `#4b5563` on `#f4f5f7` | 6.93 |
| `#1e40af` on `#dbeafe` | 7.15 |

## Self-check at 1280px and 390px

Method: static verification script (36 assertions) plus Node parse of the script block,
plus Python contrast computation. No live browser exists in this VM, so no rendered
screenshot or live viewport measurement was possible. See UNVERIFIED below.

Checks passed: all 12 students with exact names, rolls, attendance, and marks; all 4
stat values plus 1,188 present, 60 absent, ₹25.2L of ₹30L; all 5 trend values; all 3
notices; exact footer text; no external URLs, fonts, or images; viewport meta with no
`user-scalable=no` or `maximum-scale`; no `transition: all`; no `ease-in`; no `scale(0)`;
press scale 0.96; reduced-motion guard; hover gating; focus-visible rings; skip link;
accessible names on icon buttons; `role="status"` result count; table scrolls inside its
card (`overflow-x: auto`, `min-width: 560px`); `minmax(0, 1fr)` tracks; breakpoints at
1020px, 860px, and 560px; search input at 16px so iOS Safari does not zoom.

## Defects found and fixed

1. Bell panel used `width: 320px` with `max-width: calc(100vw - 32px)`. The separate
   max-width is the pattern better-layout flags. Fixed to a single bounded value:
   `width: min(320px, calc(100vw - 32px))`. The panel can never exceed the viewport.
   Found by the check script, fixed, re-verified.
2. Checker flagged fixed `height` values. Investigated each one. All sit on icon or
   control boxes (40px buttons, 36px stat icons, 18px badge, 6px meter) or the chart
   container (220px, holds bars, not wrapping text). No text box has a fixed height.
   No change needed.

## Defects left

None known. The 390px risks were checked by reading the code: stat grid collapses to
2 columns at 1020px (each card about 171px wide at 390px, values fit), the table
scrolls inside its card, the bell panel is bounded by `min()`, the sidebar becomes a
280px drawer, and the search input keeps `min-width: 0` inside a flex header.

## UNVERIFIED

- Live render at 1280px and 390px. No Chromium or Playwright exists in this VM.
  Horizontal scroll, clipping, and overlap were verified by code inspection and static
  assertions only, not by a rendered page.
- Real-browser behavior of the drawer, bell panel, search debounce, and sort buttons.
  The script parses in Node and the logic was checked by reading it, but no browser ran it.
- Screen-reader announcements and keyboard tab order. Names, roles, labels, and focus
  styles are present in the code, but no assistive technology ran against the page.
- Contrast ratios are computed from declared tokens, not measured from rendered pixels.
