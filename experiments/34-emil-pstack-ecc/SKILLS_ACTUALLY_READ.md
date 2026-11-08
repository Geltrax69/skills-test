# Skills Actually Read — Variant 34 (emil + pstack + ECC)

Variant: emil + pstack + ECC only. NO better-interface, NO other UI skill.

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — entry router; ECC first (structure), then pstack (execution).
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md` — Muse adapter for pstack (poteto-mode router, playbooks, principles, reply rules).
3. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md` — Feature playbook (this task is a new feature).
4. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC reference clone, read-only, never modified.
5. `/home/hatch/workspace/build-anything/ECC/AGENTS.md` — house rules for the ECC folder (read because the runtime flagged it; covers agent-first, TDD 80%, immutability, plan-before-execute).
6. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md` — Emil Kowalski design-engineering philosophy.

## Skills assigned but NOT used

- None. All four assigned skills (pstack adapter, feature playbook, ECC frontend-patterns, emil-design-eng) were read in full and applied. No other variant folder was read and nothing was copied from another variant.

## How each skill shaped the build

- **Global harness (ECC first, then pstack):** ECC supplied structure (plan, composition, verification before reporting); pstack supplied execution rigour inside that structure. Where they conflicted, ECC structure won (see Conflicts).
- **pstack Feature playbook:** `how` over the subsystem = reading the four skills and the fixed task/data before writing code (no existing subsystem to walk; single new file). Throughput checkpoint: blocking first step was reading all skills; independent workstreams n/a (one file, one owner); shared mutable state n/a (single JS data model, no fan-out); smallest safe decomposition = one worker (me), because the artifact is a single self-contained file. Delegation step (step 4) NOT followed: this subagent cannot spawn subagents (depth 2, `can_spawn=no`), so I owned the diff directly with the same review separation (build, then independent headless verification pass). Step 6 (rebase into small ordered commits) and step 8 (Opening a PR) n/a: task forbids commit/push. Principles applied: **subtract-before-you-add** (no libraries, no extra sections, minimal JS: one data model, one render, one filter), **prove-it-works** (Playwright headless check at 1280/390, see below), **test-behavior-not-implementation** (checked rendered rows/bars/scroll widths, not code shape), **sequence-verifiable-units** (data check, then render check, then screenshot eyeball).
- **ECC frontend-patterns:** adapted to vanilla single-file HTML (no React here). Applied: **composition over inheritance** (one reusable `.card` / `.stat` / `.panel` component, four stat instances, not four bespoke blocks); **typed model over repeated shape assumptions** (single `STUDENTS` array rendered by one function, so the 12 rows cannot drift from the data); **controlled form input** (search input filters the model and re-renders, with an empty-state row); **accessibility patterns** (aria-labels on nav/search/bell/chart, `role="img"` with a text summary of the chart). NOT applied: React hooks, Context/reducer, memoization, virtualization, code-splitting, lazy loading, error boundaries — all React-specific and inapplicable to a static single-file page with 12 rows.
- **Emil design-eng:** applied throughout. Custom easings `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)` and `--ease-in-out` (never `ease-in`, never built-in `ease` for UI motion); transitions specify exact properties (`background-color`, `transform`, `border-color`), never `transition: all`; press feedback `transform: scale(0.97)` on `:active` for nav items, bell, menu button, bottom-nav links; no animation from `scale(0)` (entrance is `translateY(8px)` + opacity, from visible scale); stat cards stagger 0/50/100/150ms (within the 30–80ms step guidance); durations 160–300ms, all under the 300ms UI cap; hover gated behind `@media (hover: hover) and (pointer: fine)` so touch devices get no hover traps; `prefers-reduced-motion: reduce` strips movement; only `transform`/`opacity` animate (bars transition transform/opacity only); professional-dashboard motion kept crisp and fast, matching the mood. The bell/notification interaction is a state indication (badge clears on press), not decoration for its own sake.

## Conflicts found and resolutions

1. **ECC "many small files" (AGENTS.md: 200–400 lines typical) vs task "one self-contained index.html".** Task wins. Resolution: single file, structured internally by commented sections (shell, sidebar, header, stats, chart, notices, table, footer) so the composition stays readable. Recorded here.
2. **ECC frontend-patterns is React/Next.js-only vs task (vanilla HTML, no build, no npm).** Resolved by adapting, not by importing React: composition, single data model, controlled input, and accessibility patterns carried over; React-only patterns (hooks, Context, memo, Suspense, virtualization) dropped as inapplicable. Recorded here.
3. **ECC "80%+ coverage, TDD mandatory" vs pstack "test-behavior-not-implementation" and a static one-file demo.** Per the global harness, 80% is an aspiration for backend code, not a gate for a small UI demo; no tests were padded to hit a number. Resolution: behaviour verification instead — Playwright headless check of the rendered page at both widths (see Self-check). Recorded here.
4. **pstack Feature playbook step 4 (delegate implementation to a subagent) vs subagent depth 2 (cannot spawn).** Resolution: owned the diff directly with review separation via the independent verification pass. Recorded here.
5. **Emil "stagger entrance animations" vs pstack "subtract-before-you-add".** No real conflict: the stagger is 4 lines of CSS on the stat cards only, decorative, never blocks interaction, and is disabled under `prefers-reduced-motion`. Kept, minimal scope.

## Self-check (Playwright Chromium, headless)

Run: `node /tmp/check34.mjs` against the built file, viewports 1280×900 and 390×900.

| Check | 1280px | 390px |
| --- | --- | --- |
| `documentElement.scrollWidth` vs `clientWidth` | 1280 = 1280, no horizontal scroll | 390 = 390, no horizontal scroll |
| Student rows rendered | 12 | 12 |
| Chart bars rendered | 5 | 5 |
| Stat cards within viewport | x 260–1252, all inside | 2×2 grid, x 16–374, all inside |
| Clipped/overflowing elements | none | none |
| Console errors / page errors | 0 | 0 |
| Sidebar visible / bottom nav visible | sidebar flex, bottom nav none | sidebar none, bottom nav block |
| Footer text (rendered) | "Fictional demo data — UI Forge Experiment 1" | same |

Data check (grep): all fixed data present verbatim — Greenwood High, Class 8-A, 2025–26, 1,248 / 72 / 95.2% / 1,188 present / 60 absent / 84% / ₹25.2L of ₹30L, all 12 students with exact roll/attendance/marks, trend 94/96/93/97/95, all three notices. Zero external URLs, zero external images/fonts in the file.

## Defects found and fixed

- None. The first headless run was already green on every check above, and the screenshot eyeball showed no defect. No hand-fixes were made after building (per the no-hand-fix rule, only objective task violations would have been fixed).

## Defects left (known, not fixed)

- **None objective.** Two cosmetic observations, neither a task violation, left as the skills produced them:
  - At 390px the "Attendance Today" stat label wraps to two lines and its sub-line ("1,188 present · 60 absent") wraps with "absent" on a second line. Text is not clipped and nothing overlaps; it is inside its card.
  - At 390px the student table scrolls horizontally *inside* its card (min-width 560px + `overflow-x: auto`), which the task explicitly allows ("table fits or scrolls inside its card").
  - In full-page screenshots the fixed bottom nav appears mid-page; that is a full-page-capture artifact of `position: fixed`, not a real defect (in a real 390px viewport it sits at the bottom, and content has 96px bottom padding so the footer is not hidden).
- **UNVERIFIED (not a defect):** real-device feel, touch interaction, Safari/Firefox rendering, and screen-reader behaviour were not checked — headless Chromium only.

## Files

- `experiments/34-emil-pstack-ecc/index.html` — the variant (single self-contained file, inline CSS+JS only).
- `experiments/34-emil-pstack-ecc/SKILLS_ACTUALLY_READ.md` — this file.
