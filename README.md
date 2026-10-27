# DS-A3: Regression Baseline and Diagnostics

## Project overview
A small, reproducible undergraduate regression experiment. All experiment and export code is in **main.ipynb**. It has been executed from a fresh kernel, with saved tables and exactly four diagnostic figures.

## Dataset
The [scikit-learn Diabetes dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset) contains 442 observations, ten numeric features, and a continuous progression target measured one year after baseline. The original data are documented by [NCSU](https://www4.stat.ncsu.edu/~boos/var.select/diabetes.html).

The notebook uses `load_diabetes(scaled=False, as_frame=True)` from scikit-learn 1.7.2. Features are `age, sex, bmi, bp, s1, s2, s3, s4, s5, s6`. There are no missing feature/target values or duplicate feature rows. Inputs are kept in their raw form until fold-specific pipeline scaling; no extra features are created. The output is a dataset target, not a clinical diagnosis.

## Methods
- Fixed 80/20 split: 353 training rows and 89 test rows; `random_state=42`.
- Five-fold training CV: `KFold(n_splits=5, shuffle=True, random_state=42)`.
- Four candidates: mean Dummy, LinearRegression, Ridge, and Lasso. Each linear candidate has `StandardScaler` inside its pipeline.
- Ridge and Lasso search only `[0.1, 1, 10, 100]`. All eight configurations and their fold scores remain in the notebook.
- Lowest mean CV MAE selects the final model before test evaluation. Exact model ties follow Dummy, Linear, Ridge, Lasso; alpha ties follow grid order.
- Test metrics: MAE, RMSE, and R-squared. Test diagnostics do not trigger tuning, switching, or deletion.
- Diagnostics: residuals, absolute residuals, training OLS Cook's distance, a training-only learning curve, BMI slices, and three largest test errors.

## Results summary

| Model | Chosen alpha | CV MAE | CV SD | Test MAE | Test RMSE | Test R2 |
|---|---:|---:|---:|---:|---:|---:|
| Dummy (mean) | - | 66.867 | 5.632 | 64.006 | 73.222 | -0.012 |
| Linear | - | 45.048 | 1.773 | 42.794 | 53.853 | 0.453 |
| Ridge | 1 | 44.999 | 1.808 | 42.812 | 53.777 | 0.454 |
| **Lasso (selected by CV)** | **0.1** | **44.956** | **1.837** | **42.805** | **53.709** | **0.456** |

CV SD is the standard deviation across folds, not a confidence interval. MAE and RMSE use target units. Lasso's CV gain over Linear is only 0.093 units (0.21%); Ridge's is 0.049 (0.11%). These gains are not practically meaningful here and are much smaller than the fold variation. The chosen Lasso retains all ten nonzero coefficients. Linear has a slightly lower test MAE, but this does not change the training-only Lasso choice.

Residuals have mean 3.96 and show local bias: prediction-quartile means are approximately 15.77, 4.14, -20.02, and 15.39. Prediction-quartile MAEs increase from 28.71 to 52.38, suggesting possible nonconstant variance. Training OLS has 18 observations above `4/353=0.0113`; the largest Cook's distance is 0.0277 (row 382). This is an OLS screening diagnostic, not exact Lasso influence. No observations are deleted.

Learning-curve validation MAE drops from 49.53 at 56 observations to 44.96 at 282 observations per fold and levels off at larger sizes; final training MAE is 43.32. The folds also helped choose alpha, so this curve is descriptive.

BMI groups use training tertiles **24.2** and **27.9**, with right-closed boundaries:

| BMI slice | Test n | MAE |
|---|---:|---:|
| Low | 38 | 34.490 |
| Middle | 23 | 47.433 |
| High | 28 | 50.288 |

The largest-error original row IDs are 56, 77, and 211. Absolute errors are 153.195, 125.381, and 110.323, respectively. The notebook and PDF show actual values, predictions, and BMI.

## How to reproduce
Tested with **Python 3.12.14**. Run the following inside the project folder in a clean Python 3.12 environment. On Windows PowerShell:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m ipykernel install --sys-prefix --name python3 --display-name "Python 3 (DS-A3)"
python -m nbconvert --execute --to notebook --inplace main.ipynb --ExecutePreprocessor.timeout=300
```

If the Windows Python launcher is unavailable, substitute the path to your Python 3.12 executable for `py -3.12`. The local `.venv` is a runtime environment, not a submission file. Alternatively run `python -m jupyterlab`, open the notebook with the registered kernel, and choose **Restart Kernel and Run All**. No dataset download is needed after installing scikit-learn. Running all cells overwrites `results.csv` and `report.pdf` using the same experiment; it does not regenerate this README or the AI disclosure.

Validation completed by Codex: all 14 code cells executed in order without notebook errors; exactly four embedded plots; split and scaler checks; training baseline mean; agreement of statsmodels OLS with sklearn Linear predictions; manual recomputation of all three metrics for every candidate; BMI group counts; row alignment; valid notebook format; clean `pip check`; and two fresh-kernel runs with byte-identical `results.csv`. The HTML notebook preview and all four PDF pages were visually inspected. No independent human rerun is claimed.

For these pinned versions, the ordered test-row ID SHA-256 is `02d2e32a71061a4379a53ff2ea22f29e8c3a6971b6ffb1a08a5fb77b8dd90ac0`. The results CSV SHA-256 is `d35b444c1fbe8dd8c9b648c4df28db464a94bfe84006b3b2dee3fc7c86b47a03`. Dataset row-hash provenance is printed in the notebook. Minor floating-point differences can occur across platforms; compare numerical values with a tolerance if bytes differ.

## Submission files
- `main.ipynb`: the only code file; saved outputs, full alpha search, diagnostics, discussion, and PDF export.
- `README.md`: methods, results, reproduction, and submission summary.
- `requirements.txt`: exact tested versions of direct experiment and notebook dependencies.
- `report.pdf`: four-page report built from the same in-memory tables and figures.
- `results.csv`: all 89 held-out rows, original row IDs, targets, BMI, all four predictions/residuals/absolute errors, and selected-model results and BMI slice. Residual means actual minus predicted; a positive residual means underprediction.
- `AI_USE.md`: actual AI assistance, modifications, verification, and pending human checks.

## Limitations
This small dataset and one random split do not establish performance on another population. The 89 test cases give noisy diagnostics and smaller subgroup samples. The selected CV score may be optimistic because CV also selects alpha; fold SD is not a significance test. Simple linear models can miss nonlinear relationships and omitted factors. No external validation, prediction intervals, or formal heteroscedasticity test is included. Regression coefficients are conditional associations affected by confounding, correlation, and shrinkage; they are not causal effects. The results should not guide clinical decisions.

## Submission Summary

This assignment builds a regression baseline for the scikit-learn Diabetes dataset, which contains 442 observations and ten original baseline features. The target is a quantitative measure of disease progression one year later. I used raw inputs, a fixed 80/20 split with random state 42, and five shuffled training folds. Standardization was fitted inside each pipeline to avoid leakage.

The candidates were a mean DummyRegressor, ordinary linear regression, Ridge, and Lasso. Ridge and Lasso searched alpha values of 0.1, 1, 10, and 100 using training CV MAE. Lasso with alpha 0.1 had the lowest CV MAE, 44.956, and was frozen before test evaluation. Its test MAE was 42.805, RMSE was 53.709, and R-squared was 0.456. Regularization improved CV MAE by less than 0.1 target units over ordinary linear regression, so the practical gain was very small.

Residual plots showed local bias and larger average absolute errors at higher predictions, suggesting possible nonconstant error variance. Eighteen training observations exceeded the Cook's distance screening threshold; none were deleted. Validation error decreased with more training data and then leveled off. The high-BMI slice was hardest, with MAE 50.288 across 28 test observations. The three largest errors included both overprediction and underprediction.

The small dataset, single split, noisy subgroup estimates, limited linear models, and lack of external validation limit generalization. Coefficients describe associations rather than causal effects, and the results are not intended for clinical decisions.
