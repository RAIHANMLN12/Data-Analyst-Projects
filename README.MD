# Data Analyst Portfolio

A collection of 9 end-to-end data analytics projects spanning Excel, SQL-adjacent data wrangling, Python (Pandas, Scikit-learn), Tableau, and NLP, covering the core workflow of a data analyst role: exploratory analysis, dashboarding, statistical testing, machine learning (regression, classification, clustering), time-series forecasting, and natural language processing. Each project is based on a real public dataset and includes a full write-up with methodology, key findings, and business recommendations.

## Projects

| # | Project | Industry | Core Skill | Tools |
|---|---|---|---|---|
| 1 | [Retail Sales Performance Dashboard](./Retail-Sales-Performance-Dashboard) | Retail | Pivot tables & dashboarding | Excel |
| 2 | [Telecom Customer Churn Exploratory Analysis](./Telecom-Churn-EDA) | Telecommunications | Exploratory data analysis | Python, Pandas |
| 3 | [HR Attrition & Workforce Dashboard](./HR-Attrition-Dashboard) | Human Resources | Interactive BI dashboarding | Tableau |
| 4 | [E-commerce Customer Segmentation (RFM + K-Means)](./Ecommerce-Customer-Segmentation) | E-commerce | Unsupervised machine learning | Python, Scikit-learn |
| 5 | [Marketing Campaign A/B Test Analysis](./Marketing-AB-Test-Analysis) | Marketing | Statistical hypothesis testing | Python, SciPy |
| 6 | [Retail/FMCG Sales Forecasting with Regression](./Retail-Sales-Forecasting) | Retail / FMCG | Time-series regression | Python, Scikit-learn |
| 7 | [Credit Risk & Loan Default Prediction](./Credit-Risk-Default-Prediction) | Banking / Fintech | Classification & imbalanced data | Python, Scikit-learn |
| 8 | [Predictive Maintenance for Industrial Equipment](./Predictive-Maintenance-RUL) | Manufacturing | Sensor time-series regression | Python, Scikit-learn |
| 9 | [Customer Review Sentiment Analysis (NLP)](./Customer-Review-Sentiment-Analysis) | E-commerce / Consumer Goods | NLP & text classification | Python, NLTK, Scikit-learn |

## Project Summaries

### 1. Retail Sales Performance Dashboard
Built an Excel-based sales dashboard for a multi-year retail transactions dataset, using pivot tables to analyze performance across region, product category, and customer, consolidated into a single executive-ready view with supporting charts. Caught and fixed a 100x scaling bug that had made Profit appear larger than Sales, then found that Furniture, despite strong sales volume, operates at a razor-thin margin and is unprofitable in two regions, while West leads on both sales and profit. **[Read more →](./Retail-Sales-Performance-Dashboard)**

### 2. Telecom Customer Churn Exploratory Analysis
Explored a ~7,000-customer telecom dataset in Python to identify churn drivers. Found contract type as the single strongest driver, with month-to-month customers churning at 42.71% versus 2.83% for two-year contracts, and churn risk concentrated in a customer's first year. **[Read more →](./Telecom-Churn-EDA)**

### 3. HR Attrition & Workforce Dashboard
Built an interactive Tableau dashboard analyzing employee attrition across department, job level, tenure, and compensation. Found Sales Representatives attrition at nearly 40%, roughly double any other role, and caught and corrected a Tableau aggregation bug (Sum vs. Average) that had inflated an income comparison by over 7x. **[Read more →](./HR-Attrition-Dashboard)**

### 4. E-commerce Customer Segmentation (RFM + K-Means)
Segmented 4,339 e-commerce customers into 5 behavioral clusters using RFM feature engineering and K-Means clustering, translating each cluster into a marketing persona with a tailored recommendation, including correctly identifying a sub-0.25% outlier group as wholesale/B2B accounts rather than a mainstream marketing segment. **[Read more →](./Ecommerce-Customer-Segmentation)**

### 5. Marketing Campaign A/B Test Analysis
Statistically validated a marketing campaign's impact using a chi-square test on over 580,000 users, confirming a statistically significant 0.77-percentage-point conversion lift (p ≈ 0.0) that came with no efficiency trade-off, supporting a clear rollout recommendation. **[Read more →](./Marketing-AB-Test-Analysis)**

### 6. Retail/FMCG Sales Forecasting with Regression
Built a time-based sales forecasting model on over 840,000 daily store-level records, comparing Linear Regression against Random Forest, then diagnosed a systematic model bias where high-volume stores were consistently underpredicted and low-volume stores overpredicted, due to the model lacking store-level baseline information. **[Read more →](./Retail-Sales-Forecasting)**

### 7. Credit Risk & Loan Default Prediction
Built a loan default classification model handling class imbalance via weighted Logistic Regression and Random Forest, selected Random Forest based on ROC-AUC (0.767) and recall rather than raw accuracy, and connected the model's output to a bank's approval-threshold risk/revenue trade-off. **[Read more →](./Credit-Risk-Default-Prediction)**

### 8. Predictive Maintenance for Industrial Equipment
Built a Remaining Useful Life (RUL) regression model on NASA C-MAPSS turbofan engine sensor data, engineered rolling-window degradation features, and diagnosed a systematic overprediction bias in the model's worst errors, tracing the root cause to engines whose data was truncated early in their operational life. **[Read more →](./Predictive-Maintenance-RUL)**

### 9. Customer Review Sentiment Analysis (NLP)
Built a sentiment classifier on 525,000+ Amazon food reviews using TF-IDF and Logistic Regression, compared against a neural network, found and fixed an HTML-artifact data cleaning bug, and showed that the neural network's initial apparent advantage had largely been an artifact of unclean data once corrected. **[Read more →](./Customer-Review-Sentiment-Analysis)**

## Repository Structure

```
data-analyst-portfolio/
│
├── Retail-Sales-Performance-Dashboard/
├── Telecom-Churn-EDA/
├── HR-Attrition-Dashboard/
├── Ecommerce-Customer-Segmentation/
├── Marketing-AB-Test-Analysis/
├── Retail-Sales-Forecasting/
├── Credit-Risk-Default-Prediction/
├── Predictive-Maintenance-RUL/
├── Customer-Review-Sentiment-Analysis/
│
└── README.md
```

Each project folder contains its own `README.md` with full methodology, results, and business recommendations, a `data/` subfolder, and the analysis notebook or workbook.

## Author

Raihan Maulana — Data Analyst Portfolio Project