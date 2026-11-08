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

---

# UI Forge — Experiment 2: Results (provisional, single run except repeats)

23 new variants (08–30), same TASK.md, same data, same constraints. Total 31 variants with Experiment 1. Built by fresh subagent builders, one per variant, isolated folders, no variant read another variant's folder.

## Uniform content check (run by the parent, 2026-11-09)

Same script as Experiment 1, re-run over **all 31 variants** (students 12/12, rolls 12/12, stats 4/4, notices 3/3, footer, external URLs 0). Experiment 1's eight reproduce their earlier numbers exactly.

| Variant | Size | Students | Rolls | Stats | Notices | Footer | External URLs |
|---|---|---|---|---|---|---|---|
| 08-layout-alone | 16.3 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 09-colors-alone | 21.2 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 10-colors-typography | 24.8 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 11-layout-typography | 24.3 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 12-typography-layout-r2 | 17.9 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 13-pstack-ecc-custom-r2 | 35.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 14-panelui-web-r2 | 32.2 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 15-astryx-pstack-ecc | 41.2 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 16-opensourceui-pstack-ecc | 25.9 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 17-panelui-pstack-ecc | 39.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 18-emil-alone | 28.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 19-better-interface-alone | 24.1 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 20-accessibility-alone | 20.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 21-taste-alone | 21.6 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 22-impeccable-alone | 32.7 KB | 12/12 | 12/12 | 4/4* | 3/3 | yes | 0 |
| 23-uiuxpromax-alone | 29.1 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 24-shadcn-alone | 44.4 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 25-prototyper-alone | 42.8 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 26-astryx-opensourceui | 28.5 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 27-panelui-astryx | 37.4 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 28-design-qa-alone | 26.6 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 29-motion | 19.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 30-full-ui-stack | 34.4 KB | 12/12 | 12/12 | 4/4* | 3/3 | yes | 0 |

\* Source-text check only: 22 renders attendance as `95.2<span>%</span>` and 30 renders stats from JS data (`value: "95.2", unit: "%"`), so the literal strings `95.2%` / `84%` do not appear in the file source. The render check below confirms all four stats appear in the rendered page for both. 17's single "external URL" match is the SVG namespace string `http://www.w3.org/2000/svg` in JS — not a network fetch.

## Render check (NEW — headless Chromium, run by the parent, 2026-11-09)

Correction to Experiment 1's "no Chromium/Playwright in this VM": `/opt/meta-chromium/chrome` cannot start here (fails "Hatch WebGL domain policy is required for browser startup"), but the Playwright Chromium binary at `~/.cache/ms-playwright/chromium-1248/chrome-linux64/chrome` (Chrome 156) **does** run. The parent drove it over CDP (Node 24, built-in WebSocket): every variant loaded from `file://` at 1280px and 390px, measuring document horizontal scroll, elements extending past the viewport, rendered text, console errors, and a screenshot per width (62 screenshots, `/tmp/renders/`, not committed).

Result for **all 31 variants, both widths**: **no document horizontal scroll, 0 console errors, all required text present in the rendered page.** This supersedes Experiment 1's "UNVERIFIED by render" for layout/scroll/console — those are now measured, headless. Feel on real hardware, touch, screen readers, and Safari/Firefox remain unverified.

Three check artefacts, investigated, not defects:

- **19-better-interface-alone, "12 names missing from rendered text":** the names render in `text-transform: uppercase` (`AARAV SHARMA`), so the case-sensitive text probe missed them. The 1280px screenshot was viewed: all 12 names, rolls, attendance and marks are visibly present.
- **15-astryx-pstack-ecc @390, "28 overflowing elements":** all 28 are the hidden off-canvas mobile nav drawer and its children (`right=-14` — positioned off-screen left, not visible). The 390px screenshot was viewed: header, search, stats, chart and bottom nav render cleanly, no visible overflow. This also confirms variant 15's own earlier CDP PASS claim, which had been flagged doubtful.
- **10/11/13/25/29 overflow flags:** same pattern — one visually-hidden label (10, `right=-9861`, an off-screen accessibility label) and hidden off-canvas sidebars/drawers at 390px (11, 13, 25, 29). No document horizontal scroll in any of them.

## Defects found and fixed, Experiment 2 (self-reported by each builder)

Same caveat as Experiment 1: "found + fixed" measures checking thoroughness as much as combo quality. A builder that checked harder finds more.

