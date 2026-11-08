# SKILLS_ACTUALLY_READ.md — Variant 27-panelui-astryx (Experiment 2, CONFLICT TEST)

## Skills actually read (in full, in order)

1. `/home/hatch/workspace/custom-skills/local-panelui/SKILL.md` — PanelUI Muse adapter (native 143 modules / web 79 docs, colour-is-a-role rule, "lead with a frame", semantic tokens, never mix native and web).
2. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/SKILL.md` — PanelUI web skill (shadcn/ui tokens, Tailwind v4, React 19; colour-as-role; compound dotted parts; Frame-first composition; label/valueText for screen readers).
3. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components.md` — component catalogue (read because the panelui-web SKILL.md instructs it before writing UI; same skill, not another skill).
4. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/kpi.md` — Kpi anatomy (Header/Icon/Title/Actions, Content/Stat/Value/Trend, Chart, Progress, Footer, Group; Trend sign→colour, goodDirection, threshold).
5. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/stat.md` — Stat anatomy (Label/Value/Unit/Badge; value sizes sm/md/lg).
6. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/bar-chart.md` — BarChart anatomy (Grid/Bar/XAxis/YAxis/Tooltip/Header; value axis always includes zero; cornerRadius 4; colour is a role; aspectRatio 2).
7. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/table.md` — Table anatomy (density comfortable, striped, framed, numbers align="end" with tabular figures, scrolls sideways inside its own box).
8. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/frame.md` — Frame anatomy (default/band/inset/plain; band = muted band + title strip + inner bordered card; Panel draws hairlines between children).
9. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/nav-rail.md` — NavRail anatomy (expandedWidth 15rem, collapsedWidth 3.5rem, variant tonal/solid, active pill + accent bar, Badge becomes a dot when folded, app drives collapse — never viewport-driven).
10. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/badge.md` — Badge (default solid; secondary/outline for kinds; four tinted state variants; dot/pulse for live states).
11. `/home/hatch/workspace/ui-skills/PanelUI/skills/panelui-web/components/card.md` — Card anatomy (Header/Title/Description/Action/Content/Footer; padding lives on the parts).
12. `/home/hatch/workspace/custom-skills/local-astryx/SKILL.md` — Astryx adapter (Meta design system, 174 components, 7 themes, theme-neutral default, themes as CSS custom property overrides, per-component imports, StyleX authoring invisible to consumers, accessibility 4.5:1 as part of the contract, CLI bootstrap via `astryx docs` / `component --list`).

## Assigned but NOT fully used / not consulted

- **Astryx CLI docs (`astryx docs tokens/theme/principles`, `astryx component <Name> --dense`)**: NOT run. The local-astryx skill says to start every Astryx task with the CLI bootstrap, but that requires `npm install @astryxdesign/core @astryxdesign/cli` and the task forbids npm/build. Consequence: exact Astryx theme-neutral token names and values were **not** verified from source; the `--ax-*` custom properties in `index.html` are my own neutral-theme adaptation of the skill's description ("a theme is a set of CSS custom property overrides"), not copied Astryx token values. This is recorded as a limitation, not a use.
- **PanelUI `setup.md` / `theming.md`**: not read — no React/shadcn project exists here (single-file HTML adaptation per TASK.md); the theming rules needed were stated in the SKILL.md itself.
- **No other skill, no other variant folder was read.**

## Conflicts hit (two design systems, one page) and how each was resolved

1. **Token collision — both systems own `:root`.** PanelUI web sits on shadcn semantic tokens (`--background`, `--primary`, `--muted`, `--destructive`, `--border`…); Astryx themes are also a full set of CSS custom property overrides over the same scope. Two `--background` definitions cannot coexist. **Resolution:** one merged semantic layer with namespaced custom properties — `--ax-*` (Astryx theme-neutral concrete values) and `--pui-*` (PanelUI role tokens) — where every `--pui-*` value is `var(--ax-*)`. Astryx owns the palette values; PanelUI owns the role names. Neither system silently wins; the mapping is explicit in the `:root` block.
2. **Colour-as-role (PanelUI) vs theme-token values (Astryx).** PanelUI forbids hex/Tailwind palette classes for a component's own colour (`color="primary|info|success|warning|destructive"`); Astryx components consume theme tokens. **Resolution:** PanelUI roles are bound to Astryx theme values at the token layer (primary→accent, success→theme success, etc.). Every coloured element on the page uses a role class (`kpi__icon--success`, `badge--warning`, `barchart` primary), never a raw hex. Roles are picked by meaning per the PanelUI rule (success = good news, warning = attention, destructive = bad, info = neutral emphasis, primary = otherwise) and no two KPI cards share the same role.
3. **Radii — shadcn `--radius` vs Astryx radius scale.** PanelUI/shadcn radius is 0.625rem-based (card 12px, inner 10px, small 8px, pill 999px); Astryx has its own radius scale (values not verified — see "not consulted" above). **Resolution:** the shadcn scale won; Astryx's radius scale was overridden to match. Recorded, not hidden: if Astryx's real scale differs, the page is PanelUI-radius.
4. **Typography — two type scales, one stack.** PanelUI: system stack, small uppercase group labels (11px, 0.09em tracking), tight heading tracking (-0.02em). Astryx: its own type scale and principles (not consulted via CLI). **Resolution:** a single scale (12/13/14/20/28px) on the system font stack, no external fonts per TASK.md. Astryx-style tight tracking (-0.02em) on page/card headings; PanelUI-style uppercase labels on nav groups and table headers. One scale, two mannerisms — both visible, neither duplicated.
5. **Component anatomy — compound parts (PanelUI) vs per-component props (Astryx).** PanelUI composes dotted parts (`Kpi.Header/Value/Trend`, `Frame.Header/Panel/Row`, `Table.Header/Row/Cell`, `NavRail.Group/Item/Badge`); Astryx imports one component per path with variant props. The same stat card could be built either way. **Resolution:** PanelUI anatomy won for structure — the page is built from Kpi, Frame, BarChart, Table, NavRail, Badge, Card anatomy (mirrored as HTML classes, e.g. `kpi__header`, `frame__panel`, `nav-rail__item`). Astryx conventions won for interaction/accessibility: per-component-style props thinking (aria labels on every icon-only control, `aria-current="page"` on the active nav item, `aria-expanded` on the rail trigger and bell, `role="progressbar"` with values on Kpi.Progress, `role="img"` + text alternative on the chart, 4.5:1-target contrast on muted text).
6. **Page layout — Astryx templates vs PanelUI "lead with a frame".** Astryx offers page templates with skeletons; PanelUI says a panel sits in a frame (muted band + title, round inner bordered card). **Resolution:** Astryx template thinking provides the page skeleton (sticky header / nav rail / content grid / footer); PanelUI Frame provides the panel chrome inside it. No Astryx template was copied (none consulted); the skeleton is the standard dashboard shape both systems agree on.
7. **Navigation — NavRail vs Astryx navigation.** Both systems have navigation components. **Resolution:** PanelUI NavRail won outright (explicit in the assignment: "PanelUI web only" for structure; its collapse contract — app-driven, never viewport-driven — is honoured by the JS toggle). At ≤640px it folds to a bottom bar, which is the mobile form of the same rail, not a second nav system. Astryx contributes only the theme tokens that colour it.
8. **Charts — PanelUI BarChart vs Astryx charts.** Astryx's chart packages (`@astryxdesign/vega`, `@astryxdesign/charts`) are published only under `@canary` — the skill says do not depend on them in production. **Resolution:** PanelUI BarChart won by default, adapted to CSS bars per TASK.md (value axis includes zero — bars sized as % of 100, never cropped; cornerRadius 4 on the growing end; single series in the `primary` role).
9. **"Do not mix" rule vs the assignment.** The Astryx skill says: "do not mix Astryx and a second component system in one project without the user choosing." **Resolution:** the user (via the parent task) explicitly chose this mix as a conflict test, so the mix is authorised — but the rule is why every conflict above is logged here instead of silently resolved.

## What each system visibly owns on the page

- **PanelUI:** NavRail (sidebar + collapse + active pill + badge-dot), Kpi.Group/Kpi stat cards (Title/Value/Trend/Progress/Footer), Frame band panels, BarChart (CSS bars, zero-based), Table (striped, numbers right-aligned), Badge state pills, Frame-row notices.
- **Astryx:** theme-neutral token values (the `--ax-*` palette every PanelUI role reads from), page-template skeleton (header/nav/content/footer), accessibility contract (aria labelling, focus-visible ring, reduced-motion handling, contrast targets).

## Self-check — 1280px and 390px (static + parse, no live browser)

Method: Python HTML parse (tag balance), data audit (12/12 students, 4/4 stats, 5/5 trend values, 3/3 notices, footer text), external-resource scan (0 external URLs/fonts/images), `node --check` on the extracted inline JS, and a CSS audit against the 1280/390 constraints. **No Chromium/Playwright exists in this VM, so there was no live render or screenshot — layout verdicts below are from code inspection, not a rendered page.**

- 1280px: app shell flexes (240px rail + fluid main, `min-width:0` on main so content cannot push the page wide); KPI grid 4 columns; split grid 1.7fr/1fr; table min-width 560px inside `.table-wrap{overflow-x:auto}` so it scrolls inside its card, never the page. No horizontal-scroll source found.
- 390px: nav rail hidden, fixed bottom nav shown (content has 96px bottom padding so nothing hides under it); header wraps (titles full-width row, search full-width row, bell in row 2); KPI grid 2 columns with 26px values (`1,248` ≈ 4ch, fits ~173px cards); chart height 150px, 5 bars + labels fit; table scrolls inside its card; notices stack in the single column. No clipped-text source found (all long strings have ellipsis or wrap).
- JS verified: `node --check` passes. Rail collapse toggles `aria-expanded`/label; nav single-active-pill works across desktop rail and mobile bottom nav; bell panel toggles and closes on outside click/Escape; search filters the 12-row table by name/roll with an empty-state note; bars grow in on load (disabled under `prefers-reduced-motion`).

## Defects found and fixed

1. **Invalid HTML nesting (found by code review):** the notification panel (`<span class="bell__panel">` containing `<p>` elements) was nested inside the `<button class="bell">` — buttons may only contain phrasing content. **Fixed:** moved the panel into a wrapping `<span class="bell-wrap">` (position:relative) beside the button; panel positioning and outside-click close still work. Re-parsed: tag balance clean.
2. **Mobile bell order (found with defect 1):** the 640px rule targeted `.bell` for `order:2`; after the wrapper fix the order rule moved to `.bell-wrap`. **Fixed.**

## Defects left

- None known from the static checks. (See UNVERIFIED below — a live render could still reveal one.)

## UNVERIFIED

- **No live-browser render or screenshot** at 1280px or 390px (no Chromium/Playwright in the VM). All layout verdicts are code-inspection, not observed rendering. In particular: `color-mix()` usage, the sticky header + sticky rail interplay, and the exact 390px KPI-card fit have not been seen in a real browser.
- **Astryx token values not verified from source** (CLI not run — npm forbidden by the task). The `--ax-*` palette is a neutral-theme adaptation, not Astryx's published theme-neutral values.
- **Contrast ratios not measured** (no render to measure against); muted text (`#5d6068` on `#f6f6f4`/`#ffffff`) was chosen to look ≥4.5:1 but not instrumentally verified.
- **Real React/Expo behaviour not applicable** — both systems were adapted to single-file HTML per the task; no React build, no `panelui-web` package install, no Astryx package install was performed.
