# Subject-wise Database — ALL 7 SSC Exams (2019–2026 window)

Updated 2026-09-22 (v2). Clean re-layout of `database_full` covering every exam, split subject-wise and pre/mains. Rows copied 1:1 — classification, solutions, images all unchanged.

## Year window

**Only 2019–2026 papers are included.** The raw scrape folders also contain 2016/2017/2018 papers (e.g. `SSC-CGL/Previous_Year_Paper_Tier_I/2016`, `SSC-MTS/.../2017`), but `database_full` never ingested them, and this database inherits that scope. Verified: 0 rows older than 2019 in any of the 144,944 rows.

## Tier rule per exam

| Exam | pre | mains | Why |
|---|---|---|---|
| SSC-CGL | `Previous_Year_Paper_Tier_I` | `Previous_Year_Paper_Tier_II` | two-tier exam |
| SSC-CHSL | `PYP_Tier_I` | `PYP_Tier_II` | two-tier exam |
| SSC-CPO | `Previous_Year_Paper_Paper_I` | `Previous_Year_Paper_Paper_II` | Paper-I/Paper-II exam |
| SSC-GD | all rows | – | single-stage exam |
| SSC-MTS | all rows | – | single-stage exam |
| SSC-Selection-Post | all rows | – | phases are separate exams, not tiers |
| SSC-Stenographer | all rows | – | single-stage exam |

## Shift field policy (verified against raw source files)

`shift` is populated from the paper's own filename/title (Shift 1/2/3/4). A paper has no `shift` value **only when the raw source itself has no shift** — confirmed by opening the raw JSON files:

- **CGL Tier-II** (8 papers, 1,200 rows): raw filenames/titles carry no shift — Tier-II is a single-session exam. The only "shift" strings inside the raw JSON are in REAS solution text ("letter shifts clockwise"), not paper metadata.
- **CPO Paper-II** (4 papers, 800 rows): same — single-session mains.
- **CHSL Tier-II 2024** (135 rows): single-session mains.
- **Selection-Post 2019 HSC/Graduation** (4 papers, 400 rows): single-session papers.

Every Tier-I / Paper-I / pre paper across all exams has its shift populated 100%.

## Row counts

| Exam / Tier | ENG | MATH | REAS | GK | COMPUTER | Total |
|---|---|---|---|---|---|---|
| CGL pre | 6,050 | 6,050 | 6,050 | 6,050 | – | 24,200 |
| CGL mains | 360 | 240 | 240 | 200 | 160 | 1,200 |
| CHSL pre | 6,880 | 7,475 | 7,978 | 7,566 | – | 29,900 |
| CHSL mains | 160 | 120 | 120 | 80 | 60 | 540 |
| CPO pre | 2,999 | 3,003 | 2,997 | 3,001 | – | 12,000 |
| CPO mains | 1,000 | – | – | – | – | 1,000 |
| GD | 6,845 | 6,845 | 6,845 | 6,845 | – | 27,380 |
| MTS | 7,350 | 6,525 | 6,525 | 7,350 | – | 27,750 |
| Selection-Post | 3,000 | 2,998 | 3,002 | 2,975 | – | 11,975 |
| Stenographer | 4,500 | 1 | 2,249 | 2,250 | – | 9,000 |
| **TOTAL** | | | | | | **144,944** |

## Duplicates policy

- CGL, CPO, GD, MTS, Stenographer: 0 duplicates in source.
- CHSL: 1 source duplicate qid (kept one copy).
- Selection-Post: 25 source duplicates (same GK question under Graduation+Matric papers of the same cycle; kept one copy each).
- Verified: output qid set == source qid set per exam; 0 dupes inside output.

## Row format

Original `database_full` row + `tier` ("pre"/"mains") + `source_subject`. All original fields preserved: qid, exam, year, held_on (ISO date, 100% populated), shift (where the exam has shifts), paper, paper_meta, subject, chapter, concept, question, options, correct, solution, solution_images, marks, type, tags.

`<exam>/years.json` lists distinct years per tier (each year exactly once). `manifest.json` has machine-readable counts.
