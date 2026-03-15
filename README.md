# Capstone Project - Industry Growth Prediction
**AI + ML | Berkeley Engineering | Berkeley Haas**  
**Author:** Graycloud Rios  
**Capstone - Final Submission**

---

## Project Summary

This project predicts whether a California industry will **grow (1)** or **decline (0)** in employment in the next year using historical labor market data from the California EDD Labor Market Information Division (LMID).

**Task:** Binary Classification  
**Primary Metric:** F1-Score  
**Best Model:** Gradient Boosting - F1 = 0.7231, ROC-AUC = 0.7563  
**Baseline (from EDA):** Logistic Regression - F1 = 0.5152  
**Improvement:** +40.4% F1 over baseline

---

## Repository Contents

| File | Description |
|---|---|
| [`Capstone_Final_Submission.ipynb`](Capstone_Final_Submission.ipynb) | Full technical notebook (data → EDA → models → results) |
| [`Capstone_Final_Report.docx`](Capstone_Final_Report.docx) | Non-technical report for final submission |
| [`CA_Industry_Outlook_Predictions.xlsx`](CA_Industry_Outlook_Predictions.xlsx) | California Industry Growth Predictions |
| [`EDD-Data.csv`](EDD-Data.csv) | California EDD monthly employment data (2010–2025) |
| `README.md` | This file |

---

## Data Source

California Employment Development Department (EDD), Labor Market Information Division  
https://labormarketinfo.edd.ca.gov/data/employment-by-industry.html

- **286 rows** × **196 columns** (raw)  
- **188 monthly** employment columns spanning Jan 2010 – Aug 2025  
- **282 industries** after cleaning  
- **3,666 modeling rows** after feature engineering

---

## Approach

### Data Pipeline
1. Load raw EDD CSV → clean (duplicates, numeric conversion, row filtering)
2. Reshape wide → long → aggregate to annual averages per industry
3. Create binary `GrowthLabel` target (next-year employment > current year)
4. Engineer 15 features, including lag, YoY change, rolling averages, trend slope, momentum, and seasonality ratio

### Models Trained
| Model | F1 | ROC-AUC | Accuracy |
|---|---|---|---|
| Gradient Boosting (Default) ★ | 0.7231 | 0.7563 | 68.1% |
| Random Forest (Default) | 0.7192 | 0.7567 | 66.5% |
| Random Forest (Tuned) | 0.7059 | 0.7570 | 65.4% |
| Gradient Boosting (Tuned) | 0.6401 | 0.7305 | 64.7% |
| K-Nearest Neighbors | 0.4928 | 0.7130 | 62.4% |
| Logistic Regression | 0.2703 | 0.7464 | 56.9% |
| SVM (RBF) | 0.1156 | 0.7089 | 53.9% |
| **Baseline (from EDA)** | **0.5152** | **0.6753** | **60.3%** |

### Top Predictive Features (Random Forest Importances)
1. `yoy_pct` - Year-over-year % employment change (10.3%)
2. `yoy_change` - Absolute YoY change (9.7%)
3. `year` - Calendar year trend (9.4%)
4. `trend3_slope` - 3-year linear trend slope (7.8%)
5. `momentum` - Acceleration of growth rate (6.7%)

---

## How to Run

```bash
# Clone the repository
git clone https://github.com/graycloudrios/Labor-Market-Predictions-California.git

# Access the directory created
cd Labor-Market-Predictions-California

# Install dependencies
pip install pandas numpy scikit-learn matplotlib seaborn

# Open the notebook
jupyter notebook Capstone_Final_Submission.ipynb
```

---

## Key Findings

- **Ensemble models** (Gradient Boosting, Random Forest) substantially outperform linear models, confirming that industry growth dynamics involve non-linear feature interactions.
- **Recent momentum** (YoY% change) is the single strongest predictor — industries that were growing tend to continue growing in the short run.
- **Seasonal volatility** (`seasonality_ratio`) is an underappreciated signal; highly seasonal industries follow distinct growth patterns.
- **Hyperparameter tuning** provided modest gains; default ensemble configurations were already near-optimal for this dataset size.

---

## Next Steps

- Integrate EDD wage data and macroeconomic indicators (GDP, CPI)
- Apply SMOTE to address mild class imbalance (67% grow / 33% decline)
- Implement walk-forward temporal cross-validation
- Test XGBoost / LightGBM for potential further improvement
- Explore sector-specific models for finer granularity
