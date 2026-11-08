# SKILLS_ACTUALLY_READ — Variant 26-astryx-opensourceui (CONFLICT TEST)

## Skills read in full, in order

1. `/home/hatch/workspace/skills-test/TASK.md` — the fixed task/data/sections/constraints (not a skill; read first, per assignment).
2. `/home/hatch/workspace/custom-skills/local-astryx/SKILL.md` — Astryx adapter, read in full. (Assigned first.)
3. `/home/hatch/workspace/custom-skills/local-opensourceui/SKILL.md` — OpenSourceUI adapter, read in full. (Assigned second.)
4. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/SKILL.md` — upstream OpenSourceUI agent kit, read in full (required by adapter step 3 / "read it in full before building UI").
5. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/design.md` — OpenSourceUI design system, read in full.
6. `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/implementation.md` — copy-paste workflow / repo layout, read in full.
7. `/home/hatch/workspace/ui-skills/opensourceui/AGENTS.md` — OpenSourceUI agent instructions, read in full (runtime house rule for that folder).

## Sources consulted (searched / partially read — NOT counted as read in full)

- `/home/hatch/workspace/ui-skills/opensourceui/skills/opensource-ui/references/catalog.md` — searched by grep for table / widget / notification / stat entries; not read cover-to-cover.
- `/home/hatch/workspace/ui-skills/opensourceui/components/table/customers-table.tsx` — read in full; adapted as the student table (see below).
- `/home/hatch/workspace/ui-skills/opensourceui/components/notifications/apple-notification-banner.tsx` — read in full; adapted as the notices panel (see below).
- Astryx repo (read-only reference, `~/workspace/ui-skills/astryx/`): `packages/core/src/Card/Card.doc.mjs` and `packages/core/src/Table/Table.doc.mjs` — head/grep excerpts only (anatomy, best practices); `packages/themes/neutral/src/neutralTheme.ts` — grepped for radius / colour tokens only. Neither doc file was read in full.
- Astryx CLI (`astryx component --list`, `astryx docs …`) was NOT run: the task forbids npm/build steps, and no Astryx install exists in this VM. Component choice therefore rests on the adapter SKILL.md + the repo `.doc.mjs` excerpts above, not on CLI output.

## Assigned but NOT used

- None. Both assigned skills were read in full and both shaped the page (Astryx: shell, tokens, Card/Table anatomy; OpenSourceUI: table + notification components, neutral stage).
- No other skill was read. No other variant folder was read.

## OpenSourceUI components adapted (2, as assigned)

1. **CustomersTable** (`components/table/customers-table.tsx`) → Class 8-A student table. Kept: bordered card, header row (title + count left, summary right), avatar + name cell, row hover `#fafafa`, `overflow-x-auto` with a min-width table, tabular numerals, `text-sm` body. Changed: customer/plan/MRR → student/roll/attendance/marks; photo avatars → initials circles (no external images allowed); 4 rows → 12 rows; added a small attendance bar per row (hidden on mobile) and a live search filter.
2. **AppleNotificationBanner** (`components/notifications/apple-notification-banner.tsx`) → Notices panel (3 notices). Kept: icon tile + title/time row + sender:message grid, 13px message text, dismiss with 260 ms fade-out, "Show notification" trigger that restores dismissed items. Changed: photo avatar → emoji/text tile; frosted glass + shadow dropped (see Conflict C3); three stacked banners instead of one triggered banner; `output` element → `article` inside a panel.

## Astryx components / patterns adapted (no React build — single-file HTML)

