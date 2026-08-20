# Data Dictionary: AI Job-Risk Index Comparison

**Supplied by your Challenge Advisor (Julie Young, swytch).** Fellows: extend and correct this as you build; you are not starting from nothing. Every count below was verified against the actual files on 2026-08-17.

## 1. What the dataset is

Three published occupation-level AI-risk indices, the official crosswalk needed to align them, and the O*NET occupational feature files, which together support one merged analysis table keyed to six-digit SOC occupation codes. All data is occupation-level aggregate. There is no PII anywhere in these sources.

## 2. Source files, as shipped

### 2.1 `occ_level.csv` (Eloundou et al., "GPTs are GPTs")
| Column | Type | Definition | Notes and quirks |
|---|---|---|---|
| `O*NET-SOC Code` | string | Eight-digit O*NET-SOC 2019 code, format `##-####.##` | 923 rows, all carry a `.xx` suffix; `.00` is the base occupation, other suffixes are detailed specialties. Rolls up to 798 unique six-digit codes; 67 six-digit codes have more than one detail row and need aggregation (unweighted mean is the accepted simple choice; document it) |
| `Title` | string | O*NET occupation title | |
| `dv_rating_alpha` | float 0-1 | GPT-4-rated share of tasks at exposure level E1 (paper's alpha) | "dv" columns are the GPT-4 ratings; "human" columns are the human annotator ratings (repo README) |
| `dv_rating_beta` | float 0-1 | GPT-4-rated E1 + 0.5 x E2 (paper's beta) | Observed range 0.000-1.000, mean 0.345. The headline measure in most reuse, including the Budget Lab comparison |
| `dv_rating_gamma` | float 0-1 | GPT-4-rated E1 + E2 | **Naming trap:** the column is called gamma but the paper calls this measure zeta, the exposure upper bound. Mean 0.548 |
| `human_rating_alpha/beta/gamma` | float 0-1 | Same three measures from human annotators | Fully populated: zero empty cells in any rating column. human_rating_beta range 0.000-0.844, mean 0.303 |

Preprocessing already performed by the authors: task-level E0/E1/E2 labels aggregated to occupation shares. Known limitation (authors' own): "A fundamental limitation of our approach lies in the subjectivity of the labeling." Keep dv and human results separate; the choice of label source is itself an analysis variable. Licence: MIT (repo LICENSE file).

### 2.2 `AIOE_DataAppendix.xlsx`, sheet "Appendix A" (Felten, Raj and Seamans)
| Column | Type | Definition | Notes and quirks |
|---|---|---|---|
| `SOC Code` | string | Six-digit **2010 SOC** code, format `##-####` | 774 rows. Verified 2010 vintage: contains 15-1132 and 15-1134 (2010-only codes), no 2018-only codes. Requires the BLS crosswalk before joining anything keyed to 2018 SOC |
| `Occupation Title` | string | 2010 SOC title | |
| `AIOE` | float | AI Occupational Exposure, standardized score | Observed range -2.670 to +1.528, mean 0.000. A relative exposure scale, not a probability; compare ranks, not magnitudes, across indices |

Other sheets (B: industry AIIE, C: county AIGE, D: application-ability matrix, E: ability-level exposure) are not needed for the core project. Preprocessing already performed: authors collapsed 832 eight-digit occupations to 774 six-digit occupations. Licence: none posted; cite the SMJ paper; do not redistribute the file, link to the authors' repo.

### 2.3 Frey-Osborne appendix table (from The_Future_of_Employment.pdf)
| Field | Type | Definition | Notes and quirks |
|---|---|---|---|
| Rank | int | 1-702, ascending probability | |
| Probability | float 0-1 | Probability of computerisation | A technical-automatability estimate for the 2010 task mix, not a job-loss forecast and not an AI-exposure measure; the authors "make no attempt to forecast future changes in the occupational composition of the labour market" |
| Label | string | Low/Medium/High risk banding used in the paper | |
| SOC code | string | Six-digit **2010 SOC** | 702 occupations |
| Occupation | string | | |

Shipped by the authors only as a PDF appendix. A machine-readable version is provided in this folder as `frey_osborne_probabilities.csv` (columns: `soc_code_2010`, `fo_computerization_probability`), extracted from the `freyOsborne` column of `autoScores.csv` in the MIT-licensed github.com/openai/GPTs-are-GPTs repository. It covers 653 of the 702 occupations (the remainder fell out of that repository's panel merge); values spot-check against the paper (for example Chief Executives, 11-1011, 0.015). Use the paper's appendix for the complete list and for verification. Do not use Kaggle or other third-party mirrors: they are unlicensed transcriptions. Licence: the CSV is covered by the MIT licence in this folder; cite Frey and Osborne (2017) for the underlying estimates.

### 2.4 `soc_2010_to_2018_crosswalk.xlsx` (BLS)
Columns: 2010 SOC code, 2010 SOC title, 2018 SOC code, 2018 SOC title. Quirks (from the companion BLS note, November 2017): `#` marks a 2018 occupation formed from part of a 2010 occupation, `##` marks merges. Splits and merges are exactly where occupations are gained or lost in the merge; count and report them rather than letting them disappear. Public domain, cite BLS.

### 2.5 O*NET 30.3 feature files (onetcenter.org, CC BY 4.0)
Recommended minimal set, each downloadable individually as Excel/CSV: Job Zones; Abilities; Skills; Work Context; Work Activities. All keyed to eight-digit O*NET-SOC 2019; each has Element Name/ID, Scale (IM importance, LV level), and Data Value columns in long format. Preprocessing needed by the team: pivot long to wide, filter to one scale (IM is conventional), roll up eight-digit to six-digit, then group the hundreds of raw elements into 20-40 domain aggregates before modeling. Known limitation: O*NET 30.3 postdates the vintages the indices were built on (Eloundou used 27.2; the 27.2 archive remains downloadable if exact-vintage features are wanted).

## 3. The merged analysis table the team will build (target schema)

One row per six-digit 2018 SOC occupation that survives the merge (expected roughly 650-750 rows; a naive uncrosswalked AIOE-Eloundou match already hits 683, so treat lower numbers as a bug signal).

| Column | Type | Source | Definition |
|---|---|---|---|
| `soc2018` | string | spine | Six-digit 2018 SOC code (join key) |
| `title` | string | O*NET | Occupation title |
| `aioe` | float | 2.2 via crosswalk | Standardized AIOE score |
| `fo_prob` | float | 2.3 via crosswalk | Computerisation probability |
| `elo_dv_beta`, `elo_human_beta` | float | 2.1 rolled up | Eloundou beta, both label sources (keep both; add alpha/gamma columns if used) |
| `rank_aioe`, `rank_fo`, `rank_elo_dv`, `rank_elo_hum` | float | derived | Percentile rank of the occupation within each index (higher = more exposed). All agreement statistics run on these, not raw scores |
| `rank_spread` | float | derived | Divergence target, for example the standard deviation of the rank columns; define once, document, keep |
| `consensus_tier` | category | derived | High / Medium / Low exposure by mean rank, plus an agreement flag (agreed vs contested) |
| `job_zone` | int 1-5 | O*NET | Preparation level |
| feature aggregates (20-40 cols) | float | O*NET | Domain means, for example cognitive abilities, physical abilities, social skills, technical skills, routine-context measures; the exact grouping is a documented team decision |
| `emp_change_pct_2024_34` | float | BLS EP Table 1.2 | Stretch: projected employment change |
| provenance flags | bool | derived | Per index: whether the row came through a crosswalk split/merge |

## 4. Known limitations and quirks of the combined dataset (read before modeling)

1. **The indices measure different constructs** (exposure, computerisation probability, LLM time-savings). Some disagreement is by construction. Compare relative rankings, never raw scores.
2. **Vintage skew:** AIOE and Frey-Osborne are on 2010 SOC, Eloundou on O*NET-SOC 2019 (2018 SOC). The crosswalk is the project's first real technical obstacle; splits and merges lose occupations at the edges. Reconciliation is a documented deliverable, not a footnote.
3. **Occupations are not people:** exposure of an occupation says nothing about any individual's job. Employment weights differ enormously across occupations; an unweighted occupation-level correlation is not an employment-weighted one. Say which you are reporting.
4. **Exposure is not displacement.** Per the ILO: exposure measures "cannot be interpreted as predictions of job displacement, productivity gains or reskilling needs."
5. **Label-source sensitivity:** Eloundou dv (GPT-4) and human ratings are distinct measures that published work treats separately. Run key results under both.
6. **Static task lists:** all indices inherit O*NET's snapshot of what each job was when surveyed.
7. **Coverage asymmetry:** 702 vs 774 vs 923 source rows. The merged table is the intersection; report what falls out and why.
