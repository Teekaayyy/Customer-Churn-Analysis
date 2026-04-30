# Project 03: Customer Churn Analysis
### Predicting Who Leaves, When They Leave, and Why It Matters Most

---

## Business Brief

Acquiring a new customer costs 5 to 7 times more than retaining an existing one. Yet most businesses only react to churn after it happens.

This project builds a churn prediction system for a telecom company and answers three questions:

1. Who is going to churn? (predictive model with probability scores)
2. Why are they churning? (feature importance analysis)
3. Who matters most? (high-value at-risk customer identification)

---

## Dataset

| Property | Detail |
|----------|--------|
| **Name** | Telco Customer Churn |
| **Direct Link** | https://www.kaggle.com/datasets/blastchar/telco-customer-churn |
| **Records** | 7,043 customers, 21 features |
| **Target** | Churn: Yes / No |
| **Industry** | Telecommunications |

---

## What Makes This Different

- Uses **SQL** for all exploratory queries before any modelling
- Builds both **Logistic Regression** and **Random Forest** and compares them properly
- Goes beyond accuracy: evaluates using **ROC-AUC, precision, recall, F1, and confusion matrix**
- Assigns a **continuous churn probability score** to every customer
- Introduces the **Priority Intervention Matrix**: cross-referencing churn risk with monthly spend to identify the customers worth fighting for most
- Feature importance communicated as a business story, not just a coefficient table

---

## Project Structure

```
customer-churn-analysis/
├── project_03_churn_analysis.ipynb
└── README.md
```

---

## Kaggle Setup

1. Search **"telco-customer-churn"** by *blastchar* on Kaggle and attach it
2. Upload `project_03_churn_analysis.ipynb`
3. Run the path finder cell first
4. Update `DATA_PATH` in Cell 3 if needed
5. Run all cells

---

## Notebook Walkthrough

### Section 1: Setup
Libraries, colour system. Red = churn, green = retained, indigo = model metrics. Consistent throughout.

### Section 2: Load and Audit
Loads the dataset. Full null audit with dtypes, unique counts. Churn distribution with percentages.

### Section 3: Cleaning and Preprocessing
Four documented cleaning steps. Key one: TotalCharges is stored as a string with spaces instead of nulls for new customers, requiring `pd.to_numeric` with `errors=coerce` before filling.

### Section 4: SQL Exploratory Analysis
Five SQL queries run before any model is touched:
- Churn rate by contract type
- Churn rate by internet service
- Churn rate by payment method
- Churn rate by tenure bucket (0-12, 13-24, 25-48, 49+ months)
- High-value customer churn rate (MonthlyCharges > 70)

### Section 5: Visual Exploratory Analysis
Two chart panels:
- 6-panel categorical churn rates (colour-coded: red = above average, amber = moderate, green = below average)
- 3-panel numeric distributions comparing churned vs retained customers on tenure, MonthlyCharges, and TotalCharges

### Section 6: Churn Prediction Model
- Binary encoding of all categorical features
- One-hot encoding for Contract, InternetService, PaymentMethod
- 80/20 stratified train/test split
- StandardScaler applied
- Logistic Regression with class_weight=balanced (handles class imbalance)
- Random Forest as a benchmark
- Both evaluated on ROC-AUC, precision, recall, F1

### Section 7: Model Evaluation Dashboard
3-panel chart:
- Confusion matrix heatmap
- ROC curve comparison (both models on the same axes)
- Predicted probability distribution (churned vs retained)

### Section 8: What Actually Drives Churn?
Top 20 feature coefficients from Logistic Regression visualised as a horizontal bar chart. Positive = increases churn risk (red), negative = reduces churn risk (green). Printed summary of top 10 drivers and top 10 reducers.

### Section 9: The Finding Most Analyses Miss
Priority Intervention Matrix. Every customer scored and placed into one of four quadrants:

| Quadrant | Action |
|----------|--------|
| High Risk + High Value | Immediate intervention |
| High Risk, Low Value | Low-cost win-back only |
| Low Risk, High Value | Loyalty programme |
| Low Risk, Low Value | Minimal investment |

Interactive Plotly scatter with threshold lines showing the quadrant boundaries. Revenue at immediate risk calculated for the top-left quadrant.

### Section 10: Executive Summary Dashboard
Dark-theme KPI card dashboard showing: total customers, churn rate, model ROC-AUC, recall score, high-risk customer count, and monthly revenue at immediate risk.

### Section 11: Findings and Recommendations
Six findings mapped to evidence. Five business recommendations with reasoning.

---

## Key Findings

| Finding | Evidence |
|---------|----------|
| Month-to-month contracts churn at ~3x the rate of annual subscribers | SQL contract analysis |
| Fibre optic customers churn more despite paying more | SQL internet service query |
| Electronic check payment has the highest churn rate of any payment method | SQL payment analysis |
| Short-tenure customers (0-12 months) are the highest-risk group | Tenure bucket query |
| Lack of online security and tech support are the top churn drivers | Feature importance chart |
| A high-value at-risk segment exists whose loss represents significant monthly revenue | Priority matrix |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Python (Pandas, NumPy) | Data manipulation |
| SQLite3 | Exploratory SQL queries |
| Scikit-learn | Logistic Regression, Random Forest, preprocessing, metrics |
| Plotly | Interactive scatter, ROC curves |
| Seaborn + Matplotlib | Confusion matrix, distributions, feature importance |

---

*Built by Jessica Dan-Odhomo - [LinkedIn](https://www.linkedin.com/in/jessica-dan-odhomo) - [GitHub](https://github.com/Teekaayyy)*