- **AppShell / SideNav / TopNav / MobileNav**: desktop sticky sidebar, sticky top bar, fixed bottom nav on mobile.
- **Card**: stat cards only (discrete metrics — the Card doc's "reorder/remove independently" test).
- **Table**: semantic `<table>` with a required horizontal scroll region.
- **theme-neutral** tokens: radius scale + accent blue `#0074e2`.
- **Badge**: the bell's rose "3" count badge.

## Conflicts hit — and how each was resolved

| # | Conflict | Resolution (winner in bold) |
|---|----------|------------------------------|
| C1 | **Radius**: Astryx container 12px / element 10px / inner 6px vs OpenSourceUI sm 6px / md 8px / lg 16px. A card cannot be both 12px and 16px. | **Astryx scale wins** for all containers/controls (12px cards, 10px buttons/nav, 6px inner bars). OSUI's 16px large-card radius dropped entirely; OSUI's 8px survives only on the search input (its own component spec). |
| C2 | **Accent colour**: Astryx neutral accent is blue `#0074e2` (+ status fills); OpenSourceUI accents are rose `#f43f5e`, cyan `#a5f3fc`, amber/teal/emerald — no blue exists in its palette. | Split by role: **Astryx blue** is the single "system" accent (chart bars, stat progress, active-nav dot). **OSUI rose** is reserved for urgent/error signals (fee bar, fee notice, bell badge); **OSUI cyan** appears exactly once as a text annotation highlight (Wednesday low). No purple anywhere (both systems agree). |
| C3 | **Elevation**: Astryx Cards are bordered *and* elevated (shadow); OpenSourceUI is flat-by-default, "no stacked card shadows, no glow, no glassmorphism". The Apple banner source itself is frosted glass + `shadow-[0_8px_32px…]` + `backdrop-blur-xl`. | **OpenSourceUI flat wins.** All cards/panels/notices are border-only, zero shadow, zero blur, zero gradient. The banner was flattened to a plain white bordered card — its glass look was the first casualty of the merge, and the most visible one. |
| C4 | **Typography**: OpenSourceUI mandates Instrument Serif (display) + Geist (UI); Astryx theme-neutral ships its own font stack; the task forbids external fonts, so neither named font is loadable. | **Fallback pairing**: Georgia/`ui-serif` stands in for Instrument Serif on brand/section headings only (Two-Voice Rule kept); `system-ui` stands in for Geist everywhere else. The pairing structure survives; the exact faces do not. |
| C5 | **Table on mobile**: OpenSourceUI CustomersTable hides a column below `md:` (`hidden … md:table-cell`); Astryx Table doc says the table scrolls by itself inside its scroll region and cards clip content that cannot wrap. | **Astryx scroll region wins.** All four data columns stay visible at every width; at 390px the table scrolls horizontally inside its card (min-width 560px). No data is hidden to fit the viewport. |
| C6 | **Buttons**: Astryx Button (theme radius-element 10px, accent fill) vs OpenSourceUI primary (ink-soft `#262626` fill, rounded-md 6px, px-4 py-2). | **OpenSourceUI anatomy wins** (6px, ink fill, px-4 py-2); Astryx contributes only the accessibility contract (visible focus, 4.5:1 contrast — white on `#262626` passes). |
| C7 | **Focus style**: OpenSourceUI — border darkens, `ring-0`, "never a coloured ring"; Astryx — every interactive component must show a visible focus indicator meeting WCAG. | Compromise: a 2px solid **ink outline** with 2px offset on `:focus-visible`. It is border-like (no coloured ring, no glow) and still visible in every state. Neither system gets its literal rule; both get their intent. |
| C8 | **Breakpoints**: OpenSourceUI — base + `md:` only, "do not introduce `sm:`"; Astryx — its own responsive token scale. | **OpenSourceUI wins**: one breakpoint at 768px (`md:`), no `sm:` anywhere. Astryx responsive tokens unused. |
| C9 | **Card philosophy**: Astryx Card doc — "Cards are NOT the default layout tool… do not wrap page sections in cards"; OpenSourceUI dashboard chrome is cards everywhere, and CustomersTable *is* a bordered card. | Split: stat metrics are genuine Astryx Cards (discrete, comparable items). Chart / table / notices live in OSUI bordered panels because the fixed task requires those sections as panels — recorded here as a deliberate Astryx violation, not an oversight. Sections themselves are headed by typography + spacing, not nested cards (no card-in-card anywhere). |
| C10 | **Spacing**: Astryx Stack gaps (token scale) vs OpenSourceUI `p-4` (16px) as the common internal padding. | **OSUI `p-4` wins** as the single padding/gap value (16px); section gaps are 24px. |
| C11 | **Dark mode**: Astryx theme-neutral is a light/dark pair (`lightDark(...)`); OpenSourceUI design.md describes a light-only neutral stage. | **Light only.** The task fixes one page and forbids extra scope; Astryx dark tokens are unused. Noted, not implemented. |
| C12 | **Notification anatomy**: OSUI Apple banner is a floating overlay (fixed width 320px, slide+fade entrance, trigger button); Astryx Banner/Toast is an inline themed surface. | Merged: the banner's *content grid* (tile + title/time + sender:message) is kept inline inside the notices panel at full width; its *overlay behaviour* (fixed width, floating, entrance slide) is dropped. Dismiss + restore behaviour kept verbatim (260 ms). |

## Self-check (1280px and 390px)

Method: code-read + scripted content checks (Python: all 12 names, all fixed numbers, 3 notices, footer text, zero external URLs, JS extracted and `node --check` passed). **No Chromium/Playwright exists in this VM, so no live render or screenshot was possible — the 1280/390 checks below are static, not visual.**

### Defects found and fixed

1. **Table overflow at 390px** — a 4-column table at full width would clip or force page scroll. Fixed: Astryx scroll region (`overflow-x:auto`, `min-width:560px` on the table) inside the card; page itself never scrolls horizontally.
2. **Bottom nav overlapping content at 390px** — the fixed bottom nav would cover the footer/table foot. Fixed: `padding-bottom:96px` on `.content` below 768px.
3. **Header crowding at 390px** — brand + search + bell + avatar on one row would overflow 358px of content width. Fixed: search wraps to a full-width second row (`order:3`, `flex-basis:100%`); bell gets `margin-left:auto` on the first row.
4. **Stat values too large at 390px** — 1.875rem values in a 2-column grid risked wrapping/clipping. Fixed: value drops to 1.5rem below 768px; stats go 4 → 2 columns (768–1023px: 2 columns; ≤767px: 2 columns).
5. **Attendance mini-bars squeezing the table** — the per-row bar + % in a 560px-min table added width pressure on mobile. Fixed: `.mini` bars hidden below 768px (percentages remain).
6. **Dismiss logic** — the empty-state check in `dismissNotice` was re-read after writing; the redundant first clause is harmless (the `every()` check governs), so the code was left as-is rather than refactored blind.

### Defects left / open

- **No visual verification** (see Method above): 1280px/390px layout, bar-chart proportions, and notice-card spacing are verified by reading CSS only. A live-browser look could still surface an overflow or overlap.
- **Emoji glyphs** (🔔, 👥, 🔬, ₹ tile, ⌕) depend on the viewer's system emoji font; on a system without emoji fonts they render as boxes or monochrome outlines. No external icon set was allowed, so there is no fallback.
- **Chart bar heights are illustrative scaling** (94% → 72% height etc.); the labelled values are exact, the bar proportions are not data-true. Same trade-off any CSS-bar chart makes; recorded, not hidden.

## UNVERIFIED items

- Live-browser render at 1280px and 390px (no Chromium in VM).
- Astryx CLI component docs (`astryx component Card/Table --dense`) — CLI not run; repo `.doc.mjs` excerpts used instead.
- Real Astryx React build / OpenSourceUI Tailwind build — both systems were adapted to single-file HTML per the task; neither was built as React.
- Exact Instrument Serif / Geist rendering — fallback fonts used (external fonts forbidden).
- Touch behaviour of the bottom nav and dismiss buttons on a real phone (code paths are standard click handlers; not exercised).
