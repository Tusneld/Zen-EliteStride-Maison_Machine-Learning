# Zen EliteStride Maison - Machine Learning Analysis
---
![Zen EliteStride Maison_ML](https://github.com/Tusneld/Zen-EliteStride-Maison_Machine-Learning/blob/11a5c077c65995f454ca1186aa30d1d8dc086c24/ML.PNG)
---
Fifth and final stage of a five-part analytics pipeline built on the same
shoe sales dataset: **Excel → Power BI → SQL → Python → Machine Learning**.
This stage asks a predictive question the earlier stages couldn't: are
order size and payment method actually predictable from the data, or does
it just look that way?

## Overview

Two supervised learning problems, both testing whether purchasing behavior
can be predicted from product and customer attributes (`Category`, `Color`,
`Brand`, `Country`, `Payment Type`/`Quantity`, `Month`, `DayOfWeek`):

1. **Regression** - predict order `Quantity` (demand)
2. **Classification** - predict `Payment Type`

`Revenue`, `Profit`, and `Cost` are deliberately excluded as inputs since
they're deterministic functions of `Product` and `Quantity` - including
them would leak the answer rather than test a genuine relationship.

## Key findings

| Task | Real data | Synthetic (signal-injected) data |
|---|---|---|
| Regression (Quantity) | R² = **-0.084** (Random Forest) | R² = **0.664** (Random Forest) |
| Classification (Payment Type) | Accuracy = **0.250** (at the 4-class random baseline) | Accuracy = **0.470** (Logistic Regression, nearly double baseline) |

**The real-data result is a genuine finding, not a broken model.** To
confirm this, the exact same pipeline was re-run on synthetic targets
engineered with a known Category/seasonal effect - R² and accuracy both
rose sharply, proving the models can detect real signal when it exists.
This means: **in this dataset, order size and payment method aren't
meaningfully driven by product, category, brand, country, or timing** - 
a genuinely useful negative result for the business, not a modeling
failure.

## Tech stack

- Python 3.9+
- pandas, numpy
- scikit-learn (Linear/Logistic Regression, Random Forest, preprocessing, cross-validation, metrics)
- XGBoost
- matplotlib, seaborn
- Jupyter Notebook

## Files in this repo

| File | Description |
|---|---|
| `shoe_sales_ml_analysis.ipynb` | Full annotated notebook - Part A (real data models), Part B (synthetic validation), Part C (summary & conclusions) |
| `ml_jupyter_setup_guide.md` | Step-by-step guide to install the additional ML libraries and run the notebook |
| `Zen_EliteStride_Maison_ML.xlsx` | Source dataset |

## How to run this

See `ml_jupyter_setup_guide.md` for full instructions. Summary:

1. `pip install jupyter pandas matplotlib seaborn openpyxl scikit-learn xgboost`
2. Put the notebook and Excel file in the same folder.
3. Run `jupyter notebook` and open `shoe_sales_ml_analysis.ipynb`.
4. Run all cells top to bottom.

## Methodology note

Every model is evaluated with both 5-fold cross-validation and a held-out
test set, using a fixed random seed (`RANDOM_STATE = 42`) for
reproducibility. Feature importance and predicted-vs-actual plots are
included for the regression task; a confusion matrix for classification.
The synthetic-data validation in Part B exists specifically to distinguish
"the model can't find a pattern" from "there's no pattern to find" - a
distinction that's easy to skip but changes the conclusion entirely.

## Part of a larger pipeline

This is the final stage of 5. The same dataset and business questions are
also answered in:

- **Excel** - pivot-table dashboard
- **Power BI** - DAX measures and interactive report
- **SQL (PostgreSQL / pgAdmin)** - schema, cleaning views, window functions, indexing, a stored procedure, and role-based access control
- **Python (pandas)** - KPI recomputation and visualization across the 7 core business questions

## Author

**Tusnelde Endjala**
