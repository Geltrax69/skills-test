# UI Forge — Experiment 1: Results (provisional, single run)

Task: "Create a modern student management dashboard with attendance, student profiles, analytics and responsive navigation." Same data, same sections, same constraints for all 8 variants. See `../TASK.md`.

## Uniform check (run by the parent, 2026-11-09)

Every variant passed content, measured the same way:

| Variant | Size | Students | Rolls | Stats | Notices | Footer | External URLs |
|---|---|---|---|---|---|---|---|
| 00-baseline | 11.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 01-typography | 14.3 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 02-typography-layout | 17.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 03-pstack-ecc | 19.8 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 04-pstack-ecc-custom | 15.3 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 05-astryx | 18.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 06-opensourceui | 23.5 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 07-panelui-web | 30.8 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |

## Defects found and fixed (self-reported by each builder, code-inspection only)

| Variant | Found + fixed | Left known |
|---|---|---|
| 00-baseline | 2 — rupee entity typo, chart bar overflow | 1 possible — search placeholder may ellipsise at 390px |
| 01-typography | 0 — 16/16 code checks PASS | 0 known |
| 02-typography-layout | 0 real — 2 flags were false positives | 0 known |
| 03-pstack-ecc | 0 — 5 check failures were check artifacts | 0 known |
| 04-pstack-ecc-custom | 7 — table overflow at 390px, stat columns, footer behind bottom nav, colour-only status, bell with no name, chart values unlabeled, input <16px | 0 known |
| 05-astryx | 1 — missing roll cell for Kabir Singh (8A-03) | 0 known |
| 06-opensourceui | 2 — typo breaking live search, wrong class average 94.1% → 93.9% | 0 known |
| 07-panelui-web | 4 — chart overflow, stat squeeze at 390px, table width, header crowding | 0 known |

"Found + fixed" is not the same as "made more mistakes". A builder that checked harder finds more. The 04 variant found the most because it ran the most checks (better-interface + all six domain skills), not necessarily because its first draft was worst.

## Provisional ranking (single run — NOT proof)

Ranked on: defects left known (0 for 7 variants, 1 possible for baseline), thoroughness of checking, accessibility work, and extra working interaction — not on rendered visual feel, which was not measured.

1. **04-pstack-ecc-custom** — most defects caught and fixed (7), text pills instead of colour-only status, named bell, labelled chart values, 16px input, 2 conflicts resolved explicitly (press scale 0.96, stagger 100ms), 15/15 checks.
2. **07-panelui-web** — most complete component system (NavRail, KPI + Progress, BarChart, Table, Frame), 4 defects fixed, 31KB.
3. **03-pstack-ecc** — working search popover + filter with empty state, Node stub-DOM render passed, 0 defects. Note: uses `100vw` in code (overflow-x hidden, but a risk).
4. **02-typography-layout** — strongest layout rules (logical properties, safe-area insets, `minmax(0,1fr)`, no `100vw`), 0 real defects.
5. **05-astryx** — strongest accessibility contract (landmarks, labelled search/bell, values in text not colour-only, reduced-motion), tokens + anatomy. Adapted to plain HTML, not a real React Astryx install.
6. **06-opensourceui** — good component adaptation (search input, customers table, step-count widget), 2 real defects fixed, including a search-breaking typo.
7. **01-typography** — cleanest type scale, 0 defects, but narrowest scope: typography only, no layout or interaction improvement over baseline.
8. **00-baseline** — control. Works, all content present, but 2 defects fixed and 1 possible left, and no skill guidance to catch them.

## What this ranking does NOT prove

- No variant was rendered in a live browser in this run. No Chromium/Playwright was available in the VM, so 1280px / 390px layout, clipping, overlap and feel are **UNVERIFIED** for all 8, by measurement.
- One run per variant. Model variation alone can change a screenshot. Repeat a promising variant before treating a win as real.
- Astryx, OpenSourceUI and PanelUI were adapted to single-file HTML here. That tests their design guidance, not their real React / Expo packages.
- Order effects were not tested in this run (e.g. Layout → Typography vs Typography → Layout). That is a follow-up experiment.

## Repeat / next experiments

- Repeat 04 and 07, then re-rank.
- Test order: Typography → Layout vs Layout → Typography.
- Test a real React build for Astryx / PanelUI-web, and a live-browser screenshot pass for all variants.