| Variant | Found + fixed | Left known |
|---|---|---|
| 08-layout-alone | 2 — app-shell grid areas, header search squeeze at 390px | 0 |
| 09-colors-alone | 3 — muted-text contrast 4.46:1 → 5.06:1, chart overflow, draft comment values | 0 |
| 10-colors-typography | 4 — thin contrast margin, heading weight 650, flat trend bars, gamut-edge warning (flagged, kept) | 0 |
| 11-layout-typography | 5 — mobile search never displayed (HIGH, cascade order), duplicated roll, fixed line-height, brand overflow, dead CSS | 0 |
| 12-typography-layout-r2 | 4 — dead CSS, inline styles, plus 2 pre-build design changes (100dvh, minmax(0,1fr)) | 1 — trend bars look near-flat on a true 0–100 scale (deliberate, honest) |
| 13-pstack-ecc-custom-r2 | 1 — bell panel max-width; 1 flagged fixed-height check investigated, no change | 0 |
| 14-panelui-web-r2 | 5 — chart 0–100 baseline, Kpi vs Stat choice, table/stat/topbar at 390px (2 are pre-build design decisions) | 0 (`color-mix()` fallback risk noted) |
| 15-astryx-pstack-ecc | 2 — dead sort button, invalid first 390px probe method | 0 (cosmetic stat sub-line wrap) |
| 16-opensourceui-pstack-ecc | 2 — calendar leading blanks, footer dash entity | 0 |
| 17-panelui-pstack-ecc | 4 — 1 checker false positive + KPI border seam, chart axis alignment, table min-width | 0 |
| 18-emil-alone | 5 — stat overflow, table scroll, footer behind bottom nav, popover overflow, `transition: all` audit | 0 |
| 19-better-interface-alone | 3 — broken aria-labelledby, duplicate bell indicators, colour-only status (design-time) | 0 |
| 20-accessibility-alone | 3 — `outline: none`, visual-only chart, pointer-only table scroll | 0 |
| 21-taste-alone | 2 — bell badge overflow, search cramped at 390px | 0 |
| 22-impeccable-alone | 4 — stat-note nowrap, notice tags at 390px, stat-grid divider, table at 390px | 0 |
| 23-uiuxpromax-alone | 3 — chart overflow, invalid CSS variable, aria-sort placement | 0 |
| 24-shadcn-alone | 2 — chart overflow, header crowding at 390px | 0 |
| 25-prototyper-alone | 3 — tooltip spill at 390px, footer behind bottom nav, header crowding | 0 (1 possible cosmetic: placeholder may ellipsise) |
| 26-astryx-opensourceui | 5 — table overflow, bottom-nav overlap, header crowding, stat size, mini-bars (1 item investigated, left as-is) | 0 (emoji-font dependence, illustrative bar heights noted) |
| 27-panelui-astryx | 2 — invalid HTML (`<p>` inside `<button>`), mobile order rule after fix | 0 |
| 28-design-qa-alone | 2 — Friday bar not highlighted, bell aria-describedby note | 0 |
| 29-motion | 2 — both check artefacts (comment words flagged) + 1 dead CSS class removed | 0 |
| 30-full-ui-stack | 1 — search squeezed to ~150px at 390px (fixed, re-screenshot confirmed) | 0 |

## Repeat evidence (02 / 04 / 07 — run 1 vs run 2, fresh builders)

| Combo | Run 1 (Exp 1) | Run 2 (Exp 2) | Did the ranking hold? |
|---|---|---|---|
| Typography → Layout (02 / 12) | 0 real defects, 0 left | 4 items (2 fixes + 2 pre-build), 1 known-left (flat bars, deliberate) | Quality held (0 real layout failures in both); defect *count* did not reproduce — different builders find different things. |
| pstack+ecc+custom (04 / 13) | 7 fixed, 0 left | 1 real fix, 0 left; same 2 conflicts resolved identically (press 0.96, stagger 100ms) | Quality held and the repeat draft was cleaner; run 1's count of 7 looks like builder thoroughness, not combo quality. Do not treat "7" as a reproducible score. |
| PanelUI web (07 / 14) | 4 fixed, 0 left | 5 fixed (incl. 2 design decisions), 0 left; same issue classes (chart baseline, table/stat at 390px) | Most reproducible of the three: same defect classes, similar count. |

**Repeat lesson:** the ranking order of the repeated combos held (all three stay top-tier), but defect counts are builder-dependent and not reproducible scores. Any future ranking should use repeat-averaged counts, not single-run counts.

## Order effect (02 vs 11 — Typography → Layout vs Layout → Typography)

- 02 (Typography → Layout): 0 real defects.
- 11 (Layout → Typography): 5 fixed, including 1 HIGH (mobile search never displayed — a base CSS rule overriding a media query by cascade order).

