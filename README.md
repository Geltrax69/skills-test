# UI Forge — Skill Experimentation Studio

A visual workspace for experimenting with installed skills, combining them in different ways, and comparing how they change AI-generated interfaces.

This is **not** another autonomous coding harness. It does not rebuild ECC or PStack. It uses them — and the other installed skills — as sources, and measures what they actually do to a UI.

## Experiment 1 — Student Management Dashboard

Same task, same data, same constraints for every variant. See `TASK.md`.

| Variant | Skill combination |
|---|---|
| 00-baseline | No additional UI skills (control) |
| 01-typography | Better Typography |
| 02-typography-layout | Better Typography + Better Layout |
| 03-pstack-ecc | PStack feature playbook + ECC frontend skills |
| 04-pstack-ecc-custom | PStack + ECC + emil-design-eng + better-interface |
| 05-astryx | Astryx (Meta design system) |
| 06-opensourceui | OpenSourceUI (copy-paste React components, adapted to single-file HTML) |
| 07-panelui-web | PanelUI web (shadcn tokens) |

Each variant lives in `experiments/<variant>/index.html`, with a `SKILLS_ACTUALLY_READ.md` recording which skills were actually read in full, in order. A skill counts as used only if its SKILL.md was read in full before building.

## The Studio — open `index.html`

The repo root `index.html` is the full Studio website (one self-contained file, no build): Overview, Skill Library (567 skills, grouped by bundle — full list in `skills-inventory.md`), Combination Lab (all 31 combinations with skills in order), Experiments (all 31 variants as clickable cards), Compare (click any variant to preview it live in an iframe, pick two side-by-side, switch viewport 1280 / 390px), and Insights (ranking, defect counts, repeat consistency, conflicts, what worked). The Studio itself is designed with better-interface guidance and emil motion values as resolved in variant 30.

## Experiment 2 — 23 more variants (total 31)

Singles (layout, colors, accessibility, emil, taste, impeccable, ui-ux-pro-max, shadcn, prototyper, design-qa, motion), pairs and order swaps (colors+typography, layout+typography vs 02's typography+layout), library+pstack+ecc combos (15–17), repeats of 02/04/07 (12–14), conflict tests (26 Astryx+OpenSourceUI, 27 PanelUI+Astryx), and the full stack (30). Results, repeat evidence, order verdict (inconclusive), and the 31-variant ranking: `experiments/RESULTS.md`.

Headless render check (Playwright Chromium 156 over CDP): all 31 variants pass at 1280px and 390px — no horizontal scroll, 0 console errors, all required text rendered. Feel on real hardware is still unverified.

## How ranking works here

Ranking is provisional and single-run unless a variant is repeated. Model variation alone can change a screenshot, so one run is evidence, not proof. Defects are counted on the rendered page at 1280px and 390px: overflow / horizontal scroll, clipped text, overlapping elements, contrast failures, broken navigation, missing required sections.

## Skill sources (read-only)

ECC is a reference clone at `~/workspace/build-anything/ECC/` — never run its install script, never copy it wholesale. PStack is at `~/workspace/skills/pstack/`. UI bundles are in `~/workspace/skills/` and `~/workspace/ui-skills/`. The global harness that routes between them is `~/workspace/custom-skills/global-harness/`.
