# Skills Actually Read — Variant 43, accessibility-interface

Variant: `experiments/43-accessibility-interface/` — student management
dashboard (Greenwood High, Class 8-A, 2025–26). One self-contained
`index.html`, inline CSS + JS only, no external images or fonts.

## Skills read in full, in order

A skill counts as used only if its `SKILL.md` was read in full before
building. All nine below were.

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
   — entry router. ECC first, then pstack; UI work routes to
   `local-skills` (`better-interface`); verify before reporting.
2. `/home/hatch/workspace/skills/skills/skills/better-accessibility/SKILL.md`
   — assigned primary skill (task item 1).
3. `/home/hatch/workspace/custom-skills/local-skills/SKILL.md`
   — better-interface router (task item 2).
4. `/home/hatch/workspace/custom-skills/local-skills/skills/better-interface/SKILL.md`
   — orchestrator; review order accessibility, layout, writing,
   typography, colors, UI; escalation triggers are HIGH on sight.
5. `/home/hatch/workspace/custom-skills/local-skills/skills/better-layout/SKILL.md`
6. `/home/hatch/workspace/custom-skills/local-skills/skills/better-writing/SKILL.md`
7. `/home/hatch/workspace/custom-skills/local-skills/skills/better-typography/SKILL.md`
8. `/home/hatch/workspace/custom-skills/local-skills/skills/better-colors/SKILL.md`
9. `/home/hatch/workspace/custom-skills/local-skills/skills/better-ui/SKILL.md`

Notes on scope:

- `better-accessibility` was read from the `~/workspace/skills/skills/`
  path named in the task. The identical copy bundled under
  `local-skills/skills/better-accessibility/` was not re-read separately;
  it is the same skill, not a second one.
- The supporting reference files beside each `SKILL.md`
  (`semantics-and-aria.md`, `focus-and-keyboard.md`, `contrast.md`, and
  the rest) were NOT read in full. Only the `SKILL.md` principle
  statements were applied.
- The two `AGENTS.md` files in those repos
  (`~/workspace/skills/skills/AGENTS.md`,
  `~/workspace/custom-skills/local-skills/AGENTS.md`) were read on a
  runtime instruction. They are house rules, not skills.
- No assigned skill went unused. The verb skills `interface-review`,
  `variant`, `break`, `state-machine` and `explain-interface` are
  user-invoked per `local-skills` and were correctly not invoked.

## How each skill shaped the page

- **better-accessibility (primary):** skip link is the first focusable
  element; landmarks `header` / `nav` / `main` / `footer`, one `h1`;
  native `<button>`/`<a>`/`<input>`/`<table>` only, no `div onClick`;
  labelled search (`<label for>`), bell as a real link with
  `aria-label="Notifications: 3 notices"`; 44px minimum targets;
  custom `:focus-visible` ring (see Conflicts); `scroll-padding-top`
  equal to the sticky header; status pills carry words, never colour
  alone (WCAG 1.4.1); chart is `role="img"` with a full text
  `aria-label` and its visual values `aria-hidden`; search announces
  through a stable pre-rendered `role="status"` region; motion gated
  behind `prefers-reduced-motion`; viewport has no `maximum-scale=1`
  or `user-scalable=no`; table wrapped in a keyboard-focusable
  `role="region"` with an `aria-label`.
- **better-layout:** group gaps are 2x inner gaps; logical properties
  (`margin-inline`, `padding-inline`, `inset-inline`) throughout;
  `minmax(0, 1fr)` tracks and `min-width: 0` on flex children; no
  `100vw`; breakpoint (900px) taken from where the sidebar stops
  fitting, not a device preset; table scrolls inside its card with a
  visible hint line; sticky header plus `scroll-padding-top`.
- **better-writing:** sentence case everywhere; verb-first and
  destination-describing labels; plain words; fixed task copy used
  verbatim (notices, footer); no "successfully", no "please", no
  "Click here".
- **better-typography:** system font stack only (task forbids external
  fonts); scale 1.5rem h1 / 1.25rem h2 / 1rem body / 0.875rem captions;
  unitless line-heights (body 1.5); `tabular-nums` on all numbers;
  `text-wrap: balance` on headings only; search input at 16px so iOS
  does not zoom; no weight below 400 under 18px.
- **better-colors:** one neutral ramp, one accent ramp (deep green),
  status hues 15 degrees or more from the accent; semantic tokens only
  in components (`--color-text`, `--color-bg-accent`, …), hex notation
  throughout; dark-on-light pairs chosen for 4.5:1 (see Left).