**Verdict: inconclusive.** 11 found more defects, but the builders differ and 11's HIGH defect is a plain CSS cascade-order mistake, not something any skill-order rule predicts. There is no evidence skill order changed the outcome; there is evidence builder variance dominates. A real order test needs the same builder (or several builders per order) — not run here.

## Conflict findings (26 / 27 / 30 — two or more design systems, one page)

| Variant | Conflicts recorded | Sharpest resolutions |
|---|---|---|
| 26-astryx-opensourceui | 12 | Radius: Astryx 12/10/6 beat OSUI 16px. Accent: Astryx blue kept as system accent, OSUI rose reserved for urgent/error. Elevation: OSUI flat won — Astryx shadows dropped, the frosted-glass notification banner was flattened (most visible casualty). Typography: neither named font loadable (no external fonts), Georgia/system-ui stand-ins. |
| 27-panelui-astryx | 9 | Token collision: one merged layer, `--ax-*` values + `--pui-*` roles, every PanelUI role bound to an Astryx value. Radii: shadcn scale won. Anatomy: PanelUI compound parts for structure, Astryx conventions for interaction/a11y. Charts: PanelUI BarChart won (Astryx charts are `@canary`-only). Astryx's "do not mix" rule was explicitly overridden by the task's conflict-test authorisation, and logged. |
| 30-full-ui-stack | 10 | Press scale emil 0.97 vs better-ui 0.96 → better-ui (exact value). Stagger emil 30–80ms vs better-ui 100ms → better-ui, stat cards only. Easing split by use. Tokens: Astryx neutral values on PanelUI shadcn semantic names. ECC 80%-coverage rule replaced with a real headless-browser check. |

