#  Bank Customer Churn & Retention Analysis

## Executive Summary

Customer churn is not only a retention problem—it is a **revenue, relationship, and balance-sheet exposure problem**.

This project analyzes **10,000 bank customers** to identify where churn is concentrated, which customer characteristics and behaviours distinguish churners from retained customers, and where targeted retention interventions can protect the greatest customer value.

The analysis combines **customer segmentation, behavioural cohort analysis, financial exposure analysis, and predictive modelling** to move from:

> **“Who is leaving?” → “Where is the risk concentrated?” → “What behaviours signal risk?” → “Which customers should the bank prioritize?”**

The project ultimately translates customer-level churn patterns into **actionable retention strategies and an ML-ready customer risk framework**.

---

## Business Problem

A bank experiencing a ~20% customer churn rate needs more than a headline churn percentage.

Management needs to understand:

* Which customer segments have the highest churn risk?
* Where is churn concentrated geographically and demographically?
* Which behavioural patterns distinguish churners from retained customers?
* How much customer balance is exposed to churn?
* Does product depth reduce or increase retention risk?
* How strongly does customer engagement influence churn?
* Which customers should receive retention interventions first?
* How can predictive modelling support proactive retention?

The objective is therefore to transform raw customer data into a **prioritized retention strategy** rather than simply reporting churn statistics.

---

# Dataset Overview

| Metric                   |          Value |
| ------------------------ | -------------: |
| Customers                |     **10,000** |
| Features                 |         **14** |
| Churned Customers        |      **2,037** |
| Retained Customers       |      **7,963** |
| Churn Rate               |     **20.37%** |
| Retention Rate           |     **79.63%** |
| Active Member Rate       |     **51.51%** |
| Multi-Product Rate       |     **49.16%** |
| Average Balance          | **$76,485.89** |
| Average Credit Score     |     **650.53** |
| Total Portfolio Balance  |   **$764.86M** |
| Churned Customer Balance |   **$185.59M** |
| Balance Exposure         |     **24.26%** |

### Dataset Schema

| Feature           | Description                   |
| ----------------- | ----------------------------- |
| `CreditScore`     | Customer credit score         |
| `Geography`       | Customer country              |
| `Gender`          | Customer gender               |
| `Age`             | Customer age                  |
| `Tenure`          | Years with the bank           |
| `Balance`         | Account balance               |
| `NumOfProducts`   | Number of bank products held  |
| `HasCrCard`       | Credit card ownership         |
| `IsActiveMember`  | Customer engagement indicator |
| `EstimatedSalary` | Estimated annual salary       |
| `Exited`          | Churn indicator / target      |

Identifiers such as `RowNumber`, `CustomerId`, and `Surname` were treated as non-predictive identifiers and excluded from modelling.

---

## 1. Data Quality & Validation

Before analysing churn behaviour, the dataset was subjected to structural and logical validation.

### Validation Results

* 10,000 rows × 14 columns
* 0 missing values
* 0 fully duplicated records
* 0 duplicate Customer IDs
* Valid ranges confirmed for:
  * Credit Score
  * Age
  * Tenure
  * Balance
  * Number of Products
  * Estimated Salary
* Binary variables were validated for expected values.

### Data Quality Conclusion

> The dataset is structurally clean and fully populated, providing a reliable foundation for segmentation and predictive modelling.

---

## 2. Portfolio Retention Performance

The overall portfolio churn rate is **20.37%**, meaning approximately 1 in 5 customers has exited.

More importantly, churned customers account for approximately **$185.59M** in balances, representing **24.26%** of total portfolio balance.

This creates a critical distinction:

> Churn risk is disproportionately important from a financial-exposure perspective because churned customers represent a larger share of portfolio balance than their share of customers.

This makes retention a **value-protection problem**, not simply a customer-count problem.

---

## 3. Demographic Risk Segmentation

### Age

| Age Group | Customers | Churn Rate |
| --------- | --------: | ---------: |
| 18–25     |       611 |      7.53% |
| 26–35     |     3,542 |      8.50% |
| 36–45     |     3,736 |     19.62% |
| **46–55** | **1,311** | **50.57%** |
| **56–65** |   **536** | **48.32%** |
| 66+       |       264 |     13.26% |

**Key Insight**

Churn is strongly concentrated among mid-to-late career customers, particularly the **46–65** age groups, where churn approaches 50%.

This makes age one of the strongest demographic risk differentiators in the portfolio.

---

### Gender

