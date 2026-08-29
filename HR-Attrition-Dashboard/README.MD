# HR Attrition & Workforce Dashboard

## Overview

This project analyzes employee attrition and workforce composition for a fictional company's HR department using an interactive Tableau dashboard. HR analytics is a fast-growing niche where the fundamentals of clear dashboarding and filtering matter more than advanced modelling.

## Business Context

HR leadership needs to understand where attrition risk is concentrated (which departments, job levels, roles, and tenure bands) so that retention budget and effort can be targeted rather than spread evenly across the entire workforce.

## Objectives

The dashboard was built to answer four core questions:

1. What is the overall attrition rate, and how does it break down by department?
2. Does attrition differ by job level, age group, or years at the company?
3. Is there a relationship between monthly income and attrition?
4. Which job roles have the highest attrition rate?

## Dataset

- **Source:** IBM HR Analytics Employee Attrition & Performance, Kaggle ([link](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset))
- **Grain:** One row per employee
- **Fields used:** Department, JobLevel, JobRole, YearsAtCompany, MonthlyIncome, Attrition

## Tools Used

- Tableau
- Calculated fields (attrition rate, tenure grouping)

## Methodology

1. Calculated the overall attrition rate across the full workforce.
2. Broke down attrition rate by department using a bar chart.
3. Broke down attrition rate by job level (1 through 5).
4. Grouped Years at Company into bands (Under 5, 5-10, 11-15, 16-20, 20+) and compared attrition rate across bands.
5. Broke down attrition rate by job role, sorted to surface the highest-risk roles.
6. Compared Monthly Income between employees who left and employees who stayed.
7. Added slicers for Department and Job Role so the dashboard can be explored interactively.

## Key Findings

**1. Sales and HR carry the highest departmental attrition risk.** Sales has the highest attrition rate at 20.63%, followed by Human Resources at 19.05%. Research & Development is comparatively stable at 13.84%.

**2. Attrition is heavily concentrated at the entry job level.** Job Level 1 employees leave at 26.34%, more than double the rate of every other level. Attrition then drops sharply at Level 2 (9.74%) before rising again at Level 3 (14.68%), then falling to its lowest points at Levels 4 and 5 (4.72% and 7.25%).

**3. Tenure shows a "danger zone in the first five years, then again after 20 years" pattern.** Employees with under 5 years at the company attrition at 24.31%, the highest of any tenure band. Attrition rate drops to its lowest in the 11-15 and 16-20 year bands (6.48% and 6.94%), then rises again to 12.12% for employees with 20+ years, suggesting a second, smaller risk period late in tenure (possibly related to retirement decisions).

**4. Sales Representative is by far the highest-risk job role.** Sales Representatives attrition at 39.76%, roughly double the next-highest roles (Laboratory Technician at 23.94% and Human Resources at 23.08%). Managers and Research Directors are the most stable roles, both under 5%.

**5. Employees who left earned noticeably less on average than those who stayed.** Average Monthly Income among employees who did not leave was 6,833, compared to 4,787 among those who left, roughly a 30% gap. This supports compensation as one contributing factor in attrition, alongside job level and tenure.

## Overall Attrition Rate

**16.12%** of employees in the dataset have left the company.

## Attrition Rate by Department

| Department | Attrition Rate |
|---|---|
| Sales | 20.63% |
| Human Resources | 19.05% |
| Research & Development | 13.84% |

## Attrition Rate by Job Level

| Job Level | Attrition Rate |
|---|---|
| 1 | 26.34% |
| 2 | 9.74% |
| 3 | 14.68% |
| 4 | 4.72% |
| 5 | 7.25% |

## Attrition Rate by Years at Company

| Years at Company | Attrition Rate |
|---|---|
| Under 5 years | 24.31% |
| 5-10 years | 11.80% |
| 11-15 years | 6.48% |
| 16-20 years | 6.94% |
| 20+ years | 12.12% |

## Attrition Rate by Job Role

| Job Role | Attrition Rate |
|---|---|
| Sales Representative | 39.76% |
| Laboratory Technician | 23.94% |
| Human Resources | 23.08% |
| Sales Executive | 17.48% |
| Research Scientist | 16.10% |
| Human Resources (Manufacturing Director) | 6.90% |
| Healthcare Representative | 6.87% |
| Manager | 4.90% |
| Research Director | 2.50% |

## Attrition Rate by Monthly Income

| Attrition | Avg. Monthly Income |
|---|---|
| No | 6,833 |
| Yes | 4,787 |

- Prioritize retention programs for Sales Representatives and Job Level 1 employees, since these two segments show attrition rates well above every other group in the dashboard.
- Investigate onboarding and early-career support, given attrition is heavily concentrated in the first 5 years of tenure.
- Monitor the 20+ year tenure band separately from the rest of the workforce, since it shows a secondary rise in attrition that may need a different retention approach (e.g. retirement planning) than early-career attrition.
- Investigate compensation for entry-level and Sales roles specifically, since retained employees earn roughly 30% more on average than those who left, and this overlaps with the highest-attrition segments identified above (Job Level 1, Sales Representative).

## Note on Data Quality

An earlier version of the Monthly Income comparison was aggregated as **Sum** instead of **Average** in Tableau, which produced a misleadingly large gap (8,424,768 vs. 1,134,541). This was corrected to use Average, resulting in the 6,833 vs. 4,787 figures reported above, which are consistent with the typical pattern in this dataset. This is a good example of why aggregation type should always be checked before reporting a comparison of means.

## Repository Structure

```
HR-Attrition-Dashboard/
│
├── data/
│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv
│
├── analysis.twb
│
└── README.MD
```

## Skills Demonstrated

- Connecting and modelling raw data in Tableau
- Calculated fields for attrition rate and tenure grouping
- Interactive dashboard design with slicers/filters
- Choosing visuals appropriate for a non-technical HR audience

## Author

Raihan Maulana — Data Analyst Portfolio Project