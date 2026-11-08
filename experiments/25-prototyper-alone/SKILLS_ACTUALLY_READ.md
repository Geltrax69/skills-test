# Skills Actually Read — variant 25-prototyper-alone

## Skill files read in full (in order)

1. `/home/hatch/workspace/custom-skills/local-prototyper-ui/SKILL.md` — read in full before building. This is the ONLY skill assigned to this variant, and it was used.

## Other files read (not skills)

- `/home/hatch/workspace/custom-skills/local-prototyper-ui/AGENTS.md` — read because the runtime injected it as house rules for that folder. It is not a skill file; it supplied the component conventions (data-slot, CVA, cn(), OKLCH tokens, surface hierarchy, `--ease-out-fluid`, Base UI primitives) that the build follows.
- `~/workspace/skills-test/TASK.md` — the fixed task/data/constraints for the experiment (read in full first, per the assignment).

## Assigned skill NOT used

None. Prototyper UI was the only assigned skill and it was used. No other skill was read, and no other variant folder was read.

## How Prototyper UI was applied (adaptation)

Prototyper UI is a React / Base UI library (copy-paste components, `bunx @prototyperco/cli add NAME`, Tailwind v4, OKLCH tokens). The experiment forbids a build step, npm/Bun, and external fonts/images, so the library was adapted to single-file HTML rather than installed — documented in the comment at the top of `index.html`:

- OKLCH tokens + surface hierarchy (`background → surface → surface-secondary → surface-tertiary`) as CSS custom properties, with a `.dark` theme variant.
- CVA-style variant classes (button: default/secondary/outline/ghost; badge: default/secondary/success/warning/outline/destructive; sizes sm/default) and a JS `cn()` helper.
- `data-slot="…"` attributes on every rendered element (172 occurrences).
- Base UI primitive *behaviour* re-implemented in accessible vanilla JS: Dropdown Menu (notification bell), Dialog (student profile — focus return, Esc, backdrop close), Toast (Sonner-style), Tooltip (chart bars, hover + keyboard focus), Tabs (attendance filter), collapsible Sidebar + bottom nav.
- CSS-first animation only, with `--ease-out-fluid`; `prefers-reduced-motion` disables it.

Deviation to note: SKILL.md says to read the component source and example in `apps/docs/registry/ui/` and `apps/docs/registry/example/` before composing. I did **not** read those registry files — the build follows the conventions stated in SKILL.md + AGENTS.md only, and no component source was copied. The CLI/MCP design bridge were not used (no React project, no live browser iteration set up).

## Self-check (1280px and 390px)

**Headless render: NOT RUN — see UNVERIFIED below.** The checks that were run:

- Node content check: 12/12 students, 12/12 rolls, 4/4 stats, 5/5 trend values, 3/3 notices, footer text exact, 0 external URLs, JS syntax OK (`node --check`), student data logic verified (≥95% filter = 6 students, <90% = 2, week average 95.0% matches the displayed value).
- Python `html.parser` tag-balance check: no unclosed tags, no mismatches.
- Code inspection against the TASK constraints at both widths (sidebar 232px + fluid main at 1280px; 2-column stats, drawer sidebar + bottom nav at 390px; table in an `overflow-x:auto` wrapper inside its card).

## Defects found and fixed (3)

1. **Chart tooltip overflow at 390px** — tooltips were centred on their bar, so the Mon and Fri tooltips would spill past the viewport edge on hover/focus. Fixed: first/last bar tooltips are anchored to the inner edge.
2. **Footer hidden behind the fixed bottom nav at 390px** — the content had bottom padding but the footer did not. Fixed: footer gets 88px bottom padding below 767px.
3. **Header crowding at 390px** — menu button + title + search + bell + theme toggle + avatar could squeeze the search input. Fixed: search input given `min-width:0`, avatar already hidden on mobile, and the (non-required) dark-mode toggle is hidden below 767px so the input keeps usable width.

## Defects left

0 known. One possible cosmetic: at 390px the search placeholder ("Search students…") may ellipsize to a few characters when the input is narrow — the input itself still works and shrinks without overflow.

## UNVERIFIED

- **No live render was run.** Headless Chromium at `/opt/meta-chromium/chrome` cannot start in this VM: it exits immediately with `Hatch WebGL domain policy is required for browser startup` (exit code 2, no screenshot produced), tried with `--headless=new --no-sandbox --disable-gpu --disable-webgl --user-data-dir=…`. The Chromium README confirms normal startup requires the image-owned WebGL policy supplied by browser-daemon, which is not present here. So the 1280px/390px checks above are code inspection + static checks only — no screenshot, no measured `scrollWidth`, no confirmed pixel layout, and the interactive primitives (menu, dialog, toast, tooltip) were not exercised in a real browser.
- Dark-mode toggle, dialog focus return, and toast timing are code-reviewed only, for the same reason.
