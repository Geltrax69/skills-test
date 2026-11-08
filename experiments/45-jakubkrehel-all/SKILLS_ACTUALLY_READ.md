# Skills actually read — variant 45-jakubkrehel-all

Variant folder: `experiments/45-jakubkrehel-all/` — `index.html` + this file only.
No other variant folder was read. Nothing was committed or pushed.

## Read in full, in order (before building)

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — global harness entry (ECC first, then pstack; UI route via the jakubkrehel domain skills).
2. `/home/hatch/workspace/skills/skills/skills/better-typography/SKILL.md`
3. `/home/hatch/workspace/skills/skills/skills/better-layout/SKILL.md`
4. `/home/hatch/workspace/skills/skills/skills/better-colors/SKILL.md`
5. `/home/hatch/workspace/skills/skills/skills/better-accessibility/SKILL.md`
6. `/home/hatch/workspace/skills/skills/skills/better-writing/SKILL.md`
7. `/home/hatch/workspace/skills/skills/skills/better-ui/SKILL.md`

Also read, as instructed by the runtime house-rules notice for the skills repo:
- `/home/hatch/workspace/skills/skills/AGENTS.md` (repo authoring conventions; no design rules taken from it).

## Assigned skills NOT used

None. All six assigned skills (better-typography, better-layout, better-colors,
better-accessibility, better-writing, better-ui) were read in full and applied.

## How each skill shaped the page

- **better-typography:** system font stack only (no external fonts, per task);
  semantic scale — h1 1.75rem/1.2, h2 1.25rem/1.3, body 1rem/1.5, small
  0.8125rem/1.5, all unitless line-heights, all sizes in `rem`, weights 400+
  below 18px; `tabular-nums` on every changing numeric value; `text-wrap:
  balance` on headings, `pretty` on paragraphs; search input at 16px so iOS
  does not zoom; uppercase overline labels at 0.05em tracking.
- **better-layout:** space groups first (8px within groups, 16px+ between,
  32px between sections), logical properties (`margin-inline-*`,
  `padding-inline-*`, `inset-inline-*`) throughout, DOM order = visual order,
  `minmax(0, 1fr)` tracks, no `100vw`, `100dvh` for the sidebar, `min-height`
  never fixed height on text boxes, `scroll-padding-block-start` for the
  sticky header, breakpoints from content (900px stats/two-col, 760px nav
  collapse), table scrolls inside its card (`overflow-x: auto` on
  `.table-wrap`, never on the page).
- **better-colors:** one neutral ramp + one blue accent ramp + only the status
  ramps actually rendered (green/amber/red); primitives (`--blue-600`,
  `--neutral-900`…) never used in components — components use semantic tokens
  only (`--color-text-primary`, `--color-accent-solid`,
  `--color-bg-surface`…); one colour, one meaning; status is never carried by
  colour alone (badge text + dot, e.g. “On track” / “Watch” / “Needs
  support”); filled colour on exactly one primary action per view (the
  Search button).
- **better-accessibility:** skip link first in the DOM, landmarks
  (`header`/`nav`/`main`/`footer`), one `h1`, labelled search input,
  `aria-label` on the icon-only bell and menu button, `aria-expanded` on the
  menu, `:focus-visible` rings, 44px minimum targets on nav/bell/menu,
  `role="status"` live region for search results, `prefers-reduced-motion`
  respected, no `maximum-scale`/`user-scalable=no`, chart given a text
  `role="img"` description plus visible % labels.
- **better-writing:** sentence case everywhere except the typographic
  overline labels (see conflict below); plain direct copy; verb-first button
  (“Search students”); search placeholder shows an example
  (“e.g. Aarav or 8A-01”); empty-search message names the query and the way
  back (“Clear the search to show all 12 students”); no “successfully”,
  no “please”, no “Oops”.