| Gender | Customers | Churn Rate |
| ------ | --------: | ---------: |
| Female |     4,543 | **25.07%** |
| Male   |     5,457 | **16.46%** |

Female customers exhibit a materially higher observed churn rate than male customers.

However, gender should be treated as a **segmentation signal rather than an intervention mechanism**; retention actions should focus primarily on behavioural and financial characteristics.

---

### Geography

| Geography   | Customers | Churn Rate | Total Balance |
| ----------- | --------: | ---------: | ------------: |
| **Germany** |     2,509 | **32.44%** |      $300.40M |
| Spain       |     2,477 |     16.67% |      $153.12M |
| France      |     5,014 |     16.15% |      $311.33M |

**Key Insight**

Germany represents the clearest geographic concentration of churn risk, with churn approximately **2×** the rate observed in France and Spain.

Because Germany also carries approximately **$300M** in customer balances, this is both a **retention-risk and financial-exposure priority**.

---

## 4. Highest-Risk Customer Segments

Combining Geography × Gender × Age reveals concentrated pockets of risk.

| Segment                  | Customers | Churn Rate | Balance |
| ------------------------ | --------: | ---------: | ------: |
| Germany · Female · 46–55 |       215 | **68.84%** | $25.91M |
| Germany · Female · 56–65 |        77 | **67.53%** |  $9.47M |
| Germany · Male · 46–55   |       188 | **64.89%** | $22.73M |
| Germany · Male · 56–65   |        71 | **61.97%** |  $8.77M |
| France · Female · 56–65  |       117 | **56.41%** |  $7.87M |

**Strategic Finding**

> Churn is not evenly distributed across the portfolio. It is heavily concentrated in specific demographic-geographic segments, particularly German customers aged 46–65.

This supports **targeted retention campaigns** rather than broad, portfolio-wide interventions.

---

##  5. Financial Risk & Balance Exposure

The portfolio contains approximately **$764.86M** in total customer balances.

Churned customers represent:

* **$185.59M**
* **24.26%** of total portfolio balance

The highest balance band also shows elevated churn:

| Balance Band | Customers | Churn Rate |
| ------------ | --------: | ---------: |
| Zero         |     3,617 |     13.82% |
| 1–50K        |        75 |     34.67% |
| 50–100K      |     1,509 |     19.88% |
| 100–150K     |     3,830 |     25.77% |
| 150–200K     |       935 |     21.93% |
| **200K+**    |    **34** | **55.88%** |

Approximately **$88.65M** of churned balance falls within the identified high-value exposure group.

**Business Implication**

> Retention prioritization should consider both probability of churn and financial exposure.

A high-risk customer with substantial balance represents a materially different business priority from a high-risk customer with little or no balance.

---

## 6. Product Depth & Churn

Product ownership reveals one of the strongest non-linear relationships in the analysis.

| Product Cohort      | Customers | Churn Rate |
| ------------------- | --------: | ---------: |
| Single Product      |     5,084 | **27.71%** |
| Core Cross-Sell — 2 |     4,590 |  **7.58%** |
| Over-Extended — 3–4 |       326 | **85.89%** |

**Key Finding**

The relationship between product depth and churn is not simply “more products = lower churn.”

Instead:

> Two-product customers exhibit substantially lower churn, while customers holding 3–4 products show extremely high churn.

This suggests a potential **product-depth tipping point**:

```text
1 Product
    ↓
High churn

2 Products
    ↓
Strongest retention profile

3–4 Products
    ↓
Very high churn
```

The result suggests that relationship depth may improve retention up to an optimal level, after which excessive product complexity or customer-product mismatch may become associated with higher churn.

This finding warrants deeper product suitability analysis rather than indiscriminate cross-selling

## 7. Customer Engagement — The Strongest Actionable Signal

Customer activity shows a substantial churn difference.

| Activity Status | Customers | Churn Rate |
|-----------------|-----------|------------|
| Active          | 5,151     | 14.27%     |
| Inactive        | 4,849     | 26.85%     |

Inactive customers therefore show an observed churn rate approximately **12.6 percentage points** higher than active customers.

This becomes even more informative when combined with credit-card ownership.

---

## 8. Digital Membership Shield

| Behavioural Cohort              | Customers | Churn Rate |
|---------------------------------|-----------|------------|
| Active + Card — Shielded        | 3,607     | 13.36%     |
| Active + No Card                | 1,544     | 16.39%     |
| Inactive + Card                 | 3,448     | 27.32%     |
| Inactive + No Card — Exposed    | 1,401     | 25.70%     |

