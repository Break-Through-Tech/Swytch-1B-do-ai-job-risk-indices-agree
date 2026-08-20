# Do AI Job-Risk Indices Agree?

**Company / Org:** swytch   
**Challenge Advisor:** Julie Young, julie@swytch.careers   
**Program:** Break Through Tech AI Studio - Fall 2026

---

## 🏢 About swytch

swytch is an early-stage career-development company working on data-driven tools that help people understand their skills and navigate career changes. Labor-market data is full of AI-risk scores that claim to say which jobs are exposed to AI. Before swytch relies on any such signal in career guidance, we want to know which ones to trust most.

---

## 🎯 The Challenge

### Project Summary

In this project, you will use three public research indices that score how exposed each U.S. occupation is to AI (the Felten-Raj-Seamans AIOE, the Eloundou et al. GPT-exposure scores, and the Frey-Osborne automation-probability estimates), joined to the O*NET occupational database, and machine learning methods (rank-correlation analysis, feature engineering, regression and tree-based models with feature-importance analysis, and optionally clustering and a small Keras model) to measure how much these AI-risk rankings agree, and to work out what kinds of occupations they disagree about and why. This helps swytch learn which AI-risk signals are solid enough to inform career guidance and which are too shaky to trust.

### What makes this project new 

Researchers have compared some of these indices before: a February 2026 Budget Lab analysis found seven modern metrics correlate at roughly 0.7 to 0.9 and disagree most about the most-exposed occupations, and a 2026 ILO brief showed that older automation measures point at different kinds of jobs than newer AI-capability measures. Nobody, however, has published a properly crosswalked occupation table that puts the Frey-Osborne measure alongside the modern indices, worked out which occupational features drive the disagreements, or turned any of it into a signal a career-guidance product could actually use. That is our contribution. The published results help us create the underlying model: when your merged table reproduces the known agreement pattern, you will know your pipeline is working and can push forward.

### Success Criteria

Success is a set of concrete artifacts, each checkable:

1. **A documented, reproducible merged dataset**: one CSV keyed to six-digit 2018 SOC codes combining all three indices (expected roughly 650 to 750 occupations), with a coverage report explaining every occupation lost in the merge, rebuildable from raw downloads by running one notebook.
2. **An agreement analysis**: the full pairwise Spearman and Kendall rank-correlation matrix across the indices, run under both of Eloundou's label sources (GPT-4 ratings and human ratings), plus an agreement-versus-exposure-level analysis and at least six written occupation case studies.
3. **A consensus table**: per-occupation exposure tier plus an agreement-confidence flag (agreed vs contested).
4. **A disagreement-drivers model**: a model predicting cross-index divergence from grouped O*NET features (Job Zone, ability/skill/work-context aggregates), with feature-importance results. A well-documented null result is an acceptable finding.
5. **A findings memo**: a plain-language writeup of what this means for trusting any single AI-risk number, aimed at swytch's guidance use case.

### Stretch Goals

- Check the indices and the consensus tier against BLS 2024-34 projected employment change by occupation.
- Sensitivity ride-along: how much do conclusions move between GPT-4-generated and human ratings in the Eloundou data?
- Fairness lens (uses the bias-measurement material from ML Foundations): do the indices disagree more for occupations held by particular demographic groups (public BLS CPS data)?
- Add a fourth index if ahead of schedule (for example the Eisfeldt et al. generative-AI exposure scores, free download, link in Resources).
- An interactive explorer of the merged table.

### Project Milestones

Use these milestones to guide your work. Your team will create a GitHub Projects board to break them into weekly tasks.

| Month | Milestone | Key activities |
|---|---|---|
| September | **Merge first: one table, documented** | Download all sources (exact links below), verify row counts (774 / 923 / 702). Roll the Eloundou eight-digit O*NET-SOC codes up to six digits. Crosswalk AIOE and Frey-Osborne from 2010 SOC to 2018 SOC with the BLS crosswalk. Join, and write the coverage report. **Checkpoint: a first working merged table (v0) by Friday, September 4, reviewed at our second meeting.** Then per-index EDA and percentile ranks. |
| October | Agreement analysis and consensus tiers | Pairwise rank correlations (both Eloundou label sources), agreement-versus-exposure analysis (our calibration against the published pattern), consensus tiers with confidence flags, occupation case studies, optional clustering. |
| November | Model the disagreements, validate, write up | Define the divergence target, build 20-40 grouped O*NET features, model with regression and tree-based methods, report feature importance. Optional Keras comparison model and BLS-projections check. Findings memo and presentation. |

