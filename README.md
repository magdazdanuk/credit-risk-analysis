# Retail Credit Risk Management & Provision What-If Simulator

## Executive Summary & Project Goal
This project analyzes a consumer loan portfolio from 2007–2015. Its objective is to identify key trends and relationships regarding:
* portfolio default rates 
* borrower risk profiles 
* collection effectiveness and capital recovery 
* profitability across credit rating tiers 

The main goal was to create an interactive BI tool and a stress-testing simulator that enable senior management and the credit risk team to quickly derive business insights and estimate capital reserve requirements in the event of an economic downturn.

---

## Main Clues & Analytical Findings

1. DTI as a Primary Risk Predictor:
   * Statistical testing confirms a highly significant difference (p < 0.001) in Debt-to-Income ratios between performing and defaulted borrowers. Higher DTI directly correlates with default spikes.

2. The High-Risk Pricing Paradox (Adverse Selection):
   * Grades A-B: Low interest rates (~7%-11%) paired with low default rates (~6%-13%) generate stable, low-provision cash flows.
   * Grades E-G: Elevated interest rates (~18%-25%+) do not compensate for default rates exceeding 30%-40% combined with low recovery rates (~12%-15%).
   * Economic Reality: A single default in Grade G wipes out the interest yield of ~4 fully paying loans in the same tier. High interest rates attract adverse selection (cash-strapped borrowers with limited refinancing options).

3. Income vs. DTI Illusion:
   * High annual income alone does not protect against default if the borrower carries an excessively high DTI ratio.

---

## Recommendations

1. Underwriting Policy & DTI Hard Caps:
   * Enforce an automated hard cutoff at DTI > 25% for credit rating grades below C.

2. Risk Appetite Adjustment on High-Risk Tiers:
   * Cease or heavily restrict uncollateralized origination in Grades F and G, or mandate co-signers/collateral guarantees.

3. Shift to Upfront Origination Fees:
   * Replace reliance on nominal high interest rates in high-risk categories with upfront Origination Fees collected at disbursement, locking in capital protection immediately.

4. Macro Scenario Capital Buffer:
   * Utilize the What-If Provision Simulator during annual ICAAP/CCAR stress testing to lock in extra capital reserves (Provision Delta) prior to projected economic downturns.
  
---

## Realization of Project

1. Requirement Gathering & Problem Definition: Defined key risk metrics and business goals for retail credit assessment.
2. Data Pipeline & Python Preprocessing: Cleaned the raw dataset, imputed missing values, engineered emp_length_years, and created target classification variables.
3. Statistical Hypothesis Testing: Conducted two-sample tests in Python to validate DTI and Income as statistically significant drivers of default risk.
4. Data Modeling & DAX Metric Development: Imported processed data into Power BI, set up relationships, and authored core DAX measures (Total Portfolio Volume, Default Rate %, Avg Recovery Rate %).
5. Dashboard UI/UX Construction: Built Page 1 (Executive KPIs) and Page 2 (Borrower Demographics), resolving sorting issues (emp_length) and applying Top-N filtering.
6. What-If Parameter & Stress Model Engine: Generated numeric range parameters in Power BI (Default Stress %, Recovery Stress %) and wrote advanced DAX variables using FORMAT for dynamic million-dollar cards.
7. Business Strategy & Documentation Translation: Translated statistical and financial outputs into executive recommendations and compiled complete documentation.

---

## Tech Stack & Tools
* Data Processing & ETL: Python (Pandas, NumPy) - Data cleaning, type casting, missing value imputation, and feature engineering.
* Data Visualization & BI: Power BI Desktop - Custom DAX metrics, dynamic What-If parameters, hierarchical sorting, and interactive filtering.
* Statistical Analysis: Hypothesis testing for continuous risk drivers.
* Domain Knowledge: Retail Credit Risk, Loss Given Default, Expected Loss, Risk-Adjusted Return on Capital, and Impairment Provisioning.
