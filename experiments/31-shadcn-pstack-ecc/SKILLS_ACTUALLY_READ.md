# SKILLS_ACTUALLY_READ — Variant 31: shadcn + pstack + ECC

## Skills read in full, in order (absolute paths)

1. `/home/hatch/workspace/custom-skills/global-harness/SKILL.md`
2. `/home/hatch/workspace/custom-skills/local-pstack/SKILL.md`
3. `/home/hatch/workspace/skills/pstack/skills/poteto-mode/playbooks/feature.md`
4. `/home/hatch/workspace/build-anything/ECC/skills/frontend-patterns/SKILL.md`
5. `/home/hatch/workspace/build-anything/ECC/AGENTS.md` (house rules for the ECC clone; read on the runtime's instruction before further work there; clone left untouched)
6. `/home/hatch/workspace/skills/shadcn/SKILL.md`
7. `/home/hatch/workspace/custom-skills/local-pstack/skills/poteto-mode/SKILL.md` (read per the local-pstack adapter step 1: poteto-mode is the router)

Assigned skills NOT used: none. All four assigned skills (local-pstack, feature playbook, ECC frontend-patterns, shadcn) were read in full and applied. The shadcn CLI (`npx shadcn@latest ...`) and `docs` commands were not run: the task forbids npm/build steps and there is no project with a `components.json`, so shadcn was applied as design tokens and component composition in plain HTML/CSS/JS, not as installed React components.

## How each skill shaped the build

- **global-harness:** ECC first for structure, pstack for execution. ECC structure wins on conflicts; pstack execution wins inside it. No commit or push (task forbids it).
- **local-pstack / poteto-mode:** Feature playbook followed. Throughput checkpoint: blocking first steps were reading the skills in full and fixing the data shape before writing markup; independent workstreams n/a (single file, no fan-out); shared mutable state n/a (single file, no parallel writers); smallest safe decomposition is one worker owning the single file. Principles applied: model-the-domain (the 12 students live in one `STUDENTS` array, the single source of truth, rendered by `map`/`filter`); subtract-before-you-add (no sections beyond the seven required); prove-it-works (headless CDP check, see below); never-block-on-the-human (all reversible choices made and reported here).
- **feature playbook:** Step 4 (delegate code-writing to a subagent) could not be followed literally: this agent is depth 2/2 and cannot spawn subagents, so I owned the diff directly with the same review separation (build, then verify against the rendered artifact). Step 8 (Opening a PR) skipped: the task forbids committing or pushing.
- **ECC frontend-patterns:** Composition over inheritance (Card = CardHeader/CardTitle/CardDescription/CardContent; Sidebar = brand + group label + menu buttons; Table = caption/thead/tbody); the debounce-hook pattern adapted to a 150 ms debounce on the search input; immutability (rows built with `filter`/`map`, never mutating `STUDENTS`); accessibility patterns (aria-expanded/aria-controls on the menu and bell buttons, Escape closes sidebar and bell panel, focus returns to the menu button, `role="progressbar"` on attendance bars, `role="img"` with a text equivalent on the chart, `aria-label` on the search input).
- **shadcn:** Semantic tokens only (`--background`, `--foreground`, `--primary`, `--secondary`, `--muted`, `--muted-foreground`, `--border`, `--input`, `--ring`, `--destructive`, `--radius: 0.625rem`, zinc/neutral palette, system font stack because external fonts are forbidden). Component selection: Sidebar + Card + Table + Badge + Avatar (always with fallback initials) + Input + Button + Separator (border-top) + Progress (thin bar under attendance %) + Chart tokens for the CSS bars. Spacing uses `gap`, not `space-x`/`space-y`. Status uses Badge variants (`secondary` = Excellent ≥95%, `outline` = Good 90–94%, `destructive` = Watch <90%), never raw colour classes.

## Conflicts resolved

1. **shadcn Chart wraps Recharts vs task "CSS bars only".** Task wins. Bars are plain CSS, styled with shadcn tokens (primary for the peak day, zinc-300 for the rest). No chart library, no external JS.
2. **shadcn components are React/Tailwind vs task "one self-contained index.html, no npm, no build step".** Adapted, not installed: the same composition (CardHeader/Title/Description/Content, SidebarMenuButton with `data-active`, Table structure) in semantic HTML with the token values inlined as CSS variables.
3. **shadcn "no manual z-index on overlay components" vs a mobile drawer needing stacking.** Adaptation recorded: minimal explicit z-index only (sidebar 40, overlay 30, topbar 20, bell panel 50), matching what Sheet/Dialog handle internally in React.
4. **ECC 80% coverage / TDD vs a single static HTML file.** Per the harness's own conflict note, coverage is a check, not a gate for a small UI demo. Behaviour was verified instead with real headless checks (search filter, empty state, bell toggle, sidebar open/Escape-close, overflow measurements at both widths).
5. **pstack Feature step 4 (delegate) vs depth-2 subagent.** Cannot spawn; I owned the diff directly. Noted above.
6. **ECC React hooks (useDebounce, useQuery) vs static HTML.** Adapted: debounce as a `setTimeout` wrapper on the search input; no fetching (data is fixed and embedded).

## Self-check (headless, measured)

Run with Chromium 1248 over CDP (Node built-in WebSocket, script in /tmp, screenshots at /tmp/check31-desktop1280.png and /tmp/check31-mobile390.png). Both screenshots were eyeballed.

- 1280px: `documentElement.scrollWidth` 1265 ≤ 1280. No horizontal overflow, no clipped text, no element extending past the viewport. 4 stat cards (224 px each, no overflow). Table fits inside its card (943 = 943). 12 student rows, 5 chart bars, 3 notices, footer text exact, bottom nav hidden, sidebar visible.
- 390px: `documentElement.scrollWidth` 390 = innerWidth. No clipped text. Stat cards stack single-column (358 px, no overflow). Table scrolls horizontally inside its card only (client 356, scroll 616, does not overflow the page) — allowed by the task. Bottom nav visible, sidebar off-canvas (translateX -256).
- Behaviour: search "priya" filters to 1 row (Priya Das); a non-matching query shows the empty state "No students match your search."; clearing restores 12 rows. Bell opens and closes. Mobile sidebar opens on hamburger tap and closes on Escape.
- Console errors: none observed (no external requests exist; the script executed and all interactions above worked).

## Defects found and fixed

1. **Invalid rupee entity (objective task violation, found in the first screenshot).** The Fees card used `&rupee;`, which is not a valid HTML entity, so it rendered literally as "&rupee;25.2L of &rupee;30L". Fixed to `&#8377;` and re-verified: the re-run screenshot shows "₹25.2L of ₹30L" and all measurements above are from the post-fix run.

## Defects / limitations left

- None objective. Notes, not defects: at 390 px the search input shrinks to near icon-width (placeholder hidden, still functional); the table scrolls inside its card at 390 px by design; chart bar height differences are small because heights are honest percentages (93–97% of the plot) with no baseline trickery; stat cards are single-column at 390 px. Real-device feel, touch, Safari/Firefox, and screen-reader behaviour are UNVERIFIED (headless Chromium only).
