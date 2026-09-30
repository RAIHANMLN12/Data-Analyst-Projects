# Predictive Maintenance for Industrial Equipment (Remaining Useful Life)

## Overview

This project uses multivariate sensor time-series data from a fleet of simulated jet engines to predict Remaining Useful Life (RUL), the number of operational cycles remaining before an engine is expected to fail. This is a high-value application at the intersection of data analytics, machine learning, and manufacturing/aviation maintenance operations.

## Business Context

Fixed-schedule maintenance (servicing equipment on a calendar, regardless of actual condition) wastes resources on healthy equipment and risks missing equipment that's degrading faster than average. A model that predicts RUL directly from sensor readings allows a maintenance team to prioritize interventions based on actual equipment condition rather than a one-size-fits-all schedule.

## Objectives

1. Explore sensor readings across engine operational cycles and identify which sensors show a clear degradation trend before failure.
2. Engineer rolling-window statistical features from the raw sensor readings.
3. Frame the problem as regression (predicting RUL directly) and train an appropriate model.
4. Evaluate the model and discuss how a maintenance team could use its output to schedule interventions.

## Dataset

- **Source:** NASA Turbofan Jet Engine Data Set (C-MAPSS), Kaggle ([link](https://www.kaggle.com/datasets/behrad3d/nasa-cmaps)), originally published by NASA's Prognostics Center of Excellence
- **Sub-dataset used:** FD001 (100 train engines, 100 test engines, one operating condition, one fault mode: HPC degradation)
- **Files used:** `train_FD001.txt` (full run-to-failure trajectories, used to compute true RUL for training), `test_FD001.txt` (trajectories truncated before failure), `RUL_FD001.txt` (true remaining RUL at each test engine's truncation point)
- **Structure:** unit number, time in cycles, 3 operational settings, 21 sensor measurements (space-delimited, no header row; column names assigned manually per the dataset's readme)

## Tools Used

- Python 3
- Pandas, NumPy
- Scikit-learn (LinearRegression, RandomForestRegressor)
- Matplotlib

## Methodology

### 1. Sensor Exploration

Plotted 21 sensors against cycle number for several sample engines to distinguish sensors with a clear degradation trend from sensors that remained flat/noisy throughout an engine's life. Fourteen sensors (2, 3, 4, 7, 8, 9, 11, 12, 13, 14, 15, 17, 20, 21) showed clear trends and were retained; the remaining sensors were excluded as uninformative.

### 2. Target Calculation (Training Set)

For each row in the training set, calculated RUL = (that engine's maximum recorded cycle) − (current cycle), giving a countdown to zero at each engine's failure point.

### 3. Feature Engineering

For each of the 14 selected sensors, calculated a rolling mean (5-cycle window) to smooth noise, and a rolling rate of change to capture the speed of degradation. All rolling calculations were computed per engine (grouped by unit number) to avoid mixing one engine's trend with another's.

### 4. Test Set Preparation

Applied the same rolling-window feature engineering to `test_FD001.txt`. Since the official RUL labels apply to each engine's final recorded cycle only, extracted a single row per test engine (the row with the maximum cycle number) to align with the single RUL value provided per engine in `RUL_FD001.txt`.

### 5. Modeling

Trained a Linear Regression model and a Random Forest Regressor on the engineered rolling features (X) against RUL (y), then evaluated both on the 100 held-out test engines.

### 6. Evaluation and Error Analysis

Evaluated using MAE and RMSE, then investigated which test engines produced the largest errors and in what direction.

## Results

### Model Comparison

| Model | MAE | RMSE |
|---|---|---|
| Linear Regression (raw) | 26.83 | 33.00 |
| Linear Regression (non-negative constraint applied) | 26.33 | 32.72 |
| Random Forest | 24.22 | 36.11 |

Random Forest achieves a lower MAE (better on average), but a higher RMSE than Linear Regression, indicating Random Forest is more accurate overall but produces a small number of very large errors that RMSE penalizes heavily; Linear Regression's errors are more consistent in size but slightly worse on average.

### Correcting Physically Impossible Predictions

Linear Regression's raw output included negative RUL predictions for several engines close to failure, which is physically impossible (an engine cannot have a negative remaining life). Applying a non-negative constraint (clipping predictions to a minimum of 0) improved both MAE (26.83 → 26.33) and RMSE (33.00 → 32.72), a small but meaningful correction that also removes results that would be immediately implausible to a maintenance engineer reviewing the output.

### Largest Errors: A Systematic Overprediction Bias

| Unit | True RUL | Predicted RUL | Absolute Error |
|---|---|---|---|
| 78 | 107 | 233.57 | 126.57 |
| 67 | 77 | 196.96 | 119.96 |
| 15 | 83 | 198.81 | 115.81 |
| 79 | 63 | 158.68 | 95.68 |
| 23 | 113 | 195.56 | 82.56 |
| 6 | 93 | 167.86 | 74.86 |
| 27 | 66 | 129.79 | 63.79 |
| 65 | 128 | 189.13 | 61.13 |
| 54 | 97 | 153.53 | 56.53 |
| 59 | 114 | 169.92 | 55.92 |

Every one of the 10 largest Random Forest errors is an **overprediction** (the model predicts more remaining life than the engine actually had), not a random mix of over- and under-predictions. This is the more dangerous error direction for predictive maintenance: an overprediction encourages delaying service, risking an unplanned failure before the next scheduled intervention, whereas an underprediction only results in unnecessarily early (but safe) servicing.

## Root Cause: Overprediction Is Concentrated in Engines Truncated Early in Their Life

Comparing the worst-performing engine (Unit 78) against the best-performing engine (Unit 38):

| | Unit 78 (worst) | Unit 38 (best) |
|---|---|---|
| Cycle at truncation | 72 | 125 |
| True RUL | 107 | 50 |
| Estimated total lifespan | 179 cycles | 175 cycles |
| % of lifespan observed | 40% | 71% |
| Absolute error | 126.57 | 0.31 |

The two engines have almost identical total lifespans (179 vs. 175 cycles), but Unit 78's test data was cut off much earlier in its life (only 40% through), while Unit 38's was cut off much later (71% through, closer to failure). Sensor rolling-mean trends for Unit 78 (plotted across sensors 2, 3, 4, 7, 8, and 9) appear comparatively stable, showing little visible degradation signal at the point of truncation, while Unit 38's sensors, observed much closer to failure, show more pronounced movement.

This indicates the model's accuracy depends heavily on how far into an engine's life the available data extends: engines observed early in their life, before meaningful degradation signal has emerged in the sensors, are harder to distinguish from engines with a longer overall lifespan, leading to systematic overprediction. This matches a well-documented characteristic of RUL prediction models generally: confidence and accuracy improve as a unit approaches failure, and are inherently weaker earlier in an asset's life.

## Key Findings

**1. Random Forest is more accurate on average, but Linear Regression is more consistent.** The MAE/RMSE trade-off between the two models reflects a real choice: Random Forest reduces typical error but is more prone to occasional large misses; Linear Regression is a safer, more predictable baseline.

**2. Model errors are not random, they are systematically biased toward overprediction in the worst cases.** All 10 of the largest errors overestimate remaining life, the error direction that most increases the risk of an unplanned failure.

**3. Prediction accuracy depends on how close to failure the available data extends.** Engines with data truncated early in their life, before clear degradation signal has emerged, are predicted far less accurately than engines observed closer to their actual failure point.

## Recommendation for Maintenance Teams

Given the systematic overprediction bias identified above, raw model output should not be used directly as a maintenance schedule. Instead:

- Apply a safety margin below the predicted RUL (e.g. schedule inspection at predicted RUL minus 30-40 cycles) rather than at the predicted value itself, since the model's largest errors consistently overestimate remaining life rather than underestimate it.
- Treat predictions for engines with limited operating history (a small number of cycles observed relative to typical fleet lifespan) with additional caution, and consider more frequent monitoring for these engines until more data accumulates and prediction confidence improves.
- Use predicted RUL to prioritize which engines receive closer inspection first (lowest predicted RUL first), rather than as an exact countdown to be trusted at face value.

## Repository Structure

```
Predictive-Maintenance-RUL/
│
├── data/
│   ├── RUL_FD001.txt
│   ├── test_FD001.txt
│   └── train_FD001.txt
│
├── notebook.ipynb
│
└── README.md
```

## Skills Demonstrated

- Working with multivariate sensor time-series data across multiple independent units
- Visual exploration to distinguish informative sensors from noise before feature engineering
- Engineering rolling-window features (moving average, rate of change) computed correctly per unit
- Framing an open-ended maintenance problem as a supervised regression task
- Applying a physically-informed constraint (non-negative RUL) to correct implausible model output
- Diagnosing a systematic, direction-specific model bias (overprediction) rather than only reporting aggregate error metrics
- Connecting model limitations to a concrete, risk-aware operational recommendation

## Author

Raihan Maulana — Data Analyst Portfolio Project