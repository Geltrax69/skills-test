# Skills Actually Read — Variant 48: ui-ux-pro-max + pstack + ECC

Folder: `experiments/48-uiuxpromax-pstack-ecc/` — `index.html` + this file only.

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — entry router; precedence ECC first, then pstack.
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md` — pstack Muse adapter (playbooks, principles, reply rules).
3. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md` — Feature playbook (plan, review, verify; throughput checkpoint; delegate; verify on matching surface).
4. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC frontend patterns (composition, compound components, hooks, accessibility, performance). Reference clone, read-only, never modified.
5. `/home/hatch/workspace/ui-skills/ui-ux-pro-max-skill/.claude/skills/ui-ux-pro-max/SKILL.md` — UI/UX Pro Max design intelligence (priority table, design-system workflow, pre-delivery checklist).
6. `/home/hatch/workspace/build-anything/ECC/AGENTS.md` — ECC house rules (read because the runtime flagged it for that folder; agent-first, TDD, 80% coverage, immutability, plan-before-execute).

## Assigned skill NOT found at the given path

- The task named `/home/hatch/workspace/ui-skills/ui-ux-pro-max-skill/SKILL.md`. That file does not exist in the repo. The skill's real entry point is `.claude/skills/ui-ux-pro-max/SKILL.md` (listed above as #5), which I read in full instead. No other variant folder was read and nothing was copied from another variant.

## How each skill shaped the build

- **global-harness:** ECC structure first, pstack execution inside it. ECC structure wins on conflicts; pstack execution wins inside that structure.
- **local-pstack / Feature playbook:** throughput checkpoint — blocking first steps (read skills, verify fixed data) ran before building; independent workstreams n/a (single file, no fan-out); shared mutable state n/a (one file, no shared writes); smallest safe decomposition is one worker (me) because the deliverable is a single self-contained file. `how` over the subsystem: the subsystem is one static HTML file, no framework, no backend. `architect`: n/a, no competing shapes worth parallel exploration in a static page. Verify on the matching surface: headless Chromium render check at 1280/390 (see Defects).
- **ECC frontend-patterns:** adapted, not copied — it is React-oriented and there is no React here. Applied: composition (Card / CardHeader / CardBody structure as HTML sections), accessibility (keyboard-operable drawer and bell, Escape closes overlays, focus returns to the trigger, visible `:focus-visible` rings, `aria-label`/`aria-expanded`/`aria-current`, chart given a `role="img"` text equivalent), and immutability (student data is static markup, not mutated by JS; the search filter only toggles row visibility).
- **ui-ux-pro-max:** ran the skill's own search tool as required for a new page: `search.py "school student management dashboard analytics" --design-system --density 8 -p "Greenwood High"`. It returned style **Data-Dense Dashboard**, palette primary `#1E40AF`, secondary `#3B82F6`, accent `#D97706`, background `#F8FAFC`, card `#FFFFFF`, foreground `#1E3A8A`, muted `#475569`, border `#DBEAFE`, destructive `#DC2626`, density 8 (dense/dashboard spacing 8–32px). Those tokens are the page's CSS variables. Supplemental searches: `"bar chart trend dashboard" --domain chart` and `"bottom navigation mobile responsive" --domain ux` (mobile-first, viewport meta, table scrolls inside its card, bottom nav ≤5 items). Pre-delivery checklist items applied: SVG icons only (no emoji), 44px minimum touch targets, visible focus, `prefers-reduced-motion` respected, contrast 4.5:1 on body text.

## Conflicts resolved (one skill picked, applied consistently)

