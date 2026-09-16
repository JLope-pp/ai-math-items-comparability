# How Comparable Are AI-Generated Mathematics Items to Those Written by Pedagogical Specialists?

Public, reproducible companion to the paper *"How Comparable Are AI-Generated Mathematics Items to Those Written by Pedagogical Specialists? Evidence on Cognitive Demand in Peruvian Public Secondary Schools."*

## Contents

```text
paper_ia_items_matematica_github/
├── data/                 Anonymized pilot data, fully in English
└── notebook/             Single notebook that reproduces every table in the paper
```

## Data (`data/`)

All files are anonymized and translated to English. The pilot involved 209 students in six public schools in Metropolitan Lima (February 2026). In this data:

- Each student is identified only by `student_id` (e.g. `EST0001`), consistent across all four student-level files. There are no names, no birthdates, no national ID numbers.
- Each school is identified only by `school_id` (`Colegio_1`...`Colegio_6`). Real school names are not included.
- `questionnaire_anonymized.csv`: student questionnaire (sex, background questions).
- `test_1_anonymized.csv`, `test_2_anonymized.csv`, `test_3_anonymized.csv`: item-level responses to the three test forms (30 items each: 15 from the item bank, 15 AI-generated).
- `item_bank_expert_review.xlsx`: the item bank plus the expert/teacher technical review of all 120 AI-generated items. Reviewer identity is a numeric code (1-6), not a name.

See **`data/CODEBOOK.md`** for the full column-by-column dictionary, including what each questionnaire code (`p1`...`p13h`) means.

**Two things worth knowing about the translation:**
1. The math item stems (`question_text_{N}` in the test files) were translated from the original Spanish pilot instrument to English, preserving every number, operation, unit, and quantity exactly. Only the wording changed.
2. As a direct consequence, **Table D1** (semantic similarity / BERTScore) in the notebook is computed on this English text, not the original Spanish wording used for the number published in the paper — so its values differ slightly from the paper. This is flagged again directly in the notebook, right above Table D1.

A couple of fields (`item_type_code`, `content_topic`) are kept as their original numeric/letter codes rather than translated, since no text label for them exists in the codebook — see the notes on those fields there.

## Notebook (`notebook/paper_replication.ipynb`)

One notebook, reading only from `../data/`, that reproduces every table cited in the paper:

- Table 1 — Technical quality by cognitive demand
- Table 2a — Reliability (Cronbach's alpha, McDonald's omega)
- Table 2b — Corrected item-total correlations
- Table 3 — Empirical distractor functioning
- Table 4 — Sex-bias analysis
- Appendix Table A1 — Item generation protocol
- Appendix Table B1 — Technical review rubric
- Appendix Table C1 — Technical quality by language model
- Appendix Table D1 — Semantic similarity / BERTScore robustness check
- Appendix Table E1 — Test blueprint

Each table is also written to `notebook/outputs/tables/` as a CSV when the notebook runs.

## How to run it

```bash
pip install pandas numpy matplotlib scipy statsmodels openpyxl
pip install factor_analyzer          # needed for Table 2a (McDonald's omega)
pip install sentence-transformers    # needed for Table D1
pip install bert-score               # needed for Table D1

jupyter notebook notebook/paper_replication.ipynb
```

Everything runs in under a minute except Table D1 (semantic similarity), which downloads a multilingual sentence-embedding model and a multilingual BERT model the first time it runs — expect a few minutes for that section only, depending on your connection and machine.

## Note on Table 1 sample sizes

Table 1 splits the 120 AI-generated items into exactly 40 per cognitive-demand level in the paper. Two items in `item_bank_expert_review.xlsx` (both from `qwen3-max`) have an inconsistent intended-demand code across their reviewer rows in the source file, which this notebook resolves by taking the first recorded value — giving an observed split of 40/43/37 instead of 40/40/40. The percentages in each row are essentially unchanged either way.

## Two settings documented, not computed (Table A1)

Most of Table A1, including the item bank's reference sources (TIMSS, ENLA, Desafia-T, NdM, and three internal batches), is computed directly from `item_bank_expert_review.xlsx`. Two rows are the exception: the 0.90 semantic-similarity threshold used during item generation, and the use of 20 reference items as generation context. These are configuration choices of Eval-IA's generation pipeline itself, not properties of the reviewed items, so they are not present in this dataset. They are documented here from Eval-IA's own technical report (AISIDE) and the paper's Methods section, not computed.
