# Skills Actually Read — Variant 46 (prototyper-pstack-ecc)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — harness entry. Precedence ECC first (structure), then pstack (execution). Applied: plan before execute, subtract first, verify before reporting, single-file demo is exempt from the ECC 80% coverage gate.
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md` — pstack Muse adapter. Applied: follow the feature playbook, keep replies short and declarative, keep comments only for a non-obvious why, verify on the real artifact.
3. `/home/hatch/workspace/custom-skills/local-pstack/skills/poteto-mode/SKILL.md` — poteto-mode router, read because the adapter names it as step 1. Applied: matched the task to the Feature playbook, wrote the throughput checkpoint below, verified before declaring done.
4. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md` — Feature playbook. Applied: see throughput checkpoint below.
5. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC frontend patterns (reference clone, read-only, never modified). Applied: composition over inheritance (Card / CardHeader / CardBody as reusable markup blocks), compound-component structure (sidebar nav, stat card, chart group), accessibility (aria-expanded on the nav toggle, aria-label on the chart, th scope, focus-visible rings, Escape closes the drawer), small functions, no mutation (search filter hides rows, never rewrites data).
6. `/home/hatch/workspace/custom-skills/local-prototyper-ui/SKILL.md` — Prototyper UI. Applied: OKLCH design tokens, surface hierarchy (background to surface to surface-secondary to surface-tertiary), `data-slot` on rendered elements, CVA-style `data-variant` classes (active nav, success/warning badges, peak chart bar), CSS-first animation with `--ease-out-fluid`, reduced-motion tiering, focus-ring styling.

House rules also read (not skills): `/home/hatch/workspace/build-anything/ECC/AGENTS.md` and `/home/hatch/workspace/custom-skills/local-prototyper-ui/AGENTS.md`.

## Skills assigned but NOT used

None. All four assigned skills were read in full and applied. No other variant folder was read. Nothing was copied from another variant.

## Feature playbook throughput checkpoint

- **Blocking first steps.** Read all assigned skills first, then fixed the data shape (12 students, trend Mon-Fri, 3 notices, 4 stats) before writing markup. Done.
- **Independent workstreams.** n/a: single self-contained file, one coherent scope, no fan-out possible.
- **Shared mutable state.** n/a: one file, one author, no shared writes. The search filter hides DOM rows and never mutates the data.
- **Smallest safe decomposition.** One worker is best: a single static file cannot be split across workers without merge cost exceeding the work. No subagent was spawned (depth 2, spawning unavailable); I owned the diff directly and verified it with a separate headless check pass.
- **Skipped playbook steps.** `how` over the affected subsystem: n/a in the usual sense, there was no existing subsystem, the task spec plus the skills were the subsystem. `architect`: no cross-boundary design exists in a single static file; the one design fork (JS-rendered table from a data model vs static HTML table) was settled by prototype reasoning in favour of static HTML, see conflicts. `interrogate`: skip, design not contested. Rebase into small ordered commits and Opening a PR: skip, the task forbids committing and pushing.

## Conflicts found and resolutions

