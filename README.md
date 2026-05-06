# Hierarchical Analysis of Structural Borrowing Costs in Texas Municipal Debt

## Project Overview

This project analyzes the structural drivers of borrowing costs in Texas municipal debt using exploratory data analysis, regression modeling, and hierarchical linear modeling. The goal is to determine whether borrowing costs are influenced only by broad market conditions or whether they are systematically shaped by pledge structure, issue purpose, issuance size, and the institutional identity of the issuing government entity.

The analysis focuses on the **Interest-to-Principal Ratio (IPR)** as the primary measure of long-term borrowing burden. A standard OLS model is compared against a **Hierarchical Linear Model (HLM)** to account for the fact that individual debt issuances are nested within broader government categories such as cities, counties, independent school districts, community college districts, and water districts.

---

## Research Objective

The primary objective of this project is to answer:

> To what extent do Pledge Type and Issue Purpose influence the Interest-to-Principal Ratio of Texas local debt, and how does this relationship vary across different Government Types?

Key research questions include:

- Do Revenue bonds carry a statistically significant borrowing-cost premium compared with General Obligation bonds?
- How much of the variation in borrowing cost is explained by Government Type?
- Does issuance size have a linear relationship with borrowing cost, or is there evidence of market saturation?
- Does a Hierarchical Linear Model provide better fit than a standard OLS regression model?

---

## Dataset

The dataset contains Texas local municipal debt issuance records with approximately **17,000+ debt issuances**.

Key variables include:

| Variable | Description |
|---|---|
| `government_type` | Category of issuing government entity |
| `government_name` | Name of the government entity |
| `issuer_name` | Issuer associated with the debt |
| `issuance_name` | Name of the debt issuance |
| `issue_purpose` | Purpose of the debt issuance |
| `closing_date` | Date of issuance closing |
| `last_principal_payment` | Final principal payment date |
| `fiscal_year` | Fiscal year of the debt record |
| `pledge_type` | Debt security type: GO, REV, LP |
| `total_principal_outstanding` | Outstanding principal amount |
| `total_interest_outstanding` | Outstanding interest amount |
| `total_debt_service_outstanding` | Total outstanding debt service |

---

## Methodology

The project follows a full data science and econometric workflow:

1. **Data Cleaning**
   - Standardized column names.
   - Converted date fields to proper date format.
   - Converted categorical variables into factors.
   - Removed duplicate records.
   - Removed zero-principal “zombie debt” records.

2. **Feature Engineering**
   - Created the Interest-to-Principal Ratio:

     ```text
     IPR = Total Interest Outstanding / Total Principal Outstanding
     ```

   - Created `log_principal` to reduce skewness in debt size.
   - Used `government_type` as the Level-2 grouping variable for hierarchical modeling.

3. **Exploratory Data Analysis**
   - Distribution of IPR.
   - IPR by Government Type and Pledge Type.
   - Relationship between issuance size and IPR.
   - Temporal trend in borrowing cost.
   - Issue Purpose and Pledge Type overlap.
   - GVIF multicollinearity diagnostics.

4. **Modeling**
   - Null Hierarchical Model for ICC estimation.
   - Baseline OLS regression.
   - Hierarchical Linear Model with Government Type as a random intercept.

5. **Model Evaluation**
   - AIC and BIC comparison.
   - RMSE comparison.
   - Residual diagnostics.
   - Random effects inspection.
   - Marginal effects interpretation.

---

## Model Specification

The final Hierarchical Linear Model is specified as:

```text
IPR_ij = β0 + β1(PledgeType_ij) + β2(IssuePurpose_ij) + β3(LogPrincipal_ij) + u_0j + ε_ij
