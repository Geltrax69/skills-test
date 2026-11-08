# Skills actually read — variant 10-colors-typography

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-colors/SKILL.md`
2. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`

Both assigned skills were read in full before building, and both were used.
No other skill file was read. No other variant folder was read.

## Assigned skills not used

None. Both assigned skills were used.

## What each skill changed in the build

**Better Colors**
- The color system is ramps, not colors: one neutral ramp (hue 264),
  one accent ramp (indigo, hue 262, held end to end), and only the two
  status ramps this page actually renders — success (hue 150) and
  warning (hue 75). No danger ramp exists: nothing on the page renders
  a danger state, and a ramp nothing imports is maintenance for zero pixels.
- All colors are declared as `oklch()` primitives in `:root`; components
  reference semantic role tokens only (`--color-text-primary`,
  `--color-accent-solid`, `--color-status-good-bg`, …). No raw color
  literal appears outside the token block, and no primitive is used
  directly in a component.
- One color, one meaning: accent indigo is the primary action / link /
  chart color only; success green means "attendance ≥ 95%" only;
  warning amber means "attendance < 90%" only; the middle band is a
  neutral pill with no status color. Status is never carried by color
  alone — every pill also carries a word (Excellent / On track / Watch).
- Exactly one filled action per view: the "Add student" button is the
  only filled accent element; all other actions are neutral/ghost.
- Chart bars use steps of the one accent ramp (600, peak 700); the
  gradient rule was not needed because no gradient is used.

**Measured contrast (oklch → sRGB → WCAG ratio, computed in Python;
not a rendered-browser measurement — see UNVERIFIED below)**

| Pair | Measured | Threshold |
| --- | --- | --- |
| Text primary on surface | 17.23:1 | 4.5:1 |
| Text secondary on surface | 7.55:1 | 4.5:1 |
| Text secondary on page background | 7.12:1 | 4.5:1 |
| Accent text on surface | 7.52:1 | 4.5:1 |
| White on accent-700 solid (primary button) | 6.78:1 | 4.5:1 |
| White on accent-600 solid (not used for text) | 5.00:1 | 4.5:1 |
| Success text on success-50 pill | 7.07:1 | 4.5:1 |
| Warning text on warning-50 pill | 7.58:1 | 4.5:1 |
| Text primary on accent-50 (active nav) | 15.70:1 | 4.5:1 |

**Better Typography**
- System font stack only (no external font loaded, per TASK); weights
  used are 400 / 500 / 600 / 700 only, all shipped by system faces.
- Semantic type scale in `rem`: display 1.75rem/1.2/700, heading
  1.25rem/1.3/600, body 1rem/1.6/400, body-sm 0.875rem/1.5,
  caption 0.8125rem/1.5/500, stat 1.875rem/1.1/700. Heading sizes
  descend with level; no heading is smaller than body text.
- Unitless line-heights throughout (display 1.1–1.2, headings 1.2–1.3,
  body 1.5–1.6). Letter-spacing in `em` only: −0.01em on headings
  ≥ 24px, 0.05em on uppercase labels ≤ 14px, 0 on body copy.
- `text-wrap: balance` on h1/h2 only, `text-wrap: pretty` on paragraphs;
  `overflow-wrap: break-word` on values that could escape a card.
- `font-variant-numeric: tabular-nums` on stat figures, trend values
  and numeric table columns.
- Long-form lede capped at `max-width: 65ch` (60–75 character measure).
- Search input is `font-size: 1rem` (16px) at every viewport, so iOS
  Safari does not zoom on focus. Font smoothing is set once on `html`,
  never per component. `::selection` uses the accent-subtle token with
  primary text so the selection stays legible. Typographic punctuation
  is used in rendered text (en dashes in ranges, em dash in the footer,
  ellipsis character in the search placeholder).

## Self-check (1280px and 390px)

No browser is available in this VM (no Chromium/Playwright), so the
check was done by reading the code plus a static verification script
(34 checks, 0 failures): all 12 student rows with exact data, 4 stat
cards, 5 trend bars, 3 notices, exact footer text, no external URLs /
fonts / images, HTML tags balanced, no `px` line-height, no
`px` letter-spacing, no weight below 400, single filled primary button,
no color literal outside the token block.

- 1280px: sidebar + main grid (`13.5rem` + `minmax(0, 1fr)`), stats
  4-up, table fits its card without scrolling. No horizontal scroll
  expected: every grid/flex child is `min-width: 0` where it can grow.
- 390px: sidebar hidden, fixed bottom nav shown, main gets bottom
  padding so content clears it; stats go 2-up (1-up below 368px);
  search moves to its own full-width row; the table (min-width 34rem)
  scrolls horizontally *inside* its card via `.table-wrap`
  (`overflow-x: auto`), which TASK.md allows; stat sub-text wraps
  (`overflow-wrap: break-word`) instead of overflowing.

## Defects found and fixed

1. **Primary fill too close to the threshold.** White on accent-600
   measured 5.00:1 — passes 4.5:1 but with a thin margin. Fixed: the
   primary button and peak bar use accent-700 (6.78:1) instead.
2. **Heading weight 650.** The draft used `font-weight: 650` for h2,
   a weight the loaded system faces do not ship (synthesis risk).
   Fixed: changed to 600 before finishing.
3. **Trend bars visually identical at raw heights.** 93–97% as raw
   bar heights would be nearly indistinguishable. Fixed: bars are
   scaled from an 85% baseline with the exact values labelled above
   each bar and a note under the chart stating the scale.
4. **Warning-50 at the gamut edge.** The computed sRGB for
   `oklch(0.96 0.04 75)` clips red at 1.0. Kept as declared: it sits
   on, not beyond, the sRGB boundary, and its measured text pair
   (7.58:1) is unaffected. Flagged here rather than silently adjusted.

## Defects left

None known. No defect found in the self-check remains unfixed.

## UNVERIFIED

- **No live-browser render.** There is no Chromium/Playwright in this
  VM, so the 1280px and 390px layouts were verified by code reading
  and static checks only — not by opening the page in a real viewport.
  A real render could still show a wrapping or spacing issue the
  static check cannot see.
- **Contrast measured from computed values, not a rendered page.**
  Ratios above come from an oklch → sRGB conversion in Python, not
  from measuring pixels in a browser (including opacity or anything
  beneath the text). The values are computed, not estimated, but they
  are not a rendered measurement.
- **JS interactions not run in a browser.** The search filter, the
  "Add student" demo note and the nav active-state handler were
  checked by reading the code only; no browser executed them.