1. **ECC 80% coverage / TDD-first vs pstack test-behavior-not-implementation vs one-file demo.** ECC AGENTS.md demands 80%+ coverage and TDD. The harness itself resolves this: coverage is a check, not the goal, and 80% is not a gate for a small UI demo. Resolution: no unit tests written. Verification is behaviour checks on the rendered page instead (data values, widths, search behaviour, zero console errors), measured with headless Chromium over CDP.
2. **ECC many small files vs pstack subtract-before-you-add vs task single-file constraint.** ECC wants many small files; pstack wants subtraction; the task mandates exactly one self-contained index.html. Resolution: task wins. One file, no extra files, no build output.
3. **ECC agent-first delegation / feature playbook delegate-to-subagent vs no spawning available.** Both ECC and the feature playbook want implementation delegated. Resolution: I owned the diff directly (a subagent forbidden to spawn satisfies the playbook by owning the diff with the same review separation), and review separation was kept by running the headless check as a distinct verification pass after building.
4. **Prototyper UI requires Bun/npx CLI + React + Tailwind vs task no npm, no build step, self-contained HTML.** The library cannot be installed or run under the task constraints. Resolution: applied its conventions only (OKLCH tokens, surface hierarchy, data-slot, CVA-style variants, CSS-first animation, focus rings). No actual Prototyper component code is in the page, and no claim is made that the library is installed.
5. **ECC frontend-patterns is React/Next.js-centric vs plain HTML.** Hooks, lazy loading, code splitting, and virtualization have no direct equivalent here. Resolution: adapted the patterns conceptually (composition, compound components, state via small vanilla JS, accessibility patterns). Virtualization and memoization are n/a for 12 static rows; lazy loading is n/a with no external assets.
6. **ECC immutability vs in-place DOM filtering.** Resolution: the search filter only toggles `hidden` on rows; the underlying rows and data are never rewritten.
7. **Static HTML table vs JS-rendered table from a typed data model.** pstack model-the-domain favours a typed model; static HTML is the single source of truth and is what checkers read. Resolution: static HTML for all content. The JS owns only behaviour (nav toggle, search filter, bell scroll). No duplicated data source exists.

## What each skill changed in the page

- Global harness: plan-first order (skills, data shape, sections in task order), one file, behaviour verification instead of coverage.
- pstack / feature playbook: sections built in the task's required order as verifiable units, then a measured headless check before finishing.
- ECC frontend-patterns: Card/CardHeader/CardBody composition, compound nav/stat/chart blocks, aria attributes, Escape-to-close drawer, focus-visible rings, small single-purpose JS functions.
- Prototyper UI: OKLCH tokens, surface hierarchy, data-slot attributes throughout, data-variant badges and active states, --ease-out-fluid transitions, prefers-reduced-motion rule.

## Self-check (measured, headless Chromium over CDP)

- 1280px: document scrollWidth 1265, innerWidth 1280, no horizontal overflow. 12 student rows, stat values 1,248 / 72 / 95.2% / 84%, chart bars 94/96/93/97/95%, 3 notices, footer text exact. Zero console errors and zero exceptions.
- 390px: document scrollWidth 390, innerWidth 390, no horizontal overflow. Same data values confirmed. Stat cards reflow to 2 columns, sidebar becomes an off-canvas drawer, bottom nav appears.
- Search behaviour: typing "Priya" leaves 1 visible row; typing "zzz" leaves 0 rows and shows the empty state; clearing restores all 12.
- Screenshots eyeballed at both widths: no clipped text, no overlapping elements, bell badge renders fully, table scrolls inside its card on narrow widths.

## Defects found and fixed

- None required a code fix. The check pass found zero objective task violations. Two check-tool flags were investigated and dismissed, not fixed, because they are intentional by design (see below). Per the task rule, no hand-polish was applied after building.

## Defects left / known limitations

- **Bell badge flagged as "clipped" by the automated overflow probe.** The badge is positioned 6px outside the bell button on purpose (standard badge placement). The button has no overflow clipping, and the screenshot confirms the red "3" badge renders fully at both widths. Left as designed.
- **Off-canvas sidebar flagged as "overflowing" at 390px by a bounding-box probe.** The sidebar is translated off-screen with translateX(-100%) when closed, which is the intended mobile drawer behaviour. Document scrollWidth is 390 at 390px width, so it creates no page-level horizontal scroll. Left as designed.
- **Search placeholder truncates with an ellipsis at 390px.** The input is flex-sized to fit the header row; the placeholder text is longer than the field. Placeholder truncation is standard input behaviour, not clipped content. Left as is.
- **Table scrolls horizontally inside its card below ~600px.** The table keeps a 560px minimum width inside an overflow-x auto wrapper, so the page never scrolls horizontally. This is the behaviour the task allows ("table fits or scrolls inside its card").
- **Not verified on real hardware.** Headless Chromium proves layout and data correctness only. Real-device feel, touch targets, Safari/Firefox rendering, and screen-reader behaviour are UNVERIFIED.
