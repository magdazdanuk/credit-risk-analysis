# Retail Credit Risk Management & Provision What-If Simulator

## Executive Summary & Project Goal
* Project Goal: Design an end-to-end Credit Risk Analytics dashboard and stress-testing provision simulator using real-world peer-to-peer lending data (LendingClub dataset). The objective is to evaluate borrower risk profiles, measure portfolio default rate, assess recovery performance, and model capital reserve requirements under macroeconomic stress scenarios.
* Timeline / Context: Developed as a comprehensive risk analytics portfolio project (2026) showcasing statistical hypothesis testing, financial modeling in DAX, and executive-level Business Intelligence reporting.

---

## Tech Stack & Tools
* Data Processing & ETL: Python (Pandas, NumPy) - Data cleaning, type casting, missing value imputation, and feature engineering.
* Data Visualization & BI: Power BI Desktop - Custom DAX metrics, dynamic What-If parameters, hierarchical sorting, and interactive filtering.
* Statistical Analysis: Hypothesis testing (Welch's t-test, Mann-Whitney U) for continuous risk drivers (DTI, Annual Income).
* Domain Knowledge: Retail Credit Risk, Loss Given Default (LGD), Expected Loss (EL), Risk-Adjusted Return on Capital (RAROC), and Impairment Provisioning.

---

## Explanation of Whole Process of Data Cleaning & Preparation

1. Target Variable Standardisation:
   * Isolated completed loan statuses into binary risk buckets: Performing (Fully Paid) vs. Defaulted (Charged Off).
   * Excluded in-flight loans (Current, In Grace Period) to prevent target leakage in default rate calculations.

2. Feature Extraction & Numerical Conversion:
   * emp_length: Parsed string expressions (e.g., "< 1 year", "10+ years") into a continuous integer column emp_length_years (0 to 10) to enable logical chronological sorting on visuals.
   * term: Extracted integer values (36, 60) from text strings for duration-based segmentations.
   * int_rate & revol_util: Cleaned percentage characters and converted to numeric float format.

3. Outlier & Skewness Handling:
   * Analyzed heavy right-skewness in annual_inc. Applied log-transformations for statistical testing while relying on median income for reporting baselines.
   * Filtered out invalid entries in categorical variables like home_ownership (NONE, ANY) to focus on primary credit profiles (RENT, MORTGAGE, OWN).

4. Derivation of Credit Risk Metrics:
   * Default Rate % = Total Charged Off Loans / Total Originated Loans
   * Avg Recovery Rate % = Total Recoveries / Total Charged Off Principal
   * Expected Loss (EL) = Portfolio Volume * Default Rate % * (1 - Recovery Rate %)

---

## Dashboard Architecture (Deep Dive)

The Power BI report is structured into 3 core interactive pages:

### Page 1: Executive Overview (KPIs & Macro Risk)
* Summary Cards: Total Portfolio Volume ($1.87B), Total Loans Count (254.19K), Average Annual Income ($72.51K), Overall Default Rate (18%).
* Portfolio Slicers: Loan Term (36 vs. 60 months slider) and Verification Status (Verified, Source Verified, Not Verified).
* Geographic Table: Risk Distribution by US State comparing regional volume against default percentages.

### Page 2: Borrower Demographics & Risk Profile
* Default Rate by Employment Length: Chronologically sorted column chart (< 1 year to 10+ years) exposing default rates across tenure.
* Default Rate by Loan Purpose: Filtered horizontal bar chart isolating the Top 7 highest-risk loan purposes (e.g., Small Business, Moving, Medical).
* Default Rate by Home Ownership: Cleaned bar visual comparing default rates across RENT, OWN, and MORTGAGE borrowers.

### Page 3: What-If / Provision Simulator (Stress Testing)
* Scenario Controls (What-If Parameters):
  * Default Stress % Slider (Range: -10% to +20% adjustment to baseline default rate).
  * Recovery Stress % Slider (Range: -30% to +10% adjustment to collection efficiency).
* Dynamic Capital Cards (DAX Formatted in Millions):
  * Baseline Expected Loss (Pre-stress loss exposure).
  * Stressed Provision Required (New required loss reserve under stress).
  * Provision Delta (Stressed Provision Required - Baseline Expected Loss capital buffer impact).
  * Stressed Default Rate % (Recalculated portfolio default rate).
* Comparative Risk Visual: Clustered Column Chart displaying Baseline Expected Loss vs. Stressed Provision Required across Credit Risk Grades (A through G).

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

## Strategic Recommendations for Senior Management

1. Underwriting Policy & DTI Hard Caps:
   * Enforce an automated hard cutoff at DTI > 25% for credit rating grades below C.

2. Risk Appetite Adjustment on High-Risk Tiers:
   * Cease or heavily restrict uncollateralized origination in Grades F and G, or mandate co-signers/collateral guarantees.

3. Shift to Upfront Origination Fees:
   * Replace reliance on nominal high interest rates in high-risk categories with upfront Origination Fees collected at disbursement, locking in capital protection immediately.

4. Macro Scenario Capital Buffer:
   * Utilize the What-If Provision Simulator during annual ICAAP/CCAR stress testing to lock in extra capital reserves (Provision Delta) prior to projected economic downturns.

---

## Realization of Project: Step-by-Step Execution List

1. Requirement Gathering & Problem Definition: Defined key risk metrics (Default Rate, LGD, EL, Provisioning) and business goals for retail credit assessment.
2. Data Pipeline & Python Preprocessing: Cleaned the raw dataset, imputed missing values, engineered emp_length_years, and created target classification variables.
3. Statistical Hypothesis Testing: Conducted two-sample tests in Python to validate DTI and Income as statistically significant drivers of default risk.
4. Data Modeling & DAX Metric Development: Imported processed data into Power BI, set up relationships, and authored core DAX measures (Total Portfolio Volume, Default Rate %, Avg Recovery Rate %).
5. Dashboard UI/UX Construction: Built Page 1 (Executive KPIs) and Page 2 (Borrower Demographics), resolving sorting issues (emp_length) and applying Top-N filtering.
6. What-If Parameter & Stress Model Engine: Generated numeric range parameters in Power BI (Default Stress %, Recovery Stress %) and wrote advanced DAX variables using FORMAT for dynamic million-dollar cards.
7. Business Strategy & Documentation Translation: Translated statistical and financial outputs into executive recommendations and compiled complete documentation.