**Conflict lesson:** two design systems *can* share one page, but every merge costs a named decision and at least one visible casualty (26 lost the glass banner; 27 lost Astryx's radius scale; 30 needed 10 recorded decisions for 6 skills + 7 component docs). The "everything" combo (30) still shipped with 0 defects left — but its cost is decision overhead, not breakage. No variant failed to build; no conflict was unresolvable.

## Provisional ranking — all 31 (single run except repeats, headless-render verified)

Ranked on: defects left known (0 for 30 of 31), thoroughness of checking, accessibility work, working interaction, conflict resolution quality, and repeat consistency — not on rendered visual feel, which was measured only for scroll/console/text, not judged.

1. **04-pstack-ecc-custom** — still the most thoroughly checked build (7 fixed, 0 left, 15/15 checks); repeat (13) reproduced its quality.
2. **13-pstack-ecc-custom-r2** — repeat held: same conflicts resolved identically, 0 left, cleanest draft of the repeats.
3. **14-panelui-web-r2** — repeat held: 5 fixed, 0 left, same defect classes as run 1; most reproducible repeat.
4. **07-panelui-web** — most complete component system (NavRail, KPI + Progress, BarChart, Table, Frame), 4 fixed.
5. **30-full-ui-stack** — the "everything" combo works: 1 defect fixed, 0 left, 10 conflicts resolved with named owners, contrast measured 5.59:1–17.74:1, live search verified in the headless render.
6. **03-pstack-ecc** — 0 defects, working search popover + filter with empty state.
7. **11-layout-typography** — 5 fixed including 1 HIGH, 0 left; strongest order-swap data point.
8. **18-emil-alone** — 5 fixed, 0 left; strongest single-skill motion/interaction discipline.
9. **26-astryx-opensourceui** — 5 fixed, 12 conflicts resolved explicitly; the most honest conflict record.
10. **02-typography-layout** — 0 real defects, strongest layout rules; repeat (12) held.
11. **12-typography-layout-r2** — repeat of 02; 1 known-left (flat bars, deliberate).
12. **17-panelui-pstack-ecc** — 4 fixed, 0 left.
13. **22-impeccable-alone** — 4 fixed, 0 left.
14. **10-colors-typography** — 4 fixed, measured contrast throughout, 0 left.
15. **08-layout-alone** — 2 fixed, 0 left; app-shell grid areas, header wrap at 390px.
16. **19-better-interface-alone** — 3 fixed, 0 left; strong a11y (named bell, text status badges, labelled chart).
17. **20-accessibility-alone** — 3 fixed, 0 left; strongest accessibility-only build (skip link, chart text alternative, keyboard-scrollable table).
18. **23-uiuxpromax-alone** — 3 fixed, 0 left.
19. **25-prototyper-alone** — 3 fixed, 0 left; accessible primitives, 1 possible cosmetic.
20. **09-colors-alone** — 3 fixed, measured contrast 4.46:1 → 5.06:1, 0 left.
21. **15-astryx-pstack-ecc** — 2 fixed, 0 left; render independently re-verified (see above).
22. **16-opensourceui-pstack-ecc** — 2 fixed, 0 left.
23. **21-taste-alone** — 2 fixed, 0 left; headless screenshots viewed by its builder.
24. **24-shadcn-alone** — 2 fixed, 0 left.
25. **27-panelui-astryx** — 2 fixed, 9 conflicts resolved, 0 left.
26. **28-design-qa-alone** — 2 fixed, 0 left; narrow scope (QA checklist, not a builder).
27. **05-astryx** — 1 fixed, 0 left; strongest a11y contract in Exp 1, adapted to plain HTML.
28. **06-opensourceui** — 2 fixed, 0 left; good component adaptation.
29. **29-motion** — 2 flagged (both check artefacts), 0 left; motion-only scope, no layout/type/color guidance.
30. **01-typography** — 0 defects, but narrowest scope: typography only, no layout or interaction improvement over baseline.
31. **00-baseline** — control. Works, all content present, but 2 defects fixed and 1 possible left, and no skill guidance to catch them.

Bottom three: **29-motion, 01-typography, 00-baseline**. Top five: **04, 13, 14, 07, 30**.

## What this ranking does NOT prove (updated)

- Layout, horizontal scroll, console errors and rendered text are now **measured** (headless Chromium, all 31, both widths) — but visual feel, taste, and real-device behaviour are still unjudged. No variant was eyeballed on real hardware.
- One run per variant except the three repeats. Defect counts did not reproduce across repeats (04: 7 → 13: 1), so counts are builder-dependent; treat the ranking as provisional.
- Order effect (02 vs 11) is inconclusive — builder variance dominates.
- Astryx, OpenSourceUI and PanelUI were adapted to single-file HTML here, not real React / Expo builds.
- The Studio's own design (repo root `index.html`) uses better-interface / emil guidance; it is a showcase, not a 32nd experiment — it was not built under the same fixed task.

## Next experiments

- Repeat the top combos with 2–3 builders each and average the defect counts before re-ranking.
- Same-builder order test (Typography → Layout vs Layout → Typography).
- Real React build for Astryx / PanelUI-web and a live-browser screenshot pass on real hardware.
- Further combinations beyond this bounded set of 31 (e.g. shadcn + pstack + ECC, taste + emil, accessibility + motion) — only with parent approval, per the Experiment 2 bound.

---


# UI Forge — Experiment 3: Results (provisional, repeats averaged where available)


25 new variants (31–55), same TASK.md, same data, same constraints. Total 56 variants with Experiments 1–2. Built by fresh subagent builders, one per variant, isolated folders, no variant read another variant's folder. (Note: the brief said "24 new variants (31–54), total 55" but listed 31–55 — 25 variants; the total is 56, not 55.)


## Uniform content check (run by the coordinator, 2026-11-09)


Same script as Experiments 1–2, re-run over **all 56 variants** (students 12/12, rolls 12/12, stats 4/4, notices 3/3, footer, external URLs 0). All 56 pass.


| Variant | Size | Students | Rolls | Stats | Notices | Footer | External URLs |
|---|---|---|---|---|---|---|---|
| 31-shadcn-pstack-ecc | 26.9 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 32-taste-emil | 31.3 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 33-taste-pstack-ecc | 18.3 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 34-emil-pstack-ecc | 22.1 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 35-better-interface-pstack-ecc | 22.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 36-shadcn-typography | 27.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 37-shadcn-layout | 27.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 38-apple-alone | 31.1 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 39-emil-astryx | 23.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 40-emil-panelui | 29.4 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 41-panelui-opensourceui | 34.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 42-motion-interface | 26.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 43-accessibility-interface | 22.8 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 44-colors-layout | 22.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 45-jakubkrehel-all | 23.6 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 46-prototyper-pstack-ecc | 23.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 47-impeccable-pstack-ecc | 31.2 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 48-uiuxpromax-pstack-ecc | 26.4 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 49-taste-typography | 18.3 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 50-layout-colors-typography | 22.6 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 51-ecc-alone | 20.8 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 52-pstack-alone | 11.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 53-pstack-ecc-custom-r3 | 25.0 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 54-panelui-web-r3 | 32.7 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |
| 55-full-ui-stack-r2 | 24.9 KB | 12/12 | 12/12 | 4/4 | 3/3 | yes | 0 |

## Render check (headless Chromium 156 over CDP, run by the coordinator, 2026-11-09)


Same method as Experiment 2, re-run over **all 56 variants at 1280px and 390px** (112 screenshots, `/tmp/renders-exp3/`, not committed). Result: **no document horizontal scroll anywhere, 0 console errors, all required text present in the rendered page** — for all 56, both widths.


Two check artefacts, investigated, not defects:


- **19-better-interface-alone, "names missing from rendered text" (both widths):** same as Experiment 2 — the names render in `text-transform: uppercase` (`AARAV SHARMA`), so the case-sensitive probe missed them. The 1280px screenshot was viewed: all 12 names visibly present.

- **Overflow flags at 390px (20 variants):** all are hidden off-canvas sidebars/drawers (`right=-14`, `-12`, `-28`) or visually-hidden labels (`right=-9861`, `-9891`) — 10, 11, 13, 15, 25, 29, 31, 32, 33, 35, 39, 42, 44, 45, 46, 47, 48, 51, 55, plus 51's hidden span at 1280px. No document horizontal scroll in any of them. The 53 screenshot at 1280px and the 31 screenshot at 390px were viewed: clean.


## Defects found and fixed, Experiment 3 (self-reported by each builder)


Same caveat as before: "found + fixed" measures checking thoroughness as much as combo quality.


| Variant | Found + fixed | Left known |
|---|---|---|
| 31-shadcn-pstack-ecc | 1 — invalid `&rupee;` entity rendering literally as text | 0 |
| 32-taste-emil | 3 — invented notice sub-lines; hamburger + bottom nav both visible at 390px; search placeholder truncated | 0 |
| 33-taste-pstack-ecc | 3 — invented notice sub-lines; chart bars overflowing the 200px plot; the word "violet" in a CSS comment | 0 |
| 34-emil-pstack-ecc | 0 — none — first headless run was already green | 0 |
| 35-better-interface-pstack-ecc | 1 — chart bars overflowing their container (no dedicated bar track) | 0 |
| 36-shadcn-typography | 2 — invented notice caption lines; header subtitle ellipsis-clipped at 390px | 0 |
| 37-shadcn-layout | 3 — invalid `&rupee;` entity; school name truncated at 390px; table caption clipped at card edge | 0 |
| 38-apple-alone | 1 — table `<caption>` clipped at 390px inside the scrolling wrapper | 0 |
| 39-emil-astryx | 4 — wrong class average 94.0% → 93.9%; invisible attendance meter bars; entity-hidden strings; invented teachers claim | 0 |
| 40-emil-panelui | 1 — invented notice sub-lines | 0 |
| 41-panelui-opensourceui | 0 — none — the merge itself broke nothing (14 conflicts recorded instead) | 0 |
| 42-motion-interface | 1 — reduced-motion CSS nulled the mobile drawer's closed state, sidebar covering the page at 390px | 0 |
| 43-accessibility-interface | 5 — header inside the desktop flex row; chart bars overflowing the track; invalid box-shadow alpha; dead CSS; bottom-nav item past the 390px viewport | 0 |
| 44-colors-layout | 5 — chart bars as ~8px slivers (no definite containing block); search squeezed to ~20px at 390px; hidden empty h2; false chart note; notices before the student table | 0 |
| 45-jakubkrehel-all | 2 — wrong class average 94.0% → 93.9%; header overflow at 390px | 0 |
| 46-prototyper-pstack-ecc | 0 — none — check pass found zero objective violations | 0 |
| 47-impeccable-pstack-ecc | 3 — chart bars overflowing their 190px track; topbar title squeezing search at 390px; unexplained pill tiers | 0 |
| 48-uiuxpromax-pstack-ecc | 1 — search input clipped at 390px (placeholder showed only "Search") | 0 |
| 49-taste-typography | 1 — mobile header crowding at 390px (~424px of content in 358px) | 0 |
| 50-layout-colors-typography | 1 — school name truncated to "Greenwo…" at 390px | 0 |
| 51-ecc-alone | 4 — 3px horizontal overflow at 390px; search collapsed to a sliver; DOM order (nav before header); entity-hidden strings | 0 |
| 52-pstack-alone | 1 — rupee entity typo `&?#8377;` rendering literally | 0 |
| 53-pstack-ecc-custom-r3 | 5 — invalid `&rupee;` entity; nav `aria-labelledby` naming links "Menu"; undefined `.visually-hidden` class; search `aria-describedby` pointing at a hidden element; bell close stealing focus | 0 |
| 54-panelui-web-r3 | 1 — KPI footers clipped at 390px ("Across Greenwoo…", "1,188 present · 60 a…") | 0 |
| 55-full-ui-stack-r2 | 1 — search placeholder clipped at 390px ("Try Aar…"); header-height token 112px → 113px (measured) | 0 |

## Repeat evidence — averages after 3 runs (04 / 07) and 2 runs (02 / 30)


| Combo | Runs | Defects fixed per run | Average | Did the ranking hold? |
|---|---|---|---|---|
| pstack+ecc+custom (04 / 13 / 53) | 3 | 7, 1, 5 | **4.33** | Yes — all three runs are the top 3. Same 2 conflicts resolved identically in all three runs (press 0.96, stagger 100ms). Run 1's "7" and run 3's "5" vs run 2's "1" show the count is builder-dependent; the quality is not. |
| PanelUI web (07 / 14 / 54) | 3 | 4, 5, 1 | **3.33** | Yes — all three runs are in the top 6. Same defect classes in runs 1–2 (chart baseline, table/stat at 390px); run 3 found a new class (KPI footer clipping). |
| Typography → Layout (02 / 12) | 2 | 0, 4 | **2.00** | Quality held (0 real layout failures in both); count did not reproduce. |
| Full UI stack (30 / 55) | 2 | 1, 1 | **1.00** | Yes — both runs 1 defect fixed, 0 left. Conflict count did NOT reproduce: 10 in run 1, 19 in run 2 (run 2 logged more granular conflicts, e.g. easing/stagger/entrance split into separate decisions). |

**Repeat lesson, strengthened:** after 3 runs, 04 still wins — highest repeat average (4.33) and all three runs in the top 3. But no defect count is a reproducible score: the same combo produced 7, 1 and 5 across three fresh builders. Rankings below use repeat averages for repeated combos, not single-run counts.


## Order findings (02 / 10 / 11 / 45 / 50)


- 02 (Typography → Layout): 0 real defects. 11 (Layout → Typography): 5 fixed incl 1 HIGH. 10 (Colors → Typography): 4 fixed. 50 (Layout → Colors → Typography): 1 fixed, 9/9 headless checks PASS. 45 (Typography → Layout → Colors → Accessibility → Writing → UI): 2 fixed.

- **Verdict: still inconclusive.** Five order points, no consistent direction: the Layout-first variants range from 1 to 5 fixed, the Typography-first variants from 0 to 4. No order rule predicts the outcome; builder variance still dominates. A real order test needs the same builder (or several builders per order) — not run here.


## ECC alone vs pstack alone vs combined (51 / 52 vs 03)


- **51 ECC alone: 4 fixed, 0 left.** ECC's frontend-patterns + frontend-design-direction carry real design guidance on their own (composition, immutable state + reducer-style dispatch, debounced search, design direction's utilitarian ops-tool tone). Ranked #15.

