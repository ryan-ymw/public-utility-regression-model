# Revenue Forecasting with Regression Analysis

A Python notebook that builds and evaluates linear regression models to forecast monthly revenue for a heating/cooling supplier, using historical production and degree-day data.

## Overview

The dataset (`AICPA_regressionAnalysisData.csv`) contains monthly observations from 2011 to 2014, split into a training set (2011-2013) and a testing set (2014), with the following fields:

- `type` — training or testing flag
- `date` — month-end date
- `revenue` — monthly revenue
- `production` — monthly production volume
- `coolDD` — cooling degree days
- `heatDD` — heating degree days

The notebook walks through exploratory analysis, correlation checks, and a series of univariate and multiple regression models, comparing each on out-of-sample accuracy.

## Workflow

1. **Data prep** — load the CSV, convert `date` to a proper datetime type, and split into training/testing sets.
2. **Exploratory analysis** — plot revenue over time and compute pairwise correlations between revenue, production, coolDD, and heatDD.
3. **Model building** — fit OLS regressions (via `statsmodels`) using the training set:
   - Univariate: Production only, CoolDD only, HeatDD only
   - Multiple regression: Production + CoolDD, Production + HeatDD, CoolDD + HeatDD
4. **Model evaluation** — for each model, predict revenue on the testing set and score it using Mean Absolute Percentage Error (MAPE).
5. **Visualization** — plot actual vs. predicted revenue for each model to compare fit quality visually.

## Results

| Model | Predictors | MAPE |
|---|---|---|
| Model 1 / mrmodel2 | Production | 0.254 |
| mrmodel3 | CoolDD | 0.296 |
| mrmodel4 | HeatDD | 0.216 |
| mrmodel1 | Production + CoolDD | 0.217 |
| mrmodel5 | Production + HeatDD | 0.139 |
| mrmodel6 | CoolDD + HeatDD | *(see notebook output)* |

The **Production + HeatDD** model produced the lowest error on the testing set among the models compared, suggesting that combining production volume with heating demand best explains the revenue pattern.

## Requirements

```
pandas
numpy
matplotlib
statsmodels
```

## Usage

1. Place `AICPA_regressionAnalysisData.csv` in the working directory.
2. Run the notebook cells in order (data prep → EDA → model fitting → evaluation → visualization).

## Notes

- All models are fit on `dt4training` and validated on the held-out `dt4testing` set to avoid data leakage.
- MAPE is used as the primary accuracy metric since it expresses error as a percentage, making it easy to compare across models regardless of the revenue scale.
