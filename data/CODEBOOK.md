# Data Codebook

All files in this folder are anonymized and in English. This codebook documents every column and code that is not self-explanatory.

## Files

- `questionnaire_anonymized.csv` — student questionnaire (background, sex), keyed by `student_id`.
- `test_1_anonymized.csv`, `test_2_anonymized.csv`, `test_3_anonymized.csv` — item-level responses to the three pilot test forms, keyed by `student_id`.
- `item_bank_expert_review.xlsx` (sheet `consolidated`) — the item bank plus the specialist/teacher technical review of all 120 AI-generated items.

## Shared identifiers

- `student_id`: anonymized student code (e.g. `EST0001`), consistent across all four student-level files.
- `school_id`: anonymized school code (`Colegio_1`...`Colegio_6`), one per real school in the pilot. Real school names are not included anywhere.

## `questionnaire_anonymized.csv`

| Column | Meaning | Values |
|---|---|---|
| `sex` | Student sex | `Female`, `Male` |
| `p1` | How old are you? | age in years |
| `p2` | Do you live in the same district as your school? | `YES`, `NO`, `DON'T KNOW` |
| `p3` | First language(s) learned in the first five years of life | numeric code |
| `p41` | Did you repeat any grade/year in PRIMARY school? | `DID NOT REPEAT`, `FIRST`, `SECOND`, `THIRD`, `FOURTH`, `FIFTH`, `SIXTH`, or a combination |
| `p42` | Did you repeat any grade/year in SECONDARY school? | same scale as `p41` |
| `p5` | Highest level of education completed by mother/main female caregiver | `NO FORMAL EDUCATION`, `INCOMPLETE PRIMARY`, `COMPLETE PRIMARY`, `INCOMPLETE SECONDARY`, `COMPLETE SECONDARY`, `TECHNICAL HIGHER EDUCATION`, `UNIVERSITY EDUCATION`, `DON'T KNOW` |
| `p6` | Same as `p5`, for father/main male caregiver | same scale |
| `p7` | At home you have... (multi-select, raw encoding) | a number whose digits 1-7 indicate which items are checked: 1=own study space, 2=computer/laptop, 3=tablet, 4=mobile internet, 5=home internet, 6=desk/study table, 7=quiet place to study. Example: `1457` means options 1, 4, 5, 7 are checked. |
| `p81` | How often do you have mobile internet access? | `NEVER`, `SOMETIMES`, `FREQUENTLY`, or `-777`/`-888` (not answered / double-marked) |
| `p82` | How often do you have home internet access (not mobile)? | same scale as `p81` |
| `p9` | Main device used to connect to the internet | `MOBILE PHONE`, `COMPUTER/LAPTOP`, `TABLET` |
| `p10a`-`p10h` | Attention / self-regulation while studying (8 statements) | `DISAGREE`, `PARTIALLY AGREE`, `AGREE` |
| `p11a`-`p11d` | Beliefs about intelligence (growth mindset, 4 statements) | same 3-point scale |
| `p12a`-`p12h` | Perceived characteristics of the test questions (8 statements) | `NO QUESTIONS`, `VERY FEW QUESTIONS`, `MOST QUESTIONS`, `ALL QUESTIONS`, or `-777` (not answered) |
| `p13a`-`p13h` | Perceived usability of the testing app (8 statements) | same 3-point scale as `p10` |

The exact statement text for `p10a`-`p10h`, `p11a`-`p11d`, `p12a`-`p12h`, and `p13a`-`p13h` is preserved from the original instrument's variable labels (Stata `.do` file, not included in this public repo since it also documents PII-adjacent fields) — codes are kept because that is how the instrument and internal analyses refer to them.

## `test_1/2/3_anonymized.csv`

| Column pattern | Meaning |
|---|---|
| `grade` | Student's grade: `1st grade`...`4th grade` |
| `item_correct_{N}` | 1 = correct, 0 = incorrect, for item N (N = the instrument's global item number, e.g. 227-316) |
| `option_selected_{N}` | Which option (`a`/`b`/`c`/`d`) the student selected for item N |
| `question_text_{N}` | The full text of math item N |

**Important — `question_text_{N}` is translated to English.** The pilot instrument was administered in Spanish to Peruvian students. For this public, English-language repository, every item stem was translated to English, preserving all numbers, operations, units, and quantities exactly as in the original — only the wording was translated, never a numeric value. Because of this, **Table D1** (semantic similarity / BERTScore) in the notebook is computed on the English text and will differ slightly from the number published in the paper, which was computed on the original Spanish text. This is called out again directly above Table D1 in the notebook.

## `item_bank_expert_review.xlsx` (sheet `consolidated`)

| Column | Meaning | Values |
|---|---|---|
| `item_id` | Item identifier | integer |
| `item_type_code` | A 2-value code (`O` / `C`) present in the original file | Not documented elsewhere in the project; kept as the original raw code. |
| `reviewer_id` | Numeric code for the teacher who reviewed the item | 1-6 (not a name) |
| `origin` | Where the item came from | `Item bank`, `AI-generated` |
| `model_or_source` | For item-bank rows, the original source batch/assessment; for AI-generated rows, the language model | `Batch 1 - Basic operations`, `Batch 2 - Math questions`, `Batch 3 - Math word problems`, `TIMSS`, `ENLA`, `Desafia-T`, `NdM`, `gemini-2.5-pro`, `qwen3-max`, `deepseek-chat`, `gpt-5-nano` |
| `content_alignment_teacher` / `content_alignment_expert` | Content-alignment rating (1-3 scale, teacher / specialist) | 1-3 |
| `distractor_quality_teacher` / `distractor_quality_expert` | Distractor-quality rating (1-3 scale) | 1-3 |
| `difficulty_level`, `item_grade_level`, `item_classroom_group` | Item-level classification codes from the original file | numeric; exact category meaning beyond what the paper describes is not documented elsewhere in the project |
| `cognitive_demand_teachers` / `cognitive_demand_expert` / `intended_cognitive_demand` | Cognitive demand (1=Low, 2=Medium, 3=High), as rated by teachers, by the specialist, or as intended at generation time | 1-3 |
| `content_topic_teachers` / `content_topic` | Content-topic code | Numeric code (1-7); no text label is available for these codes, so they are kept as the original numbers. |
| `expert_answer_correct` | Whether the specialist judged the keyed answer correct | `correct`, `incorrect` |
| `distractor_1_rationale`, `distractor_2_rationale`, `distractor_3_rationale` | The teacher/specialist's rationale for why each incorrect option is a plausible distractor (translated to English) | free text |
| `teacher_comments`, `expert_comments` | Free-text review comments (translated to English) | free text |

## Two settings not derived from this data

Table A1 in the notebook lists two generation-pipeline settings that are properties of how Eval-IA was configured, not something the review or response data can measure: the 0.90 semantic-similarity threshold used during item generation, and the use of 20 reference items as generation context. Both come from Eval-IA's own technical report (AISIDE) and the paper's Methods section rather than being computed here.