- **52 pstack alone: 1 fixed, 0 left.** pstack is process, not design: it sequenced and verified the work, but every visual choice (navy header, system stack, ad hoc spacing, 768px breakpoint) was a plain neutral default the builder named as unguided. Ranked #49.

- **03 pstack + ECC (Exp 1): 0 defects, working search popover + filter with empty state.** Ranked #9.

- **Verdict:** the combination's value is now measurable — ECC supplies the design, pstack supplies the checking rigour. ECC alone is a competent builder; pstack alone is a competent checker with nothing to check against. Neither alone beats the combination.


## Conflict findings (41 / 55 — plus 26 / 27 / 30 from Experiment 2)


| Variant | Conflicts recorded | Sharpest resolutions |
|---|---|---|
| 41-panelui-opensourceui | 14 | OpenSourceUI flat paper cards beat PanelUI Frame bands; PanelUI's 5 status roles win, but its `--info` indigo hue was remapped to OpenSourceUI cyan (OSUI bans indigo-family); NavRail wins navigation; OSUI wins radii/typography/focus/flat/shadows. |
| 55-full-ui-stack-r2 | 19 | better-ui beat Emil on press (0.96/150ms), easing and stagger (100ms), and first-render entrance (skipped entirely); PanelUI beat Astryx as the component system and accent source; better-colors beat PanelUI on the info hue (teal, ≥15° from blue accent); task beat PanelUI on viewport-driven nav collapse. |

