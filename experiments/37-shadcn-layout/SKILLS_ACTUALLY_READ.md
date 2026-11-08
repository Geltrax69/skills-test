# Skills Actually Read — Variant 37 (shadcn + better-layout)

## Skills read in full, in order

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md` — harness entry (ECC first, then pstack). Read first per the task; routed this as a UI build, single file, verify-before-report.
2. `/home/hatch/workspace/skills/shadcn/SKILL.md` — component + composition rules.
3. `/home/hatch/workspace/skills/skills/skills/better-layout/SKILL.md` — grouping, alignment, responsive structure, logical properties.
4. `/home/hatch/workspace/skills/skills/AGENTS.md` — house rules for the skills repo (read on runtime instruction after touching that folder; it governs skill authoring, not this build, and changed nothing in the page).

No assigned skill was left unread. No other variant folder was read. No other skill files were read in full (shadcn's linked rule files `rules/*.md` were listed in the SKILL.md but not opened; only the SKILL.md body itself counts as read).

## How shadcn was applied

- Dashboard composed from shadcn patterns: Sidebar + Card + Chart (CSS bars) + Table.
- Full Card composition everywhere: CardHeader / CardTitle / CardDescription / CardContent / CardFooter.
- Semantic design tokens as CSS custom properties (`--background`, `--foreground`, `--card`, `--primary`, `--secondary`, `--muted`, `--muted-foreground`, `--border`, `--input`, `--ring`, `--destructive`, `--radius: 0.625rem`). No raw color values on components.
- `Badge` variants for attendance status (Excellent / Good / Watch) instead of custom styled spans; `Separator` (`<hr class="separator">`) instead of a border div; `Avatar` with initials as fallback (no image); `Button` default variant for the single primary action ("+ Add student").
- Spacing with `gap-*` only, never `space-x-*` / `space-y-*`; equal width/height via fixed square sizes; `truncate` (ellipsis) on the header title.
- No CLI was used: the task forbids npm/build steps, so the CLI (`npx shadcn@latest add/docs/search`) and any preset were not applicable. Components were hand-composed in plain CSS following the SKILL.md patterns, not imported.

## How better-layout was applied

- Group with space, not lines: `--gap-in: 8px` inside groups, `--gap-out: 24px` between groups (3x, above the 2x minimum); the single Separator is used only between sidebar nav groups.
- Align to shared edges: one content margin `--pad: 24px` (16px at ≤480px) shared by header, content, and footer.
- Logical properties throughout: `margin-inline-start`, `padding-inline`, `inset-inline-start`, `inset-inline-end`, `margin-block-start`, `scroll-padding-block-start`. No physical left/right for layout.
- Order by importance: DOM order matches visual order (header → stats → chart/notices → table → footer); no `order`, `row-reverse`, or grid placement tricks.
- One primary action per view ("+ Add student"); secondary nav behind the sidebar/bottom-nav structure.
- Hint at hidden content: the table scrolls inside its card (`overflow-x: auto` on `.table-wrap`) and a "Scroll sideways to see all columns." hint appears at ≤480px.
- Plan for growth/clipping: `minmax(0, 1fr)` grid tracks, `min-width: 0` on flex/grid children, `min-height` (never fixed `height`) on text boxes, `100dvh` for the app shell, `width: 100%` (never `100vw`), `scroll-padding-block-start` set to the sticky header height, `viewport-fit=cover` so safe-area insets resolve.
- Breakpoints from content: stats 4 → 2 → 1 columns, chart/notices 2-col → 1-col, sidebar → bottom nav at 768px.

## Conflicts and resolutions

1. **shadcn "no manual z-index on overlay components" vs sticky header + fixed sidebar/bottom-nav.** shadcn says overlays handle their own stacking; better-layout requires content to scroll beneath stable sticky chrome. Resolution: kept z-index, but only on the three chrome elements (sidebar 40, header 30, bottom nav 40), never on content or overlays. No Dialog/Sheet/Popover exists in this page, so the shadcn rule has no overlay to govern.
2. **shadcn Separator vs better-layout "space groups first, lines last".** Resolution: space carries the structure everywhere; the Separator appears exactly once, between the sidebar's Menu and General nav groups, where space alone would blur the boundary.
3. **shadcn `truncate` on the header title vs better-layout "plan for growth and clipping".** Truncation with ellipsis hid the school name at 390px (see defects). Resolution: truncation kept as a last resort, but the layout was changed so the full name fits at 390px; truncation now only engages below ~340px.
4. **shadcn Table with a `min-width` vs better-layout "hold structure until it breaks".** The table keeps a 34rem minimum width for readability and scrolls inside its card on narrow screens rather than being squeezed or reflowed. This is the sanctioned pattern (scroll inside the card, with a visible hint), not a page-level overflow.

## Defects found and fixed (objective task violations only)

| # | Defect | Fix |
|---|--------|-----|
| 1 | Fees stat showed literal text `&rupee;25.2L of &rupee;30L` — `&rupee;` was not parsed by the headless Chromium. | Replaced with `&#8377;` (₹). Verified: probe reports `feesSub=₹25.2L of ₹30L`. |
| 2 | School name in the header truncated to "Greenwo…" / "Greenwood Hi…" at 390px. | At ≤480px, search `max-width: 8rem` and h1 `font-size: 1rem`. Verified: probe reports `h1 truncated=false` at 390px. |
| 3 | Table caption ("Class 8-A students with attendance and marks percentages.") clipped at the card edge at 390px. | Shortened caption to "Attendance and marks, 2025–26." |

## Defects left

- None known at the supported widths. The table intentionally scrolls horizontally *inside its card* at 390px (client 309px, scroll 544px) with a visible hint; this is by design, not a defect.
- Not verified: 320px width, 200% zoom, RTL mirror, real-device touch/scroll feel, live-browser look (headless Chromium only). The `dir="rtl"` mirror was not exercised; logical properties are in place but unproven in render.

## Verification (headless Chromium, `chrome-headless-shell`, probe injected into a /tmp copy only)

- 1280px: `docScrollWidth=1265`, `hScroll=false`, no overflowing elements, 4 stat cards fit (right edges 496/745/993/1241), table fits its card without scrolling (client 927 = scroll 927).
- 390px: `docScrollWidth=375`, `hScroll=false`, stat cards stack 1-per-row at full width (343px, right 359), no page-level overflow. Elements extending past the viewport are only table internals inside the scrolling `.table-wrap`.
- Screenshots eyeballed at 1280px and 390px: no overlap, no clipped content, all 12 students, all 3 notices, trend bars 94/96/93/97/95 with correct labels.
- Data sweep: all 12 names + rolls 8A-01–8A-12 present; stats 1,248 / 72 / 95.2% / 84%; 1,188 present, 60 absent; ₹25.2L of ₹30L; all three notices; footer text "Fictional demo data — UI Forge Experiment 1". Zero external URLs, zero external images/fonts.
