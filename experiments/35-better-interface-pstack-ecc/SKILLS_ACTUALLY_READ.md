# Variant 35 — better-interface + pstack + ECC (no emil)

## Skills actually read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — entry router; ECC first (structure), then pstack (execution).
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md` — Muse adapter for pstack.
3. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md` — Feature playbook.
4. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC frontend patterns (reference clone, read-only, untouched).
5. `/home/hatch/workspace/custom-skills/local-skills/SKILL.md` — better-interface router (13-skill bundle).
6. `/home/hatch/workspace/skills/skills/skills/better-accessibility/SKILL.md`
7. `/home/hatch/workspace/skills/skills/skills/better-layout/SKILL.md`
8. `/home/hatch/workspace/skills/skills/skills/better-writing/SKILL.md`
9. `/home/hatch/workspace/skills/skills/skills/better-typography/SKILL.md`
10. `/home/hatch/workspace/skills/skills/skills/better-colors/SKILL.md`
11. `/home/hatch/workspace/skills/skills/skills/better-ui/SKILL.md`

Also read (house rules surfaced by the runtime, not assigned skills):
- `/home/hatch/workspace/build-anything/ECC/AGENTS.md`
- `/home/hatch/workspace/custom-skills/local-skills/AGENTS.md`
- `/home/hatch/workspace/skills/skills/AGENTS.md`

## Assigned skills NOT used

None. All 11 assigned files were read in full before building and each shaped the page (see below).
Not read: `better-interface/SKILL.md` itself (the router file, not one of the 6 domain skills assigned), the
supporting reference files linked from the domain skills (e.g. `semantics-and-aria.md`, `contrast.md`),
and the pstack leaf skills (`tdd`, `architect`, `arena`, etc.) — the Feature playbook routes to them only
as steps fire, and none fired for a single static file. No emil skill was used, per the brief.

## What each skill changed (measured)

- **global-harness**: precedence ECC structure over pstack execution; 80% coverage treated as an
  aspiration, not a gate for a one-file demo; verification is the headless self-check below, reported
  honestly as UNVERIFIED in a real browser.
- **local-pstack / Feature playbook**: throughput checkpoint — Blocking first steps: read all skills,
  fix the data set. Independent workstreams: n/a, single file, no fan-out. Shared mutable state: n/a,
  one static page, no shared writes. Smallest safe decomposition: one author, one file, because
  delegation would add coordination with no parallel slice. Playbook step 4 (delegate implementation
  to a subagent) was not followed literally: this agent cannot spawn subagents, so it owned the diff
  directly, which the playbook itself allows for a subagent forbidden to spawn. Steps 6–8 (rebase into
  commits, interrogate, open a PR) are n/a: do not commit or push per the brief.
- **ECC frontend-patterns**: adapted, not ported. Card/Header/Body composition became semantic
  `<section class="card">` blocks with a head and body; the controlled-form pattern became the labeled
  search input with live filtering and a `role="status"` result count; keyboard/focus patterns became
  native buttons/links, `:focus-visible` rings, Escape closes the mobile nav, focus returns via the
  menu button. React hooks, lazy loading, virtualization, and code splitting do not apply to a static
  single file with 12 rows (see Conflicts).
- **better-accessibility**: skip link first, landmarks (`header`/`nav`/`main`/`aside`/`footer`),
  native `<button>`/`<a>` only, `aria-label` on the icon-only bell, `aria-expanded` on the menu button,
  44px minimum targets, chart given `role="img"` with a text equivalent, status pills carry text labels
  (never color alone), motion gated behind `prefers-reduced-motion`, viewport has no `maximum-scale`.
- **better-layout**: group gap 32px is 2x the 16px within-group gap, logical properties
  (`margin-inline-start`, `inset-inline-start`, `padding-inline-end` via `inset-inline`), DOM order
  matches visual order, breakpoints from content (1080px stats/split collapse, 860px nav collapse),
  `minmax(0, 1fr)` tracks, `min-width: 0` on grid children, table scrolls inside its card via
  `.table-wrap { overflow-x: auto }`, `scroll-padding-top` for the sticky header, `100dvh` for the
  off-canvas sidebar, no `100vw`.
- **better-writing**: sentence case throughout, one vocabulary (Students / Teachers / Attendance / Fees),
  verb-first control labels ("Search students…", "Open navigation"), plain copy, no "successfully" or
  "please", notices reproduced verbatim, footer reproduced verbatim.
