# SKILLS_ACTUALLY_READ.md — variant 22-impeccable-alone

## Skill files read in full, in order

1. `/home/hatch/workspace/ui-skills/impeccable/skill/SKILL.src.md` — read in full before building. (Assigned skill; source of truth. No Impeccable CLI/install was run, per task instruction.)
2. `/home/hatch/workspace/ui-skills/impeccable/skill/reference/craft-floor.md` — read in full immediately before editing, as SKILL.src.md Setup step 3 requires.
3. `/home/hatch/workspace/ui-skills/impeccable/skill/reference/new-work.md` — read (output capped by the reader at 100 KB, so the tail past ~offset 129 was not seen; the direction/contract procedure it describes was not executed — see note below).
4. `/home/hatch/workspace/ui-skills/impeccable/skill/reference/operate.md` — read in full. (Mode depth for this Operate surface: dashboard.)

Also read (repo house rules, not a skill file):
- `/home/hatch/workspace/ui-skills/impeccable/AGENTS.md` — read in full after the runtime flagged it.

## Assigned skill not used

None. Impeccable was the only assigned skill and it was used. What was deliberately NOT used, per the task and the situation:
- Impeccable CLI / `impeccable context` / `concept-seed` / decision page — not run (task forbade CLI/install; there is no user present to answer a direction round, and TASK.md is the fixed brief that wins. This is an Operate surface, which the skill makes code-led by contract, so no comp round was owed.)
- No other skill was read. No other variant folder was read.

## How the skill shaped the build

- Mode: **Operate** (dashboard the user works in) — precision/restraint over expression.
- Colour strategy: **Restrained** — cool paper ground tinted from ink (`#edf0f3` / `#17233b`), one teal accent (`#0f766e`) for primary actions/current selection only, semantic green/amber/red reserved for data states.
- One system sans family, fixed rem scale, tight ratio, tabular numerals for all data (operate.md).
- Craft-floor bans honoured: no eyebrow/kicker above headings; no identical icon+heading+text card grid as page structure (the four required stats are one ruled band, each with its own context line and meter, not four clone cards); no gradient text, no glass, no side-stripe borders, no hard offset shadows, no emoji/glyph icons (all icons are drawn inline SVG, one stroke/weight); browser surfaces themed (selection, focus ring, caret, scrollbar).
- One authored motion moment only: attendance bars and stat meters grow once on load (150–650 ms, exponential ease-out), with `prefers-reduced-motion` respected.
- Working controls with real states: live search filters the register with count + empty state, `/` focuses search, bell scrolls to Notices, hover/focus/active states on nav and buttons.

## Self-check (1280px and 390px)

**Render check attempted, not achieved:** headless Chromium at `/opt/meta-chromium/chrome` would not start in this shell — it exits with `Hatch WebGL domain policy is required for browser startup` (its README states normal startup requires the image-owned WebGL policy supplied by browser-daemon, which is unavailable here). No Playwright/alternate browser exists in the VM. So no screenshot or live measurement was run; the check below is the code-based check TASK.md allows as fallback.

**Code-based verification performed:**
- Automated content audit (scripted grep/python): all 12 students with exact rolls/attendance/marks; stats 1,248 / 72 / 95.2% (1,188 present, 60 absent) / 84% (₹25.2L of ₹30L); trend 94/96/93/97/95; all 3 notices verbatim; footer text exact; 0 external URLs; no external fonts/images; inline JS passes `node --check`.
- Layout audit by reading the CSS: at 1280 the sidebar (232px) + fluid main fits, stat band is 4 columns, table (min-width 640px) fits its ~990px panel without scrolling; at 390 the sidebar hides, bottom nav appears, header wraps with search full-width, stat band becomes 2×2, table scrolls inside its card via `overflow-x:auto` on `.table-wrap` (page itself does not scroll horizontally — every fixed/min width is contained by an overflow wrapper or a grid with `min-width:0`).

## Defects found and fixed (by code review, not by render)

1. Stat-note text (`1,188 present · 60 absent`) is `nowrap` + ellipsis on desktop; at 390px it would clip — fixed: `white-space:normal` at the ≤900px breakpoint.
2. Notice tags (`Event`/`Fees`/`Meeting`) would crowd the notice rows at 390px — fixed: hidden on mobile.
3. Stat band 2×2 grid at mobile would double-draw the left divider on the first item of row 2 — fixed: `nth-child(3)` has its left border removed and rows 2+ get a top border.
4. Table at 390px would force page-wide horizontal scroll — fixed by design: `min-width:640px` table inside an `overflow-x:auto` wrapper, so it scrolls inside its card only.

## Defects left / UNVERIFIED

- **No rendered screenshot at 1280px or 390px** — headless Chromium cannot start in this environment (see above). Therefore: no horizontal scroll, no clipped text, no overlap, stat-card fit, and bar proportions are **UNVERIFIED by render**; they are established only by the code audit above.
- **Bar visual proportions unmeasured** — trend values span only 93–97%, so bars will look near-equal in height; values are labelled, but the visual read has not been eyeballed.
- **Feel on real hardware unverified** — hover/focus states, sticky header blur, and the single grow-on-load motion have not been seen in a live browser.
- `new-work.md` tail (past ~100 KB) was not read; nothing in the build depends on it, but the record is incomplete on that file.
