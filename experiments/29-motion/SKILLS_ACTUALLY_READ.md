# SKILLS_ACTUALLY_READ — variant 29-motion

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/local-animate/SKILL.md` — read in full before building. USED.
2. `/home/hatch/workspace/custom-skills/local-accessible-animation/SKILL.md` — read in full before building. USED.

No other skill file was read. No other variant folder was read.

## Assigned skills NOT used

None — both assigned skills were used.

## What each skill changed in the build

- **animate (Emil Kowalski philosophy):** gate first — no animation on search/keyboard actions (100+/day tier); entry/chart/count-up only on the rare once-per-load tier; hover kept near-imperceptible (2px lift, 180ms). Exact tokens from the skill: `--ease-out cubic-bezier(0.23,1,0.32,1)`, `--ease-in-out cubic-bezier(0.77,0,0.175,1)`, `--ease-drawer cubic-bezier(0.32,0.72,0,1)`; transform/opacity only; no `scale(0)`, no `ease-in`, no `transition:all`, no width/height animation; 30–80ms stagger (40ms sections, 60ms bars); CSS animations for predetermined entry/chart motion, rAF only for the dynamic count-up; drawer 250ms with symmetric exit; hover gated to `(hover:hover) and (pointer:fine)`.
- **accessible-animation (tiered reduced motion):** Tier 1 removed under `prefers-reduced-motion: reduce` (entry/drawer/chart translate & scaleY, hover lift); Tier 2 softened to a ≤150ms opacity fade; Tier 3 kept (colour/background/focus-ring transitions). Global `0.01ms` safety net (not `0s`, so transitionend still fires) with intentional Tier-3 overrides layered after it. JS count-up gated by `matchMedia` with a live `change` listener; `?reduce=1` forces the reduced path and `?t=N` freezes CSS animations mid-flight for screenshots.

## Self-check (1280px and 390px)

Method: code-read check + static validation script (77 assertions: all 12 students/rolls, all 4 stats, trend values, 3 notices, footer text, section counts) + `node --check` on the extracted inline JS. **No headless browser was available in this VM** (no Chromium/Playwright), so no live render/screenshot was possible — see UNVERIFIED below.

Checks passed:
- 1280px: sidebar 220px + content max-width 1120px; stats 4 columns; table min-width 560px fits inside its card without page scroll.
- 390px: stats collapse to 2 columns (col ≈174px; largest value "95.2%" at 25px fits); header search wraps to full width, subtitle hidden; sidebar becomes an off-canvas drawer (translateX −102%) with overlay; table scrolls inside its card (`overflow-x:auto`); chart gap shrinks to 10px.
- No external URLs/fonts/images, no `transition:all`, no `scale(0)`, no `ease-in`, no infinite animation, no parallax/marquee/spin.

## Defects found and fixed

1. **False-positive check failures, not code defects:** the first validation run flagged `transition:all`, `scale(0)`, `marquee`, `parallax` — all were words in the file's header comment explaining what was rejected. Re-ran with comments stripped: all absent from actual code. No code change needed.
2. **Dead CSS:** unused `.rise-6` stagger class removed.

## Defects left

None known from the code-read check.

## UNVERIFIED (needs a live browser)

- No live render at 1280px or 390px — horizontal scroll, stat-card fit, drawer behaviour, and chart-bar percentage heights (`height:94%` etc. inside a flex track) are verified by code reading only, not by a screenshot.
- Motion feel (entry stagger, bar-grow timing, count-up easing, drawer curve) not eyeballed; the `?t=N` / `?reduce=1` screenshot harness is implemented but not exercised, and the normal vs. reduced-motion screenshots the accessible-animation skill requires were not captured.
- Reduced-motion tiering (fade surviving the `0.01ms` global net via specificity) is reasoned from CSS cascade rules, not observed.
