# 00-baseline — Skills Actually Read

Variant: 00-baseline (control). Built directly from TASK.md, no UI skill applied.

## Skills read in full

None. No UI skill file was read.

- Did NOT read: local-skills / jakubkrehel skills, emil-design-eng, designer-skills, Astryx, OpenSourceUI, PanelUI, pstack UI skills, ECC UI/frontend skills, or any other UI skill.
- Did NOT read any other variant folder.
- Only source read in full: `~/workspace/skills-test/TASK.md` (the fixed task, data and constraints — not a skill).

## Skills assigned but NOT used

None assigned to this variant. It is the control: by design, no skill combination was applied.

## Self-check — 1280px and 390px

Method: code-reading check only. No headless browser was available in this VM (no Chromium, Firefox, or Playwright installed — `which chromium chromium-browser google-chrome firefox` returned nothing, Playwright is not installed for Python or Node), so no rendered screenshot or measured `scrollWidth` check was possible. Static content checks were run instead and all passed: 12 student rows, 4 stat cards, 5 chart bars, 3 notices, all TASK.md numbers present, no external resources/fonts/images, viewport meta present, table inside an `overflow-x: auto` wrapper.

Reasoning for the two widths:

- 1280px: header fits on one row (school + search max 420px + bell); sidebar 200px + main; stats 4 columns (~260px each); chart/table grid 1.6fr/1fr; table min-width 520px fits inside the ~1000px card without scrolling.
- 390px: header wraps with search full-width on its own row; sidebar hidden, bottom nav shown (footer padded to clear it); stats 2 columns (~178px each, detail text wraps, does not clip); grid collapses to 1 column; table min-width 520px scrolls inside its card via `.table-wrap` while the page has `overflow-x: hidden` and `main` has `min-width: 0`, so the page itself does not scroll horizontally.

## Defects found and fixed

1. Rupee entity typo: the Fees stat detail was written as `&?#8377;25.2L` and would have rendered literally. Fixed to `&#8377;25.2L of &#8377;30L`.
2. Chart bar overflow risk: bars were percentage heights of the whole 180px bar column, so value label + bar + day label summed to more than the chart height. Fixed by putting each bar in a fixed 130px `.bar-track` (value ~16px + track 130px + day ~20px = ~166px < 180px).

## Defects left

- No rendered check at 1280px or 390px — UNVERIFIED in a real browser. The layout conclusions above are from code reading only; a live-browser screenshot could still reveal a wrap/overlap the code reading missed.
- Search placeholder text is long and may be ellipsised by the browser at 390px (placeholder only, not content).
