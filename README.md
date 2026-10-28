# DS-A3: Regression Baseline and Diagnostics

## Overview and dataset
Predict one-year disease progression using the ten original features in the [sklearn Diabetes dataset](https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset). There are 442 observations and no missing values. Inputs are loaded with `scaled=False`.

All experiment code is in `main.ipynb`. It contains the model comparison, four diagnostic plots, BMI slices, error cases, and short conclusions.

## Methods
Use an 80/20 split (353 training, 89 test; seed 42) and five shuffled training folds. Compare mean Dummy, Linear, Ridge, and Lasso using CV MAE. Each linear model has a StandardScaler inside its pipeline. Ridge and Lasso search `[0.1, 1, 10, 100]`; all configurations and fold scores remain in the notebook. Select the final model before test evaluation.

## Results

| Model | Alpha | CV MAE | Test MAE | Test RMSE | Test R2 |
|---|---:|---:|---:|---:|---:|
| Dummy | - | 66.867 | 64.006 | 73.222 | -0.012 |
| Linear | - | 45.048 | 42.794 | 53.853 | 0.453 |
| Ridge | 1 | 44.999 | 42.812 | 53.777 | 0.454 |
| **Lasso (selected)** | **0.1** | **44.956** | **42.805** | **53.709** | **0.456** |

Regularization adds little over Linear. Residuals show local bias and possible heteroscedasticity. Eighteen training rows exceed the Cook's distance screening threshold. The learning curve levels off; the high-BMI group has the largest test MAE (50.288, n=28).

## How to reproduce
Use Python 3.12. In an activated environment, run:

```powershell
python -m pip install -r requirements.txt
python -m ipykernel install --sys-prefix --name python3 --display-name "Python 3 (DS-A3)"
python -m nbconvert --execute --to notebook --inplace main.ipynb --ExecutePreprocessor.timeout=300
```

Alternatively open the notebook in JupyterLab, restart the kernel, and run all cells. The notebook recreates `results.csv`; `report.pdf` is a saved report. PDF layout code is kept out of the notebook to keep the coursework focused on the experiment.

The simplified notebook was executed successfully, with four embedded figures and unchanged model scores. Its results CSV is byte-identical to the original run.

## Files
- `main.ipynb`: experiment code, outputs, and necessary discussion.
- `README.md`: overview, results, and reproduction instructions.
- `requirements.txt`: tested dependency versions.
- `report.pdf`: a concise report with the four diagnostic figures.
- `results.csv`: 89 held-out rows with actual values, all four models' predictions and errors, BMI, and BMI groups. Residual means actual minus predicted.

## Limitations
A small dataset and one split limit generalization, especially in small subgroups. Linear models may miss nonlinear relationships or unmeasured factors. Coefficients are associations rather than causal effects; predictions are not clinical diagnoses.

## Submission Summary

This assignment builds a regression baseline for the scikit-learn Diabetes dataset, which contains 442 observations and ten original baseline features. The target is a quantitative measure of disease progression one year later. I used raw inputs, a fixed 80/20 split with random state 42, and five shuffled training folds. Standardization was fitted inside each pipeline to avoid leakage.

The candidates were a mean DummyRegressor, ordinary linear regression, Ridge, and Lasso. Ridge and Lasso searched alpha values of 0.1, 1, 10, and 100 using training CV MAE. Lasso with alpha 0.1 had the lowest CV MAE, 44.956, and was frozen before test evaluation. Its test MAE was 42.805, RMSE was 53.709, and R-squared was 0.456. Regularization improved CV MAE by less than 0.1 target units over ordinary linear regression, so the practical gain was very small.

Residual plots showed local bias and larger average absolute errors at higher predictions, suggesting possible nonconstant error variance. Eighteen training observations exceeded the Cook's distance screening threshold; none were deleted. Validation error decreased with more training data and then leveled off. The high-BMI slice was hardest, with MAE 50.288 across 28 test observations. The three largest errors included both overprediction and underprediction.

The small dataset, single split, noisy subgroup estimates, limited linear models, and lack of external validation limit generalization. Coefficients describe associations rather than causal effects, and the results are not intended for clinical decisions.
