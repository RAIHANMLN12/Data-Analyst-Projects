# E-commerce Customer Segmentation (RFM + K-Means)

## Overview

This project segments an e-commerce customer base using RFM analysis (Recency, Frequency, Monetary value) combined with K-Means clustering, then translates the resulting clusters into actionable marketing personas. Customer segmentation is one of the most commonly requested analytics use cases across e-commerce and retail.

## Business Context

Treating every customer the same way wastes marketing budget. A retailer needs to know which customers are highly engaged and worth investing in, which are at risk of churning and need a win-back offer, and which don't fit the standard retail pattern at all. This project builds that segmentation from raw transaction data.

## Objectives

1. Clean raw transaction data into an analysis-ready format.
2. Engineer Recency, Frequency, and Monetary (RFM) features per customer.
3. Scale the RFM features and apply K-Means clustering, choosing the number of clusters using the elbow method.
4. Profile each cluster and translate it into a business-usable persona.
5. Provide a marketing recommendation for each segment.

## Dataset

- **Source:** Online Retail, UCI Machine Learning Repository ([link](https://archive.ics.uci.edu/dataset/352/online+retail))
- **Grain:** One row per item per transaction (order line level)
- **Fields used:** CustomerID, InvoiceNo, InvoiceDate, Quantity, UnitPrice

## Tools Used

- Python 3
- Pandas, NumPy
- Scikit-learn (StandardScaler, KMeans)
- Matplotlib, Seaborn
- Jupyter Notebook

## Methodology

1. **Data cleaning:** Removed cancelled orders (invoices prefixed with "C"), transactions with missing Customer ID, and rows with negative quantity.
2. **Feature engineering:** Aggregated transaction-level data into one row per customer, calculating Recency (days since last purchase), Frequency (number of distinct invoices), and Monetary (total amount spent).
3. **Scaling:** Standardized the RFM features (StandardScaler) so that Recency, Frequency, and Monetary contribute equally to the clustering, regardless of their differing raw scales.
4. **Cluster selection:** Used the elbow method to evaluate cluster counts and selected K=5.
5. **Clustering:** Applied K-Means with K=5 on the scaled RFM features.
6. **Cluster profiling:** Calculated the mean Recency, Frequency, and Monetary value per cluster, and cross-checked cluster size to identify whether any cluster represented a genuine customer segment versus a small number of extreme outliers.
7. **Visualization:** Built a scatter plot (Recency vs. Monetary, log scale).

## Cluster Summary

| Cluster | Recency (days) | Frequency | Monetary | Count | % of Customers | Persona |
|---|---|---|---|---|---|---|
| 0 | 43.26 | 73.84 | 1,346.98 | 3,036 | 69.96% | Standard / Occasional Customers |
| 1 | 248.48 | 27.79 | 490.45 | 1,055 | 24.31% | At Risk / Lapsed Customers |
| 2 | 15.08 | 488.34 | 12,082.72 | 238 | 5.48% | Loyal High-Value Customers |
| 3 | 1.00 | 5,807.75 | 70,925.29 | 4 | 0.09% | Outlier Wholesale/B2B Accounts |
| 4 | 6.67 | 827.83 | 190,863.46 | 6 | 0.14% | Outlier Wholesale/B2B Accounts |

## Key Findings

**1. The customer base is dominated by moderately engaged, occasional shoppers.** Cluster 0 makes up nearly 70% of all customers, with moderate recency and low-to-moderate frequency and spend. This is the addressable mass market where small improvements in engagement have the largest aggregate revenue impact.

**2. A quarter of the customer base has effectively gone dormant.** Cluster 1 shows an average recency of 248 days (over 8 months since last purchase), the highest of any cluster, alongside the lowest frequency and monetary value. This segment represents significant churn risk and a clear target for win-back campaigns.

**3. A small segment of loyal, high-value customers drives disproportionate revenue.** Cluster 2 accounts for just 5.5% of customers but shows strong recency, high frequency, and monetary value more than 8 times higher than Cluster 0. Retaining and growing this segment is likely more cost-effective than acquiring new customers of similar value.

**4. K-Means correctly isolated a tiny group of extreme outliers as separate clusters.** Clusters 3 and 4 together represent only 10 customers (0.23% of the base) but show frequency and monetary values orders of magnitude above every other cluster. This pattern is inconsistent with typical retail behavior and strongly suggests wholesale, reseller, or B2B accounts rather than individual retail shoppers.

## Cluster Recommendations

**Cluster 0, Standard / Occasional Customers.** Run broad, low-cost reactivation campaigns such as newsletters, seasonal promotions, and product recommendations designed to nudge this segment toward more frequent purchasing. Even a small uplift here has outsized revenue impact given the segment's size.

**Cluster 1, At Risk / Lapsed Customers.** Deploy a targeted win-back campaign with a meaningful incentive, such as a significant discount or a personalized offer referencing past purchase history. Generic promotions are unlikely to re-engage a segment this disengaged.

**Cluster 2, Loyal High-Value Customers.** Protect and grow this group through a formal loyalty or VIP tier, proactive cross-sell and upsell offers, and early access to new products. Retaining this segment is likely more cost-effective than acquiring new customers of similar value.

**Clusters 3 and 4, Outlier Wholesale/B2B Accounts.** Exclude this group from mass-market segmentation entirely. Manage them through dedicated account management with negotiated pricing and personalized service, since a standard retail promotion would be both irrelevant and potentially costly at this order volume.

## Overall Summary

The clustering results reveal a customer base overwhelmingly composed of standard, moderately engaged shoppers (Cluster 0), with a substantial at-risk segment (Cluster 1) that requires active win-back effort to prevent further churn. A small but highly valuable loyal segment (Cluster 2) represents the healthiest part of the retail customer base and should be prioritized for retention. Finally, the clustering surfaced a tiny group of extreme outliers (Clusters 3 and 4) whose transaction volumes are inconsistent with typical retail behavior, indicating wholesale or B2B accounts that require an entirely different, non-marketing-driven engagement strategy. Overall, customer value in this dataset is highly concentrated: a small fraction of customers (Cluster 2 plus the outlier accounts) accounts for a disproportionate share of total transaction volume and revenue, reinforcing the importance of retention-focused strategy over broad-based acquisition.

## Repository Structure

```
Ecommerce-Customer-Segmentation/
│
├── data/
│   └── Online Retail.xlsx
│
├── notebook.ipynb
│
└── README.md
```

## Skills Demonstrated

- Feature engineering (RFM) from transaction-level data
- Feature scaling prior to distance-based clustering
- Applying and validating an unsupervised learning algorithm (K-Means, elbow method)
- Critically evaluating cluster output for business plausibility, including identifying and correctly handling statistical outliers
- Translating unsupervised model output into business-usable customer personas and marketing recommendations

## Author

Raihan Maulana — Data Analyst Portfolio Project