**The first real technical obstacle, named now so it does not surprise you in October:** the three indices were built on different occupation-code vintages. AIOE and Frey-Osborne use 2010 SOC codes; Eloundou uses O*NET-SOC 2019 codes (which map to the 2018 SOC). Merging them requires the official BLS crosswalk, and the crosswalk contains splits and merges that gain or lose occupations at the edges. This is real data-engineering work and it is Milestone 1, not a preliminary step.

**A second one:** these indices measure different things (relative "exposure," probability of computerisation, LLM time-savings share). Some disagreement is by construction. That is why every comparison in this project uses relative rankings, not raw scores.

> **Note for the team:** Please create a GitHub Projects board in this repository to break these milestones into weekly tasks. Go to the **Projects** tab, then **New project**, choose **Board**, and add columns for each month.

---

## 📊 Dataset

**Name and Source:** Three published AI-exposure indices (AIOE; Eloundou et al. "GPTs are GPTs"; Frey-Osborne), the O*NET occupational database, and the official BLS SOC crosswalk.
**Format:** CSV and Excel (xlsx), plus one PDF appendix table.
**Size:** the three index files total under 1 MB; with O*NET feature files the full working set stays under 100 MB.
**Location:** exact files, verified 2026-08-17:

| File | Where | What it is |
|---|---|---|
| `data/occ_level.csv` | https://github.com/openai/GPTs-are-GPTs | Eloundou et al. occupation-level exposure. 923 rows, eight-digit O*NET-SOC 2019 codes. Columns `dv_rating_*` are GPT-4 ratings, `human_rating_*` are human annotator ratings; `_alpha`, `_beta`, `_gamma` are the paper's E1, E1+0.5xE2, and E1+E2 measures (the column named gamma is the measure the paper calls zeta). MIT licence. |
| `AIOE_DataAppendix.xlsx`, sheet "Appendix A" | https://github.com/AIOE-Data/AIOE | Felten-Raj-Seamans AIOE scores. 774 rows, six-digit 2010 SOC codes, standardized scale. **Download it from the authors' repository (no licence file is posted there, so we link rather than copy); cite the paper as the README requests.** |
| The Future of Employment (appendix table) | https://www.oxfordmartin.ox.ac.uk/downloads/academic/The_Future_of_Employment.pdf | Frey-Osborne probability of computerisation, 702 occupations, six-digit 2010 SOC, in the paper's appendix. A machine-readable version covering 653 of the 702 occupations is provided in this repository (`data/frey_osborne_probabilities.csv`, derived from the MIT-licensed research repository above); use the paper's appendix for the full list and for spot-checking. **Do not use Kaggle or other third-party CSV copies of this table: they are unlicensed transcriptions.** |
| O*NET 30.3 files (Job Zones, Abilities, Skills, Work Context, Work Activities) | https://www.onetcenter.org/database.html | Occupational features, individually downloadable, no account needed. CC BY 4.0: credit the O*NET 30.3 Database and the U.S. Department of Labor, Employment and Training Administration. |
| `soc_2010_to_2018_crosswalk.xlsx` and its explanatory note (PDF) | https://www.bls.gov/soc/2018/crosswalks_used_by_agencies.htm | The official 2010-to-2018 SOC crosswalk and the note explaining its split (#) and merge (##) notation. Public domain; cite BLS. Tip: bls.gov blocks plain script downloads; use a browser or set a browser-like user agent. |
| BLS Employment Projections Table 1.2 (stretch only) | https://www.bls.gov/emp/tables.htm (single-file download: https://www.bls.gov/emp/ind-occ-matrix/occupation.xlsx) | Projected 2024-34 employment change by occupation. Public domain; cite BLS. |

### Key Details

