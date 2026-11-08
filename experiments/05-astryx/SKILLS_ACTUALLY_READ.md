# Variant 05-astryx — Skills Actually Read

## Skills read in full, in order

1. `/home/hatch/workspace/skills-test/TASK.md` — the fixed experiment task, data, required sections and constraints (read in full first, per the assignment).
2. `/home/hatch/workspace/custom-skills/local-astryx/SKILL.md` — Astryx, Meta's React design system, Muse adapter (read in full before building).

No other skill file was read. No other variant folder was read.

## Assigned skill NOT used

None. Astryx was used. Note its contributor skills/commands in the repo (`.claude/` — `writing-component-docs`, `dense-compression-protocol`, `create-component`, `create-theme`) were NOT used: the Astryx skill itself says they are for building Astryx, not for using it.

## How Astryx was applied — and the adaptation

This experiment forbids a build step / npm / React, so no `@astryxdesign/core` package was imported and no `astryx` CLI was run here. Applied from Astryx instead:

- **Theme-neutral tokens as CSS custom properties** — colour, spacing (`--space-*`), radius, typography scale, shadow. Theme = token overrides, per the skill.
- **Component anatomy** — AppBar (title / search / bell / avatar), Sidebar navigation (one `aria-current="page"` item), Card, Stat (label / value / delta / Progress), Table (header row, `th scope`, Badge for status), Button, Notices list. One named component per job, per-component thinking.
- **Accessibility contract** — semantic landmarks (`header`, `aside`/`nav`, `main`, `footer`), labelled search (`label for="q"`), bell with `aria-label` + text alternative for the unread dot, chart and progress bars with `role="img"` + text `aria-label` (values are labelled, not colour-only), status Badges carry text (`Good` / `Watch` / `Needs support`, not colour alone), visible `:focus-visible` outline, 4.5:1 text contrast (ink `#171717` / muted `#57534e` on white/subtle surfaces; accent `#1d4ed8` on white is 4.5:1+ for large/bold text and is not used for small body text), `prefers-reduced-motion` respected.
- **Theming first, no swizzle** — no component source was ejected; customisation is token overrides only.

## Self-check — 1280px and 390px (by reading the code; see UNVERIFIED below)

Checked by code inspection + a Python content check (all 12 students, all 12 rolls, all fixed numbers, all 3 notices, footer text, 13 table rows, no external http refs, no external images/fonts).

- **1280px:** sidebar 220px + `main{flex:1;min-width:0}`; stats 4 columns `minmax(0,1fr)`; chart + notices in a 2-column grid; table `min-width:560px` inside `.table-wrap{overflow-x:auto}`, so the table scrolls inside its card, not the page. No horizontal scroll expected.
- **390px:** sidebar hidden, bottom nav fixed with `main` bottom padding (76px) so content is not hidden behind it; appbar wraps, search goes full-width on its own row, school sub-line hidden; stats 2 columns `minmax(0,1fr)` with value reduced to 22px; chart gap reduced; table still scrolls inside its card. No horizontal scroll, no clipped text, no overlap expected.

## Defects found and fixed

1. **Kabir Singh row was missing its roll cell** (`8A-03`) in the first draft — attendance/marks would have shifted one column left for that row. Found by the content check (roll count was 11, not 12). Fixed: roll cell added; re-check shows 12 rolls, 13 table rows.

## Defects left

None known from the code check.

## UNVERIFIED

- No live-browser / headless screenshot check was possible: no Chromium/Chrome binary is available in this VM (`which chromium chromium-browser google-chrome` found none), and the task forbids a build/npm step. The 1280px / 390px verdicts above are code-inspection only, not rendered proof. Rendered feel, actual overflow at real widths, and the live look are UNVERIFIED.
- No real Astryx React UI was built — this is a token/anatomy/a11y adaptation to single-file HTML, not a `@astryxdesign/core` consumer install. The Astryx skill's own verification (build the consumer project, run tests, check in a live browser) could not be met inside this experiment's constraints.
