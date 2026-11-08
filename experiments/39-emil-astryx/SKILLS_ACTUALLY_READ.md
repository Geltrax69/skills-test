# Skills Actually Read — Variant 39 (emil-astryx)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — harness entry (ECC first, then pstack). Applied lightly: plan-then-build, verify before reporting. No ECC skill files were read beyond this entry; the task assigned only the two skills below.
2. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md` — read in full before building. **Used.**
3. `/home/hatch/workspace/custom-skills/local-astryx/SKILL.md` — read in full before building. **Used.**

No assigned skill was left unused. No other variant folder was read. No package was installed and no npm was used (per task); Astryx was adapted to single-file HTML, not imported.

## How each skill shaped the page

### Emil Kowalski (design engineering)
- Custom easings only: `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)`; `--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1)`. No `ease-in` easing is used anywhere (the string `ease-in-out` appears only as Emil's own prescribed variable name for on-screen movement).
- Motion is transform + opacity only; no `transition: all`; no `scale(0)` entry (stat cards rise from `translateY(8px)` + opacity, bars grow via `scaleY` from the bottom).
- Press feedback: `:active { transform: scale(0.97) }` on nav items, bell, and menu button, 160 ms.
- Stagger: 40 ms steps on the four stat cards and five chart bars (Emil's 30–80 ms range).
- Hover gated behind `@media (hover: hover) and (pointer: fine)`.
- `prefers-reduced-motion: reduce` removes transform motion (opacity-only / none).
- Search filtering is instant with no animation (Emil: never animate keyboard-initiated actions).
- Sidebar drawer uses a 200 ms ease-out transform transition (Emil's 200–500 ms drawer range).

### Astryx (theme-neutral, adapted from React to HTML)
- Theme: `theme-neutral` — neutral stone/grey surfaces (`#f5f5f4` background, `#ffffff` cards, `#171717` text, `#e5e5e5` borders), dark-neutral accent, semantic success/warning/danger/info colours; text contrast aimed at the 4.5:1 contract in the Astryx docs.
- Tokens: 4 px spacing scale, card radius 12 px, control radius 8 px, pill radius 999 px, system type stack (no external fonts), tabular numerals for figures.
- Component anatomy recreated as HTML/CSS analogues (no React, no package): Card (header + body), Table (uppercase header row, avatar + name cell, status Badge pills), Button/bell with press state, navigation with section labels + active item, avatar initials, notice list with coloured dots.
- Accessibility per Astryx's contract: `aria-label`/`aria-current` on nav and bell, `role="img"` + text alternative on the CSS chart, `:focus-visible` outline, table `scope="col"`.
- Astryx CLI bootstrap (`astryx docs …`, `component --dense`) was NOT run: it requires npm installs, which the task forbids. Token values above are the neutral-theme adaptation from the skill file itself, not CLI output.

## Conflicts and resolutions
1. **Emil stagger vs Astryx restraint/accessibility.** Emil prescribes staggered entrances; Astryx's guidance stresses restraint and accessibility. Resolution: kept the stagger but minimal (40 ms steps, 240 ms total, transform + opacity only) and fully disabled under `prefers-reduced-motion`. Emil wins on motion detail, Astryx wins on the accessibility gate.
2. **Tokens vs motion ownership.** Emil gives no colour/radius/typography tokens; Astryx gives no motion values. No numeric clash: Astryx owns tokens and component anatomy, Emil owns easing, duration, and press feedback. Applied consistently throughout.
3. **Astryx is React-only; the task is single-file HTML.** Resolution: adapted, did not install. Components are hand-built HTML/CSS analogues; this is a deliberate, disclosed substitution, not a real Astryx build.

## Derived / added values (not in the fixed data — disclosed)
- Weekly trend average 95.0% and class averages (attendance 93.9%, marks 85.6%) are computed from the given figures.
- ₹4.8L pending = ₹30L − ₹25.2L, computed.
- Bell badge "3" = the three notices. Status badges (Excellent / Good / Needs support) are my added labels, thresholds not in the task. Notice sub-lines ("All classes · Main hall", "10:00 AM") are invented framing around the exact notice texts.
- An early draft claimed "6 per class on average" for teachers — unverifiable from the fixed data — and it was removed before finishing.

## Defects found and fixed
1. **Analytics text wrong:** class average attendance first written as 94.0%; recomputed from the 12 rows as 93.9% and fixed.
2. **Attendance meter bars invisible:** `.meter` was a `<span>` with `width`/`height` but no `display`, so the bars never rendered. Fixed with `display:inline-block` (verified in the re-rendered screenshot).
3. **Fixed strings hidden behind HTML entities:** a raw-text check missed "2025–26", "₹25.2L of ₹30L", and the footer text because they were written as `&ndash;` / `&#8377;` / `&mdash;`. Replaced those three strings with literal UTF-8 characters so the rendered text and the source text match.

## Self-check (headless Chromium, `chrome-headless-shell`)
- `document.documentElement.scrollWidth` vs `innerWidth`: 1265 vs 1280 at 1280 px; 375 vs 390 at 390 px — **no horizontal scroll at either width**.
- Screenshots eyeballed at 1280 px and 390 px: four stat cards in a row at desktop / stacked at mobile, chart + notices side by side at desktop, table scrolls inside its card at mobile, no clipped or overlapping text observed.
- Static check: all fixed strings present (school, class, year, all 12 students with roll/attendance/marks, trend 94/96/93/97/95, all three notices, footer text), 12 table rows, no external URLs, no external images/fonts, HTML tags balanced.
- Note: the system Chromium (`/opt/meta-chromium/chrome`) refused to start ("Hatch WebGL domain policy is required"); the check used the Playwright-cached `chrome-headless-shell` binary instead.

## Defects left
- **Cosmetic, left deliberately (no hand-polish rule):** at 390 px the notification bell wraps onto its own row above the full-width search. No overflow, clipping, or overlap — it is just a slightly awkward header stack. Not fixed, per the rule against hand-fixing the page to look better than the assigned skills produced.
- **Chart bars near-identical in height:** values 93–97% are plotted as a percentage of the chart height, so the differences are barely visible. That is faithful to the data; rescaling would misrepresent it. Left as is.
- **UNVERIFIED:** feel on a real device, touch behaviour of the drawer/bottom nav, and screen-reader behaviour were not checked — headless only.