Total conflicts across the five conflict tests: 12 + 9 + 10 + 14 + 19 = **64**. Run 2 of the full stack recorded 19 vs run 1's 10 — the merge did not get worse; the logging got more granular. No conflict was unresolvable in any run.


## Provisional ranking — all 56 (repeats averaged where available, headless-render verified)


Ranked on: defects left known (0 for 55 of 56; 1 deliberate in variant 12), repeat-averaged defect counts for repeated combos, thoroughness of checking, accessibility work, working interaction, conflict-resolution quality, and repeat consistency — not on rendered visual feel, which was measured only for scroll/console/text, not judged.


| # | Variant | Skills | Defects fixed (avg if repeated) | Left | Size |
|---|---|---|---|---|---|
| 1 | 04-pstack-ecc-custom | pstack feature + ECC frontend-patterns + emil-design-eng + better-interface | 7 (avg 4.33) | 0 | 15.3 KB |
| 2 | 53-pstack-ecc-custom-r3 | pstack feature + ECC frontend-patterns + emil-design-eng + better-interface | 5 (avg 4.33) | 0 | 25.0 KB |
| 3 | 13-pstack-ecc-custom-r2 | pstack feature + ECC frontend-patterns + emil-design-eng + better-interface | 1 (avg 4.33) | 0 | 35.7 KB |
| 4 | 14-panelui-web-r2 | local-panelui + panelui-web | 5 (avg 3.33) | 0 | 32.2 KB |
| 5 | 07-panelui-web | local-panelui + panelui-web | 4 (avg 3.33) | 0 | 30.8 KB |
| 6 | 54-panelui-web-r3 | local-panelui + panelui-web | 1 (avg 3.33) | 0 | 32.7 KB |
| 7 | 55-full-ui-stack-r2 | pstack feature + ECC frontend-patterns + emil-design-eng + better-interface + local-astryx + local-panelui (web) | 1 (avg 1.00) | 0 | 24.9 KB |
| 8 | 30-full-ui-stack | pstack feature + ECC frontend-patterns + emil-design-eng + better-interface + local-astryx + local-panelui (web) | 1 (avg 1.00) | 0 | 34.4 KB |
| 9 | 03-pstack-ecc | pstack feature + ECC frontend-patterns | 0 | 0 | 19.8 KB |
| 10 | 43-accessibility-interface | Better Accessibility + better-interface | 5 | 0 | 22.8 KB |
| 11 | 44-colors-layout | Better Colors + Better Layout | 5 | 0 | 22.0 KB |
| 12 | 11-layout-typography | Better Layout + Better Typography | 5 | 0 | 24.3 KB |
| 13 | 18-emil-alone | emil-design-eng | 5 | 0 | 28.7 KB |
| 14 | 26-astryx-opensourceui | local-astryx + local-opensourceui | 5 | 0 | 28.5 KB |
| 15 | 51-ecc-alone | ECC frontend-patterns + frontend-design-direction | 4 | 0 | 20.8 KB |
| 16 | 39-emil-astryx | emil-design-eng + local-astryx | 4 | 0 | 23.0 KB |
| 17 | 17-panelui-pstack-ecc | local-panelui (web) + pstack feature + ECC frontend-patterns | 4 | 0 | 39.7 KB |
| 18 | 22-impeccable-alone | impeccable | 4 | 0 | 32.7 KB |
| 19 | 10-colors-typography | Better Colors + Better Typography | 4 | 0 | 24.8 KB |
| 20 | 12-typography-layout-r2 | Better Typography + Better Layout | 4 (avg 2.00) | 1 deliberate | 17.9 KB |
| 21 | 02-typography-layout | Better Typography + Better Layout | 0 (avg 2.00) | 0 | 17.0 KB |
| 22 | 47-impeccable-pstack-ecc | impeccable + pstack feature + ECC frontend-patterns | 3 | 0 | 31.2 KB |
| 23 | 37-shadcn-layout | shadcn + Better Layout | 3 | 0 | 27.7 KB |
| 24 | 08-layout-alone | Better Layout | 2 | 0 | 16.3 KB |
| 25 | 32-taste-emil | taste-skill + emil-design-eng | 3 | 0 | 31.3 KB |
| 26 | 33-taste-pstack-ecc | taste-skill + pstack feature + ECC frontend-patterns | 3 | 0 | 18.3 KB |
| 27 | 19-better-interface-alone | better-interface + 6 domain skills | 3 | 0 | 24.1 KB |
| 28 | 20-accessibility-alone | Better Accessibility | 3 | 0 | 20.7 KB |
| 29 | 23-uiuxpromax-alone | ui-ux-pro-max | 3 | 0 | 29.1 KB |
| 30 | 25-prototyper-alone | local-prototyper-ui | 3 | 0 | 42.8 KB |
| 31 | 09-colors-alone | Better Colors | 3 | 0 | 21.2 KB |
| 32 | 45-jakubkrehel-all | Better Typography + Better Layout + Better Colors + Better Accessibility + Better Writing + Better UI | 2 | 0 | 23.6 KB |
| 33 | 38-apple-alone | apple-design-skill | 1 | 0 | 31.1 KB |
| 34 | 36-shadcn-typography | shadcn + Better Typography | 2 | 0 | 27.7 KB |
| 35 | 15-astryx-pstack-ecc | local-astryx + pstack feature + ECC frontend-patterns | 2 | 0 | 41.2 KB |
| 36 | 16-opensourceui-pstack-ecc | local-opensourceui + pstack feature + ECC frontend-patterns | 2 | 0 | 25.9 KB |
| 37 | 21-taste-alone | taste-skill | 2 | 0 | 21.6 KB |
| 38 | 24-shadcn-alone | shadcn | 2 | 0 | 44.4 KB |
| 39 | 28-design-qa-alone | design-qa-checklist | 2 | 0 | 26.6 KB |
| 40 | 27-panelui-astryx | local-panelui (web) + local-astryx | 2 | 0 | 37.4 KB |
| 41 | 41-panelui-opensourceui | local-panelui (web) + local-opensourceui | 0 | 0 | 34.0 KB |
| 42 | 06-opensourceui | local-opensourceui | 2 | 0 | 23.5 KB |
| 43 | 05-astryx | local-astryx | 1 | 0 | 18.0 KB |
| 44 | 35-better-interface-pstack-ecc | better-interface + 6 domain skills + pstack feature + ECC frontend-patterns | 1 | 0 | 22.0 KB |
| 45 | 31-shadcn-pstack-ecc | shadcn + pstack feature + ECC frontend-patterns | 1 | 0 | 26.9 KB |
| 46 | 48-uiuxpromax-pstack-ecc | ui-ux-pro-max + pstack feature + ECC frontend-patterns | 1 | 0 | 26.4 KB |
| 47 | 49-taste-typography | taste-skill + Better Typography | 1 | 0 | 18.3 KB |
| 48 | 50-layout-colors-typography | Better Layout, then Better Colors, then Better Typography | 1 | 0 | 22.6 KB |
| 49 | 40-emil-panelui | emil-design-eng + local-panelui (web) | 1 | 0 | 29.4 KB |
| 50 | 42-motion-interface | animate + accessible-animation + better-interface | 1 | 0 | 26.0 KB |
| 51 | 52-pstack-alone | pstack feature playbook | 1 | 0 | 11.0 KB |
| 52 | 34-emil-pstack-ecc | emil-design-eng + pstack feature + ECC frontend-patterns | 0 | 0 | 22.1 KB |
| 53 | 46-prototyper-pstack-ecc | prototyper-ui + pstack feature + ECC frontend-patterns | 0 | 0 | 23.7 KB |
| 54 | 29-motion | animate + accessible-animation | 2 | 0 | 19.7 KB |
| 55 | 01-typography | Better Typography | 0 | 0 | 14.3 KB |
| 56 | 00-baseline | (no skills — control) | 0 | 0 | 11.7 KB |

