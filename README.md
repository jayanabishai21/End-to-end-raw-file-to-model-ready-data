# ML Assignment 4: End-to-end, raw file to model-ready data

This repository takes a raw, uncleaned census dataset and turns it into a clean, model-ready matrix.
**No model is trained.** The work stops at `X_train`, `X_test`, `y_train`, `y_test`.

## Dataset

**Adult (Census Income)**, UCI Machine Learning Repository
Source: https://archive.ics.uci.edu/dataset/2/adult
File used: https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data

15 columns, more than 32,000 rows, mixed numeric and text columns, genuine missing values (`?` in
`workclass`, `occupation` and `native_country`). The target is `income` (`<=50K` or `>50K`).

The notebook reads the file straight from the URL, so no CSV upload is needed to run it.

## Files

| File | What it is |
|---|---|
| `assignment4_endtoend.ipynb` | The full notebook, run top to bottom with Restart and run all |
| `adult_clean.csv` | The cleaned data with the new features, before scaling and encoding |
| `pipeline.joblib` | The fitted pipeline (fitted on the training rows only) |
| `README.md` | This file |

## How the notebook is organised

Four functions, each in its own cell:

* `load_data()` reads the raw file as published
* `clean(df)` fixes whitespace and labels, removes duplicates and impossible values, drops `education` and `fnlwgt`
* `add_features(df)` adds `net_capital`, `potential_experience`, `is_married`, `hours_bracket`, `gain_topcoded`
* `build_pipeline()` returns an unfitted imputing, encoding and scaling pipeline

The final cell calls them in order, splits 80/20 (stratified, `random_state=42`), fits the pipeline on the
training rows only, and prints the shape of all four outputs. The next cell checks the result with `assert` statements.

## Before and after

Copy the table printed by the "Question 7" cell of the notebook into this spot after you run it:

| Stage | Rows | Columns | Missing values | Duplicate rows |
|---|---|---|---|---|
| raw | | | | |
| final | | | | |

## Main decisions

The notebook ends with the full list, with the number behind each one. In short:

* Removed exact duplicate rows
* Dropped `education` (same information as `education_num`) and `fnlwgt` (a survey weight)
* Filled missing text values with `Unknown`, because missing `workclass` and `occupation` are informative
* Kept `capital_gain = 99999` rows and flagged them instead of deleting them
* Used Yeo-Johnson on the skewed money columns
* Split before fitting anything, so no information leaks from the test rows

## Run it yourself

1. Open `assignment4_endtoend.ipynb` in Google Colab
2. Runtime, then Restart and run all
3. Allow the two downloads (`adult_clean.csv`, `pipeline.joblib`) at the end