The gap between the exposed and shielded cohorts is approximately

**12.3 percentage points**.

### Strategic Finding

Customer engagement is a more actionable retention signal than card ownership alone.

Credit-card ownership does not independently eliminate churn risk. Its value becomes more meaningful when combined with active customer engagement.

This points toward **digital reactivation and engagement-building strategies** as potentially stronger retention levers than simply increasing card ownership.

> These are observed associations in the dataset and should not be interpreted as causal effects.

---

## 9. Behavioural Risk Framework

The EDA was translated into actionable customer groups rather than stopping at descriptive statistics.

| Retention Priority          | Customer Profile                          | Churn Rate |
|-----------------------------|-------------------------------------------|------------|
| Product Suitability Review  | 3–4 products                              | 80.28%     |
| Priority Retention          | High-value + single-product + inactive    | 33.45%     |
| Card Activation Push        | Inactive + card                           | 25.79%     |
| Digital Reactivation        | Inactive + no card                        | 24.77%     |
| Relationship Expansion      | Active + single-product                   | 18.92%     |
| Standard Monitoring         | Lower-risk customers                      | 5.56%      |

### Business Interpretation

The analysis suggests that retention strategy should be **risk-tiered** rather than one-size-fits-all.

Different customer profiles require different interventions:

- **High-risk / high-value** → proactive retention  
- **Inactive customers** → digital reactivation  
- **Inactive cardholders** → card/channel activation  
- **Single-product active customers** → selective cross-sell  
- **3–4 product customers** → product suitability review  
- **Low-risk customers** → standard monitoring  

---

##  10. Key EDA Findings

**01 — Churn is financially material**  
20.37% of customers churned, but those customers represented 24.26% of portfolio balances, exposing approximately **$185.59M**.

**02 — Risk is highly concentrated**  
Germany records 32.44% churn, while several Germany × Age segments exceed 60% churn.

**03 — Age is a major risk differentiator**  
Customers aged 46–65 show churn rates close to 50%, compared with less than 9% among customers aged 18–35.

**04 — Product depth has a tipping point**  
Two-product customers show only 7.58% churn, compared with 27.71% for single-product customers and 85.89% among 3–4 product customers.

**05 — Engagement is highly actionable**  
Inactive members churn at 26.85%, versus 14.27% for active members.

**06 — Behavioural combinations outperform isolated indicators**  
The strongest retention signals emerge when activity, product depth, card ownership, age, geography, and financial exposure are evaluated together.

---

# Machine Learning Pipeline

## Feature Engineering & Leakage Audit

- Dropped pure identifiers (`RowNumber`, `CustomerId`, `Surname`) — no predictive value, leakage/PII risk.
- Dropped EDA-only derived groupings (`AgeGroup`, `BalanceBand`, etc.) in favor of their underlying raw numeric/categorical fields, to avoid redundant double-counting of the same signal.
- Engineered 6 new features (`BalanceSalaryRatio`, `IsZeroBalance`, `TenureAgeRatio`, `IsOverExtended`, `EngagementScore`, `CreditPerAge`) — confirmed none derive from or leak the target.
- Final model used **16 features**, one-hot encoded for Geography and Gender.

## Train/Test Strategy

- **Stratified 80/20 split** — preserves the ~20% churn rate in both train and test sets.
- Class imbalance (~3.9:1 retained:churned) handled via `class_weight='balanced'` (Logistic Regression, Random Forest) and sample weighting (Gradient Boosting) — rather than synthetic resampling (SMOTE) — to keep the test set's real-world distribution intact for honest evaluation.

## Models Trained

| Model | Notes |
|-------|-------|
| Logistic Regression | Baseline linear model, `class_weight='balanced'` |
| Random Forest | 300 trees, `class_weight='balanced'` |
| Gradient Boosting | 200 estimators, sample-weighted for imbalance |

> **Note:** XGBoost was evaluated during development but excluded from the final pipeline after causing repeated local kernel crashes (an environment-level OpenMP/binary compatibility issue, not a data or methodology problem). The three-model comparison above already spans a linear model and two different ensemble/boosting strategies, which is sufficient for a robust, defensible comparison.

---

## 🏆 Model Selection: Best Model & Evaluation Method

**Evaluation Method:** All three models were scored on the held-out 20% test set (never seen during training) across five metrics — Precision, Recall, F1, ROC-AUC, and PR-AUC.

