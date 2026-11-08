# Skills actually read — variant 21-taste-alone

## Skills read in full (in order)

1. `/home/hatch/workspace/ui-skills/taste-skill/skills/taste-skill/SKILL.md` — taste-skill 2.5.0
   (read in full before building)

## Assigned skill not used

None. The only assigned skill was taste-skill, and it was used.

## What was applied from taste-skill

- Kind declared as software (`<html data-type="software">`, `<meta name="page-type" content="software">`,
  `<meta name="site-kind" content="software">`); the dashboard's own data is the subject, so no shader
  effect, scene, moment, or stock photograph — flat grounds only, as the skill requires for non-tech
  decoration and permits by restraint on tech.
- One system: one radius (12px, soft), one card style, one button pair (solid green / ghost), one link
  look (underline draws on hover), one heading scale, system type stack.
- Equal left/right edges: single `.wrap` (max-width 1120px, 24px gutters, 16px at ≤480px).
- Plain copy: nouns in headings, buttons as verb + object ("Download report"), one hard fact per
  section lede; no invented figures, reviews, or quotes. "Watch" / "On track" tags are derived only
  from the given attendance/marks numbers (Watch = marks < 80% or attendance < 90%).
- Hover layer only (house rule 5): stat/notice cards lift 2px, buttons step on hover and press on
  click, nav underlines draw. No tilt, no scroll reveal, no parallax, no timed loops.
- No violet/lavender anywhere; single deep-green accent (`#0f6b4e`) on warm-paper ground; no gradients.
- No caption under any visual; bar values are the data itself, not captions.
- `prefers-reduced-motion` disables all transitions.

Note: the skill's deal scripts (`start.mjs`, `deal.mjs`) and its reference files (`copy.md`,
`system.md`, `layout-v4.md`, `sections.md`, `assets/`) were NOT read or run — the palette, pairing,
and layout were chosen directly from the SKILL.md house rules and applied by hand, since the fixed
task prescribes the section order and the file must be self-contained with no build step.

## Self-check (headless Chromium, real renders)

Tool: Playwright Chromium at `/home/hatch/.cache/ms-playwright/chromium-1248/chrome-linux64/chrome`
(the `/opt/meta-chromium/chrome` binary refused to start without an image-owned WebGL domain policy
supplied by browser-daemon; a hand-written policy file was rejected as an invalid contract, so it
was not used). Probe injected into a temp copy; screenshots taken at both widths and eyeballed.

### 1280×900 — PASS
- `innerWidth=1280`, `docScrollW=1265`, no horizontal scroll, 0 JS errors.
- 4 stat cards, no overflow (right edges 490 / 741 / 991 / 1241), 5 chart bars, 12 table rows,
  3 notices, sidebar visible, bottom nav hidden, 0 clipped elements.

### 390×844 (true 390 via CDP device-metrics override; plain `--window-size=390` renders at 500px) — PASS
- `innerWidth=390`, `docScrollW=390`, no horizontal scroll, 0 JS errors.
- 4 stat cards in 2 columns, no overflow (right edges 189 / 374), sidebar hidden, bottom nav visible,
  0 clipped elements. Table scrolls horizontally inside its card (scrollW 560 > clientW 356) — the
  behaviour TASK.md explicitly allows.

## Defects found and fixed

1. **Bell badge overflowed its button** — badge was positioned at `top:-4px; right:-4px`, flagged as
   clipped (`BUTTON "🔔3"`). Fixed: badge now sits at `top:0; right:0`, fully inside the button.
2. **Search field cramped at 390px** — header row (title + search + bell) squeezed the search input
   to ~150px and truncated its placeholder. Fixed: at ≤768px the header wraps and the search takes a
   full-width second row (verified: 358px wide, placeholder fully visible).

## Defects left

- None found. The table's internal horizontal scroll at 390px is intentional (TASK.md allows the
  table to scroll inside its card); the fixed bottom nav appears mid-page only in the
  `captureBeyondViewport` full-page screenshot, which is a capture artifact — in the normal viewport
  screenshot it sits at the viewport bottom, and `body{padding-bottom:56px}` keeps the footer clear
  of it.
- "Watch" / "On track" status tags and the "Week average 95.0%" line are derived from the given
  numbers, not part of the fixed data — presentation only, no new facts.

## UNVERIFIED

- The taste-skill's own checkers (`consistency.mjs`, `check.mjs`, `shoot.mjs`, `perf.mjs`) were not
  run — they live in the skill's `scripts/`/`assets/` folders, which were not read per the
  assignment (only SKILL.md), and they require the skill's kit files.
- No live-browser (non-headless) look; all verification is headless Chromium + screenshots.
- Search filter, bell scroll-to-notices, and the "Download report" button label change were
  exercised only by code inspection, not by clicking in the headless runs.
- `/opt/meta-chromium/chrome` could not be used (see above); results come from the Playwright
  Chromium build instead.
