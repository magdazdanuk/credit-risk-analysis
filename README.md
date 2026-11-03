# Retail Credit Risk Management & Provision What-If Simulator

## Executive Summary & Project Goal
This project analyzes a consumer loan portfolio from 2007–2015. Its objective is to identify key trends and relationships regarding:
* portfolio default rates 
* borrower risk profiles 
* collection effectiveness and capital recovery 
* profitability across credit rating tiers 

The main goal was to create an interactive BI tool and a stress-testing simulator that enable senior management and the credit risk team to quickly derive business insights and estimate capital reserve requirements in the event of an economic downturn.

---

## Main Clues 

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

## System Architecture & Workflow

1. **ETL (Python):** Cleanses, standardizes, and transforms raw peer-to-peer lending data, handling missing values, right-skewed outliers, and numerical type conversions.
2. **Feature Engineering & Target Encoding (Python):** Isolates completed loans to prevent target leakage, defines binary credit risk buckets (Performing vs. Defaulted), and engineers continuous chronological variables for downstream risk modeling.
3. **Analytics & Transformation (DAX):** Leverages DAX formulas. Drives exploratory data analysis to validate business hypotheses and confirms the adverse selection effect in high-risk tiers.
4. **Business Intelligence & Forecasting (Power BI):** Visualizes credit risk KPIs, demographic distributions, and deploys an interactive What-If stress-testing simulator to forecast capital reserve requirements under macroeconomic stress scenarios.

## Python

### 1. Feature Selection & Target Encoding
To accurately calculate default rates, the dataset was filtered to include only completed loan lifecycles. In-flight loans were excluded to prevent target leakage. A binary risk flag (`is_default`) was engineered to streamline DAX calculations in Power BI.

```python
# 1. Isolate completed loans to prevent target leakage
df_closed = df_sub[
    df_sub["loan_status"].isin(["Fully Paid", "Charged Off", "Default"])
].copy()

# 2. Engineer binary target variable (1 = Default, 0 = Performing)
df_closed["is_default"] = (
    df_closed["loan_status"].isin(["Charged Off", "Default"]).astype(int)
)
```
### 2. Change data type
Below are preview screenshots of the interactive Power BI dashboard.
```python
# Convert percentage strings to strict numeric formats
for col in ["int_rate", "revol_util"]:
    df_closed[col] = df_closed[col].astype(str).str.replace("%", "").str.strip()
    df_closed[col] = pd.to_numeric(df_closed[col], errors="coerce")

# Extract integer duration from text (e.g., "36 months" -> 36)
df_closed["term_months"] = df_closed["term"].str.extract(r"(\d+)").astype(float)

# Standardize employment length using a predefined dictionary map (emp_map)
# Example: "< 1 year" -> 0, "10+ years" -> 10
df_closed["emp_length_years"] = df_closed["emp_length"].astype(str).str.strip().map(emp_map)
```

## Dashboard & Visual Reports

### Page 1: Executive Portfolio Overview


## Tech Stack & Tools
* Data Processing & ETL: Python (Pandas, NumPy) - Data cleaning, type casting, missing value imputation, and feature engineering.
* Data Visualization & BI: Power BI Desktop - Custom DAX metrics, dynamic What-If parameters, hierarchical sorting, and interactive filtering.
* Statistical Analysis: Hypothesis testing for continuous risk drivers.
* Domain Knowledge: Retail Credit Risk, Loss Given Default, Expected Loss, Risk-Adjusted Return on Capital, and Impairment Provisioning.
