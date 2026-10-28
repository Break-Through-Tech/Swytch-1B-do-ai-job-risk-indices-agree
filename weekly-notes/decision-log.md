# Decision log

Decisions, assumptions, and why, for the Challenge Advisor (asked for in meeting three). Newest first. Each entry links
to where the work is.

## TL;DR

- **The indices disagree mostly because of education level.** Overall, Frey-Osborne is negatively correlated with
  Eloundou and AIOE. Within each O\*NET Job Zone it is weakly positive (ρ ≈ 0.15–0.4). Job Zone is the confounder
  (Simpson's paradox). Compare occupations within Job Zone, not just overall.
- **Exposure ≠ risk.** Eloundou and AIOE measure how much AI can touch a job; Frey-Osborne measures the chance the
  whole job is automated. High-education jobs are exposed but not at risk. Within each Job Zone, only risk tracks
  BLS-projected decline.
- **The results are robust** to the SOC crosswalk (hand-mapped, automated, or untransformed) and to Kendall vs
  Spearman. The within-Job-Zone result also holds for every Eloundou variant; the overall correlation does not for α.
- **Clustering uses k = 3** (k = 2 ties on silhouette, but k = 3 isolates the jobs all three indices call exposed).

## Decisions

| #   | Date       | Decision                                                                                                                                                                                                      | Why                                                                                                                                                               | Alternatives considered                                                                 | Where                                                                |
| --- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| D8  | 2026-10-27 | Read "try the kindle" in the meeting-three notes as Kendall's τ.                                                                                                                                              | It sits next to "if it changes the correlations", and Kendall is the standard second rank correlation.                                                            | — (confirm with the advisor).                                                           | §2                                                                   |
| D7  | 2026-10-27 | Read "trade datasets" as _swap one dataset for an alternative_: Eloundou human-rated, α and γ in place of GPT-4 β.                                                                                            | Uses data we already have; tests whether findings depend on GPT-4 rating. Trading data with other teams needs their agreement first.                              | Exchanging datasets with other teams (still open).                                      | [`advisor_meeting_4.ipynb`](../notebooks/advisor_meeting_4.ipynb) §4 |
| D6  | 2026-10-27 | Case studies picked by 2025 employment among jobs the advisor named: elementary and secondary teachers, general office clerks, secretaries, customer service reps, retail salespersons (+ registered nurses). | Advisor asked for jobs relevant to most people; employment is an objective way to pick them.                                                                      | Picking the largest index disagreements (more dramatic, less relatable).                | §6; full write-ups stay in issue #13                                 |
| D5  | 2026-10-27 | Split occupations into four exposure/risk groups at the 50th percentile; exposure = mean of Eloundou β and AIOE ranks.                                                                                        | Eloundou and AIOE correlate 0.86, so averaging them is a fair single exposure score; the median cut is simple to explain.                                         | Using clusters instead of fixed cuts.                                                   | §5                                                                   |
| D4  | 2026-10-27 | Job Zone is binned to the nearest whole zone (`pd.cut` at 2.5 / 3.5 / 4.5; halves round down).                                                                                                                | 22 occupations have fractional zones because they average several O\*NET occupations.                                                                             | Dropping fractional ones (loses data); `round()` (rounds halves to even, inconsistent). | §1                                                                   |
| D3  | 2026-10-27 | Hand-mapped the 18 SOC 2010 codes the automated crosswalk drops (17 Frey-Osborne, 1 AIOE), by title match, then broad-code prefix, then 2 by hand. Used only as a sensitivity check; **not added to v1**.     | It recovers 75 occupations (physicians, nurses, postsecondary teachers) but moves correlations by ≤ 0.06, so v1 stays as is and the change is documented instead. | Adding them to v1 (would change every downstream number for little gain).               | §3, `data/interim/hand_crosswalk_dropped_codes.csv`                  |
| D2  | 2026-10-27 | Spearman stays the headline correlation; Kendall reported as a check.                                                                                                                                         | Kendall gives the same signs and conclusions with smaller values by construction.                                                                                 | Switching to Kendall.                                                                   | §2                                                                   |
| D1  | 2026-10-27 | Clustering uses k = 3.                                                                                                                                                                                        | Silhouette for k = 2 (0.463) and k = 3 (0.461) is a tie; k = 3 separates the "high on all three" group, the one place the indices agree.                          | k = 2 (briefly used, then reverted).                                                    | [`clustering.ipynb`](../notebooks/clustering.ipynb)                  |

## Assumptions still in force

- Percentile ranks among the 693 occupations scored by all three indices are the comparison scale (Task 7).
- Counts are unweighted: every occupation counts once regardless of employment, except where a figure says otherwise.
- Frey-Osborne (2013) predates LLMs; its "risk" means automation by the technology of that time.
- O\*NET features are averaged over O\*NET children for each SOC 2018 code.

## Open questions for the advisor

- Should agreement results from now on be reported within Job Zone by default?
