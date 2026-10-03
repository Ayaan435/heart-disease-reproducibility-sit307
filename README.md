# Reproducibility Audit: Stacking Ensemble for Heart Attack Prediction (SIT307 Task 11.1HD)

Ayaan Ali · s223975455 · Bachelor of Artificial Intelligence · Deakin University, Burwood

This project reproduces Bhagat, Sharma and Agarwal (2025), "An efficient stacking-based ensemble technique for early heart attack prediction", Multimedia Tools and Applications, vol. 84, pp. 36351–36375, and proposes a leakage-aware, externally validated alternative.

## Links

- **Report (PDF):** [SIT307-11.1HD-Report.pdf](SIT307-11.1HD-Report.pdf)
- **Notebook:** [SIT307_11_1HD.ipynb](SIT307_11_1HD.ipynb)
- **Video presentation (YouTube, unlisted):** https://youtu.be/Gs2coDPZy4Q

## Key findings

**Part 1: Reproduction**

- The dataset used in the paper (Kaggle redistribution, 1025 rows) contains only 302 unique patients: 723 rows are exact duplicates.
- In a random 80/20 split, 202 of the 205 test records also appear in training, so high-capacity models score near 100% by recognising records rather than generalising.
- After removing duplicates, the paper's stacking ensemble scores 0.8388 ± 0.0373 across 30 splits, statistically indistinguishable from a single logistic regression (0.8377 ± 0.0425, paired t-test p = 0.758). The reported 98.53% is not attributable to the method.
- Section 5.4 explains why: the accuracy a model gains from leaked records is bounded by the fraction leaked times its train–test gap. Logistic regression gains nothing because it cannot memorise individual patients. The ensemble is inflated beyond its bound because the duplicates also leak through its internal cross-validation, shifting the meta-learner's weight towards the memorising models.
- Three of the paper's eight reported metrics are copies of other metrics under the wrong label (Section 5.5).

**Part 2: Proposed protocol**

- Duplicate-aware validation, cross-site feature harmonisation (8 transferable features), mutual-information feature selection, sigmoid calibration and a cost-sensitive (F2) decision threshold.
- Trained on Cleveland only and tested on three unseen hospitals (Hungary, Switzerland, Long Beach VA).
- Internal accuracy 0.807 and AUC 0.876. Mean external AUC 0.792. Recall improves by 16 to 22 points at every external site.

## Repository contents

| File | Purpose |
|---|---|
| `SIT307_11_1HD.ipynb` | Full notebook: reproduction, deduplication, 30-split analysis, leakage decomposition (Table 5), proposed protocol and external validation |
| `SIT307-11.1HD-Report.pdf` | Final report (revised after tutor feedback) |
| `heart.csv` | Kaggle redistribution of the UCI heart disease data used in the paper |
| `fig_leakage.png` | Fig. 2: effect of removing duplicates on test accuracy |
| `fig_stability.png` | Fig. 3: accuracy across 30 stratified splits |
| `fig_external.png` | Fig. 4: external ROC and calibration curves |

## How to run

1. Open `SIT307_11_1HD.ipynb` in Google Colab.
2. Click **Runtime → Run all**.

The notebook downloads `heart.csv` from this repository and the three external databases from the UCI repository automatically, so no manual upload is needed. It uses Python 3, scikit-learn 1.6.1 and XGBoost, with a fixed random seed of 42. Every number in Tables 2–8 of the report is produced by the notebook.

## Revision note (03-10-2026)

Revised in response to tutor feedback:

- The paper's model is now labelled "Stacking ensemble [1]" throughout, to separate it from the method proposed in this report.
- Added Section 1.1 (Contributions) and Section 5.4 (Why some models are immune to leakage), with a new notebook cell that produces Table 5.

## Acknowledgement of GenAI use

Claude (Anthropic), Gemini, Microsoft Copilot and QuillBot were used as described in Section 10 of the report. All experiments were run and verified by me.

## Reference

M. Bhagat, A. Sharma and P. Agarwal, "An efficient stacking-based ensemble technique for early heart attack prediction," Multimedia Tools and Applications, vol. 84, pp. 36351–36375, 2025. DOI: 10.1007/s11042-024-19293-7
