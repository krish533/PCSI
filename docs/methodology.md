# Methodology in brief

## Corpus and panel

- 87,159 cleaned sentences from 518 policy documents (identified by university, year and file) at 150 U.S. research universities, 1944–2025. The raw file also holds one isolated 1925 sentence, kept for provenance and excluded from the analysis window.
- Documents from the same university and year are pooled: 480 university-years with policy text.
- Each score is carried forward until a newer document appears: 4,277 annual policy-in-force observations. Every row keeps `Source_Year` and `Is_Carried_Forward`, so analyses can use only years with new text.

## Human coding

Sentences were labeled **supportive** (help, guidance, encouragement, collaboration, shared interests), **restrictive** (mandatory assignment, categorical duties, unilateral authority, prohibitions, sanctions, compliance) or **neutral** (definitions, descriptions, procedure). Coders labeled the main message of the whole sentence; disagreements were adjudicated. The adjudicated sentences and coder-level labels were not preserved.

## Classifier

- Fine-tuned `bert-base-uncased`, three classes, maximum input 256 tokens.
- 2,458 training and 274 held-out sentences; held-out accuracy 0.901 and macro-F1 0.894 (from `metrics.json`, recorded at training time).
- Temperature scaling, T = 0.8462 (`temperature_scaling.json`), applied before probabilities are used.

## Index

Sentence score: `s = 0.5 × P(neutral) + P(supportive)`, so restrictive = 0, neutral = 0.5, supportive = 1.
PCSI for a university-year is the unweighted mean of its sentence scores, equivalently `0.5 + 0.5 × (mean P(supportive) − mean P(restrictive))`.

## Main analyses (all descriptive)

| Analysis | Script | Output |
|---|---|---|
| Distribution, alternative aggregations, variance shares, cohort and observed-year trends | `generate_strengthened_results.py` | `paper_outputs/tables/strengthened_results/` |
| Institutional robustness, Shapley shares, clause-topic trends, exact shift-share, rule form, alternative constructions | `generate_revision_analyses.py` | `paper_outputs/tables/revision_analyses/` |
| Figures 1–3 | `generate_final_manuscript_figures.py`, `generate_revision_analyses.py` | `paper_outputs/figures/` |
| Independent recomputation of 39 reported numbers | `audit_manuscript_numbers.py` | `revision_analyses/audit_report.csv` |
| Manuscript tables | `manuscript/build_tables.py` | `manuscript/tables/` |

The shift-share decomposition is computed university by university (midpoint shares and means; a category missing in one period takes its mean from the other period) and then averaged with equal weights, so the parts add up exactly to the observed change.

## Labels

The data hold a single `Carnegie R1` flag. Universities without the flag are labeled **non-R1** in the paper; the flag's Carnegie edition is not recorded, so this group is not presented as official R2.
