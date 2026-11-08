# Skills actually read — Experiment 32 (taste-skill + emil-design-eng)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — entry router (ECC first, then pstack). Read first per the task.
2. `/home/hatch/workspace/ui-skills/taste-skill/skills/taste-skill/SKILL.md` — tasteskill 2.5.0, the anti-slop frontend skill.
3. `/home/hatch/workspace/ui-skills/taste-skill/skills/taste-skill/references/copy.md` — read in full; taste-skill requires it before the first page.
4. `/home/hatch/workspace/ui-skills/taste-skill/skills/taste-skill/references/system.md` — read in full; same requirement.
5. `/home/hatch/workspace/ui-skills/taste-skill/skills/taste-skill/references/layout-v4.md` — read in full; same requirement.
6. `/home/hatch/workspace/ui-skills/taste-skill/skills/taste-skill/references/sections.md` — read up to the tool's 100 KB read cap (through the Product plan and most of the Place plan; the tail beyond the cap was not read). The Software plan, which governs this dashboard, was read in full.
7. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md` — Emil Kowalski design-engineering philosophy. Read in full.

No other variant folder was read. (A directory listing of `experiments/` showed folder names only; no variant file was opened.)

## Assigned skills NOT used

None. Both assigned skills (taste-skill, emil-design-eng) were read in full and applied.

## What each skill contributed

### taste-skill (applied)

- **Kind declared:** software. `<html data-type="software">`, `<meta name="page-type" content="software">`, `<meta name="site-kind" content="software">`. The dashboard itself is the software plan's `app-frame`: live HTML showing the school's own data, not a marketing hero.
- **One system:** one accent (deep green `#0b6e4f`, never violet), one ink, flat warm-paper grounds, one card style, one button pair (filled primary / bordered secondary), soft radius (12px cards, 8px controls), one link look.
- **Equal edges:** a single `.wrap` line (max 1200px, 24px gutters, 16px under 640px) for header, content and footer.
- **Copy:** h1 names the category and the buyer ("Student dashboard for Class 8-A"); one hard fact on the first screen ("1,188 of 1,248 students present today"); every heading is a noun ("Today at Greenwood High", "Attendance this week", "Students in Class 8-A", "Notices"); buttons are verb + object ("Take attendance"); no invented facts — every number is from the brief.
- **Hover layer:** linked cards lift 2px, buttons step on hover and press on click, links underline.
- **No photographs:** the task forbids external images and requires a CSS-bar chart, so the page is flat colour, type and initials discs — no stock photos passed off as the school's, no drawn logo look-alikes.
- **No caption under any visual; no placeholder/sample labels; no fact invented.**

### emil-design-eng (applied)

- **Animation decision framework:** a dashboard is opened many times a day, so motion is minimal — no entrance stagger, no scroll reveal, no animation on the search (keyboard-initiated) input.
- **Custom easings:** `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)`, `--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1)`; never `ease-in` on UI.
- **Durations under 300ms:** hover/press 160ms, popover 180ms, drawer 250ms.
- **Buttons press:** `transform: scale(0.97)` on `:active`.
- **Only transform and opacity animate.** Transitions, not keyframes, so the popover/drawer retarget cleanly.
- **Origin-aware popover:** the notification dropdown scales from `transform-origin: top right` (its trigger), starting at `scale(0.95)` + opacity 0 — never from `scale(0)`.
- **Hover gated** behind `@media (hover: hover) and (pointer: fine)`; touch devices get no false hover.
- **Reduced motion:** `prefers-reduced-motion: reduce` removes all transform movement.

## Conflicts between the two skills, and resolutions