- **better-ui:** outer radius = inner radius + padding (16px card / 12px
  inner); shadows for elevation, borders only for structure (table/divider);
  `border: 1px solid transparent` under shadow rings for forced-colors mode;
  press scale exactly `0.96` over 150ms `ease-out`, enabled buttons only;
  transitions name exact properties only (never `all`); hover styles gated
  behind `@media (hover: hover)`; icons use `currentColor`, one stroke
  weight (1.5px) matching the 400-weight text beside them; staggered
  entrance (100ms steps, opacity + 4px blur + 12px translateY over 300ms)
  on the stat cards only, gated behind `prefers-reduced-motion:
  no-preference`.

## Conflicts resolved

1. **better-writing (sentence case as default) vs better-typography
   (uppercase labels ≤14px take 0.05em tracking).** Resolution: typography
   wins for the four stat-card overlines only (“Students”, “Teachers”,
   “Attendance”, “Fees” rendered uppercase with 0.05em tracking, as a
   deliberate overline style); every other label, heading and button stays
   sentence case per better-writing. The two treatments never mix on one
   element type.
2. **better-ui (staggered entrance, press scale) vs better-accessibility
   (honour `prefers-reduced-motion`).** Resolution: accessibility wins —
   all motion (entrance stagger and press scale) runs only under
   `prefers-reduced-motion: no-preference`; under reduced motion all
   animation/transition is removed.
3. **better-layout (group with space, not lines) vs better-ui (borders for
   structure).** Resolution: layout wins for grouping — sections and cards
   are separated by space; borders are kept only where better-ui assigns
   them a structural job (table cell dividers, notice items, input/bell
   boundaries).
4. **better-colors (several coloured fills are fine when they encode distinct
   states) vs better-colors (one colour, one meaning).** No real conflict:
   green/amber/red encode three distinct student statuses, each also named
   in text, so each colour keeps one meaning.

## Self-check — defects found and fixed

Objective task violations only; no hand-polishing after building.

1. **FIXED — wrong derived figure.** The analytics line first said class
   average attendance was 94.0%. Recomputed from the 12 fixed attendance
   values (sum 1,127 ÷ 12 = 93.9%). Corrected to 93.9%; marks average
   85.6% (1,027 ÷ 12) was already correct.
2. **FIXED — header overflow at 390px.** Static width audit: menu (44px) +
   title + search input (140px) + button (~134px) + bell (44px) + gaps and
   padding exceeded 390px, so the top bar would have forced horizontal
   scroll. Fixed by wrapping the search form to a full-width second row
   below 760px (`flex-wrap: wrap`, search `flex: 1 1 100%`); the first row
   is then menu + title + bell only.
3. **Checked by script, all pass:** all 12 students present with correct
   roll/attendance/marks, all fixed figures present (1,248 / 72 / 95.2% /
   1,188 present / 60 absent / 84% / ₹25.2L of ₹30L), trend values
   94/96/93/97/95, all three notices verbatim, footer text verbatim,
   13 table rows (1 header + 12 students), no external URLs, no `<img>`,
   no `<link>`, no `100vw`, no fixed `100vh`.

## Defects left / not verified

- **Live-browser render at 1280px and 390px NOT run.** No Chromium/Playwright
  binary is available in this VM (`which chromium google-chrome` returned
  nothing), so no-horizontal-scroll, no-clipping and no-overlap at either
  width are established by code audit + the static checks above, not by a
  rendered screenshot. Needs a real-browser check by the parent.
- **Contrast ratios NOT measured with a tool.** Pairs were chosen from the
  skills' guidance (dark neutral text on white, white text only on the
  600-level blue fill, 700-level text on 100-level status backgrounds) but
  no numeric contrast measurement was run; the 4.5:1 / 3:1 claims are
  therefore unmeasured, not verified.
- **Notice `datetime` values are derived, not in the task.** The visible
  notice text is verbatim from the task; the machine-readable `datetime`
  attributes (2026-11-13, 2026-11-20, 2026-11-25) were added for
  accessibility and verified against the calendar (13 Nov 2026 is a Friday,
  20 Nov 2026 is a Friday, 25 Nov 2026 is a Wednesday). If the parent
  prefers no derived dates, they can be removed without touching visible copy.
