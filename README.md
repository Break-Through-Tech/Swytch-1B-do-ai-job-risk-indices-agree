# Do AI Job-Risk Indices Agree?

---

### 👥 **Team Members**

| Name            | GitHub Handle | Contribution |
| --------------- | ------------- | ------------ |
| Kamsi Ozorji    | @anothergrind |              |
| Aarna Patel     | @aarnap24     |              |
| Mehzbeen Sandhu | @msandhu1901  |              |

---

## 🎯 **Project Highlights**

**Example:**

- Developed a machine learning model using `[model type/technique]` to address `[challenge project task]`.
- Achieved `[key metric or result]`, demonstrating `[value or impact]` for `[host company]`.
- Generated actionable insights to inform business decisions at `[host company or stakeholders]`.
- Implemented `[specific methodology]` to address industry constraints or expectations.

---

## 👩🏽‍💻 **Setup and Installation**

1. **Clone the repository**
   ```bash
   git clone https://github.com/Break-Through-Tech/Swytch-1B-do-ai-job-risk-indices-agree.git
   cd Swytch-1B-do-ai-job-risk-indices-agree
   ```
2. **Create and activate a virtual environment** in the repo root
   ```bash
   python -m venv .venv
   # Windows (PowerShell)
   .venv\Scripts\Activate.ps1
   # macOS / Linux
   source .venv/bin/activate
   ```
3. **Install dependencies**
   ```bash
   pip install -r requirements.txt ipykernel
   ```
4. **Register the venv as a Jupyter kernel** (gives it a distinct name in the kernel picker)
   ```bash
   python -m ipykernel install --user --name swytch-ai-risk --display-name "Python (Swytch AI risk .venv)"
   ```
5. **Get the data:** the raw files live in `data/` (see [`data/DATA-DICTIONARY.md`](data/DATA-DICTIONARY.md) for sources and licences).
6. **Run the notebook:** open [`notebooks/soc.ipynb`](notebooks/soc.ipynb), choose the **Python (Swytch AI risk .venv)** kernel, and **Run All**.

---

## 🏗️ **Project Overview**

This project is part of the **Break Through Tech AI Studio** (Fall 2026), with **swytch** as the host company. swytch builds data-driven career-development tools and wants to know which published AI-risk scores are reliable enough to use in career guidance.

**Objective:** measure how much three public AI-exposure indices agree about U.S. occupations, and work out which kinds of occupations they disagree about and why:

- **Felten-Raj-Seamans AIOE:** AI Occupational Exposure (2010 SOC codes)
- **Eloundou et al. "GPTs are GPTs":** GPT-4 and human-rated LLM exposure (O*NET-SOC 2019, rolls up to 2018 SOC)
- **Frey-Osborne:** probability of computerisation (2010 SOC codes)

**Scope:** build one merged, crosswalked table keyed to six-digit 2018 SOC codes, run a rank-agreement analysis, build a consensus/contested table, model the drivers of disagreement with O*NET features, and write a findings memo. See [`Challenge-Project-Overview.md`](Challenge-Project-Overview.md) for the full brief.

**Why it matters:** AI-risk numbers are widely quoted but rarely checked against each other. If the indices disagree, relying on any single score in career advice could point people in the wrong direction.

---

## 📊 **Data Exploration**

### Datasets

| File | Source | Format / size | Keyed to |
| --- | --- | --- | --- |
| `soc_2010_to_2018_crosswalk.xlsx` | BLS (public domain) | Excel, 900 rows x 4 columns | 2010 SOC -> 2018 SOC |
| `occ_level.csv` | Eloundou et al. (MIT) | CSV, 923 occupations | O*NET-SOC 2019 (8-digit) |
| `frey_osborne_probabilities.csv` | Frey & Osborne via GPTs-are-GPTs repo (MIT) | CSV, 653 occupations | 2010 SOC |
| `AIOE_DataAppendix.xlsx` | Felten, Raj & Seamans | Excel, 774 occupations | 2010 SOC |

Full column definitions and quirks are in [`data/DATA-DICTIONARY.md`](data/DATA-DICTIONARY.md).

---

## 🧠 **Model Development**

**You might consider describing the following (as applicable):**

- Model(s) used (e.g., CNN with transfer learning, regression models)
- Feature selection and Hyperparameter tuning strategies
- Training setup (e.g., % of data for training/validation, evaluation metric, baseline performance)

---

## 📈 **Results & Key Findings**

**You might consider describing the following (as applicable):**

- Performance metrics (e.g., Accuracy, F1 score, RMSE)
- How your model performed
- Insights from evaluating model fairness

**Potential visualizations to include:**

- Confusion matrix, precision-recall curve, feature importance plot, prediction distribution, outputs from fairness or explainability tools

---

## 🚀 **Next Steps**

**You might consider addressing the following (as applicable):**

- What are some of the limitations of your model?
- What would you do differently with more time/resources?
- What additional datasets or techniques would you explore?

---

## 📝 **License**

Specify how your project can be used by others. Choose an appropriate license and link it here (e.g., MIT, Apache 2.0). Make sure your Challenge Advisor approves of the selected license type.

**Example:**
This project is licensed under the MIT License.

---

## 📄 **References** (Optional but encouraged)

Cite relevant papers, articles, or resources that supported your project.

---

## 🙏 **Acknowledgements** (Optional but encouraged)

Thank your Challenge Advisor, host company representatives, TA, and others who supported your project.
