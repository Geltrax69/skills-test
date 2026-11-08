# Skills Actually Read — Variant 18-emil-alone

## Skills read in full (in order)

1. `/home/hatch/workspace/custom-skills/local-emil-design-eng/SKILL.md` — read in full before building. This was the ONLY skill assigned and the ONLY skill read.

## Assigned skills NOT used

- None. `emil-design-eng` was assigned and was used. No other skill was assigned, and no other skill file (or any other variant folder) was read.

## How the skill was applied

- Custom easing curves only: `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)`, `--ease-in-out`, `--ease-drawer`. No built-in `ease-in` anywhere (ease-in is never used for UI animation per the skill).
- Every `transition` names exact properties — never `transition: all`.
- Press feedback: `transform: scale(0.97)` on `:active` for nav items, bell, avatar; `scale(0.98)` on notices; `scale(0.94)` on bottom-nav items.
- No element animates from `scale(0)`: the bell popover enters from `scale(0.95)` + opacity, with `transform-origin: top right` (origin-aware, scaling from its trigger, not center).
- Motion is transform/opacity only: chart bars and meters grow via `transform: scaleX/scaleY` with `transform-origin` at the base; no width/height/padding animation.
- Stagger: stat cards enter 0/40/80/120 ms apart; notices 40/80/120 ms. Delays kept in the 30–80 ms band between items.
- Transitions, not keyframes, for the interruptible/dynamic UI (popover, bars, meters); the entrance stagger uses the skill's own keyframe pattern and never blocks interaction.
- Hover gated behind `@media (hover: hover) and (pointer: fine)` so touch taps don't stick.
- `prefers-reduced-motion: reduce` removes movement (keeps opacity/color only).
- No animation on the frequent path: sidebar nav switching changes state instantly, no transition on the active change itself.

## Self-check (code-read; no headless browser available in this VM)

Checked at 1280px and 390px by reading the code and by grep verification:

- 12/12 student rows present with exact names/rolls/attendance/marks; 4/4 stat cards (1,248 / 72 / 95.2% / 84%); trend 94/96/93/97/95; 3/3 notices; footer text exact; zero external URLs, fonts, or images.
- 1280px: sidebar 232px + content max-width 1080px; stats 4 columns via `minmax(0, 1fr)`; chart+notices side by side (stacks ≤1020px); table `min-width: 560px` inside `.table-wrap { overflow-x: auto }`, so it scrolls inside its card rather than overflowing the page.
- 390px: sidebar hidden, bottom nav shown (fixed, `safe-area-inset-bottom` respected, content/footer get 96px bottom padding so nothing hides behind it); stats become 2 columns with `minmax(0, 1fr)` and stat value drops to 24px so "95.2%" cannot clip; header hides the title suffix and avatar so search + bell fit; popover capped at `calc(100vw - 32px)`; notice tags hidden to avoid crowding; table still scrolls inside its card.

## Defects found and fixed

1. **Stat cards could overflow at 390px** — initial 4-column grid would squeeze values. Fixed: 2-column grid ≤900px, `minmax(0, 1fr)` everywhere, smaller stat value at mobile.
2. **Table could force page-level horizontal scroll at 390px** — fixed with `overflow-x: auto` on `.table-wrap` and `min-width: 560px` on the table so scrolling is contained inside the card.
3. **Bottom nav could cover footer/content on mobile** — fixed with 96px bottom padding on `.content` and `.footer` at ≤720px.
4. **Popover could overflow a 390px viewport** — fixed with `max-width: calc(100vw - 32px)`.
5. **`transition: all` / `ease-in` / `scale(0)` patterns** — grep-verified absent from the stylesheet (only a code comment mentions `transition: all` as a prohibition).

## Defects left

- None known from the code-read check.

## UNVERIFIED

- **No live render/screenshot check**: no Chromium or Playwright exists in this VM, so the 1280px/390px checks above are code-read only — actual rendered overflow, clipping, or overlap has NOT been visually confirmed.
- **Motion feel**: easing/press/stagger timing follows the skill's numbers but has not been eyeballed in a real browser; perceived responsiveness is unverified.
- **Touch behaviour**: bottom-nav press feedback and popover tap behaviour not tested on a real touch device.
