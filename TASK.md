# UI Forge — Experiment 1: Student Management Dashboard

Fixed baseline task. Every variant gets this exact task, these exact data, these exact constraints. Only the skill combination differs.

## Task (verbatim for every variant)

"Create a modern student management dashboard with attendance, student profiles, analytics and responsive navigation."

## Fixed fictional data (use exactly these numbers)

- School: Greenwood High, Class 8-A, 2025–26
- Students: 1,248 total, 72 teachers, 12 students in Class 8-A
- Attendance today: 95.2% (1,188 present, 60 absent)
- Fees collected: 84% (₹25.2L of ₹30L)
- Class 8-A students (name, roll, attendance %, marks %):
  1. Aarav Sharma, 8A-01, 96%, 88%
  2. Diya Patel, 8A-02, 94%, 91%
  3. Kabir Singh, 8A-03, 89%, 76%
  4. Ananya Gupta, 8A-04, 98%, 93%
  5. Rohan Mehta, 8A-05, 92%, 82%
  6. Ishita Verma, 8A-06, 97%, 89%
  7. Arjun Nair, 8A-07, 90%, 79%
  8. Sneha Reddy, 8A-08, 95%, 86%
  9. Vikram Joshi, 8A-09, 88%, 74%
  10. Priya Das, 8A-10, 99%, 95%
  11. Aditya Rao, 8A-11, 93%, 84%
  12. Kavya Iyer, 8A-12, 96%, 90%
- Attendance trend (Mon–Fri): 94, 96, 93, 97, 95 (%)
- Notices: "Science fair on Friday", "Fee deadline 20 Nov", "Parent-teacher meet 25 Nov"

## Required sections (every variant, same order)

1. Header with school name, search, and a notification bell
2. Responsive navigation (sidebar on desktop, collapsible / bottom nav on mobile)
3. Four stat cards: Students 1,248, Teachers 72, Attendance 95.2%, Fees 84%
4. Attendance trend chart (Mon–Fri, CSS bars — no external chart library)
5. Student table / cards with name, roll, attendance %, marks %
6. Notices panel with the three notices above
7. Footer: "Fictional demo data — UI Forge Experiment 1"

## Fixed constraints (every variant)

- One self-contained file: `index.html` in your variant folder. Inline CSS + JS only. No build step, no npm, no external images.
- Must work at 1280px wide and at 390px wide with no horizontal scroll, no clipped text, no overlapping elements.
- No external fonts or images required; system fonts are fine unless your assigned skill says otherwise.
- Do not copy another variant. Do not read another variant's folder.
- Record what you actually did in `SKILLS_ACTUALLY_READ.md` in your variant folder: list every skill file you read in full (absolute path), in order, and any skill you were assigned but did NOT use. A skill counts as used only if you read its SKILL.md in full before building. Naming a skill is not using it.
- Self-check before finishing: open your page in a headless check if you can, otherwise check by reading the code — verify at 1280 and 390: no horizontal scroll, stat cards do not overflow, table fits or scrolls inside its card, text is not clipped. List the defects you found and fixed, and any defect left, in `SKILLS_ACTUALLY_READ.md`.

## Variant folders

- `experiments/00-baseline/`
- `experiments/01-typography/`
- `experiments/02-typography-layout/`
- `experiments/03-pstack-ecc/`
- `experiments/04-pstack-ecc-custom/`
- `experiments/05-astryx/`
- `experiments/06-opensourceui/`
- `experiments/07-panelui-web/`