- **better-ui:** outer radius = inner radius + padding; layered
  transparent shadows with a transparent border underneath for
  forced-colors mode; transitions name exact properties only, 150ms
  `ease-out`; press scale 0.96 gated behind `no-preference` and
  `hover: hover`; no animation on high-frequency interactions (row
  hover, tab switch); icons use `currentColor`/text, not separate
  assets.
- **better-interface / global-harness:** domains reviewed in the
  router's order; escalation triggers treated as blockers; nothing
  hand-fixed after building to beat the skills (see Defects).

## Conflicts resolved

1. **Focus ring:** `better-accessibility` prefers the browser's
   unmodified indicator; a custom ring needs a project focus token.
   Resolution: custom `:focus-visible` ring using the token
   `--color-focus`, 3px solid with a 2px offset, reaching 3:1 against
   adjacent colours. `outline: none` never appears as a rule.
2. **Colour fixes:** `better-colors` says leave colours alone when a
   pair fails and change them only when asked. That governs fixing an
   existing system. This is a new system, so high-contrast pairs were
   chosen at build time instead of "fixed" later.
3. **Press scale vs reduced motion:** `better-ui` wants 0.96 press
   scale; `better-accessibility` owns the reduced-motion requirement.
   Resolution: scale and hover styles live inside
   `@media (prefers-reduced-motion: no-preference)` and
   `@media (hover: hover)`; under `reduce`, all transitions and
   animations are removed.
4. **Breakpoint vs fixed check widths:** `better-layout` says
   breakpoints come from content, while the task checks 1280px and
   390px. Resolution: the 900px breakpoint is where the sidebar stops
   fitting; both required widths were then verified headless.
5. **Table at 390px:** the task allows the table to scroll inside its
   card, and `better-interface` escalates content reachable only past
   a scroll edge with no visible cue. Resolution: the scroll region is
   keyboard-focusable, labelled, and has a visible "swipe sideways"
   hint line.

## Defects found and fixed (objective task violations only)

1. Header was inside the `.app` flex row on desktop, so it would have
   rendered beside the sidebar instead of above it. Moved `<header>`
   out of `.app`.
2. Chart bar percentage heights overflowed the 200px track (value +
   bar + label exceeded the column height). Added a `.bar-track` flex
   child so percentages resolve against the track. Bars now render at
   118–123px, proportional to 93–97%.
3. Duplicated `box-shadow` in `.card`, the first with an invalid alpha
   (`/ 06`). Removed the broken line.
4. Dead empty `.table-wrap::after` rule. Removed.
5. Bottom nav: at 390px the last item ("Notices", `LI`/`A`) extended
   to x=423, past the 390px viewport — reachable only by scrolling the
   nav. Gave nav items equal flex shares with smaller padding and
   font size. Re-verified: zero elements outside the viewport.
6. Two static-check FAILs turned out to be check artefacts, not
   defects: `outline: none` appears only inside a CSS comment, and the
   section-order check was matching CSS selectors before the body.
   Body order confirmed: header, nav, overview, attendance, students,
   notices, footer.

## Defects left / not verified

- No screen-reader run, no keyboard-only tab walk in a live browser,
  no 200% zoom check, no 320px check (headless checks were 1280px and
  390px only). Live-browser look and feel are UNVERIFIED.
- Contrast pairs were chosen dark-on-light by design (text `#17251c`
  on white, muted `#5c6e62` on white, accent `#14532d` on white, white
  on `#14532d`) but were not measured with a contrast tool.
  Instrument-measured contrast is Not verified.
- No real-device touch check; 44px targets are by CSS only.
- The chart's `role="img"` + `aria-hidden` children pattern and the
  `role="status"` element doubling as `aria-describedby` were not
  verified with a screen reader.
- Desktop sticky nav assumes a 64px header (`top: 64px`). Not observed
  wrong at 1280px, but if the header ever wraps taller, a few pixels
  of nav could sit under it.

## Verification (headless Chromium, Playwright shell)

- Static checks: 50/50 pass — all fixed data, all 12 students, trend
  values, notices, footer text, landmarks, labels, focus styles,
  reduced-motion guard, no external URLs, tag balance clean.
- Render at 1280px: `docScrollW` 1265 (scrollbar only), no horizontal
  scroll, stat cards 228px with no overflow, table fits its card
  (596/596), no overflowing elements, bars 118–123px.
- Render at 390px: `docScrollW` 375, no horizontal scroll, stat cards
  162px (2 columns) with no overflow, table scrolls inside its card
  (309 client / 560 scroll), no overflowing elements, bars 118–123px.
- Live search: typing "priya" leaves 1 visible row and announces
  "Showing 1 of 12 students for “priya”." via the status region.
- System Chrome (`/opt/meta-chromium/chrome`) refused to start
  (Hatch WebGL domain policy); the Playwright headless shell was used
  instead. Screenshots were not eyeballed.
