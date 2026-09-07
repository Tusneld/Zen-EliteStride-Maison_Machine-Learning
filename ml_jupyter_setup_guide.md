# Zen EliteStride Maison — Machine Learning Setup Guide

This guide covers the additional setup needed on top of the Python/Jupyter
stage to run `shoe_sales_ml_analysis.ipynb`.

If you already completed the Python stage setup, you have Python and
Jupyter installed already — you only need a few extra libraries for this
one.

---

## Step 1: Install the additional libraries

In your terminal:
```
pip install scikit-learn xgboost
```

What each one adds on top of what you already have (pandas, matplotlib,
seaborn):
- **scikit-learn** — train/test splitting, preprocessing (one-hot
  encoding), the Linear/Logistic Regression and Random Forest models,
  and evaluation metrics
- **xgboost** — the XGBoost gradient boosting models used alongside
  Random Forest for comparison

If you haven't done the Python stage setup at all yet, install everything
in one go:
```
pip install jupyter pandas matplotlib seaborn openpyxl scikit-learn xgboost
```

---

## Step 2: Put the files in one folder

Create a project folder and place these inside it:
- `shoe_sales_ml_analysis.ipynb`
- `Zen_EliteStride_Maison_ML.xlsx` (or whichever copy of the dataset you're using — just make sure the filename in the notebook's `SOURCE_FILE` variable matches it **exactly**, including spaces vs underscores)

> **Reminder from the Python stage:** File Explorer can display a filename
> slightly differently than what's actually on disk. If you hit a
> `FileNotFoundError`, run this in a new cell to see the real filename and
> your current folder:
> ```python
> import os
> print("Current folder:", os.getcwd())
> print("Files here:", os.listdir())
> ```

---

## Step 3: Launch Jupyter and run the notebook

1. In your terminal, navigate to the project folder and run:
   ```
   jupyter notebook
   ```
2. Open `shoe_sales_ml_analysis.ipynb`.
3. Run all cells top to bottom (**Cell → Run All**), or step through with
   **Shift + Enter** one cell at a time to read each explanation as you go.

The notebook is organized in three parts:
- **Part A** — models trained on the real data
- **Part B** — the same models re-run on a synthetic target with a known,
  injected effect, to confirm the modeling approach itself works
- **Part C** — a summary table comparing the two, plus conclusions

---

## Step 4: Confirm it worked

The regression section in Part A should print an R² **close to zero (or
negative)** for all three models — this is expected, not an error. Part B's
regression should then print a **substantially higher R²** (roughly 0.5-0.7)
on the synthetic target, confirming the models can pick up a real signal
when one exists.

If Part B's numbers *aren't* meaningfully higher than Part A's, something
went wrong in the synthetic data generation step — check that the
`category_effect` and `seasonal_amplitude` cells ran without error before
the model cells.

---

## Troubleshooting

- **`ModuleNotFoundError: No module named 'xgboost'` (or `sklearn`)** — the
  install in Step 1 didn't take in the same environment Jupyter is using.
  Re-run the `pip install` command from the same terminal you use to
  launch `jupyter notebook`.
- **Notebook runs very slowly** — the Random Forest and XGBoost models
  use 300 trees each and cross-validation, which is normal to take a few
  seconds per cell on this dataset size (500 rows) but shouldn't take
  minutes. If it does, check nothing else is heavily using your CPU.
- **Different numbers than expected** — `RANDOM_STATE = 42` is set
  throughout specifically so results are reproducible. If your numbers
  differ substantially, confirm you're running against the same,
  unmodified dataset.
