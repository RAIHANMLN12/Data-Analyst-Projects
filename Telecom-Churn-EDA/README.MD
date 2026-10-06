# Telecom Customer Churn Exploratory Analysis

## Overview

This project explores a telecom customer dataset to identify which customer characteristics are most strongly associated with churn. Churn analysis is one of the most common analyst tasks in subscription-based industries, since retaining an existing customer is typically far cheaper than acquiring a new one.

## Business Context

A telecom provider wants to understand why customers leave, so that retention efforts (offers, contract incentives, service improvements) can be targeted at the customers most likely to churn, rather than applied broadly and inefficiently.

## Objectives

The analysis was structured to answer five core questions:

1. What is the overall churn rate?
2. Does churn rate differ by contract type?
3. Is there a relationship between tenure and churn?
4. Which services are associated with higher or lower churn?
5. How does monthly charge relate to churn?

## Dataset

- **Source:** Telco Customer Churn, Kaggle ([link](https://www.kaggle.com/datasets/blastchar/telco-customer-churn))
- **Grain:** One row per customer
- **Fields used:** Contract, tenure, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies, PhoneService, MonthlyCharges, Churn

## Tools Used

- Python 3
- Pandas
- Matplotlib, Seaborn
- Jupyter Notebook

## Methodology

1. Calculated the overall churn rate across the full customer base.
2. Cross-tabulated churn rate against contract type to compare month-to-month, one-year, and two-year customers.
3. Grouped customers into tenure bands (0-12, 13-24, 25-36, 37-48, 49-60, 61-72 months) and compared churn rate across bands.
4. Cross-tabulated churn rate against internet service type and each individual add-on service (Online Security, Online Backup, Device Protection, Tech Support, Streaming TV, Streaming Movies, Phone Service).
5. Compared the distribution of Monthly Charges between churned and retained customers.

## Key Findings

**1. Contract type is the strongest driver of churn.** Churn rate drops sharply as contract length increases: month-to-month customers churn at 42.71%, one-year contract customers at 11.27%, and two-year contract customers at just 2.83%. Customers with no long-term commitment are by far the highest-risk segment.

**2. Churn risk is heavily concentrated in a customer's first year.** New customers (0-12 months tenure) churn at 47.68%, nearly seven times the rate of customers with 61-72 months tenure (6.61%). Churn risk declines steadily and consistently with every additional tenure band, indicating the first year is the critical retention window.

**3. Customers without support or security add-ons churn at roughly double the rate of those with them.** Customers without Online Security, Tech Support, Online Backup, or Device Protection all churn at 39-42%, compared to 15-23% for customers who have each respective service. Fiber optic internet customers also churn substantially more (41.89%) than DSL customers (18.96%), while customers with no internet service at all have the lowest churn rate of any group (7.40%).

**4. Churned customers pay more per month.** Customers who churned had a median Monthly Charge of approximately Rp 80, compared to approximately Rp 65 for customers who stayed, suggesting price sensitivity plays a role alongside contract and service factors.

## Overall Churn Rate

| Churn | % of Customers |
|---|---|
| No | 73.46% |
| Yes | 26.54% |

## Churn Rate by Contract Type

| Contract | No | Yes |
|---|---|---|
| Month-to-month | 57.29% | 42.71% |
| One year | 88.73% | 11.27% |
| Two year | 97.17% | 2.83% |

## Churn Rate by Tenure Group

| Tenure Group | Churn Rate |
|---|---|
| 0-12 months | 47.68% |
| 13-24 months | 28.71% |
| 25-36 months | 21.63% |
| 37-48 months | 19.03% |
| 49-60 months | 14.42% |
| 61-72 months | 6.61% |

## Churn Rate by Service

| Internet Service | No | Yes |
|---|---|---|
| DSL | 81.04% | 18.96% |
| Fiber optic | 58.11% | 41.89% |
| No internet service | 92.60% | 7.40% |

| Add-on Service | Without It (Churn Yes) | With It (Churn Yes) |
|---|---|---|
| Online Security | 41.77% | 14.61% |
| Online Backup | 39.93% | 21.53% |
| Device Protection | 39.13% | 22.50% |
| Tech Support | 41.64% | 15.17% |
| Streaming TV | 33.52% | 30.07% |
| Streaming Movies | 33.68% | 29.94% |

Phone Service showed little difference in churn rate between customers with and without it (26.71% vs. 24.93%), indicating it is not a meaningful churn driver on its own.

## Business Recommendations

- Prioritize retention outreach on new, month-to-month customers, since this segment combines the two highest-risk factors identified in the analysis.
- Bundle or promote Online Security and Tech Support more actively, since customers without these services churn at roughly double the rate of those with them.
- Investigate the Fiber optic experience specifically (pricing, service quality, or competitive pressure), given its churn rate is more than double that of DSL.
- Consider incentives that convert month-to-month customers onto one or two year contracts, given the steep drop in churn associated with longer commitments.

## Repository Structure

```
Telecom-Churn-EDA/
│
├── data/
│   └── WA_Fn-UseC_-Telco-Customer-Churn.csv
│
├── notebook.ipynb
│
└── README.md
```

## Skills Demonstrated

- Exploratory data analysis (EDA) in Python
- Cross-tabulation of categorical variables against a binary outcome
- Grouping and binning a continuous variable (tenure) for comparative analysis
- Comparing distributions between groups (Monthly Charges by churn status)
- Translating statistical findings into business recommendations

## Author

Raihan Maulana — Data Analyst Portfolio Project