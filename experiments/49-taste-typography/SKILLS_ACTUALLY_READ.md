# Variant 49 — taste + typography: skills actually read

## Skills read in full, in order (before building)

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
   — read first per the task. Applied only as routing: ECC structure first, then
   pstack execution; single-file UI demo, so no delegation, no ECC 80% coverage
   gate (a check, not the goal for a small UI demo), verify before reporting.
2. `/home/hatch/workspace/ui-skills/taste-skill/skills/taste-skill/SKILL.md`
   — taste-skill 2.5.0, the anti-slop frontend skill.
3. `/home/hatch/workspace/skills/skills/skills/better-typography/SKILL.md`
   — better-typography.

No other skill file was read in full. No other variant folder was read or copied.
(`~/workspace/skills/skills/AGENTS.md` was also read, as required by a runtime
house-rules note for that repo; it is repo authoring guidance, not a design skill,
and changed nothing in the build.)

## What each assigned skill contributed

### Taste (taste-skill 2.5) — applied where the task allows it
- Kind declared on the page: software dashboard
  (`<html data-type="software">`, `<meta name="page-type" content="software">`,
  `<meta name="site-kind" content="software">`).
- One system throughout: one button style, one card style, one radius scale
  (12px / 8px), flat colour grounds, plain copy, one hard fact per section head.
- Equal left/right content edges: a single `.wrap` (max-width 72rem,
  1.5rem side padding desktop, 1rem mobile) shared by header, shell and footer.
- Hover micro-interactions only: cards lift 2px with a soft shadow, nav links
  and bell respond on hover, no tilt, no carousel, no scroll-linked animation,
  no reveal/fade-in/parallax (taste §6).
- No violet or lavender anywhere in the page's own design (taste §6):
  palette is warm paper `#f6f4ef`, white surfaces, slate ink `#17252f`,
  teal accent `#0f766e` / `#0b5e57`, amber `#b45309` for the fee notice only.
- No caption under any visual (taste house rule 1); there are no images at all.

### Better-typography — applied throughout
- One font family only (system stack, no external font per task constraints):
  `system-ui, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`.
- Semantic rem type scale, one size + unitless line-height + weight per role:
  display 1.75rem / 1.2 / 700, h2 1.25rem / 1.3 / 650, body 1rem / 1.6 / 400,
  small 0.875rem / 1.5 / 400, caption 0.8125rem / 1.5 / 600, stat 2rem / 1.1 / 700.
  Headings descend with level (h1 28px > h2 20px > body); no weight 100–300
  below 18px anywhere.
- Letter-spacing in `em` only: −0.02em on display/stat (≥24px), −0.01em on h2,
  0.05em on uppercase labels ≤14px, 0 on body copy.
- `text-wrap: balance` on headings, `text-wrap: pretty` on paragraphs,
  `text-align: start`, `overflow-wrap: break-word` on stat values.
- `font-variant-numeric: tabular-nums` on stat values, stat notes, chart
  values, table numeric columns and the bell badge.
- Search input is `font-size: 1rem` (16px), so no iOS zoom; font smoothing
  (`antialiased` / `grayscale`) set once on `html`, never per component.
- Typographic punctuation: en dashes in “2025–26” and “Monday to Friday”
  wording, “…” in the search placeholder, em dash in the footer line.
  No `font-feature-settings`, no px line-heights, no `justify`.

## Conflicts and resolutions

1. **Taste wants photographs / real product UI on every screen; the task
   forbids external images and fonts.** Taste §2–§3 (12–20 photographs on a
   place page, software's own UI as live HTML, real full-colour icons) cannot
   be satisfied in a self-contained file with no external assets. Resolution:
   task constraints win. No images, no icons-as-files; the bell is a text
   glyph, the brand mark is a flat teal “G” tile. Taste's system discipline
   (one system, equal edges, flat grounds, hover layer, no violet) was applied
   instead of its photographic programme. Recorded, not silently dropped.
2. **Taste wants one section per screen, hero alone on the first screen;
   the task requires all seven sections in one dashboard in a fixed order.**
   Resolution: task wins. The dashboard is a dense tool, not a landing page;
   sections stack in the required order with consistent 1.5rem gaps, and the
   header + stat cards occupy the first screen at 1280px without a hero.
3. **Taste's deal step (deal.mjs / start.mjs, Node + Chrome checker) vs the
   task's no-build-step, no-npm, single-file constraint.** Resolution: no
   dealer scripts were run and no starter.html was used; palette, pairing and
   hero were chosen by hand from the skill's rules (flat teal on warm paper,
   soft radius) and recorded here. The skill's `consistency.mjs` / `check.mjs`
   / `shoot.mjs` browser checks could not be run (no Chromium/Playwright in
   this sandbox — see Self-check).
