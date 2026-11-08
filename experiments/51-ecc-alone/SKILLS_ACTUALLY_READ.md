# SKILLS_ACTUALLY_READ — Variant 51-ecc-alone (ECC ALONE, no pstack)

## Skills read in full, in order
1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — entry router (ECC first, then pstack; ECC structure wins on conflict).
2. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md` — ECC reference clone, read-only, never modified.
3. `/home/hatch/workspace/build-anything/ECC/skills/frontend-design-direction/SKILL.md` — ECC reference clone, read-only, never modified.

No pstack skill or playbook was read for this variant. No other variant folder was read or copied. No other skill file was read in full.

## Design direction chosen (frontend-design-direction)
- Purpose: a teacher/admin's daily scan of one class — attendance first, then students, then notices.
- Audience: a class teacher repeating this workflow daily; needs scannable numbers, not marketing.
- Tone: utilitarian, dense, calm (a SaaS operations tool, not a landing page).
- Memorable detail: the "Attendance today" strip — one teal-accented line (95.2%, 1,188 present, 60 absent) above the stat cards.
- Constraints: single self-contained HTML file, system fonts only, no external assets, 1280px + 390px, no horizontal scroll.
- Palette: multi-dimensional by design (teal / blue / amber / green accents on ink + warm paper), per the skill's "keep palettes multi-dimensional" rule. No purple gradient, no decorative blobs, no oversized hero, no cards inside cards (stat cards, chart card, table card and notices card are siblings, never nested).

## What was applied from frontend-patterns (adapted to vanilla HTML — it is React-oriented)
- **Composition over inheritance / Card, CardHeader, CardBody**: every section is a `.card` with `.card-header` + `.card-body`; stat cards are the same Card with a coloured top border.
- **Context + Reducer pattern**: one immutable `state` object (`query`, `sortKey`, `sortDir`, `sidebarOpen`) and a `dispatch(action)` function; every update builds a new object with `Object.assign({}, state)` — never mutated in place (also the ECC immutability principle).
- **useToggle**: sidebar open/closed is a boolean in state, toggled by the hamburger / bottom-nav Menu button, with `aria-expanded` and a scrim to close.
- **useDebounce**: search input dispatches `SET_QUERY` only after 300 ms of no typing.
- **Memoization / "copy before sorting"**: the table renders a filtered, `slice().sort()`-ed copy; the `STUDENTS` source array is never mutated.
- **Controlled form + validation state**: the search is a controlled input; a no-match search shows the empty state ("No students match your search" + Clear search button) instead of a blank table.
- **Accessibility patterns (keyboard navigation / focus management)**: `Escape` closes the sidebar, focus is returned to the search box after clearing, `:focus-visible` outlines, `aria-label`/`aria-current` on nav, `prefers-reduced-motion` disables transitions.

## What was inapplicable from frontend-patterns (recorded, not silently dropped)
- React/Next.js specifics: `useState`/`useReducer`/`useContext` hooks, JSX, `React.memo`, `useCallback`, `Suspense`/lazy code-splitting, `@tanstack/react-virtual` virtualization (12 rows need no virtualization), Framer Motion (`AnimatePresence`/`motion`), Zod schemas, SWR/React Query data fetching, error boundaries as classes — none of these exist in vanilla HTML; each was adapted to the plain-JS equivalent listed above or omitted as not applicable to a 12-row static-data page.
- The skill gives no colour, typography, spacing or layout tokens — all visual decisions came from frontend-design-direction.

## What was applied from frontend-design-direction
- Evidence contract: product surface / job / hierarchy / target viewports (1280, 390) recorded here; content states recorded below.
- Content states: **Empty** (no-match search → explanation + Clear action) is implemented. **Loading / Error / Partial / Success / Permission** do not apply: all data is fixed and inline, there is no fetch, no mutation, no auth — so there is nothing to load, fail, or permission-gate. (Per the skill, a reason is recorded when a state does not apply.)
- "Use motion sparingly but deliberately": the only motion is the 0.2 s sidebar slide and the bell's notices flash; both are state-clarifying, and both are disabled under `prefers-reduced-motion`.
- "Verify text fit on mobile and desktop": verified by headless render (see Defects).
- Anti-patterns avoided: no purple gradient, no blobs, no hero copy, no UI cards inside cards, no new dependency for a flourish, no feature-description copy inside the UI.

## Conflicts between the two assigned skills — and resolution
1. **frontend-patterns is React/Next.js-only; frontend-design-direction assumes a real product surface with components, tokens and routing — but the task mandates one self-contained vanilla HTML file with no build step.** Resolution: frontend-patterns won on *structure* (composition, reducer state, debounce, copy-before-sort, a11y), frontend-design-direction won on *look and behaviour* (direction, tokens, states, anti-patterns). React-only mechanics were adapted, not imported; nothing was installed. No visual conflict arose between the two skills.
2. **frontend-design-direction's "rendered finish gate" requires screenshots / a JSON evidence record and `node <skill>/scripts/check-evidence.js`.** The checker script lives in the installed skill, which does not exist here (ECC is a reference clone; the skill file references a script path that is not part of the two assigned SKILL.md files). Resolution: ran the equivalent gate manually instead — headless Chromium renders at 1280 and 390, measured `documentElement.scrollWidth`, row counts, and a live search probe (see Defects). No evidence JSON was fabricated to claim a checker pass.
3. **ECC AGENTS.md (read as a house rule of the ECC folder) demands TDD + 80% coverage and agent-first delegation.** Resolution: not applicable to a single static HTML file with no test runner in scope; the task's own self-check (read code + headless render) was used instead, and the work was done directly (one file, no parallel delegation needed). This is recorded, not hidden.

## Defects found and fixed (objective task violations only — no hand-polishing)
1. **FOUND (headless Chromium, true 390px viewport): document scrollWidth 393 vs viewport 390 — 3px horizontal overflow.** Cause: the search `<input>`'s intrinsic min-width pushed the sticky header 3px past the viewport (avatar right edge at 393). FIX: `.search{min-width:0}` + `input{min-width:0}` and `flex:1 1 auto`. Re-check: 390px scrollWidth 375, no overflowing element outside the intentionally off-canvas sidebar.
2. **FOUND (same 390px re-check, visual): after fix 1 the search input collapsed to a ~0px sliver in the header — present but unusable.** Cause: flex shrink in a crowded one-row header. FIX: at ≤420px the header wraps and the search takes a full second row (351px wide, verified). This is a task-violation fix (header must contain a usable search), not a cosmetic improvement.
3. **FOUND (source-order check): sidebar `<nav>` preceded the header in the DOM, so a source-order read listed nav before header.** FIX: moved the sidebar after `<div class="main">` in the DOM and kept its visual left-column position with explicit grid placement (`.sidebar{grid-column:1}`, `.main{grid-column:2}`), with a ≤900px override back to a single column. Visual layout unchanged at both viewports.
4. **FOUND (text check): `2025–26` and the footer used `&ndash;` / `&mdash;` entities, so a raw-text grep for the literal strings failed.** FIX: literal UTF-8 `–` / `—` characters in visible text (charset is UTF-8). Rendered text was already correct; this only makes the source match it.

## Defects left (known, not fixed — cosmetic only, per the no-hand-fix rule)
- Sidebar footer ("Teacher on duty: Class 8-A / 12 students · 72 teachers school-wide") wraps awkwardly at desktop width ("school-wide" on its own line). Cosmetic; left as the skills produced it.
- At 390px the chart day label "Fri · Today" wraps to two lines. It fits inside its column and does not overlap; left as is.
- The student table scrolls horizontally *inside* its card at 390px (min-width 560px table in an `overflow-x:auto` wrapper). This is the task-allowed behaviour ("table fits or scrolls inside its card"); the page itself does not scroll horizontally.

## Self-check evidence (headless Chromium, real renders)
- 1280px: `documentElement.scrollWidth` 1265 (viewport 1280; 15px is the scrollbar), 12 student rows, 4 stat cards, 0 overflowing elements, 0 console errors observed in dump.
- 390px (headless shell, true 390 viewport — the full Chromium binary clamps `--window-size` to a 500px minimum): scrollWidth 375, 12 rows, search input 351px wide, 0 overflowing elements except the intentionally off-canvas sidebar (`translateX(-100%)`, does not affect page scroll).
- JS probe: typing "priya" in search (debounced) → 1 row; Clear search restores 12.
- Data check (scripted grep of the file): all 12 students with exact name/roll/attendance/marks, trend 94/96/93/97/95, notices verbatim, stats 1,248 / 72 / 95.2% / 84% / ₹25.2L of ₹30L, footer text verbatim, no external URLs, no `<img>`.
- Not verified: real-device touch feel, Safari/Firefox rendering, screen-reader pass — headless Chromium only.

## Assigned skill NOT used
- None. Both assigned ECC skills were read in full and both were used (see above). No pstack skill was read or used, by design of this variant.
