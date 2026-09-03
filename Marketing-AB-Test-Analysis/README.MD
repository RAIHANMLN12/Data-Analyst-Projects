# Marketing Campaign A/B Test Analysis

## Overview

This project statistically evaluates the results of an A/B test comparing a marketing ad campaign against a public service announcement (PSA) baseline, determining whether the observed difference in conversion rate is statistically significant or could plausibly be due to chance. A/B testing is a core skill for any analyst supporting a marketing or growth team.

## Business Context

Before committing ad spend at scale, a company needs to know whether advertising actually drives more conversions than not advertising at all, and whether that improvement is large enough and efficient enough to justify the investment. This project answers that question using a formal hypothesis test rather than relying on a raw percentage comparison alone.

## Objectives

1. Calculate the conversion rate for the control (psa) and treatment (ad) groups.
2. Run a statistical significance test to determine if the difference in conversion rate is real.
3. Calculate a confidence interval for the size of the difference.
4. Check whether the treatment group differs in efficiency, not just raw conversion rate.
5. Deliver a clear business recommendation.

## Dataset

- **Source:** Marketing A/B Testing, Kaggle ([link](https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing))
- **Grain:** One row per user
- **Fields used:** test group (ad / psa), converted (True / False), total ads

**Group definitions:**
- **psa (control):** Users who only saw a public service announcement, effectively a no-advertising baseline.
- **ad (treatment):** Users who were shown the actual marketing campaign.

## Tools Used

- Python 3
- Pandas
- SciPy (chi-square test)
- Matplotlib
- Jupyter Notebook

## Methodology

1. Calculated conversion rate for each group by taking the mean of the converted field (True/False) within each test group.
2. Built a 2x2 contingency table (test group x converted) using the raw counts.
3. Ran a chi-square test on the contingency table to test whether conversion is independent of group assignment.
4. Calculated a 95% confidence interval for the difference in conversion rates between the two groups.
5. Since the dataset does not include a direct cost field, used total ads (ad impressions delivered) as a proxy for spend, comparing average ads per person and conversions per 1,000 ads between groups.
6. Synthesized the statistical and efficiency results into a business recommendation.

## Results

### Conversion Rate by Group

| Test Group | Not Converted | Converted |
|---|---|---|
| ad | 97.45% | 2.55% |
| psa | 98.21% | 1.79% |

### Raw Counts

| Test Group | Not Converted | Converted |
|---|---|---|
| ad | 550,154 | 14,423 |
| psa | 23,104 | 420 |

### Statistical Significance

- **Chi-square statistic:** 54.01
- **P-value:** approximately 0.0
- **Result:** Reject the null hypothesis. There is a statistically significant difference in conversion rate between the ad and psa groups.

### Confidence Interval

The difference in conversion rate between ad (2.55%) and psa (1.79%) is **0.77 percentage points**, with a **95% confidence interval of approximately 0.60 to 0.94 percentage points**. This indicates the true improvement from advertising is consistently positive and not simply an artifact of the large sample size.

### Efficiency Comparison

| Test Group | Total Users | Conversions | Total Ads | Avg. Ads per Person | Conversions per 1,000 Ads |
|---|---|---|---|---|---|
| ad | 564,577 | 14,423 | 14,014,701 | 24.82 | 1.029 |
| psa | 23,524 | 420 | 582,481 | 24.76 | 0.721 |

Average ad exposure per person is nearly identical between the two groups (24.82 vs. 24.76), meaning the ad group's higher conversion rate was not achieved by simply showing each person more ads. On a per-impression basis, the ad group also converted more efficiently, delivering 1.029 conversions per 1,000 ads shown versus 0.721 for psa.

## Key Findings

**1. Advertising produces a statistically significant lift in conversion.** The ad group converted at 2.55% versus 1.79% for psa, a result far too consistent to be explained by chance (p ≈ 0.0).

**2. The effect size is small in absolute terms but reliable.** A 0.77 percentage point lift, with a tight 95% confidence interval (0.60 to 0.94 points), confirms a real but modest improvement per user, one that becomes substantial in aggregate given the scale of the audience.

**3. The improvement came at no efficiency cost.** Despite converting more users, the ad group did not require meaningfully more ad exposure per person, and was in fact more efficient on a per-impression basis than the psa baseline.

## Recommendation

**We recommend rolling out the ad campaign company-wide.**

The ad group achieved a conversion rate of 2.55%, compared to 1.79% for the psa (control) group, a difference of 0.77 percentage points that is highly statistically significant (chi-square = 54.01, p ≈ 0.0). The 95% confidence interval indicates the true improvement lies between approximately 0.60 and 0.94 percentage points, confirming this is a reliable, consistently positive effect rather than a chance result inflated by sample size alone.

Importantly, this improvement did not come at a proportionally higher cost. Average ad exposures per person were nearly identical between groups (24.82 for ad vs. 24.76 for psa), and the ad group actually delivered more conversions per 1,000 ads shown (1.03 vs. 0.72 for psa). This indicates the ad campaign is both more effective and more efficient than the psa baseline, making the case for rollout straightforward with no meaningful efficiency trade-off to weigh against it.

## Repository Structure

```
Marketing-AB-Test-Analysis/
│
├── data/
│   └── marketing_AB.csv
│
├── notebook.ipynb
│
└── README.md
```

## Skills Demonstrated

- Formal hypothesis testing (chi-square test) on a business metric
- Computing and interpreting p-values and confidence intervals
- Evaluating a business metric on both effectiveness and efficiency, not effectiveness alone
- Communicating a statistical result as a clear, decision-ready business recommendation

## Author

Raihan Maulana — Data Analyst Portfolio Project