- **better-typography**: system font stack only (no external fonts per constraints), descending scale
  (h1 1.5rem, h2 1.125rem, body 1rem), unitless line-heights (1.2 headings, 1.5 body),
  `tabular-nums` on stat values, chart values and table numbers, search input at 16px (iOS zoom rule),
  `text-wrap: balance` on headings, `text-wrap: pretty` on paragraphs, sizes in `rem`, weights 400+.
- **better-colors**: one neutral ramp + one accent ramp (`#1d4ed8`) + only the status steps actually
  rendered (green/amber/red pills); semantic tokens only in components, hex notation consistently;
  one color, one meaning (accent = primary/interactive only); status never carried by color alone;
  contrast pairs chosen to pass 4.5:1 for text (dark `#1a2332`/`#4d5866` on white/page, white on
  `#1d4ed8`, dark status text on light status backgrounds). Contrast values were chosen from known
  pairings, not measured in a browser; see Defects left.
- **better-ui**: outer radius 12px = inner 8px + padding, shadows for elevation with transparent
  borders, transitions name only the changing property (`background-color`, `transform`), press scale
  exactly 0.96 over 150ms, hover styles behind `@media (hover: hover)`, motion gated behind
  `prefers-reduced-motion: no-preference`, icons via `currentColor`/text glyphs only.

## Conflicts resolved

1. **ECC frontend-patterns is React/Next.js; the task is one static HTML file.** Resolved by
   adapting patterns (composition, controlled form, focus management) to semantic HTML + vanilla JS
   and skipping React-only patterns (hooks, lazy loading, virtualization, code splitting). The 12-row
   table needs no virtualization.
2. **ECC 80% coverage / TDD vs. a one-file demo.** Resolved per the global harness: coverage is a
   check, not the goal, and 80% is not a gate for a small UI demo. Verification is the static +
   parse self-check below, not a test suite. No tests were padded or invented.
3. **Feature playbook "delegate implementation to a subagent" vs. no spawning at depth 2.**
   Resolved by owning the diff directly, which the playbook allows for a delegate forbidden to spawn,
   keeping the same review separation (build, then self-check against the task).
4. **better-ui staggered entrances / press scale vs. better-accessibility reduced-motion.**
   Resolved by gating all motion behind `prefers-reduced-motion: no-preference` and keeping the only
   motion the press scale; no entrance stagger was added (subtract before you add).
5. **better-colors prefers `oklch()` for a new system vs. a file with no existing system.**
   Resolved by using hex consistently throughout, which the skill allows ("a consistent hex system
   beats hex with oklch() scattered through it").

## Defects found and fixed (objective task violations only)

1. **Chart bars overflowed their container (found in self-check, fixed).** First build set each
   bar's height as a percentage of the whole 180px chart column, but the column also held the value
   label and day label, so value + bar + day exceeded the column height and bars spilled above the
   chart. Fixed by adding a `.bar-track` flex child that owns the bar height; labels now sit outside
   the track. Chart height raised to 200px so the 93–97% range stays visibly distinct.
2. **No other defects found.** Static check passed on first build: HTML parses with no unclosed or
   stray tags, all 12 student rows with exact name/roll/attendance/marks, all 3 notices verbatim,
   all 4 stat values, trend 94/96/93/97/95, footer verbatim, no external URLs/fonts/images,
   no `100vw`, no `maximum-scale`.

## Defects left / not verified

- **Live render at 1280px and 390px NOT verified.** No Chromium/Playwright in this sandbox
  (`which chromium` empty, no `playwright` module). The 1280/390, no-horizontal-scroll,
  no-clipped-text claims rest on static reasoning: `minmax(0, 1fr)` grids, `min-width: 0`, table
  scrolling inside its card, wrapping topbar with full-width search at mobile, 2-column stats at
  390px, bottom nav with `padding-bottom` clearance. A real-browser check is still needed.
- **Contrast ratios NOT measured in a browser.** Pairs were selected to pass (see above) but no
  computed contrast was measured; `better-colors` requires measured values, so these are
  chosen-not-measured.
- **Feel on real hardware NOT verified.** Press scale, hover gating, and nav behavior are
  code-correct but untested by hand.

## Self-check method

Python `html.parser` tag-balance check (0 errors), string checks for every fixed data point
(12/12 students, 3/3 notices, 4/4 stats, 5/5 trend values, footer verbatim), `node -e "new Function(js)"`
for the inline script (parses OK). No browser was available, so no screenshot or scroll measurement.
