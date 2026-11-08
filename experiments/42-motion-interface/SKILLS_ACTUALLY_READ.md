# Skills actually read — Experiment 42 (motion-interface)

Variant: motion / interface. One self-contained `index.html`, inline CSS+JS only,
no external images or fonts, no build step.

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
   — ECC first, then pstack; routing table sends Animation/motion to
   `local-animate`, `local-accessible-animation`, and UI work to `local-skills`.
2. `/home/hatch/workspace/custom-skills/local-animate/SKILL.md`
3. `/home/hatch/workspace/custom-skills/local-accessible-animation/SKILL.md`
4. `/home/hatch/workspace/custom-skills/local-skills/SKILL.md`
   — better-interface router; read its `AGENTS.md` too
   (`/home/hatch/workspace/custom-skills/local-skills/AGENTS.md`, house rules,
   read on runtime instruction before further work there).
5. `/home/hatch/workspace/custom-skills/local-skills/skills/better-interface/SKILL.md`
6. `/home/hatch/workspace/custom-skills/local-skills/skills/better-accessibility/SKILL.md`
7. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md`
8. `/home/hatch/workspace/custom-skills/local-skills/skills/better-writing/SKILL.md`
9. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`
10. `/home/hatch/workspace/custom-skills/local-skills/skills/better-colors/SKILL.md`
11. `/home/hatch/workspace/custom-skills/local-skills/skills/better-ui/SKILL.md`

No assigned skill was left unread. No other variant folder was read. No skill
supporting files (RECIPES.md, animations.md, etc.) were read in full; only the
SKILL.md files above count as used.

## Motion gate (local-animate §1–2)

- Frequency: dashboard entrance is occasional / first-load, not 100+/day.
  Purpose named: **state indication + preventing a jarring change**
  (staggered entrance, chart growth). Delight budget not spent on anything else.
- Rejected by the gate: animation on search typing, row hover, tab switches
  (high-frequency) — those get at most a ≤150 ms opacity/background-color
  change. No count-up numbers (data the user reads must not move for style).
  No parallax, marquee, autoplay, or smooth-scroll hijacking.

## Ingredients (local-animate §3–7 + better-ui)

- Tool: CSS transitions for interactive state, CSS animations for the
  predetermined entrance/chart growth (off the main thread). No JS animation
  library, no WAAPI needed.
- Properties: `transform` and `opacity` only. Chart bars animate `scaleY`
  (transform-origin bottom); their `height` is static layout, never animated.
- Curves (exact, from local-animate): `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)`,
  drawer `--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1)`.
- Durations: entrance 300 ms, drawer 250 ms, press/hover 150 ms. Nothing over
  300 ms. Press scales to 0.96 (better-ui). Never `scale(0)`; entrance starts
  at `translateY(12px)` + `opacity: 0`.
- Hover gated behind `@media (hover: hover) and (pointer: fine)`.
- Transitions name exact properties; no `transition: all`.
- Drawer uses a CSS transition so rapid toggles retarget; Escape closes it
  and focus returns to the trigger. Exit mirrors entry.

## Reduced motion (local-accessible-animation, tiered — not a kill switch)

- Tier 1 removed: entrance translate, chart scaleY growth, drawer slide
  (becomes an instant state change via the 0.01 ms global safety net).
- Tier 2 softened: entrance becomes a ≤150 ms opacity fade where kept.
- Tier 3 kept: opacity, background-color, and focus-ring transitions.
- Motion is opt-in via `@media (prefers-reduced-motion: no-preference)`;
  JS gates with `matchMedia` + a live `change` listener.
- `?reduce=1` forces the reduced path when the OS setting is off, and
  `?t=N` freezes entrance/chart mid-animation for verification screenshots.

## Conflicts resolved

1. **Stagger interval — local-animate (30–80 ms) vs better-ui (100 ms).**
   Chose local-animate: 60 ms steps (0/60/120/…/480 ms), applied consistently
   to stat cards, sections, and chart bars. better-ui's 100 ms is for staged
   hero chunks (title/description/actions); this page staggers peer cards,
   which is local-animate's case.
