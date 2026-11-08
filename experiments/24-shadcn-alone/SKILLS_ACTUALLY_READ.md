# Skills Actually Read — Variant 24-shadcn-alone

## Skills read in full (in order)

1. `/home/hatch/workspace/skills/shadcn/SKILL.md` — read in full before building.

## Assigned skills NOT used

- None. shadcn was the only assigned skill, and it was used.

Note: the skill's linked rule files (`rules/styling.md`, `rules/composition.md`,
`rules/forms.md`, `rules/icons.md`, `rules/base-vs-radix.md`, `cli.md`,
`design-system.md`, etc.) were **not** read — only the top-level `SKILL.md`
was read in full, per the task. The principles applied (semantic tokens,
variants before custom styles, `gap-*` spacing, full Card composition, Badge
over custom spans, Avatar with fallback, Separator over `<hr>`, `data-icon`
on button icons) all come from the SKILL.md itself.

## Adaptation note

shadcn/ui is a React + Tailwind component system (components are added as
source via `npx shadcn@latest add`; the skill's workflow is CLI-driven).
This experiment is a single self-contained HTML file with no build step, no
npm, and no React, so the components were hand-rendered in HTML/CSS
following the skill's anatomy and rules:

- Tokens: shadcn default ("new-york") light theme semantic variables
  (`--background`, `--foreground`, `--primary`, `--secondary`, `--muted`,
  `--accent`, `--border`, `--input`, `--ring`, `--radius: 0.625rem`).
  No raw color values in component styles.
- Composition (per the skill's Component Selection table, "Dashboard =
  Sidebar + Card + Chart + Table"): Sidebar, Card (full CardHeader /
  CardTitle / CardDescription / CardContent / CardFooter), Table, Badge
  (variants default / secondary / outline / destructive), Avatar with
  initials fallback, Input, Button (variants default / secondary / outline /
  ghost; sizes default / sm / icon), Separator, Alert, Progress.
- Rules honored: `gap-*` only (no `space-x-*`/`space-y-*`); single size
  token for square elements; `truncate`-style handling via `white-space:
  nowrap` + card-internal scroll where needed; no manual `z-index` on the
  popover (it stacks by DOM order); icons are inline SVG with `data-icon`
  on button icons and no sizing classes on icons inside components.
- The shadcn `Chart` component wraps Recharts; the fixed task requires CSS
  bars with no external chart library, so the trend is CSS bars using the
  `--chart-*` tokens.
- No CLI commands were run: there is no `components.json` project here and
  the task forbids npm/build steps, so `npx shadcn@latest add/docs/search`
  were not applicable.

## Self-check (1280px and 390px)

**Headless Chromium render: NOT RUN — blocked.** `/opt/meta-chromium/chrome`
refuses to start in this sandbox in every invocation tried (plain headless,
`--disable-webgl`, fresh `--user-data-dir`): it exits immediately with
`ERROR:chrome/app/chrome_main_delegate.cc:1283] Hatch WebGL domain policy is
required for browser startup`, exit code 2, no screenshot produced. The
browser broker (`/opt/hatch/bin/browser-broker`) is a leased-VM live-browser
daemon, not a headless tool available to this subagent. No render was
claimed. Verification below is static (code reading + computed layout math
+ tag-balance parse + `node --check` on the inline JS), as TASK.md allows
when a headless check is unavailable.

Static checks performed (all passed):
- All 12 students, all 4 stat values, trend values 94/96/93/97/95, all 3
  notices, and the exact footer text are present; 12 data rows in the table.
- Zero external URLs, zero `<link>`/`<script src>` tags, no external fonts
  or images (system font stack; avatars are initials; icons are inline SVG).
- HTML tag balance: no unclosed or stray tags. Inline JS passes
  `node --check`.
- Layout math at 1280px: content width 976px; stat columns 232px each
  (value "1,248" at 1.875rem fits); table card inner width 560px vs table
  `min-width` 544px, so the table fits without scrolling at desktop width.
- Layout math at 390px: stat columns 173px each (2-col grid; "95.2%" at
  1.5rem fits); table scrolls inside its card via `.table-scroll`
  (`overflow-x:auto`); sidebar hidden, bottom nav shown, content has
  5.5rem bottom padding so the fixed bottom nav cannot cover content.

## Defects found and fixed

1. **Chart bars overflowed the chart area (found by code review).** The
   bar `height:%` was relative to the full `.chart-col` height, but the
   column also holds the value label and day label, so value + 90%-height
   bar + label exceeded the 11rem chart height and would have pushed
   upward out of the card. Fixed by wrapping each bar in a
   `.chart-track` (`flex:1; min-height:0`) so bar percentages resolve
   against the remaining track space only.
2. **Header could crowd at 390px.** The "Add Student" outline button plus
   search plus bell plus menu button risked squeezing the search input to
   nothing. Fixed by hiding the "Add Student" button below 768px
   (`.hide-mobile`) and adding `min-width:0` to `.header-search` and
   `.input-wrap` so the search input can shrink instead of overflowing.

## Defects left

- None known from the static check. (See UNVERIFIED below.)

## UNVERIFIED

- **No live render was produced** (headless Chromium blocked by the Hatch
  WebGL domain policy — see above). Therefore: no horizontal scroll, no
  clipped text, no overlap, and the exact rendered stat-card/table
  behaviour at 1280px and 390px are **unverified in a real browser**;
  they rest on the static checks and layout math above. A live-browser
  look/screenshot is still needed before this variant can be ranked
  with confidence.
- The bell popover, mobile sidebar drawer, and live search filter are
  JavaScript-driven; their behaviour is syntax-checked only, not
  exercised in a browser.
- `color-mix()` (used for the sticky header backdrop tint and button hover
  states) is supported in modern Chromium but was not render-tested here.
