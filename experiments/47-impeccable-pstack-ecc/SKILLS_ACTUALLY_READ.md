# SKILLS_ACTUALLY_READ — Variant 47 (impeccable + pstack + ECC)

## Skills read in full, in order (before building)

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md`
3. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md`
4. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` (reference clone, read-only, not modified)
5. `/home/hatch/workspace/ui-skills/impeccable/skill/SKILL.src.md`

## Additional references read (prescribed by skill 5 itself, before editing)

- `/home/hatch/workspace/ui-skills/impeccable/skill/reference/craft-floor.md` — read immediately before the UI edit, as the skill instructs.
- `/home/hatch/workspace/ui-skills/impeccable/skill/reference/operate.md` — Operate-mode depth for dashboards.
- `/home/hatch/workspace/build-anything/ECC/AGENTS.md` — house rules for the ECC folder (prompted by the runtime on first touch; read-only).
- `/home/hatch/workspace/ui-skills/impeccable/AGENTS.md` — house rules for the impeccable folder (prompted by the runtime on first touch; read-only).

## Assigned skills NOT used

None of the five assigned skills went unused. Notes on partial applicability:

- **pstack feature playbook, step 4 (delegate implementation to a subagent):** not executable. This session is depth 2/2 with no spawn ability, so I owned the diff directly with the same review separation (build, then self-check). No nested agent was launched.
- **pstack feature playbook, step 6 (rebase into small ordered commits) and step 8 (Opening a PR):** skipped. The task forbids committing and pushing. No commits were made.
- **ECC 80% coverage / TDD workflow:** not applicable to a single static HTML file with no build step. Verification was a headless-style static check instead (see below), per pstack prove-it-works.
- **impeccable `impeccable context` launcher:** not run. It downloads/runs an external binary and there is no PRODUCT.md/DESIGN.md for this one-off demo; the task forbids npm/build steps and external assets. I read the existing context directly instead, which the skill itself prescribes as the fallback. No `PRODUCT.md`/`DESIGN.md` was invented.
- **impeccable `shape`/`new-work` references:** not loaded. The task fixes the content, sections, order and data exactly, so there was no visual-world decision to make; the mode was Operate (dashboard), taken from the task itself.

## What each skill changed in the build

- **global-harness (ECC first, then pstack):** ECC structure governed the page skeleton (plan first, composition, accessibility patterns); pstack governed execution (prove-it-works verification, subtract-before-you-add). Single file, no extra files created.
- **local-pstack / feature playbook:** throughput checkpoint — blocking first steps (read skills, fix data), independent workstreams n/a (single file, one worker), shared mutable state n/a (no shared state), smallest safe decomposition = one worker owns the whole file (named reason: single self-contained file, no fan-out possible). Verifiable units: data correctness, then layout, then interaction.
- **ECC frontend-patterns:** applied in vanilla-JS translation (no React allowed by the task). Composition via repeated card/table structure; controlled search input (debounce not needed for 12 rows); keyboard/focus patterns (Escape closes drawer and bell panel, focus-visible rings, `aria-sort` on sortable headers, `aria-expanded` on toggles); error/empty state (the "No students match your search" row).
- **impeccable (Operate mode + craft floor):** one system sans family, fixed rem scale, tight type ratio, restrained palette (paper `#f4f2ec`, ink, single Greenwood green accent, semantic amber/red only for flags); no eyebrow/kicker above headings; no gradient text; no glassmorphism; no hard offset shadows (soft offset+blur only); radii 12–16px; inline SVG icons in one consistent stroke (no emoji/glyph icons); tabular numerals for data; themed `::selection` and focus rings; 150–250 ms transitions that convey state only (hover, drawer); no page-load choreography; real content everywhere, working controls (search filter, column sort, drawer, bell panel).

## Conflicts resolved

