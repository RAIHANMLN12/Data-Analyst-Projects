# Retail/FMCG Sales Forecasting with Regression

## Overview

This project builds a regression-based sales forecasting model for a retail chain with many stores, incorporating seasonality, promotions, and holiday effects. Demand forecasting directly supports inventory and staffing decisions, making it one of the highest-value analytics use cases in retail and FMCG.

## Business Context

A retail chain needs to forecast daily sales per store to plan inventory levels and staffing in advance. Under-forecasting risks stockouts and lost sales; over-forecasting ties up capital in excess inventory and overstaffing. This project builds and evaluates a forecasting model using only information that would realistically be known in advance (calendar effects, promotions, store category), and honestly reports where that model succeeds and where it breaks down.

## Objectives

1. Engineer time-based features (day of week, month, holiday flag, promotion flag).
2. Split the data using a time-based split appropriate for time series forecasting.
3. Train and compare a linear regression model against a Random Forest model.
4. Evaluate both models using MAE and RMSE.
5. Identify which store or time features drive the largest forecast errors.
6. Visualize actual vs. predicted sales for a sample of stores.

## Dataset

- **Source:** Rossmann Store Sales, Kaggle ([link](https://www.kaggle.com/c/rossmann-store-sales/data))
- **Files used:** `train.csv` (daily sales per store, includes true Sales values) merged with `store.csv` (store-level metadata: StoreType, Assortment, CompetitionDistance, etc.) on the `Store` column, using a left join
- **Files excluded:** `test.csv` (no Sales values, meant for Kaggle competition submission) and `sample_submission.csv` (submission format template only), neither usable for building or evaluating a model with known outcomes

## Tools Used

- Python 3
- Pandas, NumPy
- Scikit-learn (LinearRegression, RandomForestRegressor)
- Matplotlib
- Jupyter Notebook

## Methodology

### 1. Data Preparation and Cleaning

- Merged `train.csv` with `store.csv` on `Store` using a left join, preserving every sales record while attaching store metadata.
- Filtered out rows where `Open == 0`, since closed stores trivially have zero sales and would distort model evaluation without reflecting real demand patterns.
- Excluded the `Customers` column from the feature set. Although highly correlated with Sales, the actual number of customers on a future day is not knowable in advance, including it would constitute data leakage and produce misleadingly optimistic results.

### 2. Feature Engineering

- Extracted `Month` and `Year` from the `Date` column (`DayOfWeek` was already present in the raw data).
- Created a binary `IsHoliday` flag from the raw `StateHoliday` column (which contained multiple holiday-type codes) to simplify holiday effects into a single indicator.
- One-hot encoded `StoreType` and `Assortment` (with `drop_first=True` to avoid redundant, collinear columns).
- Final feature set: `DayOfWeek`, `Month`, `Promo`, `IsHoliday`, `SchoolHoliday`, `StoreType_b`, `StoreType_c`, `StoreType_d`, `Assortment_b`, `Assortment_c`.

### 3. Time-Based Train/Test Split

Split chronologically rather than randomly, since random splitting on time series data risks leaking future information into training and produces an unrealistically optimistic evaluation.

- **Train:** 2013-01-01 to 2015-05-31 (785,781 rows)
- **Test:** 2015-06-01 to 2015-07-31 (58,611 rows, the most recent 2 months)

### 4. Model Training and Evaluation

Trained a Linear Regression model (as an interpretable baseline) and a Random Forest model (100 trees), then evaluated both on the held-out test set using MAE and RMSE.

| Model | MAE | RMSE |
|---|---|---|
| Linear Regression | 1,953.84 | 2,709.45 |
| Random Forest | 1,885.02 | 2,660.99 |

Random Forest outperformed Linear Regression on both metrics, though the improvement was modest (about 3.5% lower MAE) rather than dramatic, suggesting the available features capture most of the learnable signal, and the remaining error is driven by factors discussed below rather than model choice alone.

### 5. Error Analysis

Analyzed absolute error (Random Forest predictions) broken down by store and by time-based features:

**By day of week:** Sunday showed by far the highest average error (2,760.92), compared to a range of 1,709–2,174 for other days, likely reflecting less consistent demand patterns on the one day most stores are closed.

**By promotion status:** Promotion days showed higher average error (2,191.40) than non-promotion days (1,620.40). This indicates the model struggles more, not less, on promotion days, because a binary promo flag does not capture the varying scale of different promotions' impact.

**By holiday status:** Holiday effect on error was comparatively small (2,000.66 vs. 1,884.85 for non-holidays).

**By store:** Error was heavily concentrated in a small number of stores. The highest-error store (Store 1114, average absolute error 15,193) had error roughly 27 times larger than the lowest-error store (Store 766, average absolute error 479).

### 6. Root Cause Investigation: Store 1114 vs. Store 766

Investigated why error was so concentrated in specific stores:

- **Store 1114** averages 20,667 in daily sales (std. dev. 3,453) and shows two sharp, extreme sales spikes (likely tied to year-end periods) that the available calendar features (Month, IsHoliday) are too coarse to capture.
- **Store 766** averages 6,124 in daily sales (std. dev. 1,446) with a stable, repeating seasonal pattern, which the model predicts very accurately.
- Critically, plotting actual vs. predicted sales for several stores revealed a systematic pattern: the model consistently **underpredicts** stores with sales well above their category average (e.g. Store 1114) and **overpredicts** stores well below their category average (e.g. Store 1, Store 25), while stores close to their category's average (e.g. Store 766) are predicted accurately.

This happens because the model has no way to identify an individual store, only its category (StoreType, Assortment). Within any given category, store-level sales volume can vary widely, and the model effectively predicts the category average rather than each store's specific baseline. The model correctly captures the shape of weekly and seasonal patterns, but systematically misses each store's absolute sales level.

## Key Findings

**1. Random Forest modestly outperforms Linear Regression, but the gap is small.** The 3.5% MAE improvement suggests the current feature set is close to its ceiling; further gains are more likely to come from better features than from a more complex model.

**2. Forecast error is concentrated in specific stores, not spread evenly.** A small number of stores account for a disproportionate share of total error, driven by extreme sales volatility (Store 1114) that current calendar-based features cannot explain.

**3. The model has a systematic bias tied to store-level baseline sales.** Stores far from their category's average sales level are consistently mispredicted in a predictable direction (high-volume stores underpredicted, low-volume stores overpredicted), because the model has no feature representing each store's individual sales history or scale.

**4. Promotions increase forecast error rather than reducing it.** A binary Promo flag does not capture how much a given promotion actually lifts sales, so promotion days are harder, not easier, for the model to predict accurately.

## Recommendations for Model Improvement

- Add a feature representing each store's historical average sales (e.g. trailing 90-day average) to give the model a sense of store-level baseline, directly addressing the systematic bias identified above.
- Incorporate `CompetitionDistance` and competition open date, currently available in the merged dataset but unused, since store-level competitive pressure may explain some of the extreme volatility seen in high-error stores.
- Consider modelling promotion impact with more granularity than a binary flag, if promotion type or intensity data becomes available.
- Consider a hierarchical or per-cluster modelling approach, grouping stores by sales scale before forecasting, rather than a single model applied uniformly across stores of very different sizes.

## Repository Structure

```
Retail-Sales-Forecasting/
│
├── data/
│   ├── train.csv
│   └── store.csv
│
├── notebook.ipynb
│
└── README.md
```

## Skills Demonstrated

- Correct handling of a multi-file dataset, including identifying which files were usable given the presence of known outcomes
- Left-join merging of a fact table with a reference/lookup table
- Identifying and preventing data leakage (excluding Customers as a feature)
- Time-based feature engineering and categorical encoding
- Time-based train/test splitting, avoiding random splits on time series data
- Comparing an interpretable baseline model against a more flexible tree-based model using MAE and RMSE
- Root-cause analysis of model error by store and time-based features, going beyond aggregate metrics to diagnose a systematic model bias

## Author

Raihan Maulan — Data Analyst Portfolio Project