Bottom three: **29-motion, 01-typography, 00-baseline** — unchanged from Experiment 2. Top five: **04, 53, 13, 14, 07**.


## What this ranking does NOT prove (updated)


- Layout, horizontal scroll, console errors and rendered text are now **measured** (headless Chromium 156, all 56 variants, both widths) — but visual feel, taste, and real-device behaviour are still unjudged. No variant was eyeballed on real hardware.

- Defect counts did not reproduce across repeats (04: 7 → 1 → 5; 07: 4 → 5 → 1; 30 conflicts: 10 → 19), so counts are builder-dependent; this ranking uses repeat averages for repeated combos, but single-run counts elsewhere are still one builder's thoroughness.

- Order effect is still inconclusive after five order points — builder variance dominates.

- Astryx, OpenSourceUI and PanelUI were adapted to single-file HTML here, not real React / Expo builds. The ui-ux-pro-max skill's real entry point is `.claude/skills/ui-ux-pro-max/SKILL.md` (the repo-root path given in the Experiment 3 brief does not exist) — variant 48 read the correct file; variant 23 (Exp 2) used the path as given then.

- The Studio's own design (repo root `index.html`) uses better-interface / emil guidance; it is a showcase, not a 57th experiment — it was not built under the same fixed task.


## Next experiments


- Same-builder order test (one builder builds both orders) to settle the order question.

- Real React build for Astryx / PanelUI-web and a live-browser screenshot pass on real hardware.

- Further combinations beyond this bounded set of 56 — only with parent approval, per the Experiment 2 bound.