1. **Impeccable craft floor bans the "hero-metric template" (big number + small label) and identical icon+heading+text card grids — the task REQUIRES four stat cards (Students 1,248 / Teachers 72 / Attendance 95.2% / Fees 84%).** Resolution: the brief wins (impeccable's own rule). The four cards exist with their exact required values, but they are not identical clones: each carries a different supporting detail (class size, teacher ratio, present/absent split bar, fees progress track) and a different icon tint, so the grid reads as four distinct facts, not four copies of one template.
2. **ECC frontend-patterns is written for React/Next.js (hooks, memo, virtualization) — the task requires one self-contained HTML file, inline CSS+JS only, no build step, no npm.** Resolution: applied the pattern spirit in vanilla JS (composition, controlled input, a11y patterns) and skipped React-specific mechanics (hooks, memo, code-splitting, virtualization — 12 rows need none of it).
3. **ECC demands 80%+ test coverage and TDD — a static demo page has no test runner and the task forbids npm.** Resolution: coverage aspiration recorded as not applicable; verification is the static/headless-style check below (prove-it-works, not a coverage number).
4. **pstack feature playbook mandates delegating code-writing to a subagent — this session cannot spawn.** Resolution: I owned the diff directly; the self-check below is the review separation.
5. **Impeccable bans a system display face and wants a sourced display font — the task forbids external fonts.** Resolution: system sans stack throughout (Operate mode permits system fonts; no display face is used anywhere).

## Derived values (computed from the fixed data, not invented)

- Teacher ratio "1 teacher per 17 students": 1,248 / 72 = 17.3.
- Weekly attendance average 95.0%: (94+96+93+97+95) / 5 = 95.0.
- Fees still due "₹4.8L": ₹30L − ₹25.2L.
- Status pills: green "On track" (attendance ≥90% and marks ≥80%), amber "Needs attention" (attendance <90% or marks <80%), red "Needs attention" (marks <75% — Vikram Joshi, 74%). Rule is stated in the table footer. The two "Steady" rows (Rohan Mehta 92/82, Aditya Rao 93/84) meet the green thresholds but sit in the middle of the class on both measures; labelled "Steady" to distinguish them from the top group. This labelling is presentation judgement, not fixed data.

## Self-check (static; no live browser available in this session)

- Node `new Function()` parse of the inline script: OK.
- Python `html.parser` tag-balance check: no mismatches, nothing unclosed.
- All required strings verified present by script: school, class, year, all four stats, present/absent counts, fee amounts, all 12 students with roll/attendance/marks, all five trend values, all three notices, footer text verbatim.
- External references: 0 `http(s)` URLs, 0 `<img>` / external `<link>` — no external images or fonts.
- Layout reasoning (not a render): `box-sizing: border-box` throughout; sidebar is off-canvas below 1024px with a fixed bottom nav; stats go 4 → 2 columns; the table wrapper has `overflow-x: auto` with `min-width: 620px` so it scrolls inside its card at 390px; the bell panel is capped at `100vw − 2rem`; the topbar title is hidden below 480px so the search keeps room.

## Defects found and fixed (objective task violations / breakage only)

1. **Chart bars could overflow their 190px track.** First version put the value label, a percentage-height bar and the day label in one flex column, so the column content exceeded the track height. Fixed by wrapping each bar in a `.bar-track` (`flex: 1; min-height: 0`) so the percentage height resolves against the track alone.
2. **Topbar title squeezed the search input at 390px.** Fixed by hiding the (redundant — school name is also in the sidebar brand and page head) topbar title below 480px and giving the search `min-width: 0`.
3. **Status pill tiers were unexplained.** Vikram Joshi (88/74) was red while Kabir Singh (89/76) was amber with the same label text. Fixed by stating the tier rule in the table footer (amber: attendance <90% or marks <80%; red: marks <75%).

## Defects left / unverified

- **No live render check.** No Chromium/Playwright is installed in this sandbox, so the 1280px and 390px renders were verified by code reading and the static checks above, not by screenshots. Horizontal scroll, clipping and overlap at both widths are UNVERIFIED in a real browser — the parent should run the uniform render check. Nothing was hand-fixed for looks after building; the three fixes above are objective breakage/rule-clarity fixes, listed here.
- **DOM order vs section order.** In the DOM the sidebar (`<aside>`) precedes the header/main column, because it is the left flex column. Visually the order is header → nav → stats → chart → table → notices → footer, as required. If the uniform checker asserts DOM order strictly, this will flag; it was not changed because the layout structure requires it.
