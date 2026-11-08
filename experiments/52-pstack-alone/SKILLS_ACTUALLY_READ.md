# Skills Actually Read — Variant 52, pstack alone

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — entry router. Read first per the task. It routes a new feature to ECC structure plus the pstack feature playbook. For this variant ECC was deliberately NOT read.
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md` — Muse adapter for pstack. It mandates reading the poteto-mode router skill in full at the start of a rigorous task.
3. `/home/hatch/workspace/custom-skills/local-pstack/skills/poteto-mode/SKILL.md` — pstack router. Read because the adapter (item 2) requires it before any playbook work. It names the Feature playbook and its reply and comment rules.
4. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md` — Feature playbook. Read in full before building.

## Assigned skills NOT used

None. Every assigned skill was read in full. No ECC skill was read for this variant, by design.

## No other skill files read

No pstack principle leaf skill (`principle-*`), no workflow leaf skill (`how`, `architect`, `tdd`, `unslop`, `technical-writing`, `no-comments`), and no design skill of any kind was read. This matters because poteto-mode says to cite only principles whose leaf SKILL.md was read this session. No principle is cited anywhere in this file or in the page, because none was read.

## Feature playbook steps, applied or skipped

- Step 1, `how` over the affected subsystem. Skipped. Reason: there is no existing subsystem. The page is a greenfield single file, so there was nothing to walk through.
- Step 2, `architect` for parallel design exploration. Skipped. Reason: a single static HTML file has no function boundaries or cross-boundary design to explore in parallel.
- Step 3, throughput checkpoint. Applied in reduced form. Blocking first steps were the skill reads and the fixed data. Independent workstreams: n/a, single file, no fan-out. Shared mutable state: n/a, no concurrent writers. Smallest safe decomposition: one worker (this agent). Delegation was impossible because this agent runs at depth 2 of 2 and cannot spawn subagents.
- Step 4, delegate code-writing to a subagent. Skipped. Reason: cannot spawn (depth 2 of 2, `can_spawn=no`). This agent owned the diff directly, keeping the same review separation by verifying the built file against the task data afterwards instead of passing through a delegate report.
- Step 5, verify on the matching surface. Applied. Headless Chromium check via CDP at 1280x800 and 390x844, plus screenshot eyeballing. See Verification below.
- Step 6, rebase into small ordered commits. Skipped. Reason: the task forbids committing and pushing.
- Step 7, `interrogate` before shipping. Skipped. Reason: the design was not contested. There was no competing design to adjudicate.
- Step 8, Opening a PR. Skipped. Reason: the task forbids committing and pushing.

## What pstack alone did not guide

pstack is a process skill, not a design skill. It says how to sequence, verify, and report work. It says nothing about what the page should look like. Every visual decision below was unguided by any skill read, and none of it was borrowed from a design skill. I used plain, neutral defaults rather than inventing a design system.

- Colour palette. Nothing guided the choice of navy header (`#1f3a93`), blue chart bars, grey page background (`#f4f5f7`), or green/red attendance pills. Defaults picked for contrast and legibility only.
- Typography. No type scale, font choice, or hierarchy guidance. Used the system font stack only, with sizes chosen ad hoc (18px brand, 28px stat values, 14px body).
- Spacing and layout. No spacing scale or grid guidance. Used ad hoc gaps (16px cards, 20px page padding) and a 2fr/1fr split for chart versus notices.
- Responsive behaviour. The task fixed the requirement (sidebar desktop, bottom nav mobile, no horizontal scroll). pstack gave no breakpoint, no nav pattern, and no table strategy. I picked 768px as the sidebar to bottom-nav switch, 900px for the 4 to 2 column stat grid, and a horizontally scrolling table wrapper on narrow screens.
- Chart form. The task fixed CSS bars only and the five values. Bar proportions, labels above bars, and day labels below were ad hoc.
- Interaction details. Search filtering of the table, the bell badge showing 3, and the bell scrolling to notices were my own additions. pstack does not name them. The search exists because the header requires a search box and a dead input would be worse than a filtering one. The badge count 3 matches the three notices.
- Copy tone. The stat subtexts ("12 in Class 8-A", "Across all classes", "1,188 present · 60 absent") are plain restatements of the fixed data, not guided wording.

## Defects found and fixed

1. Rupee entity typo in the Fees stat card. Found while writing the file, before any check. The detail line was written as `&?#8377;25.2L of &?#8377;30L`, which would have rendered literally instead of as ₹. Fixed to `&#8377;25.2L of &#8377;30L`. This was an objective task violation (wrong data rendering), so fixing it is allowed by the task rule.
2. No other defects found by the headless check or the screenshots.

## Defects left

None found. Two observations, neither a task violation.

- At 390px the student table is wider than the card (549px versus a 366px card) and scrolls horizontally inside its card wrapper. The task explicitly allows this ("table fits or scrolls inside its card"). Page-level horizontal scroll is zero.
- The chart bars for 93 to 97 percent look nearly identical in height. That is honest. The values differ by only 4 points, so near-equal bars are the correct rendering.
- At 390px the search placeholder is cut off mid-word inside the input ("Search students by nar..."). That is normal input placeholder behaviour, not clipped page content, and the input itself fits.

## Verification (measured, headless Chromium via CDP)

- 1280x800. `documentElement.scrollWidth` 1265 equals `clientWidth` 1265. Zero overflowing elements. Sidebar visible, bottom nav hidden. Stat values read back as 1,248 / 72 / 95.2% / 84%. Table has 12 rows. Chart labels read back as 94 / 96 / 93 / 97 / 95 percent. All three notices present verbatim. Footer reads "Fictional demo data — UI Forge Experiment 1".
- 390x844. `documentElement.scrollWidth` 390 equals `clientWidth` 390. Zero page-level overflow. Sidebar hidden, bottom nav visible as flex. Same data read back as at 1280. Screenshots at both sizes eyeballed: no clipped text, no overlapping elements, stat cards do not overflow.
- Not verified on real hardware, in a real browser, or on Safari or Firefox. Headless verification proves layout correctness, never feel.
