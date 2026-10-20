# Data dictionary

## `replication/data/raw/policy_sentences_cleaned_combined.csv`

One row per cleaned sentence (87,160 rows, including one 1925 sentence outside the analysis window).

| Column | Meaning |
|---|---|
| `Institution` | University name |
| `Year` | Year of the policy document |
| `File` | Source document file name |
| `Sentence_Number` | Position of the sentence in the document |
| `Sentence_Cleaned` | Cleaned sentence text |
| `Cleaned_Char_Count` | Characters in the cleaned sentence |
| `STATE` | Two-letter state code |
| `Private` | 1 = private, 0 = public |
| `Carnegie R1` | 1 = flagged R1 in the supplied metadata; edition not recorded |
| `MEDSCHOOL` | 1 = university has a medical school |
| `Urbanicity (cat)` | Rural, Semi-Urban or Urban (as supplied; source crosswalk not recorded) |
| `Land-Grant Institution` | 1 = land-grant university (Morrill Acts of 1862 and 1890) |
| `Stem program` | 1 for all rows in this sample |
| `Type` | Private/Public × R1/R2 label derived from `Private` and `Carnegie R1`; "R2" here means "not R1" |
| `Med`, `LG` | Y/N versions of `MEDSCHOOL` and `Land-Grant Institution` |

## `replication/data/derived/sentence_scores_canonical.csv`

The raw columns above plus classifier output for every sentence:

| Column | Meaning |
|---|---|
| `P_restrictive_calib`, `P_neutral_calib`, `P_supportive_calib` | Temperature-scaled class probabilities (sum to 1) |
| `SRN_score` | `P_supportive_calib − P_restrictive_calib` |
| `ToneScore_0_1` | Sentence score used in PCSI: `P_supportive_calib + 0.5 × P_neutral_calib` |
| `Pred_Label` | Most likely class |

## `replication/data/derived/policy_level_indices_institution_year.csv`

One row per university and year from its first observed policy through 2025 (4,296 rows in the full archive). `pipeline/04_build_primary_analysis_panel.py` restricts it to 1944–2025 (4,277 rows) as `policy_level_indices_primary_1944_2025.csv`, which the analyses use.

| Column | Meaning |
|---|---|
| `Institution_Year_Key`, `Institution`, `Year` | Identifiers |
| `Source_Year` | Year of the document whose score this row carries |
| `Is_Carried_Forward` | 1 = no new document this year; score carried from `Source_Year` |
| Institutional columns | As in the raw file |
| `n_sentences`, `n_words` | Size of the pooled source text |
| `Mean_Tone_Score` | **PCSI** (mean of `ToneScore_0_1`) |
| `Median_Tone_Score` | Median-based alternative |
| `supportive_share`, `neutral_share`, `restrictive_share` | Shares of sentences by most likely class |
| `*_per_1000w` | Dictionary counts per 1,000 words (supportive, restrictive, sanction, second person, inclusive "we", legalese, IP-technical, Madey and Roche legal-event terms) |
| `obligation_modal_share`, `procedural_share`, `long_sentence_share`, `mean_sentence_length` | Components of the word-based diagnostics |
| `Tone_Index`, `Clarity_Index`, `Legal_Load_Index` | Word-based diagnostics (Appendix E of the paper) |
