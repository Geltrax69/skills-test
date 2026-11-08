# Skills Actually Read — Variant 38 (apple-alone)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — harness entry (ECC first, then pstack; UI routing: read the assigned UI skill first).
2. `/home/hatch/workspace/custom-skills/local-apple-design-skill/SKILL.md` — the assigned skill, read in full before building.
3. `/home/hatch/workspace/custom-skills/local-apple-design-skill/AGENTS.md` — house rules for the Apple skill repo (read per runtime instruction; confirms the load-the-always-load-set rule).
4. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/accessibility.md` — always-load set.
5. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/layout.md` — always-load set.
6. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/typography.md` — always-load set.
7. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/color.md` — always-load set.
8. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/designing-for-ios.md` — always-load set (phone platform).
9. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/designing-for-macos.md` — always-load set (desktop platform).
10. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/cross-platform.md` — required by the skill for non-SwiftUI/UIKit/AppKit work (this is a web page).
11. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/sidebars.md` — on screen (desktop nav).
12. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/tab-bars.md` — on screen (mobile nav).
13. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/lists-and-tables.md` — on screen (student table).
14. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/charting-data.md` — on screen (attendance chart).
15. `/home/hatch/workspace/custom-skills/local-apple-design-skill/references/hig/search-fields.md` — on screen (header search).

## Assigned skills NOT used

None. The assigned skill (`local-apple-design-skill`) was read in full and applied; the global harness entry was also read in full. No other variant folder was read and nothing was copied from another variant.

## How the Apple HIG guidance was applied

- **Type:** system font stack only (no external fonts); scale from `typography.md` Large default — Large Title 34, Title 1 28, Headline 17 semibold, Body 17, Footnote 13, Caption 12; one typeface family throughout.
- **Colour:** semantic roles from `color.md` — grouped background `#F2F2F7`, white grouped cards, label / secondary label, separator; one accent (system blue `#007AFF`, darker `#005FC4` for small text) used only for interactive elements and the chart; status is never colour alone (attendance % carries a word: Excellent / Good / Needs attention).
- **Layout / navigation:** size classes, not device type (`layout.md`) — regular width gets a one-level sidebar (hideable, no critical action at its bottom, `sidebars.md`); compact width gets a 5-tab bottom tab bar with single-word labels that navigate, never act (`tab-bars.md`), plus `env(safe-area-inset-bottom)`.
- **Search:** placeholder names the scope ("Search students"), filters as you type (`search-fields.md`); bell carries a badge for the 3 unread notices and scrolls to Notices.
- **Lists / chart:** grouped inset cards; table has descriptive column headings; chart is a common bar type with a value label on every bar, a text summary, and an `aria-label` giving all five values (`charting-data.md`); bars use a 0–100 scale (honest — the week is genuinely flat, 93–97%).
- **Accessibility (`accessibility.md`):** 44px touch targets, visible `:focus-visible` ring, skip link, `prefers-color-scheme` dark mode with no app-level appearance switch, `prefers-reduced-motion` respected, contrast-checked text colours (secondary label `#6C6C70` ≈ 4.9:1 on white; status text colours ≥ 5:1).
- **Judgment call (not a guideline):** the Friday bar is green to mark "today"; 95% is also in the Excellent band (≥95%), so the two meanings coincide rather than conflict.

## Self-check (headless Chromium, Playwright's chrome-headless-shell)

- 1280px: `innerWidth=1280 scrollWidth=1265`, no horizontal overflow; all 4 stat cards inside viewport; table fits fully inside its card (no inner scroll). JS syntax check passed (`node --check`).
- 390px: `innerWidth=390 scrollWidth=375`, no horizontal overflow; stat cards 166px wide, no overflow; screenshots eyeballed at both widths — no clipped text, no overlapping elements.
- Static data check: all 12 students (name, roll, attendance%, marks%), all 4 stats, trend values 94/96/93/97/95, all 3 notices, footer text, and search placeholder verified present; no external URLs, images, or fonts.

## Defects found and fixed

1. **Table caption clipped at 390px (objective task violation — clipped text).** The `<caption>` lived inside the table, which has `min-width: 520px` inside a ~341px scrolling wrapper, so the caption text was cut off at the card edge in the initial view. Fixed by moving the description out of the table into a wrapping `<p class="section-sub">` above the table, with the table linked to it via `aria-describedby`; the unused `caption` CSS rule was removed. Re-checked at 390px: description right edge 338px < 390px, not clipped; at 1280px also clean.
2. **Counting-artifact false alarms in my own static check** (grep counts of `student-name` / `bar-col` / `notice-text` included CSS selectors). Not a page defect — re-counted by exact class attribute: 12 students, 5 bars, 3 notices. No page change needed.

## Defects left

- **Table scrolls horizontally inside its card at 390px** (table min-width 520px vs ~341px card content width). This is explicitly allowed by the task ("table fits or scrolls inside its card"), so it is left as designed; the in-card scrollbar is visible and the page itself never scrolls horizontally.
- **Not verified on a real device / real browser.** Headless Chromium only: Safari, Firefox, touch feel, and Dynamic Type at the largest accessibility sizes were not checked. Dark mode follows `prefers-color-scheme` but was not screenshot-verified.