1. **Taste "one section per screen" (each section exactly 1–2 screens tall) vs the task's 7-section working dashboard.** Forcing every section to a full screen would make a sparse, unusable dashboard. Resolution: taste-skill's rule was written for marketing pages; this page is the software plan's `app-frame`, so sections size to their content. Kept from taste-skill: equal edges, one h2 per section, separation by space/ground, no neighbour heading peeking. Task order governs.
2. **Taste "linked card lifts 2px" vs Emil "hover seen tens of times/day: remove or drastically reduce".** Resolution: kept the smallest motion both allow — a 2px lift, transform-only, 200ms ease-out, gated to fine pointers. No photo zoom (no photos), no tilt, no other hover motion.
3. **Emil "stagger elements entering together (30–80ms)" vs taste "never a card/row/tile that animates on scroll; no reveal/fade-in".** Resolution: no entrance stagger at all. Emil's own frequency table supports this — a dashboard is seen far too often for entrance animation — so taste-skill's no-reveal rule stands.
4. **Taste "real photographs lead; the product shown big" vs task "no external images; CSS bars only".** Resolution: task constraint wins. No photos; the chart is CSS bars per the task; student portraits are initials discs (UI, not photographs).
5. **Taste "no fact twice" vs the bell popover listing the same three notices as the Notices panel.** Resolution: kept both. The popover is the notification list (standard app-frame pattern, the bell's whole job); the panel is the section. Noted as a deliberate UI pattern, not copy duplication.
6. **Taste "deal palette/pairing/hero with the kit scripts; load Google Fonts" vs task "no npm, no external fonts".** Resolution: scripts not run, no webfonts. Palette hand-set to the skill's rules (one accent, 90–95% neutral, never violet); system font stack used. The kit checkers (`consistency.mjs`, `check.mjs`, `deal.mjs`, `start.mjs`) were therefore not run — see Verification.

## Verification (headless Chromium, real browser engine)

- Screenshots at 1280×900 and 390×844, plus full-page shots (1280×1900, 390×3400), read by eye.
- DOM measurement probes on /tmp copies (deliverable untouched): at 1280, `scrollWidth 1265 = viewport 1280 − scrollbar`, zero overflowing elements, 4 stat cards 226×131 in one row, table fits its card (934 = 934, not scrollable), bar heights 147/150/146/152/149 px proportional to 94/96/93/97/95. At 390 (dump-dom clamps viewport to 500, so the 390 evidence is the true-390 screenshot plus the 500-viewport probe: `scrollWidth 485 < 500`, no page-level overflow; table scrolls inside its card, 560 > 435, as the task allows).
- JS probe: search "diya" → "1 of 12 students", only Diya Patel visible; search "zzz" → "0 of 12 students" + empty-state row; bell click → popover opens, `aria-expanded="true"`.
- All fixed data verified present and exact: Greenwood High, Class 8-A, 2025–26; 1,248 / 72 / 95.2% (1,188 present, 60 absent) / 84% (₹25.2L of ₹30L); all 12 students with roll, attendance and marks; trend 94/96/93/97/95; all three notices; footer text verbatim.

## Defects found and fixed

1. **Invented notice details (build-time, objective task/copy violation).** First draft added "bring your project to the school hall", "pay the term fees before the deadline", "parents meet the class teacher" — facts the brief never gave. Removed; notice meta lines now carry only the given date/day (Friday / 20 Nov / 25 Nov).
2. **Hamburger + bottom nav both visible at 390px (found in screenshot).** Redundant double navigation. Fixed: hamburger hidden under 640px; the bottom nav owns navigation there (hamburger remains for 641–900px, where there is no bottom nav).
3. **Search placeholder truncated to "S" at 390px (found in screenshot).** Fixed in two steps: hiding the hamburger freed width, and the placeholder was shortened to "Search" (full description kept in `aria-label`).

## Defects left

1. **Search placeholder at 390px shows only "Search".** The input is ~110px wide at 390px once the school name and bell are in the header; the field itself works (typing scrolls, full label in `aria-label`). Cosmetic only; not a task violation.
2. **Table scrolls horizontally inside its card at 390px.** Allowed by the task ("table fits or scrolls inside its card"). The Marks column is off-screen until the table is scrolled. No page-level horizontal scroll.
3. **Taste-skill's own checkers not run** (`consistency.mjs`, `check.mjs`, `perf.mjs`, `deal.mjs`, `start.mjs`): they need the kit's Node scripts and Google Fonts, both excluded by the task constraints. Replaced by the headless Chromium screenshots + DOM probes above. Live-browser look, real-device feel, and screen-reader pass are UNVERIFIED.
4. **"Take attendance" is an anchor to the student table**, not a working attendance-taking flow — the brief names no such flow, so no behaviour was invented. The link lands where its label says.

## Files

- `experiments/32-taste-emil/index.html` — the variant (self-contained, inline CSS+JS only).
- `experiments/32-taste-emil/SKILLS_ACTUALLY_READ.md` — this file.
