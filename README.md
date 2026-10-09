# 🏦 Bank Customer Churn & Retention Analysis

## Executive Summary

Customer churn is not only a retention problem—it is a **revenue, relationship, and balance-sheet exposure problem**.

This project analyzes **10,000 bank customers** to identify where churn is concentrated, which customer characteristics and behaviours distinguish churners from retained customers, and where targeted retention interventions can protect the greatest customer value.

The analysis combines **customer segmentation, behavioural cohort analysis, financial exposure analysis, and predictive modelling** to move from:

> **“Who is leaving?” → “Where is the risk concentrated?” → “What behaviours signal risk?” → “Which customers should the bank prioritize?”**

The project ultimately translates customer-level churn patterns into **actionable retention strategies and an ML-ready customer risk framework**.

---

## 🎯 Business Problem

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

# 📊 Dataset Overview

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

## 📈 2. Portfolio Retention Performance

The overall portfolio churn rate is **20.37%**, meaning approximately 1 in 5 customers has exited.

More importantly, churned customers account for approximately **$185.59M** in balances, representing **24.26%** of total portfolio balance.

This creates a critical distinction:

> Churn risk is disproportionately important from a financial-exposure perspective because churned customers represent a larger share of portfolio balance than their share of customers.

This makes retention a **value-protection problem**, not simply a customer-count problem.

---

## 👥 3. Demographic Risk Segmentation

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

## 🎯 4. Highest-Risk Customer Segments

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

## Machine Learning Strategy

The EDA establishes the business context for a predictive churn model.

The machine-learning workflow is designed to answer:

> **Which customers are most likely to churn, and how should the bank prioritize intervention?**

### Feature Engineering

Potential predictive features include:

- Age groups  
- Credit-score bands  
- Balance bands  
- Product depth  
- Customer activity  
- Credit-card ownership  
- Geography  
- Gender  
- Tenure  
- Financial exposure indicators  
- Behavioural interaction features  

Identifiers such as `CustomerId`, `RowNumber`, and `Surname` are excluded.

A **leakage audit** is performed before model training to ensure that features do not contain information derived from the target or future customer outcomes.

---

## Model Development

The modelling pipeline follows a reproducible preprocessing and evaluation framework.

### Candidate Models

- **Logistic Regression** — interpretable baseline  
- **Random Forest** — non-linear ensemble  
- **Gradient Boosting** — boosted tree model  
- **XGBoost / LightGBM** — where available  

### Evaluation Metrics

Because churn represents approximately 20% of the dataset, accuracy is **not** used as the primary success criterion.

Models are evaluated using:

- Precision  
- Recall  
- F1 Score  
- ROC-AUC  
- PR-AUC  
- Confusion Matrix  

The final classification threshold is optimized according to the business cost of missed churners versus unnecessary retention interventions.

---

##  Threshold Optimization

Rather than automatically using a 0.50 probability threshold, the model evaluates multiple thresholds to determine the most commercially appropriate operating point.

The objective is to balance:
```text
Churners Captured
        ↓
False Retention Alerts
        ↓
Intervention Capacity
        ↓
Financial Exposure Protected

```

This transforms the model from a purely statistical classifier into a **decision-support system**.

---

## 🔍 Model Explainability

Model interpretation will identify the variables contributing most strongly to predicted churn.

The analysis will use:

- Feature importance  
- Permutation importance  
- SHAP-based explanation (where appropriate)  
- Customer-level prediction drivers  

The objective is not only to predict churn but to explain:

> **Why does the model believe this customer is at risk?**

This enables the bank to connect predictions to appropriate retention actions.

---

## 💳 Customer Risk Scoring

The final model will produce a customer-level risk score.

### Example Framework

| Risk Tier  | Probability | Recommended Action       |
|------------|-------------|--------------------------|
| Critical   | Very High   | Immediate retention      |
| High       | High        | Targeted intervention    |
| Medium     | Moderate    | Engagement campaign      |
| Low        | Low         | Standard monitoring      |

Risk scoring can then be combined with financial exposure to prioritize customers using a framework such as:
 > **Churn Probability × Customer Balance = Retention Priority**



This ensures the bank does not simply target the customers most likely to churn, but prioritizes those where intervention could protect the **greatest value**.

---

## 💼 Business Impact Framework

The final ML evaluation will measure more than model accuracy.

Key business metrics include:

- High-risk customers identified  
- Actual churners captured  
- Churn recall  
- False-positive customers  
- Churned balance identified  
- High-value customers captured  
- Intervention coverage  
- Retention opportunity by risk tier  

The intended output is a **prioritized retention portfolio**, not merely a prediction label.

---

## 🎯 Recommended Retention Strategy

Based on the EDA, the bank should consider a differentiated retention framework:

1. **Protect High-Value At-Risk Customers**  
   Prioritize customers combining high balance, inactivity, and elevated churn risk.

2. **Reactivate Inactive Customers**  
   Use targeted digital engagement campaigns to bring inactive customers back into regular banking activity.

3. **Review Product Suitability**  
   The exceptionally high churn observed among 3–4 product customers suggests the need to investigate product fit, complexity, or customer-product mismatch.

4. **Use Selective Cross-Selling**  
   Two-product customers demonstrate substantially lower churn, while active single-product customers represent a potential cross-sell opportunity.

5. **Prioritize Geographic Risk**  
   Germany requires specific retention investigation given its materially higher churn rate.

6. **Use Predictive Risk Scoring**  
   Move from reactive churn reporting toward proactive customer-level intervention.

---

## 🏆 Executive Takeaway

The analysis demonstrates that bank churn is **not evenly distributed** across the customer base.

The greatest opportunities lie in identifying the intersection of:

> **Customer Risk × Behaviour × Product Relationship × Financial Exposure**

The strongest signals identified include:

- Age concentration  
- Geographic concentration  
- Product-depth effects  
- Customer inactivity  
- Financial exposure  

The resulting framework moves the analysis from:

> “20.37% of customers churned”

to:

> “Which customers are at risk, why they are at risk, how much value is exposed, and what intervention should happen next?”

That is the foundation for a scalable **customer retention analytics and predictive churn management framework**.

---

## 🛠️ Technology Stack

* **Analytics & Programming:** [ Python, Pandas, NumPy,]

* **Visualization:** [ Matplotlib, Seaborn]
  
* **Machine Learning:** [ Jupyter Notebook, Git & GitHub,]

# Author

**Akintunde Teslim Akinmuyiwa**  
Data Analyst | Procurement & Business Analytics Professional

### Core Focus
Data Analytics • SQL • Python • Power BI • Machine Learning • Procurement Analytics • Business Intelligence

**Portfolio:** [GitHub](https://github.com/)

---

## Project Value

This project demonstrates the ability to move beyond descriptive dashboards and build an **end-to-end analytical solution** that connects customer data to business decisions:

`Data` → `Diagnosis` → `Risk Segmentation` → `Financial Exposure` → `Retention Strategy` → `Predictive Modelling` → `Business Action`


