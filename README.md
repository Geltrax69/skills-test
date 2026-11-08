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

## How ranking works here

Ranking is provisional and single-run unless a variant is repeated. Model variation alone can change a screenshot, so one run is evidence, not proof. Defects are counted on the rendered page at 1280px and 390px: overflow / horizontal scroll, clipped text, overlapping elements, contrast failures, broken navigation, missing required sections.

## Skill sources (read-only)

ECC is a reference clone at `~/workspace/build-anything/ECC/` — never run its install script, never copy it wholesale. PStack is at `~/workspace/skills/pstack/`. UI bundles are in `~/workspace/skills/` and `~/workspace/ui-skills/`. The global harness that routes between them is `~/workspace/custom-skills/global-harness/`.
