# SSC All-Exam English-Only Datasets

Two English-only SSC previous-year-question datasets, rebuilt 2026-09-22. **Zero Hindi anywhere** — verified programmatically over every file (Devanagari scan = 0 hits across all 1,610 files).

```
subjectwise-db/   <- cleaned subject-wise question database (the analysis-ready DB)
all-exam/         <- the raw scraped paper files, exam by exam (the source papers)
```

---

## 1. `subjectwise-db/` — question database (subject-wise + pre/mains)

**144,944 unique questions** across all 7 SSC exams, split by subject and by pre/mains:

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

**Years:** only 2019–2026. Verified 0 rows older than 2019. Each exam folder has `years.json` listing the distinct years per tier — each year appears exactly once, read that file for year filters instead of seeing the same year on question after question.

**Why does CPO mains have only English?** Because SSC CPO Paper-II *is* an English paper. Checked at the raw-source level: all 1,200 Paper-II questions are English (Grammar 561, Cloze 150, Verbal Ability 142, Para Jumbles 100, Idioms 70, Vocabulary 68, plus small synonym/antonym/OWS/spelling counts). Zero Maths/Reasoning/GK exist in Paper-II. CHSL mains, by contrast, has all five subjects.

**Why is Stenographer MATH just 1 question?** The Steno exam has no Quant section. The source contains exactly one stray maths question (a depreciation question from 2020); it is kept rather than silently dropped.

**Why does shift show missing on some papers?** It does not — those papers genuinely have no shift system. Verified against the raw files: CGL Tier-II (8 papers), CPO Paper-II (4 papers), CHSL Tier-II 2024, and 4 Selection-Post 2019 papers are single-session exams; their raw filenames/titles carry no Shift marker at all. Every Tier-I/Paper-I/pre question has Shift 1/2/3/4 populated 100%. `held_on` (exam date) is populated 100% on all 144,944 rows.

**Duplicates:** CGL/CPO/GD/MTS/Steno had zero source duplicates. CHSL had 1, Selection-Post had 25 (same GK question filed under both Graduation and Matric level papers of the same cycle) — each duplicate kept exactly once. Verified: the question-id set on disk matches the source per exam, 0 dupes inside the output.

### Why did some content say the same thing twice? (the "2 times" question)

The original scraped solutions were **bilingual**: an English explanation followed by the same point repeated in Hindi, e.g. "Inflection Point (महत्वपूर्ण मोड़)" or "flows (बहना)". That is why many rows looked duplicated. In this dataset all Hindi translation segments have been **stripped** (13,619 cleanups in this DB): only the English text remains. Where a whole *question* belongs to a Hindi-language section (see below) it was removed entirely.

---

## 2. `all-exam/` — raw scraped papers

The original paper files exactly as scraped (one JSON per paper, exam/tier/year/shift folder structure), English-only:

| Exam | Papers | Questions (English) | Notes |
|---|---|---|---|
| SSC-CGL | 292 | 29,600 | Tier-I + Tier-II |
| SSC-CHSL | 364 | 36,540 | Tier-I + Tier-II |
| SSC-CPO | 80 | 16,000 | Paper-I + Paper-II |
| SSC-GD | 268 | 27,380 | 4,620 Hindi-section questions REMOVED |
| SSC-MTS | 333 | 31,650 | |
| SSC-Selection-Post | 120 | 12,000 | |
| SSC-Stenographer | 53 | 10,600 | |

### What was removed and why

1. **Hindi translation segments** (41,416 fields across 1,376 papers): every solution and option carried the same explanation twice — once in English, once in Hindi. All Hindi segments stripped, English kept.
2. **Hindi-section questions in SSC-GD** (4,620 questions): GD Constable includes a Hindi-language section (Hindi grammar/vocabulary concept labels such as sentence-types, idioms, synonyms, antonyms, word-forms). These are Hindi-language test items, so per the English-only requirement they are **removed from the raw files** (27,380 GD questions remain, matching the subjectwise DB exactly).

This is why GD raw questions (27,380) do not equal the raw scrape total (32,000) — the difference is exactly the Hindi section, removed on purpose. All other exams keep their full question counts.

---

## Verification log (2026-09-22)

- Devanagari scan over **every file in this repo**: **0 hits** (1,610 files).
- subjectwise-db: qid sets match the source database per exam; year window 2019–2026; tier/subject purity 0 mismatches; `held_on` 100%; shift 100% where the exam has shifts.
- all-exam: 1,562 paper files; per-paper question_count recomputed after cleanup; GD kept-questions (27,380) == subjectwise GD total (27,380).

## Row format (subjectwise-db)

Each `questions.jsonl` line = one question with: qid, exam, year, held_on, shift, paper, paper_meta, subject, source_subject, chapter, concept, question, options, correct, solution, solution_images, marks_pos, marks_neg, type, tags, tier.
