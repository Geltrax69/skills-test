# Variant 06 — OpenSourceUI — Skills Actually Read

Assigned skill: **OpenSourceUI only.** No other UI skill was read.

## Files read in full, in order

1. `/home/hatch/workspace/skills-test/TASK.md` — the fixed task, data, required sections and constraints.
2. `/home/hatch/workspace/custom-skills/local-opensourceui/SKILL.md` — the Muse adapter for OpenSourceUI (read in full before building).
3. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/SKILL.md` — upstream OpenSourceUI agent skill (workflow, design rules, catalog use).
4. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/design.md` — upstream design system (tokens, typography, elevation, component rules, do/don't).
5. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/implementation.md` — upstream implementation guide (stack, repo layout, copy-paste rules).
6. `/home/hatch/workspace/ui-skills/opensourceui/AGENTS.md` — upstream multi-agent setup guide (house rules for that repo; read on the runtime's instruction).
7. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/catalog.md` — searched (not read cover-to-cover) for stat / widget / table / notification / input entries; used to pick the three components below.

## Components copied / adapted (source files read in full)

All three are from `~/workspace/ui-skills/opensourceui/components/`, adapted from React/Tailwind (`cn()`, `forwardRef`, `lucide-react`, `next/image`) to plain HTML/CSS/JS in a single file, because this variant is a no-build single-file page. The adaptation is noted in the HTML comments.

1. `/home/hatch/workspace/ui-skills/opensourceui/components/inputs/search-input.tsx` — **Search Input**. Kept: h-10, rounded-lg, `border-neutral-200`, focus border `neutral-900` with `ring-0`, search icon left, clear button when a value is present, `role="searchbox"`, sr-only label. Adapted: `forwardRef`/`cn()`/lucide dropped; clear button is plain JS that clears, re-filters and returns focus. Used as the header search; it live-filters the student table.
2. `/home/hatch/workspace/ui-skills/opensourceui/components/table/customers-table.tsx` — **Customers Table**. Kept: header row (title + count + right-hand summary), `overflow-x-auto` wrapper, `min-width` table, hairline row dividers, row hover on neutral-50, tabular numbers, truncated name + sub-line. Adapted: `next/image` avatars replaced with initials circles (no external images allowed); columns changed to Student / Attendance / Marks (not Customer / Plan / MRR); summary changed to class average (Marks 85.6% · Attendance 93.9%).
3. `/home/hatch/workspace/ui-skills/opensourceui/components/widgets/step-count-widget.tsx` — **Step Count Widget**. Kept: editorial stat-card pattern — small muted label, big light tabular number, label line, footer row with goal/context + %. Adapted: the source uses `rounded-3xl` + a shadow; `design.md` says flat by default and `rounded-lg` for small cards, so the stat cards here use `rounded-lg` and no shadow. One card per stat (Students, Teachers, Attendance, Fees), in a 2-column grid on mobile / 4-column on desktop.

Design language applied from `design.md`: paper-white stage, ink `#171717`, body `#404040`, muted `#737373`, hairline borders `#e5e5e5`/`#f5f5f5`, flat surfaces, rose `#f43f5e` only for the notice dots / bell badge, cyan `#a5f3fc` only as a highlight mark on the Friday chart note, Instrument Serif (system serif fallback, no external fonts) for display headlines, system sans for UI text, focus by darker border only, base + `md:` (768px) breakpoint only.

## Assigned skill NOT used

None. OpenSourceUI was used. (Its upstream `source_inventory.txt` was not read — the catalog + component files were sufficient to locate the three components; nothing was invented about their APIs.)

## Other skills

None read. No other UI skill, no other variant folder was read.

## Self-check — 1280px and 390px

Method: code-reading check plus automated checks — no headless browser is available in this VM (no Chromium/Chrome binary), so no rendered screenshot was taken. JS syntax checked with `node --check` (PASS). All TASK.md data verified present by script (12 table rows, all 12 students, all three notices, all four stats, footer text). No external URLs, images, fonts or scripts in the file.

Layout reasoning, 1280px: sidebar 220px + content, 4 stat cards in a row, table `min-width:480px` fits inside the card without scrolling, chart bars fit.
Layout reasoning, 390px: sidebar hidden, bottom nav shown; stat grid is 2 columns, each cell ≈173px — the largest value "1,248" at 36px fits; header wraps (title, then full-width search, then bell); bell panel is `min(320px, 100vw − 32px)`; table is 480px minimum inside an `overflow-x-auto` wrapper, so it scrolls inside its card and the page does not scroll horizontally.

Defects found and fixed:
1. **Typo in the inline script** — `addEventListener` was mistyped, which would have broken live search. Fixed; `node --check` now passes.
2. **Wrong class average** — I first wrote Attendance 94.1%; the 12 attendance values sum to 1127, i.e. 93.9%. Fixed (Marks 85.6% was already correct: 1027/12).
3. **"sm:" string in the file** — the automated check flagged `sm:`; it appears only in comments stating the design rule "base + md: only, no sm: breakpoint". No `sm:` media query or class exists. Left as-is (comments are intentional).

Defects left: none known from the code-reading check.

## UNVERIFIED

- **No rendered check:** no headless browser in this VM, so the page was not opened at 1280px or 390px and no screenshot was taken. The no-horizontal-scroll / no-clipping / no-overlap claims above rest on code reading and the measurements stated, not on a render.
- **No live-browser check** of the search filter, bell panel, or bottom nav interaction (JS syntax is valid, behaviour is untested in a browser).
- A real OpenSourceUI component was not built in a React/Next project here — the three components were adapted to plain HTML/CSS, as the task requires; that adaptation itself is what is being tested, not a copy-paste React integration.
