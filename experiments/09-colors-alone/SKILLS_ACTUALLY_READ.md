# Skills Actually Read — Variant 09-colors-alone

## Skills read in full (in order)

1. `/home/hatch/workspace/custom-skills/local-skills/skills/better-colors/SKILL.md`
   — the ONLY assigned skill. Read in full before building.

## Assigned skill NOT used

None. Better Colors was read in full and applied (see below).

## Other files read

- `~/workspace/skills-test/TASK.md` (the fixed task, read in full first, per the assignment)
- `~/workspace/custom-skills/local-skills/AGENTS.md` (house rules for the skills repo, read in full when prompted by the runtime before continuing work there)

No other skill was read. No other variant folder was read or opened.

## How Better Colors was applied

- **Ramps, not colors:** one neutral ramp (hue 262, low chroma), one accent ramp
  (hue 262), plus only the status ramps this page renders (success 155, warning 85,
  danger 25). No second accent hue.
- **Every step has a job:** each ramp step maps to a role (page bg, surface, border,
  solid fill, body text); no step exists that no role consumes.
- **Primitives vs semantics:** `--<hue>-<step>` primitives are never applied;
  `--color-<role>` semantic tokens point at primitives and are the only tier
  components reference. No hex / `oklch()` literal and no primitive appears in
  any component rule.
- **Hue held across each ramp**, vividness peaking mid-ramp, light-end steps
  ~0.04–0.05 OKLCH L. Single notation throughout: `oklch()`.
- **One color, one meaning:** accent 262 / success 155 / warning 85 / danger 25 are
  all >15° apart. Status is never carried by color alone — every chip also carries
  a text label (On track / Watch / At risk) and a dot.
- **Exactly one filled action per view:** only "+ Add student" gets the filled
  accent background; the bell and all other buttons stay neutral.
- **Gradient interpolation space:** the single header strip is one hue, interpolated
  `in oklab` (the default).
- **Contrast measured, not estimated:** all 15 rendered foreground/background pairs
  were computed from the declared OKLCH values (OKLCH → sRGB → relative luminance
  → WCAG ratio) with a Python script. Results:

  | Pair | Ratio | vs 4.5:1 |
  | --- | --- | --- |
  | text-primary on bg-page | 16.97:1 | PASS |
  | text-secondary on bg-page | 7.13:1 | PASS |
  | text-muted on bg-page | 5.06:1 | PASS |
  | text-primary on bg-surface | 17.72:1 | PASS |
  | text-secondary on bg-surface | 7.44:1 | PASS |
  | text-muted on bg-surface | 5.28:1 | PASS |
  | white on accent-solid (primary button) | 5.44:1 | PASS |
  | accent-text on accent-subtle (active nav) | 6.82:1 | PASS |
  | accent-text on bg-surface | 7.70:1 | PASS |
  | accent-text on surface-raised (row hover) | 6.75:1 | PASS |
  | success-text on success-bg (chip) | 6.87:1 | PASS |
  | warning-text on warning-bg (chip) | 7.13:1 | PASS |
  | danger-text on danger-bg (chip) | 7.40:1 | PASS |
  | white on danger-solid (bell badge) | 5.95:1 | PASS |
  | placeholder muted on search bg | 5.06:1 | PASS |

  The same measured values are recorded in the comment block at the top of
  `index.html`.

## Self-check (1280px and 390px)

No Chromium/Playwright is available in this VM, so the check was by reading the
code plus scripted content checks (12/12 student rows, 12/12 rolls, 4/4 stats,
5/5 trend bars, 3/3 notices, footer text exact, zero external URLs, div tags
balanced 45/45).

- **1280px:** sidebar 232px + content max-width 1120px; stats grid 4 columns;
  table fits its card without scrolling. No horizontal scroll expected.
- **390px:** sidebar hidden, bottom nav shown; stats grid 2 columns
  (~171px each, `minmax(0, 1fr)` so no overflow); search wraps to its own row;
  header row (title + bell + button) measures ~354px < 390px; table
  `min-width: 560px` sits inside `.table-scroll { overflow-x: auto }`, so it
  scrolls inside its card, not the page. No horizontal scroll expected.

## Defects found and fixed

1. **Contrast FAIL — muted text on page background (found by measurement, fixed).**
   `--neutral-500` at OKLCH L 0.56 measured 4.46:1 against `--color-bg-page`
   (and the search placeholder on the same background) — below 4.5:1. Per the
   skill, the fix changes lightness, not hue: darkened the step to L 0.53.
   Re-measured: 5.06:1 on bg-page, 5.28:1 on bg-surface. PASS.
2. **Draft contrast values in the file comment were estimates (fixed).** The first
   draft of the CSS comment block listed round-number ratios written before
   measuring. Replaced with the computed values from the table above.
3. **Chart bars overflowed their chart box (found by code reading, fixed).**
   Bar heights were percentages of a `.bar-wrap` that also had to hold the value
   label and day label, so value + bar + label exceeded the 180px chart height
   and would have overflowed upward into the card head. Fixed by adding a
   `.bar-track` flex child (`flex: 1; min-height: 0; align-items: flex-end`) so
   bar percentages resolve against the track only, with labels outside it.

## Defects left

None known. No HIGH color findings remain; per the skill's reporting format:
**Approve** on the color system as inspected (token values, gamut of declared
colors, measured pairs). Note the skill's own caveat: without a browser, the
background actually rendered behind the text (including opacity / stacking) was
not measured — see UNVERIFIED below.

## UNVERIFIED

- **No live-browser render.** No Chromium/Playwright exists in this VM, so the
  page was never opened at 1280px or 390px. The no-horizontal-scroll,
  no-clipped-text and no-overlap verdicts rest on code reading and the
  measurements above, not on a rendered screenshot. A real render could still
  reveal a layout defect (e.g. header wrapping at exactly 390px, bottom-nav
  label fit, chart bar rendering).
- **Rendered-pair contrast in a browser** (actual pixels behind text, including
  any opacity/stacking) was not measured; only the computed ratios from the
  declared token pairs were. The two are expected to match because all tokens
  are solid (no alpha / `color-mix`), but that was not confirmed in a browser.
- **OKLCH support** in the viewing browser is assumed (all modern browsers);
  no fallback sRGB values were declared, since the page is a single-file demo
  and `oklch()` is the project's established notation per the skill.
