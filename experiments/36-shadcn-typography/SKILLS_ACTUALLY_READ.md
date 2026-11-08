# Skills Actually Read — Variant 36: shadcn + better-typography

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — read first, per task (ECC first, then pstack). Routing only; no ECC/pstack skill file was read beyond this entry because the task is a single-file UI build and the harness routes UI work to the assigned skills.
2. `/home/hatch/workspace/skills/shadcn/SKILL.md` — read in full before building.
3. `/home/hatch/workspace/skills/skills/skills/better-typography/SKILL.md` — read in full before building.
4. `/home/hatch/workspace/skills/skills/AGENTS.md` — house rules for the skills repo, read when the runtime flagged it (documentation-only repo; no build/lint/test tooling; nothing to run).

## Assigned skills NOT used

None. Both assigned skills were read in full and applied (see below).

## How each skill shaped the build

### shadcn (adapted to a single file — no React, no CLI, no npm)

The task forbids a build step and npm, so there is no `components.json`, no CLI run, and no `npx shadcn@latest docs/add` — those workflow steps are not applicable here and were not faked. The skill's patterns were carried over as plain CSS/HTML:

- **Semantic design tokens** (`--background`, `--foreground`, `--card`, `--primary`, `--secondary`, `--muted`, `--muted-foreground`, `--border`, `--destructive`, `--radius: 0.625rem`) — no raw color values on components.
- **Dashboard composition per the skill's table**: Sidebar + Card + Table + Badge. Full Card composition everywhere: `card-header` / `card-title` (h2) / `card-description` / `card-content`.
- **Badge variants** for status: `badge-secondary` (Good), `badge-success` (Excellent), `badge-warning` (Watch), `badge-destructive` (3 new notices).
- **Gap-based spacing** (`flex`/`grid` with `gap-*`), never `space-y-*`; `truncate` shorthand (`overflow: hidden; text-overflow: ellipsis; white-space: nowrap`) on brand name and header title only.
- **Separator** (`<hr class="separator">`) instead of a border div; **Avatar** with fallback initials (KT) in the sidebar footer.
- Chart is CSS bars only, per the task; shadcn's `Chart` (Recharts) component is not available in a no-build file, so the bar chart is hand-built divs — a deliberate substitution, not a shadcn component.

### better-typography

- **One semantic type scale** in `rem`: display 1.5rem / title 1.125rem / body 1rem / small 0.875rem / caption 0.75rem, each paired with a unitless line-height (display 1.2, title 1.3, body 1.6, small 1.5). No weight below 400 under 18px.
- **Headings descend with level**: h1 1.5rem/700, h2 1.125rem/600, h3 0.875rem/600; no heading smaller than body except the deliberate uppercase overline.
- **Letter-spacing in `em`**: −0.01em on headings ≥24px, 0.05em on uppercase labels ≤14px, 0 on body.
- **`font-variant-numeric: tabular-nums`** on all changing/numeric values (stat values, table number columns, bar values).
- **`text-wrap: balance` on headings, `text-wrap: pretty` on paragraphs**; `text-align: start` throughout.
- **Typographic punctuation**: en dashes in "2025–26" and "Fictional demo data — UI Forge Experiment 1" (em dash, as in the required footer string), `&rsquo;` avoided — no invented copy remains.
- **Input at 16px on mobile**: search input is 0.875rem on desktop, forced to 1rem at ≤768px so iOS Safari does not zoom.
- **Font smoothing on the root only** (`html`, never per component). System font stack only — no new typeface proposed or loaded (task also forbids external fonts).
- **Measure**: no long-form prose exists on this dashboard, so no measure cap was needed; card descriptions wrap with `text-wrap: pretty`.

## Conflicts between the two skills

**No direct conflict.** One adaptation tension, resolved in favour of better-typography:

- shadcn's `truncate` shorthand (ellipsis) on the header subtitle clipped the required "Student Management Dashboard · Class 8-A · 2025–26" line at 390px. better-typography treats clipped text at small widths as HIGH severity. Resolution: the subtitle now wraps (`text-wrap: pretty`, no `nowrap`/ellipsis); `truncate` is kept only where the truncated string is decorative/identifiable (brand name, page title), never on required data text.

## shadcn pieces deliberately NOT applied (not applicable, not a conflict)

- CLI workflow (`info`, `search`, `docs`, `add`, presets) — no project, no package manager, task forbids npm.
- `cn()`, `asChild`/`render`, `FieldGroup`/`Field`, `InputGroup`, `ToggleGroup`, `Dialog`/`Sheet`/`Drawer` titles, `Toast`/`sonner`, `Skeleton`, chat primitives — React-only APIs with no plain-HTML equivalent in a single file. The closest equivalents were used where they exist (native `<button>`, `<table>`, badges, separator, avatar fallback).
- Dark mode: shadcn says use semantic tokens, not manual `dark:` overrides. No dark mode was built; tokens are in place so one could be added without overrides.

## Self-check (headless Chromium, Playwright headless shell)

Probe injected into a /tmp copy only; the deliverable was not modified for the check.

- 1280px: `scrollWidth 1265 ≤ innerWidth 1280`, no horizontal overflow, no overflowing elements.
- 390px: `scrollWidth 375 ≤ innerWidth 390`, no page-level horizontal overflow. The student table's cells extend past 390px inside `.table-wrap` (`overflow-x: auto`) — the table scrolls inside its card, which the task explicitly allows.
- Screenshots eyeballed at both widths: stat cards fit (2×2 at 390px), chart bars render, notices and table are legible, bottom nav present at 390px, sidebar present at 1280px.

## Defects found and fixed

1. **Notices carried invented caption lines** ("Bring your project models…", etc.) under the three required strings. Fixed: captions removed; notices now show exactly the three specified strings. (Objective task violation — fixed data must be used exactly.)
2. **Header subtitle clipped at 390px** — `white-space: nowrap` + ellipsis cut "…Class 8-A · 202…" (visible in the 390px screenshot). Fixed: subtitle wraps to two lines; re-checked at 390px, full text visible, still no horizontal overflow.

## Defects left

- **None known.** Table horizontal scrolling inside its card at 390px is intentional and task-permitted, not a defect.
- **Not verified (real hardware/browser):** feel, touch targets on the bottom nav, and rendering in Safari/Firefox — headless Chromium only. Live-browser look is UNVERIFIED.

## Data verification

All fixed data grepped present in `index.html`: 12 students with exact names/rolls/attendance/marks, 1,248 / 72 / 95.2% (1,188 present, 60 absent) / 84% (₹25.2L of ₹30L), trend 94/96/93/97/95, all three notices verbatim, footer string verbatim, "Greenwood High", "Class 8-A", "2025–26". Zero external URLs in the file (no `http`, no external fonts/images). Weekly average 95.0% shown under the chart is the arithmetic mean of the given trend (475 ÷ 5), not invented data. Status badges (Excellent ≥95% attendance, Good 90–94%, Watch <90%) are a presentation layer derived from the given attendance values.
