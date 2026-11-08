# Skills Actually Read — Variant 15-astryx-pstack-ecc

## Skills read in full, in order

1. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/SKILL.md` — poteto-mode (pstack). Read in full before building.
2. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md` — Feature playbook, matched by poteto-mode for a new-behavior build. Read in full before building.
3. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC frontend-patterns. Read in full before building.
4. `/home/hatch/workspace/build-anything/ECC/AGENTS.md` — ECC house rules, read because the runtime flagged it for the ECC folder (not an assigned skill; listed for honesty).
5. `/home/hatch/workspace/custom-skills/local-astryx/SKILL.md` — Astryx (Muse adapter). Read in full before building.

No other skill file and no other variant folder was read.

## Assigned skills not used

None. All three assigned skills (pstack, ECC frontend-patterns, Astryx) were read in full and applied. Notes on partial application:

- pstack Feature playbook step 1 (`how`) and step 2 (`architect`): their leaf skill files were NOT read in full. The substance was applied inline instead: the data shape was named first (SCHOOL / STUDENTS / TREND / NOTICES / STATS plus one reducer-style state object), and the component composition was designed before any code was written. No separate design exploration was run because the task is a single fixed file with fixed data.
- pstack Feature playbook step 4 (delegate code-writing to a subagent): NOT done. This agent runs at depth 2 and cannot spawn subagents, so I owned the diff directly and kept review separation via the headless self-check script instead.
- pstack Feature playbook step 8 (Opening a PR): not applicable. The task forbids pushing.
- pstack leaf principle skills (e.g. principle-model-the-domain, principle-sequence-verifiable-units, unslop): named only where their idea was applied; their SKILL.md files were not read in full, so they are not claimed as used.

## What each skill changed

- **pstack / Feature playbook.** Throughput checkpoint: blocking first steps were the TASK.md + skill reads; independent workstreams n/a (single file, one author); shared mutable state is the single file, written once then verified; smallest safe decomposition is one worker (me). Data shape first, then code. Laziness Protocol kept the page to the seven required sections plus the assigned interactions, no extra widgets. Verification on the real surface: headless Chromium via CDP, not a proxy.
- **ECC frontend-patterns.** Composition over inheritance: Card / CardHeader / CardBody as reusable classes, table and chart composed from small parts. Compound components: sidebar nav with active item, filter-chip toggle group, sortable table headers. Context + Reducer adapted to vanilla JS: one immutable state object, `dispatch()` replaces it and re-renders, derived data (filtered + sorted students) computed once per render. Debounce hook adapted for search (150 ms). Accessibility patterns: Escape closes overlays, focus returns to the trigger, `aria-sort` on headers, `aria-expanded` on bell and nav toggle, `role="dialog"` on the notification panel, focus-visible outlines. Error boundary adapted as a `window.onerror` fallback message in the table count line.
- **Astryx.** No React build step exists for this task, so Astryx was adapted, not installed: theme-neutral tokens became CSS custom properties (`--ax-color-*`, `--ax-space-*` on a 4px scale, `--ax-radius-*`, `--ax-shadow-*`, type scale); component anatomy became BEM-style classes (`.ax-card` / `.ax-card__header` / `.ax-card__body`, `.ax-button` primary/ghost/subtle/icon, `.ax-badge` success/warning/danger/neutral/accent, `.ax-avatar`, `.ax-input`, `.ax-table`, `.ax-nav`); the accessibility contract was honored with 4.5:1-or-better text contrast (secondary text `#5b5b57` on white) and visible focus states. No Astryx component was imported; no CLI command was run (no consumer project exists). The seven-theme system was not exercised; neutral was used throughout.

## Conflicts resolved

- ECC "many small files, 200-400 lines each" vs the fixed constraint of one self-contained `index.html`: the task constraint won. The file is one file by design.
- ECC "TDD, 80%+ coverage" vs a static single-file page with no test runner: adapted. Verification is a headless Chromium check (CDP device metrics + DOM probe + interaction script) instead of a unit suite. No coverage claim is made.
- Astryx "install `@astryxdesign/core`, import per component" vs "no build step, no npm, no external images/fonts": adapted to tokens + anatomy in plain CSS/JS, as described above.
- pstack "delegate to a subagent" vs depth-2 no-spawn limit: I wrote the file directly; the headless check is the review separation.
- ECC AGENTS.md "use ecc:planner / ecc:code-reviewer agents" vs no subagent spawning: not done, same reason as above.

## Self-check (headless Chromium via CDP, real 1280px and 390px device metrics)

Method: Node script driving the Playwright-bundled Chromium over CDP (`Emulation.setDeviceMetricsOverride`), DOM probe plus interaction script, screenshots viewed. Plain `--window-size=390` was found to report `innerWidth=500` (headless minimum window), so the 390px check was redone through CDP device metrics, which reported a true `innerWidth=390`.

Results at 1280px:

- No horizontal scroll (`docScrollW=1265`, `innerWidth=1280`).
- 12 student rows, 5 chart bars, 3 notices, 0 JS errors, 0 clipped elements.
- Stat card right edges: 490 / 741 / 991 / 1241, all inside the viewport.
- Table fits inside its card (`scrollW=943`, `clientW=943`).
- Sidebar static, bottom nav hidden, footer text exact: "Fictional demo data — UI Forge Experiment 1".

Results at 390px:

- No horizontal scroll (`docScrollW=390`, `innerWidth=390`).
- 12 student rows, 5 chart bars, 3 notices, 0 JS errors, 0 clipped elements.
- Stat cards in a 2-column grid, right edges 189 / 374, inside the viewport.
- Table scrolls inside its card (`scrollW=660`, `clientW=324`); the page itself does not scroll horizontally.
- Sidebar becomes a hidden drawer, bottom nav visible, hamburger present.

Interactions verified live in headless Chromium:

- Search "priya" (debounced) filters to 1 row, Priya Das; clearing restores 12.
- Sort by marks twice gives descending order with Priya Das (95%, Distinction) first.
- "Watch <90%" filter shows exactly Kabir Singh and Vikram Joshi.
- Bell opens the panel with 3 notices; "Mark all read" hides the unread dot.
- Search "zzz" shows the empty state and "Showing 0 of 12 students."

## Defects found and fixed

1. Roll column header was a button that did nothing when clicked. Fixed: plain header text, only Name / Attendance / Marks are sortable buttons.
2. First 390px probe was invalid: `--window-size=390` produced `innerWidth=500` in headless Chrome. Fixed by re-running the check through CDP device metrics override at a true 390px width.

## Defects left

1. Cosmetic only: at 390px the Attendance stat card sub-line "1,188 present · 60 absent" wraps onto two lines. Nothing is clipped or overlapping; the wrap is graceful. Left as-is.
2. The mobile bottom nav is a fixed overlay; it covers the bottom of the content while scrolling, which is its intended behavior, and the content and footer carry extra bottom padding so nothing is permanently hidden.

## UNVERIFIED items

- Live-browser look and feel on real hardware (desktop and a real phone). Headless Chromium screenshots were viewed, but rendering, touch feel, and real-device performance are unverified.
- Keyboard-only navigation was implemented (focus-visible outlines, Escape handling, focus return) but not exercised end to end with a keyboard in the headless check.
- Astryx accessibility claims are repo-documented, not verified in this page; the contrast and ARIA work above is my own implementation, checked only by the DOM probe and screenshots.
- No real user tested the search, sort, filter, bell panel, or mobile drawer.