4. **Taste “one type moment (name set large)” vs typography “headings descend
   with level”.** No real conflict: the school name is the h1 and the largest
   type on the page (1.75rem), with every h2 smaller (1.25rem).

## Self-check

Static verification only (Python string checks over the built file); no
headless browser was available in this sandbox (no Chromium/Chrome binary,
no Playwright module), so the 1280px / 390px render is **UNVERIFIED in a real
browser** and must be eyeballed before the variant is ranked on look/feel.

Checks that passed:
- All fixed data present and exact: Greenwood High, Class 8-A, 2025–26;
  1,248 students, 72 teachers, 12 in Class 8-A; 95.2% (1,188 present,
  60 absent); fees 84% (₹25.2L of ₹30L); all 12 student rows with exact
  name / roll / attendance% / marks%; trend 94, 96, 93, 97, 95;
  all three notices verbatim; footer exactly
  “Fictional demo data — UI Forge Experiment 1”.
- Sections in the required order in the body: header → sidebar nav →
  stats → attendance chart → student table → notices → footer.
- No external URLs, no `<link>`, no `<img>`, no `@import`, no gradients,
  no violet/purple hex in the palette.
- Week average in the chart footnote recomputed: (94+96+93+97+95)/5 = 95.0%.

Layout reasoning (not browser-measured):
- 1280px: sidebar 232px + main; stats 4 columns; table min-width 576px fits
  the ~880px main column without scrolling.
- 390px: sidebar hidden, fixed bottom nav (4 links, safe-area padding);
  stats 2 columns (each ~171px; “95.2%” at 2rem and the uppercase labels fit);
  table scrolls inside its card via `.table-scroll { overflow-x: auto }`
  (min-width 576px table), so the page itself has no horizontal scroll;
  chart bars are flex columns with `min-width: 0`.

## Defects found and fixed

1. **Mobile header crowding (found by measurement reasoning, fixed).**
   At 390px the brand mark + “Greenwood High” + fixed-width search + bell
   summed to ~424px against 358px of available width, so the search would
   have been squeezed to ~75px. Fix: below 48rem the brand mark is hidden,
   the h1 drops to 1.25rem, and the search becomes `flex: 1; min-width: 0`
   so it takes the remaining space. This is an objective task-constraint
   fix (no clipped/overflowing header), not a taste polish pass.
2. **Check-script false alarms (not page defects, no change made).**
   An early grep-style check flagged “violet” (the word appears in a CSS
   comment stating “no violet”) and section order (`.site-footer` first
   occurs as a CSS rule before the body sections). Re-checked by index in
   the body: palette hexes contain no violet hue and body order is correct.

## Defects left

- **No live-browser verification.** 1280px / 390px rendering, horizontal
  scroll, stat-card overflow and table scrolling were reasoned about, not
  measured in a browser. Left for the experiment's render pass.
- **Taste's photographic / effect programme is absent by design** (see
  conflict 1): no photographs, no shader effect, no dealt hero composition.
  This is a deliberate, recorded consequence of the task constraints, not
  an oversight — but it means the variant cannot show what taste-skill
  does at its best.
- **Search placeholder is the only affordance for filtering**; there is no
  visible “clear” control. Minor; the input clears by deleting text.

## Assigned skills NOT used

None. Both assigned skills were read in full before building and both
shaped the page (taste: system/edges/hover/no-violet; typography: scale,
line-height, spacing, tabular numerals, input size). The global-harness
entry skill was also read first, per the task.
