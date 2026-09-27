# Reproducing and Improving a Published Heart Attack Prediction Study

**SIT307 Machine Learning — Task 11.1HD**
Ayaan Ali (s223975455) · Bachelor of Artificial Intelligence · Deakin University

A reproducibility audit of Bhagat, Sharma & Agarwal (2025), *"An efficient
stacking-based ensemble technique for early heart attack prediction"*,
Multimedia Tools and Applications, vol. 84, pp. 36351–36375.

## Summary of findings

The paper reports 98.53% accuracy for a six-model stacking ensemble. My
reproduction scored **100%** — which turned out to be the problem, not a success.

- The 1025-row dataset contains **723 exact duplicate records** (70.5%); only
  302 patients are unique.
- **202 of the 205 test records (98.5%)** also appear in the training set.
- After deduplication the ensemble falls to **0.8388 ± 0.0373** over 30 random
  splits, statistically indistinguishable from a single logistic regression
  (paired t-test p = 0.758).
- Logistic regression is *unaffected* by the duplicates, confirming the
  mechanism is memorisation rather than learning.
- Three internal inconsistencies in the published results tables are documented
  in the report.

## Proposed alternative

A revised evaluation protocol rather than a new classifier: duplicate-aware
repeated cross-validation, cross-site feature harmonisation to eight
transferable features, mutual-information selection, sigmoid calibration, a
cost-sensitive decision threshold (0.21, tuned for F2), and **external
validation on three hospitals the model never saw**.

| | Published | Proposed |
|---|---|---|
| Headline accuracy | 98.53% | 80.7% internal, 73.1–91.9% external |
| Evaluation | single 80/20 hold-out | repeated stratified 5-fold (×6) |
| Features | 13, three site-specific | 8 transferable |
| External validation | none | Hungary, Switzerland, Long Beach VA |
| Mean external AUC | not reported | 0.792 |
| Recall change | — | +16 to +22 points at every site |

## Repository contents

| File | Purpose |
|---|---|
| `SIT307_11_1HD.ipynb` | Full analysis — runs start to finish, no manual upload |
| `heart.csv` | The redistributed dataset used by the original paper |
| `fig_flowchart.png` | Fig. 1 — published vs proposed pipeline |
| `fig_leakage.png` | Fig. 2 — effect of duplicate removal |
| `fig_stability.png` | Fig. 3 — accuracy across 30 splits |
| `fig_external.png` | Fig. 4 — external ROC and calibration |
| `SIT307-11.1HD-Report.pdf` | Written report |

## How to run

Open `SIT307_11_1HD.ipynb` in Google Colab and click **Runtime → Run all**.
Cell 1 downloads `heart.csv` from this repository automatically; the four UCI
databases are fetched from the UCI repository at runtime. No files need to be
uploaded and no paths need editing.

## Data sources

- `heart.csv` — the Kaggle redistribution used by the original paper
- UCI Heart Disease Data Set (Cleveland, Hungarian, Switzerland, Long Beach VA):
  https://archive.ics.uci.edu/dataset/45/heart+disease

## Video

Walkthrough (unlisted YouTube): https://youtu.be/Gs2coDPZy4Q 