- All data is occupation-level aggregate. **Zero PII anywhere.** No scraping is needed or permitted; everything above is a direct download.
- **A data dictionary is provided in this repository** (`data/DATA-DICTIONARY.md`) with feature definitions, data types, preprocessing already performed by the index authors, and known limitations and quirks, including the gamma/zeta naming trap and the crosswalk edge cases. Start there; extend it as you build.
- Known limitations, honestly: the indices measure different constructs on different code vintages; exposure scores are research estimates, not predictions of job loss (the ILO's 2026 brief is explicit about this); and O*NET 30.3 postdates the vintages the indices were built from.

---

## 🛠️ Suggested Approach

**ML Problem Type:** Regression, Clustering (optional), Deep Learning / Neural Networks (optional comparison)

**Recommended libraries:**
- From your ML Foundations toolkit: pandas, NumPy, matplotlib, scikit-learn, and optionally TensorFlow/Keras. Rank correlations need no extra installs: `DataFrame.corr(method='spearman')` and `method='kendall'` are built into pandas.
- Random Forest (scikit-learn) is the recommended workhorse for the disagreement model. It is a short step beyond the decision trees you trained in ML Foundations, and that step is part of the learning here.
- Flagged as **new learning** if you choose them (fine, but budget time): K-Means clustering, XGBoost, SHAP, seaborn/plotly. None are required by the core plan.

**Evaluation metrics:**
- Agreement: Spearman and Kendall rank-correlation coefficients; rank-variance by exposure level.
- Disagreement model: R-squared and RMSE on a held-out split, plus permutation feature importance.
- Everything on ranks, not raw scores, for the construct-validity reason above.

---

## 📚 Resources to Get Started

**Background reading (start with the first two, they frame the project):**
- The Budget Lab at Yale, "Labor Market AI Exposure: What Do We Know?" (2026): https://budgetlab.yale.edu/research/labor-market-ai-exposure-what-do-we-know
- ILO research brief, "Workers' exposure to AI: what indicators tell us and what they don't" (2026): https://www.ilo.org/publications/workers%E2%80%99-exposure-ai-what-indicators-tell-us-%E2%80%93-and-what-they-don%E2%80%99t
- The three index papers: Felten, Raj and Seamans (Strategic Management Journal 42(12):2195-2217, 2021); Eloundou et al. (Science 384(6702):1306-1308, 2024; open version arXiv:2303.10130); Frey and Osborne (Technological Forecasting and Social Change 114:254-280, 2017; open PDF linked in the Dataset table).
- For the curious: "Towards the Terminator Economy" (arXiv:2407.19204) on why the oldest index can point the opposite way from the newest ones.

**Technical tutorials:**
- pandas merging and joining: https://pandas.pydata.org/docs/user_guide/merging.html
- scikit-learn Random Forests: https://scikit-learn.org/stable/modules/ensemble.html#forests-of-randomized-trees
- Permutation feature importance: https://scikit-learn.org/stable/modules/permutation_importance.html

**Code examples:**
- The Eloundou data repository itself (https://github.com/openai/GPTs-are-GPTs) includes the authors' processing code, useful for seeing how researchers handle these files.

*Feel free to explore beyond these, and share anything interesting you find with me!*

---

## 🤝 How We'll Work Together

**Official check-ins:** during our biweekly 45-minute AI Studio Lab Section meeting block (2nd and 4th week of every month). I will review the repository before each meeting and open with observations, so keep your work pushed.

**Other ways to reach me with questions:**
- Discord: Swytch-1 https://discord.com/channels/1304110328153636875/1537918287927840868
- Email julie@swytch.careers; please copy your teammates and your AI Studio Coach.
- I aim to respond within 48 hours. Please reach out to your AI Studio Coach with urgent questions.

**Recommended free coding / collaboration tools:**
- Google Colab (free tier is sufficient for everything in this project)
- GitHub Projects board in this repository for task tracking

---

## 🚀 Getting Started

1. **Review this overview document** and note any questions for our first meeting.
2. **Download every file in the Dataset table** and confirm you can open each one in Colab. Check the row counts against the numbers listed (774 / 923 / 702). If anything does not match, say so at the first meeting: that is exactly the kind of observation this project is about.
3. **Read the data dictionary** in this repository.
4. **Read the GitHub Projects documentation** here: https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects

I'm excited to work with you!

---

## ❓ Questions?

Please bring any questions to our first meeting during the week of August 24th (Break Through Tech's Bridge to Studio - Session C).
