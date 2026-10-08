# Bank Marketing Campaign Analysis — Term Deposit Subscription

## Project Overview

This project analyzes a Portuguese bank's direct-marketing campaign data to identify customer segments associated with higher term-deposit subscription rates and develop a **pre-call customer scoring model** for campaign prioritization.

The analysis covers **41,188 customer-contact observations and 21 variables** from telephone marketing campaigns conducted between 2008 and 2010.

The project combines exploratory data analysis, customer segmentation, predictive modeling, and business-oriented campaign recommendations.

### Tools

Python · Pandas · NumPy · Matplotlib · Scikit-learn · Jupyter

### Deliverables

- 5 Jupyter notebooks
- PDF analytical report
- PowerPoint presentation
- Reproducible result tables and visualizations

---

## Business Problem

The bank contacts customers to promote term deposits, but only a relatively small proportion of contacted customers subscribe.

The key business questions are:

1. What customer characteristics are associated with higher subscription rates?
2. Which campaign factors are associated with conversion?
3. Which customer segments should receive greater targeting priority?
4. Can a pre-call model rank customers by subscription propensity?
5. Can campaign resources be allocated more efficiently using these rankings?

---

## Headline Results

| Finding | Result |
|---|---:|
| Baseline conversion | **11.3%** (4,639 of 41,188 contact records) |
| Previous campaign responders | **65.1%** conversion |
| Never-contacted customers | **8.8%** conversion |
| Previous responder lift | **5.8×** |
| Age 65+ conversion | **47.3%** |
| Under-25 conversion | **24.0%** |
| Student conversion | **31.4%** |
| Retired conversion | **25.3%** |
| Cellular conversion | **14.7%** |
| Fixed telephone conversion | **5.2%** |
| First-call conversion | **13.0%** |
| Tenth-call conversion | **3.6%** |
| K-Means clusters | **4** |
| Gradient boosting test ROC-AUC | **0.81** |
| Top-decile conversion | **53%** |
| Top-20% capture of subscribers | **~67%** |

---

## Analytical Approach

### 1. Data Understanding

- Inspected dataset dimensions and variable types
- Created a data dictionary
- Examined missing and unknown values
- Identified duplicate records
- Analyzed the target variable and class imbalance
- Reviewed distributions and potential outliers

### 2. Data Cleaning & Feature Engineering

- Removed exact duplicate records
- Retained `"unknown"` categories where missingness could itself contain information
- Converted `pdays = 999` into a meaningful never-contacted indicator
- Capped extreme campaign-contact counts
- Created age-band and job-group features
- Prepared a modeling dataset using information available before the current call

### 3. Exploratory Analysis

Conversion rates and relative lift were analyzed across:

- Age
- Job
- Education
- Marital status
- Loan/default status
- Contact channel
- Campaign month
- Number of campaign contacts
- Previous campaign outcome
- Customer contact history
- Selected macroeconomic variables

### 4. Customer Segmentation

Two approaches were used:

- Rule-based customer segments using demographic and campaign characteristics
- K-Means clustering using pre-call customer attributes

Four clusters were selected using clustering diagnostics.

The resulting silhouette score was modest (~0.18), so the clusters are interpreted as **overlapping customer profiles rather than sharply separated customer groups**.

### 5. Conversion Prediction

Three models were evaluated:

- Logistic Regression
- Random Forest
- Gradient Boosting

The final gradient boosting model achieved a test **ROC-AUC of approximately 0.81** using a random train/test split.

Model performance was evaluated using:

- ROC-AUC
- PR-AUC
- Decile lift
- Cumulative gains
- Permutation importance
- Profit simulation
- Time-forward robustness testing

### Why Call Duration Was Excluded

`duration` represents the duration of the current phone call.

It is only known **after the call has occurred**.

Using it to decide whom to contact would therefore introduce **data leakage** into a pre-call targeting model.

When call duration is included, model performance becomes artificially inflated.

---

## Key Business Insights

### 1. Previous campaign success is a strong targeting signal

Customers who previously responded positively to a campaign showed substantially higher subscription rates than customers without a successful previous interaction.

This suggests that previous campaign history can be useful when prioritizing future outreach.

### 2. Contact channel matters

Customers contacted through cellular communication showed substantially higher conversion than customers contacted through fixed telephone channels.

This supports prioritizing cellular outreach where operationally appropriate.

### 3. Repeated contacts show diminishing conversion

Conversion declines as the number of contacts during the current campaign increases.

This suggests that repeatedly contacting low-propensity customers may generate diminishing returns.

### 4. Customer segments differ substantially

Older customers, students, retired customers, and customers with favorable previous campaign histories show higher observed conversion rates.

These segments should be evaluated for targeted outreach rather than applying a uniform campaign strategy.

### 5. Model-based targeting can improve campaign efficiency

The predictive model ranks customers according to their estimated subscription propensity.

The highest-scoring customers show substantially higher observed conversion rates than the overall campaign population, demonstrating the potential value of prioritization.

---

## Illustrative Profit Scenario

An illustrative scenario was used to demonstrate the potential business value of targeted outreach.

Assumptions:

- Cost per call: **€10**
- Value of successful deposit: **€100**

Under these assumptions, prioritizing approximately the top 19% of model-ranked customers produces substantially higher estimated profit per call than contacting customers without prioritization.

**These values are illustrative only and should be replaced with the bank's actual campaign costs and deposit economics before operational use.**

---

## Model Robustness & Limitations

The random-split test ROC-AUC of approximately **0.81 should not be interpreted as expected future campaign performance**.

A time-forward validation approach, where earlier observations are used for training and the latest observations are used for testing, produced a lower ROC-AUC of approximately **0.64–0.67**.

This indicates potential temporal drift and highlights the importance of validating the model on more recent campaign data before deployment.

Other limitations include:

- The dataset covers a single historical period (2008–2010), which may limit its generalizability to current campaigns
- Economic variables may reflect the macroeconomic conditions of that period
- K-Means segmentation produced modest separation
- Observational relationships should not be interpreted as causal effects
- Profit estimates depend on illustrative assumptions
- The model requires validation on current campaign data before operational deployment

---

## Project Structure

```text
Bank-Marketing-Analysis/
│
├── data/
│   ├── bank_marketing.csv
│   └── bank_marketing_clean.csv
│
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_exploratory_analysis.ipynb
│   ├── 04_customer_segmentation.ipynb
│   └── 05_conversion_prediction.ipynb
│
├── outputs/
│   ├── figures/
│   └── tables/
│
├── report/
│   └── Bank_Marketing_Analysis.pdf
│
├── presentation/
│   └── Bank_Marketing_Analysis.pptx
│
├── requirements.txt
├── README.md
└── .gitignore