1. **ui-ux-pro-max chart search vs task:** the chart search recommends a line chart for trend-over-time. The task mandates CSS bars only. Task wins. Bars carry direct value labels plus a text summary and a `role="img"` description as the accessibility fallback the skill itself prescribes.
2. **ui-ux-pro-max typography vs task:** the design system pairs Fira Sans / Fira Code via Google Fonts. The task forbids external fonts. Task wins. System sans stack for text, `ui-monospace` stack for numbers (same data/monospace intent, no download).
3. **ECC frontend-patterns vs task:** the skill is React/Next.js patterns (hooks, context, lazy loading). The task is one self-contained HTML file with no build step. Task wins. Only the framework-neutral patterns (composition, accessibility, focus management) were adapted to vanilla HTML/CSS/JS.
4. **ECC 80% coverage / TDD-first vs static demo:** ECC AGENTS.md requires TDD and 80%+ coverage. There is no test harness, no JS logic worth unit tests, and no build step in this variant. Recorded as n/a: verification is the headless render self-check below, not a test suite. No tests were padded to hit a number (per global-harness conflict note 1).
5. **ECC "many small files" vs task:** ECC prefers many small files. The task requires exactly one self-contained `index.html`. Task wins.
6. **Feature playbook "delegate implementation" vs this run:** the playbook mandates delegating code-writing to a subagent. I cannot spawn subagents at depth 2, so I owned the diff directly and kept review separation via the independent headless render check. No PR step: the task forbids committing/pushing.
7. **ECC AGENTS.md "use agents proactively" vs this run:** same as #6 — no delegation possible at this depth; noted, not silently skipped.

## Design decisions not from a skill (recorded, not hand-fixed polish)

- Student rows are hard-coded HTML, not JS-rendered from a data array, so the data is present even with scripting off and for any static checker. JS only filters, toggles the drawer/bell.
- Status pills (Excellent / Good / Needs support) are derived from the given marks/attendance (≥85% marks and ≥94% attendance = Excellent; 79–84% marks = Good; below = Needs support). They are presentation only; no given data was altered.
- Weekly average 95.0% shown in the chart subtitle is computed from the given Mon–Fri values (94+96+93+97+95)/5 = 95.0. Today 95.2% is kept separate and labelled as today.
- The bell badge "3" matches the three notices; the bell dropdown lists the same three notices.

## Self-check (headless Chromium, Playwright headless shell)

- 1280px: `scrollWidth` 1280 = viewport, no horizontal scroll, 0 console errors, 12 student rows, bars 94/96/93/97/95, sidebar visible, bottom nav hidden. Screenshot eyeballed: clean.
- 390px: `scrollWidth` 390 = viewport, no horizontal scroll, 0 console errors, stat cards in 2 columns (right edges 189/378 ≤ 390), table scrolls inside its card (table 560 wide inside a 332px scroll container, by design), bottom nav shown, sidebar off-canvas (translateX(-232px), intentional; flagged by the overflow scan only because it is translated off-screen).

## Defects found and fixed (objective task violations only)

1. **Search input clipped at 390px** (found in screenshot: placeholder showed only "Search"). Fix: below 820px the header wraps and the search moves to a full-width second row. Re-checked at 390px: search is 366px wide, right edge 378 ≤ 390, full placeholder visible, still no horizontal scroll. This was a task violation (clipped text), not polish.
2. **Meta Chromium refused to launch** (`Hatch WebGL domain policy is required for browser startup`, exit 13). Not a page defect. Switched the check to the Playwright-bundled `chromium_headless_shell` binary; check then ran clean.

## Defects left

- None known. Notes, not defects: (a) in a full-page screenshot the fixed bottom nav appears overlaid on mid-page content — a screenshot-stitching artifact of fixed positioning; in a real viewport the content has 96px bottom padding so nothing is hidden at the page bottom. (b) The sidebar footer wraps "2025–26" across two lines at 1280px; text is fully visible, not clipped. (c) Monospace numerals render "1,248" and "95.2%" with wide digit spacing; values are complete and unclipped. None of these were hand-fixed after the check, per the no-hand-fix rule.
- UNVERIFIED: real-device touch feel, Safari/Firefox rendering, screen-reader pass. Headless Chromium proves layout correctness only.

## Assigned skills not used

- None. All four assigned skills were read in full and used (global-harness, local-pstack, pstack Feature playbook, ECC frontend-patterns, ui-ux-pro-max — five files counting the harness entry the task required first).
