# Credit Risk & Loan Default Prediction

## Overview

This project builds a classification model that predicts the probability a loan applicant will default, using the German Credit Risk dataset. This is a foundational task in consumer lending and one of the most common advanced portfolio projects for analysts targeting banking or fintech roles.

## Business Context

A lender needs to decide whether to approve a loan application. Approving more loans increases interest revenue but also increases exposure to defaults; being overly cautious reduces defaults but also turns away creditworthy customers and forgoes revenue. This project builds a model to predict default risk from applicant characteristics, and evaluates it in a way that reflects this trade-off rather than relying on a single accuracy number.

## Objectives

1. Clean the data and handle class imbalance appropriately.
2. Engineer relevant features from the available applicant data.
3. Train and compare Logistic Regression against a Random Forest classifier.
4. Evaluate both models using precision, recall, F1-score, and ROC-AUC, not accuracy alone.
5. Identify the top features driving default risk and discuss the risk/revenue trade-off.

## Dataset

- **Source:** German Credit Risk (With Target), Kaggle ([link](https://www.kaggle.com/datasets/kabure/german-credit-data-with-risk))
- **Grain:** One row per loan applicant
- **Rows:** 1,000
- **Fields:** Age, Sex, Job, Housing, Saving accounts, Checking account, Credit amount, Duration, Purpose, and the target field, Risk (good / bad)

**Note on dataset selection:** the more commonly linked Kaggle version of this dataset (`uciml/german-credit`) does not include the Risk target column and cannot be used for a classification task. This project uses the corrected version that includes it.

## Tools Used

- Python 3
- Pandas
- Scikit-learn (LogisticRegression, RandomForestClassifier)
- Matplotlib, Seaborn

## Methodology

1. **Data cleaning:** Confirmed no missing values across all 1,000 rows (`df.info()` shows 1000 non-null for every column). Checking account and Saving accounts were retained as categorical text fields.
2. **Encoding:** Categorical columns (Sex, Housing, Saving accounts, Checking account, Purpose) were encoded via a column transformer pipeline; numeric columns (Age, Job, Credit amount, Duration) were passed through unchanged.
3. **Class imbalance:** The target class split is 800 good / 200 bad (80% / 20%), a moderate imbalance. `class_weight='balanced'` was used in both models rather than SMOTE, appropriate given the dataset's small size (1,000 rows), where synthetic oversampling of an already-small minority class risks generating unrealistic examples.
4. **Train/test split:** Stratified split preserving the 80/20 class ratio, with 200 rows held out for testing.
5. **Modeling:** Trained Logistic Regression and Random Forest, both with `class_weight='balanced'`.
6. **Evaluation:** Evaluated both models on the test set using accuracy, precision, recall, F1-score, confusion matrix, and ROC-AUC.
7. **Feature importance:** Extracted feature importances from the Random Forest model to identify the strongest predictors of default risk.

## Results

### Model Comparison Summary (bad class)

| Model | Precision | Recall | F1-score | ROC-AUC |
|---|---|---|---|---|
| Logistic Regression | 0.587 | 0.45 | 0.509 | 0.749 |
| Random Forest | 0.566 | 0.50 | 0.531 | 0.767 |

### Random Forest

| Metric | bad | good |
|---|---|---|
| Precision | 0.57 | 0.80 |
| Recall | 0.50 | 0.84 |
| F1-score | 0.53 | 0.82 |

**Accuracy:** 0.735

**Confusion Matrix**

| | Predicted bad | Predicted good |
|---|---|---|
| Actual bad | 30 | 30 |
| Actual good | 23 | 117 |

### Logistic Regression

| Metric | bad | good |
|---|---|---|
| Precision | 0.59 | 0.79 |
| Recall | 0.45 | 0.86 |
| F1-score | 0.51 | 0.82 |

**Accuracy:** 0.74

**Confusion Matrix**

| | Predicted bad | Predicted good |
|---|---|---|
| Actual bad | 27 | 33 |
| Actual good | 19 | 121 |

### Feature Importance (Random Forest, Top 10)

| Rank | Feature | Importance |
|---|---|---|
| 1 | Credit amount | 0.210 |
| 2 | Age | 0.160 |
| 3 | Duration | 0.156 |
| 4 | Checking account (unknown/missing) | 0.062 |
| 5 | Job | 0.053 |
| 6 | Checking account (little) | 0.040 |
| 7 | Saving accounts (little) | 0.028 |
| 8 | Checking account (moderate) | 0.025 |
| 9 | Purpose (radio/TV) | 0.025 |
| 10 | Purpose (car) | 0.024 |

## Key Findings

**1. Random Forest outperforms Logistic Regression on every metric that matters most for this business problem, despite a slightly lower overall accuracy.** Random Forest has higher recall on the "bad" class (0.50 vs. 0.45), a higher F1-score (0.531 vs. 0.509), and a higher ROC-AUC (0.767 vs. 0.749). Since missing an actual bad-credit applicant (a false negative) is typically more costly to a lender than being overly cautious with a good applicant, recall and ROC-AUC matter more here than raw accuracy, and Random Forest is the stronger model on every metric that reflects that priority.

**2. Both models still miss roughly half of actual bad-credit applicants.** With recall on "bad" at only 0.45 to 0.50, neither model is close to reliably catching defaulters. This is a material limitation: in the Random Forest confusion matrix, 30 of 60 actual bad-risk applicants were misclassified as good, meaning nearly half of true default risk would be approved for lending under this model as-is.

**3. ROC-AUC around 0.75-0.77 indicates moderate, not strong, discriminative power.** An ROC-AUC of 0.5 would mean no better than random guessing, and 1.0 would mean perfect separation between good and bad applicants. Scores in the 0.75-0.77 range mean both models can meaningfully distinguish risk levels better than chance, but there is real room for improvement before this would be reliable enough to automate lending decisions without human review.

**4. Loan-specific numeric features dominate over demographic or categorical ones.** Credit amount, Age, and Duration together account for over 52% of total feature importance in the Random Forest model, far ahead of any single categorical feature (the next highest, "Checking account unknown," is only 0.062). This suggests the size and length of the loan itself, along with applicant age, carry more predictive signal than account status or loan purpose in this dataset.

**5. Applicants with no checking account history show up as a meaningfully important signal.** The "Checking account unknown" category (representing applicants with no checking account on record) ranks 4th in importance, ahead of most explicit account-status categories, suggesting the mere absence of a checking account relationship is itself informative for risk.

## Business Trade-off Discussion

Both models output a predicted probability of default rather than a fixed decision, and a lender chooses an approval threshold based on that probability. Lowering the threshold for approval (approving more applicants, including some higher-risk ones) increases loan volume and interest revenue but also increases exposure to defaults; raising the threshold reduces defaults but also rejects some creditworthy applicants and forgoes their revenue.

Given the current recall of only 0.45 to 0.50 on the "bad" class, and an ROC-AUC around 0.75-0.77, neither model is precise enough yet to support an aggressive approval strategy without materially higher default risk than the confusion matrix suggests. A cautious deployment approach would be to use the model's predicted probability as one input among several in the approval decision (not a sole automated cutoff), and to prioritize further improving recall on the "bad" class, since the cost of an undetected default is generally higher than the cost of an unnecessarily cautious rejection.

## Known Gaps and Next Steps

- Given the ~50% recall ceiling on the bad class for both models, consider testing SMOTE as an alternative to class weighting, testing threshold tuning below the default 0.5 cutoff to trade some precision for higher recall on the bad class, or exploring Gradient Boosting as a third model for comparison.
- Consider plotting the ROC curve itself (not just the AUC score) to visualize the precision/recall trade-off at different thresholds, which would support a more concrete recommendation on where to set the approval cutoff.

## Repository Structure

```
Credit-Risk-Default-Prediction/
│
├── data/
│   └── german_credit_data.csv
│
├── notebook.ipynb
│
└── README.md
```

## Skills Demonstrated

- Identifying an unusable dataset (missing target column) before building on it
- Handling class imbalance via class weighting, with an explicit rationale for choosing it over SMOTE
- Encoding a mix of ordinal, nominal, and numeric features
- Comparing an interpretable baseline model against a tree-based model using metrics appropriate to an imbalanced classification problem
- Extracting and interpreting feature importance
- Framing model output as a business risk/revenue trade-off rather than a single accuracy figure

## Author

Raihan Maulana — Data Analyst Portfolio Project