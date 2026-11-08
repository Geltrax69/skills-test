# Skills Actually Read — Variant 16-opensourceui-pstack-ecc

## Skills read in full, in order

1. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/SKILL.md` — pstack (poteto-mode).
   Then its Feature playbook, read in full:
   `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md`.
2. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC frontend-patterns
   (reference clone only; its install script was never run, no `ecc:` command was used).
3. `/home/hatch/workspace/custom-skills/local-opensourceui/SKILL.md` — OpenSourceUI Muse adapter.
   Then its catalog, searched (not read cover-to-cover):
   `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/catalog.md`.
   Component sources read in full before adapting (paths from `source_inventory.txt`):
   - `/home/hatch/workspace/ui-skills/opensourceui/components/table/customers-table.tsx`
   - `/home/hatch/workspace/ui-skills/opensourceui/components/dropdowns/notification-dropdown.tsx`
   - `/home/hatch/workspace/ui-skills/opensourceui/components/widgets/daily-activity-calendar-widget.tsx`
   House rules read as instructed by the runtime (not assigned skills, listed for completeness):
   - `/home/hatch/workspace/build-anything/ECC/AGENTS.md`
   - `/home/hatch/workspace/ui-skills/opensourceui/AGENTS.md`

## Assigned skills not used

None. All three assigned skills were read in full and applied. Two pstack leaf skills the
Feature playbook names (`how`, `architect`) were not read: this is a single self-contained file
with a fixed task and fixed data, so there was no subsystem to investigate and no competing
design to explore in parallel; the playbook's throughput checkpoint was applied inline instead
(see below). No subagent was spawned (depth limit); the Feature playbook's delegate step was
therefore done by the owner directly, with review separation kept by verifying the built file
after writing it rather than during.

## How each skill shaped the build

- **pstack / Feature playbook.** Throughput checkpoint, written before building:
  - Blocking first steps: read TASK.md and all three skills in full before any code.
  - Independent workstreams: n/a — single file, one owner, no fan-out.
  - Shared mutable state: n/a — one file; the only shared state is the single `STUDENTS`
    data model rendered from one place.
  - Smallest safe decomposition: one owner, one file, verify after writing.
  Principles applied: Model the Domain (one `STUDENTS` array + `statusFor()` deriving the
  status pill, instead of scattered conditionals); Prove It Works (JS executed headlessly
  under a DOM stub, see Self-check); Laziness Protocol (no extra libraries, no extra
  sections beyond the seven required plus the calendar widget).
- **ECC frontend-patterns.** Composition over inheritance (Card / CardHeader / CardBody
  structure reused for stat cards, trend, notices, table); a debounced search (the skill's
  `useDebounce` pattern, 200 ms, in vanilla JS); keyboard navigation and Escape/outside-click
  close on the notification dropdown (the skill's Dropdown / focus-management patterns);
  derived state kept pure (sort/filter produce a new array, never mutate `STUDENTS`).
  React itself was not used: the fixed constraint is one self-contained HTML file, so the
  patterns were translated to vanilla HTML/CSS/JS rather than copied as React.
- **OpenSourceUI.** Neutral stage throughout: paper white, ink `#171717`, hairline `#e5e5e5`,
  sparse emerald/amber/rose accents only for state; flat surfaces, no gradients, no glow,
  no glassmorphism; `rounded-md` buttons/chips, `rounded-lg` cards; base styles plus `md:`
  (768px) variants only, no `sm:` breakpoints; input focus is a darker border, no coloured
  ring; `prefers-reduced-motion` respected. Three components adapted to single-file HTML
  (React/Tailwind/`cn()`/`lucide-react`/`next/image` removed; inline SVG bell/search icons
  drawn by hand; initials avatars replace the source's external avatar images):
  1. **Customers Table** (`table/customers-table.tsx`) → Class 8-A student table. Kept:
     header row with title + count and a right-hand meta figure (MRR → class average marks
     85.6%), `overflow-x-auto` table with `min-width`, avatar + name + sub-line cell, row
     hover, tabular numerals. Changed: plan → status pill (Excellent / Good / Needs support,
     derived from attendance), last-active column dropped (roll shown instead), search +
     sort chips added.
  2. **Notification Dropdown** (`dropdowns/notification-dropdown.tsx`) → header bell.
     Kept: square bell trigger, unread badge (3), gray inset tray, each notice a small
     bordered card row with title/body/time, Clear (mark-all-read) action, Escape and
     outside-click close, `aria-expanded`/`aria-haspopup`/`role="menu"`. Changed: the three
     fixed notices replace the demo notifications; clearing shows "Nothing new".
  3. **Daily Activity Calendar Widget** (`widgets/daily-activity-calendar-widget.tsx`) →
     November 2026 calendar in the side column. Kept: dark card, 7-column grid, emerald
     active days, orange highlighted today. Changed: fixed to November 2026 (today = Mon 9 Nov,
     verified with `date`), school days Mon–Fri active, legend + note added.

## Conflicts resolved

- **OpenSourceUI says "copy the component into a React/Next.js + Tailwind v4 project"; TASK.md
  says one self-contained HTML file, no build, no npm.** TASK.md wins. Components were
  hand-translated to plain HTML/CSS/JS, preserving layout, states and behaviour, not pasted
  as React. Noted as an adaptation above, not a silent substitution.
- **ECC is written for React/Next.js; the target is vanilla HTML.** Same resolution: patterns
  (composition, debounce, keyboard nav, pure derived state) were applied; framework code was not.
- **OpenSourceUI design rules use Geist / Instrument Serif; TASK.md allows system fonts and
  forbids external fonts.** System font stack used; no external font loaded.
- **ECC AGENTS.md prescribes TDD and 80%+ coverage; the task is a single static file.**
  No test suite exists for a static page. Compensated with the headless JS execution check
  below (data, sort, calendar cell count) plus a static content audit.
- **pstack Feature playbook says delegate code-writing to a subagent; this run cannot spawn
  one.** Done by the owner directly; verification was done after the build as a separate
  step (see Self-check).

## Self-check at 1280px and 390px

No Chromium/Playwright in this VM, so no live render. Checked by (a) executing the page's
`<script>` headlessly under a minimal DOM stub in Node, (b) a static audit of the HTML/CSS,
and (c) verifying every fixed number with Python.

- Content audit: all 12 students present with exact names/rolls/attendance/marks; stat cards
  1,248 / 72 / 95.2% / 84%; 1,188 present, 60 absent; ₹25.2L of ₹30L; trend 94/96/93/97/95
  (average 95.0% as shown); all 3 notices verbatim; footer literal
  "Fictional demo data — UI Forge Experiment 1"; class average 85.6% (1027/12 = 85.58%).
- JS execution: `node --check` clean; stub run renders 12 table rows, first row Priya Das
  (marks sort descending, correct), count text "12 students", calendar grid 37 cells
  (7 weekday headers + 30 days).
- 1280px: sidebar 232px + content max 1080px; stats 4 columns; table full width (min-width
  520px fits inside the ~800px card); bell tray 264px inside the viewport.
- 390px: sidebar hidden, bottom nav shown; stats 2 columns; header search shrinks
  (`min-width:0`, `flex:1`); bell tray capped at `calc(100vw - 32px)`; table scrolls inside
  its card via `.table-wrap{overflow-x:auto}`; no element uses a fixed width wider than the
  viewport (widest fixed widths: sidebar 232px and search 420px max, both gated to `md:` or
  `max-width`); `box-sizing:border-box` everywhere; viewport meta present.
- No external URLs, fonts, images or scripts in the file (grep-verified).

## Defects found and fixed

1. **Calendar leading blanks wrong.** First draft used 6 leading blanks with a comment
   claiming 1 Nov 2026 was a Sunday. `date -d 2026-11-01` shows it IS a Sunday, so the
   correct leading count is 0, and the active school days are 2–6 Nov (Mon–Fri), not 3–6.
   Fixed: `leading = 0`, `active = [2,3,4,5,6]`, highlight day 9. Verified: 37 cells.
2. **Footer used an HTML entity for the dash.** The uniform check string
   "Fictional demo data — UI Forge Experiment 1" did not match literally because the file
   had `&mdash;`. Replaced with the literal — character in the footer.

## Defects left

- None known in code. Layout at both widths is established by code inspection and the
  checks above, not by a rendered screenshot.

## UNVERIFIED

- **No live-browser render or screenshot** at 1280px or 390px (no Chromium/Playwright in
  this VM). No-horizontal-scroll, no-clipping and no-overlap at both widths are therefore
  verified by code inspection and static checks only, not by observation.
- **No real interaction test** (bell open/close, search typing, sort chips) in a browser;
  the handlers were executed only under the Node DOM stub, which does not test layout,
  focus visibility, or touch behaviour.
- **OpenSourceUI components were adapted, not copied**: the React sources were read and
  hand-translated; no upstream build/test of the adapted HTML was run, and the upstream
  `verify` gate was not run here.