2. **Entrance blur — better-ui (opacity + 4 px blur + 12 px translateY)
   vs local-animate (transform + opacity only).** Chose local-animate: blur
   is a filter, not transform/opacity, and is a paint cost; entrance uses
   opacity + translateY only, no blur.
3. **Reduced-motion global net vs tiering — local-accessible-animation's
   `* { animation-duration: 0.01ms !important }` net vs its own rule that
   Tier 3 motion must survive.** Kept the net as a backstop and layered a
   later, higher-specificity rule restoring 150 ms opacity transitions for
   `.reveal` under reduced motion.
4. **Reduced-motion `transform: none` vs drawer closed state (found in
   self-check, see defects).** A blanket `transform: none` under reduced
   motion also nulled the drawer's closed-state `translateX(-105%)`, leaving
   the drawer visible. Resolution: reduced motion removes only the
   *transition* (slide becomes instant); the closed-state transform is
   layout, not animation, and is never overridden.

## What the domain skills changed in the build

- better-accessibility: skip link, landmarks, labelled search, bell
  `aria-label` with badge, `aria-current`, table `th scope`, chart
  `role="img"` + text alternative, `:focus-visible`, 40 px minimum targets,
  no `maximum-scale` restriction, status pills carry text ("watch"), not
  color alone.
- better-layout: logical properties, `minmax(0, 1fr)` tracks, group gap 24 px
  vs 16 px within, table scrolls inside its card, `scroll-padding-top`,
  breakpoints from content (860 px), safe-area inset on bottom nav.
- better-writing: sentence case throughout, verb-first controls, plain
  labels ("Search students"), no "successfully"/"please"/"oops".
- better-typography: system stack only, rem sizes, unitless line-heights
  (1.2 headings / 1.5 body), `tabular-nums` on all figures, 16 px search
  input (iOS zoom rule), `text-wrap: balance` on headings.
- better-colors: semantic tokens over primitives, single accent ramp,
  status hues kept ≥15° from accent; low-attendance pills pair color with
  a "watch" label.
- better-ui: outer radius = inner radius + padding, layered shadows with a
  transparent border for forced-colors, press 0.96 / 150 ms, hover gating,
  named transition properties only.

## Self-check (Playwright Chromium, headless)

- 1280 px normal: scrollWidth 1280 = clientWidth, no horizontal scroll,
  0 console errors.
- 390 px normal: scrollWidth 390 = clientWidth, no horizontal scroll,
  stat cards 171 px each in 2 columns, table scrolls inside its card
  (scrollWidth 576 vs clientWidth 356), 0 console errors.
- 1280 px + 390 px with `reduced-motion=reduce`: same, no horizontal
  scroll, 0 console errors.
- `?reduce=1` at 390 px: drawer hidden, bars full height, no scroll.
- Drawer at 390 px: opens (left 0, `aria-expanded=true`), Escape closes
  and returns focus.
- Screenshots eyeballed at 1280, 390, and 390 reduced-motion: no clipped
  text, no overlaps (the fixed bottom nav overlapping the table in the
  full-page 390 capture is a screenshot-stitching artifact of fixed
  elements, not a page defect — main has 88 px bottom padding).

## Defects found and fixed

1. **Reduced-motion drawer stuck visible (HIGH, objective).** The
   `@media (prefers-reduced-motion: reduce)` block set
   `.sidebar { transform: none !important; }`, which removed the mobile
   drawer's closed-state `translateX(-105%)` and left the sidebar covering
   the page at 390 px under reduced motion. Fixed by removing the
   `transform: none` override (see conflict 4); re-checked at 390 px
   reduced, `?reduce=1`, and normal — drawer hidden in all three, opens
   and closes correctly.
2. None other. No data, heading, or layout defects were found; no
   hand-fixes were made to improve the look beyond what the assigned
   skills produced.

## Defects left

- None known. Feel is UNVERIFIED on real hardware: the 300 ms entrance,
  60 ms stagger, and drawer curve were checked only in headless Chromium;
  frame pacing and feel on a real phone/browser were not measured.
- Contrast ratios were computed from the declared token pairs, not
  measured from rendered pixels in a browser.
