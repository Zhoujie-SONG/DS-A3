# AI Use Disclosure

## Assistance used
OpenAI Codex helped write and revise the single Jupyter notebook, run the experiment in a Python 3.12.14 environment, inspect numerical and visual outputs, and draft the README and four-page PDF. It also prepared the Git commit and the user-requested GitHub upload. The user supplied the task requirements, dataset choice, candidate models, split, alpha grid, and deliverable constraints.

Codex generated the code, result explanations, PDF export, and submission summary. All reported numbers and charts come from actual executed cells; they were not supplied from memory or invented. The notebook uses sklearn's bundled raw Diabetes inputs and records the library version and source links. Official scikit-learn dataset and statsmodels Cook's distance documentation were consulted.

## Suggestions accepted and modifications actually made
- Accepted a mean Dummy baseline, fold-local StandardScaler pipelines, one shared shuffled five-fold CV, and training-only MAE selection, as required by the task.
- Kept all four alpha values for each regularized model, including poor configurations. The selected Lasso was not replaced after seeing the slightly lower Linear test MAE.
- After execution, replaced initially general diagnostic wording with observed residual-bin directions, the actual learning-curve plateau, and the directions of the three largest errors. No hyperparameters, split, or model-selection rule changed.
- The first execution launcher, `python -m jupyter nbconvert`, dispatched to an unrelated existing Anaconda installation and failed before notebook execution. It was replaced with `python -m nbconvert` in an isolated environment, and the environment's Python kernel was explicitly registered. Reproduction instructions use the corrected route.
- Interpreted the very small CV differences as limited practical gains, rather than claiming a meaningful or statistically significant advantage. Cook's distance is explicitly an OLS diagnostic; it is not presented as exact penalized-model influence.

No separate human acceptance, rejection, or editing of AI suggestions has been confirmed. These are changes made by Codex during this session, not fabricated actions by the student.

## Verification performed by Codex
- Executed all 14 code cells from a fresh kernel in order, without cell errors, and retained the outputs. Repeated the full run after revising prose and export instructions; the 89-row results CSV was byte-identical.
- Checked the 353/89 split, disjoint row IDs, training-fold separation, original ten features, training-only scaler means, and the mean baseline target.
- Checked all four models' saved residuals and independently recomputed MAE, RMSE, and R-squared from their predictions inside the notebook.
- Confirmed the training OLS predictions agree with sklearn LinearRegression and retained the high-influence rows without deleting them.
- Checked exactly four figure outputs, BMI slice counts, error-case row alignment, notebook structure, and the full alpha search output.
- Checked installed dependency consistency with `pip check` and recorded exact direct dependency versions in requirements.txt.
- Rendered and visually inspected the notebook HTML preview and all four PDF pages, including the result tables and all four diagnostic charts.

## Pending user verification
The user has not yet confirmed an independent rerun, manual review of the reasoning or code, familiarity with the methods, the course's AI policy, or the final platform submission. These remain **pending user verification**. Codex does not claim that a student or instructor performed any of these steps. The user should review and adapt the work and disclosure to their course requirements before submitting.