**PR-AUC (Precision-Recall Area Under Curve)** was used as the primary, deciding metric — not raw accuracy and not ROC-AUC. With ~20% class imbalance, ROC-AUC can look artificially strong because it's diluted by a large number of easy true negatives. PR-AUC instead measures directly how well a model identifies the minority (churn) class without flooding Retention with false alerts — exactly the trade-off that matters for a real outreach campaign.

### Test Set Results (ranked by PR-AUC)

| Model | Precision | Recall | F1 | ROC-AUC | PR-AUC |
|-------|-----------|--------|-----|---------|--------|
| 🏆 Gradient Boosting | 0.51 | 0.76 | 0.61 | 0.87 | 0.71 |
| Random Forest | 0.54 | 0.70 | 0.61 | 0.86 | 0.70 |
| Logistic Regression | 0.44 | 0.75 | 0.56 | 0.84 | 0.64 |

**Selected Model:** Gradient Boosting — highest PR-AUC (0.71) and highest ROC-AUC (0.87) of the three, with a strong recall/precision balance at the default threshold. Random Forest was a close second; Logistic Regression, as expected for a linear baseline, trailed both ensemble methods.

> **Note:** due to the stochastic nature of ensemble training, Random Forest and Gradient Boosting occasionally trade places by a very small margin on re-runs — the notebook always dynamically selects and reports whichever model wins PR-AUC on that run, rather than hardcoding a winner.

---

## 🎚 Threshold Optimization

The default 0.5 classification threshold is a statistical convention, not a business decision. Because missing a churner (false negative) costs the bank far more than one wasted retention offer (false positive), the threshold was re-optimized to favor recall, subject to a minimum precision floor of 35% (so outreach campaigns aren't mostly wasted effort).

| | Default (0.5) | Business-Optimal (0.30) |
|--|---------------|-------------------------|
| Precision | ~0.51 | 0.351 |
| Recall | ~0.76 | 0.892 |
| F1 | ~0.61 | 0.504 |

At the business-optimal threshold, the model captures **89.2%** of actual churners in the test set — up from 76% at the default threshold — a deliberate trade of some precision for substantially more coverage of at-risk customers.

---

## 🔍 Model Explainability (SHAP)

Beyond feature importances, SHAP (SHapley Additive exPlanations) was used to:

- Produce a global summary plot showing which features push predictions toward churn vs. retention across the whole test sample.
- Generate a customer-level force plot for an example high-risk customer, showing exactly which features drove that individual's score — giving relationship managers a concrete talking point for every flagged customer, not just a black-box probability.

---

## 💰 Business Impact

Every customer was scored with the final model and assigned a risk tier:

| Risk Tier | Customers | Avg. Churn Probability | Actual Churn Rate | Financial Exposure |
|-----------|-----------|------------------------|-------------------|--------------------|
| 🔴 Critical | 1,676 | 85.4% | 70.8% | $130.4M |
| 🟠 High | 1,779 | 56.1% | 28.0% | $97.1M |
| 🟡 Medium | 2,363 | 33.9% | 10.1% | $74.3M |
| 🟢 Low | 4,182 | 12.7% | 2.7% | $36.6M |

### Deployment Impact (test set, business-optimal threshold)

| Metric | Value |
|--------|-------|
| High-Risk Customers Flagged | 1,033 (of 2,000 test customers) |
| Actual Churners Captured | 363 of 407 |
| Churn Capture Rate (Recall) | 89.2% |
| Balance Exposure Captured | $32,996,296 |
| False Alerts (wasted outreach) | 670 |
| Intervention Efficiency (Precision) | 35.1% |

**Bottom line:** at the business-optimal threshold, the model would flag roughly **51.6%** of the customer base for outreach and capture **~89%** of would-be churners within that group — a targeted, ranked, dollar-weighted list that is dramatically more efficient than a blanket campaign to all 10,000 customers.

## Technology Stack

* **Analytics & Programming:** [ Python, Pandas, NumPy,]

* **Visualization:** [ Matplotlib, Seaborn]
  
* **Machine Learning:** [ Jupyter Notebook, Git & GitHub,]

# Author

**Akintunde Teslim Akinmuyiwa**  
Data Analyst | Procurement & Business Analytics Professional

### Core Focus
Data Analytics • SQL • Python • Power BI • Machine Learning • Procurement Analytics • Business Intelligence

---

## Project Value

This project demonstrates the ability to move beyond descriptive dashboards and build an **end-to-end analytical solution** that connects customer data to business decisions:

`Data` → `Diagnosis` → `Risk Segmentation` → `Financial Exposure` → `Retention Strategy` → `Predictive Modelling` → `Business Action`


