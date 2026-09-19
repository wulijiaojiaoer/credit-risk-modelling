# Reproducibility and analytical notes

## Status of the published results

The notebook and two PNG figures contain **saved outputs from the original analysis**. `lightgbm_test_metrics.json` transcribes the rounded metrics from the original LightGBM test-evaluation output. None of those values comes from a new training run of this repository edition.

For this portfolio preparation, notebook JSON was parsed, every code cell was checked for Python syntax, and source/output content was inspected for private paths and applicant-row previews. Full training, dependency installation, and a clean-kernel Run All were not performed. The original execution counts were not strictly sequential, so saved outputs alone do not prove clean execution from top to bottom.

## Publication changes

- Added an introduction, data-path settings, and README documentation.
- Changed both main CSV reads to `DATA_PATH`, defaulting to `data/lending_analytics.csv`. The legacy `train.csv` fallback now resolves beside that input; it is not used when the notebook is run in order, because the training split already exists.
- Replaced four hard-coded XGBoost `device="cuda"` settings with `device=XGB_DEVICE`; the default is CPU and the environment variable `CREDIT_RISK_XGB_DEVICE` can override it. Renamed the study label to remove the GPU-specific suffix. Numerical agreement with the saved GPU outputs is not guaranteed.
- Removed the `df.head()` applicant-preview call and its saved table, private notebook metadata, execution counters, and stderr warning logs that contained local machine paths.
- Reworded the legacy group-coordination message to describe its shared model-evaluation function. Authorship is presented as Tianjiao Jia's independently completed work, originally developed for QBUS6810.
- Added comments identifying the inherited XGBoost diagnostic issues below. Other model specifications, split seeds, tuning logic, and saved aggregate results were retained.

No raw dataset or original course report is included. The repository is a documented portfolio edition, not a fully refactored modelling library.

## Known methodological and implementation limitations

1. **Evaluation scope.** The initial target-aware EDA covers all observations before splitting. The original report also compares model families on test results. Therefore the overall project should not be described as strictly leakage-free or as having a pristine, single-use test set. A new temporal/external holdout is needed for an independent assessment after finalizing the whole workflow.

2. **XGBoost overfitting diagnostic.** The inherited code compares total payoff on the training and validation sets even though the training set has three times as many rows. That total-payoff gap is not a valid overfitting measure. Compare payoff per applicant and other appropriately normalized validation metrics in a future revision. The code's refit decision depends on this original diagnostic, so it has been preserved and flagged rather than silently changed while retaining old results.

3. **XGBoost post-refit validation.** When the final XGBoost model is refitted on train plus validation, the subsequently displayed validation score is an in-sample diagnostic. It must not be interpreted as an independent validation result.

4. **Bureau-history flag.** In the original history-imputation block, the `BUREAU_` prefix can include previously engineered, non-missing enquiry summaries. Consequently the all-missing test used to create `NO_BUREAU_HISTORY` can fail to identify applicants with no bureau history, leading to median imputation instead of the intended no-history handling. Correcting this requires retraining and refreshing results; no corrected performance is claimed here.

5. **Random Forest selection and categorical encoding.** The original Random Forest configuration is selected by a hard-coded name, preserving the recorded choice rather than automatically reselecting on a new run. Categorical values are one-hot encoded and validation/test columns are aligned to training columns; the portfolio does not claim a general rare-category grouping procedure.

6. **Decision assumptions and probability quality.** Fixed loan size, simple interest, alternative return, and loss severity are scenario assumptions. Discounting, individual exposure, operating costs, collection timing, acceptance effects, and realized cash flows are not fully modelled. Ranking performance does not by itself establish calibrated probabilities, which matter for payoff thresholds.

7. **Generalization and responsible use.** Random splitting does not measure future-period drift. Applicant descriptors, including demographic variables, require subgroup assessment and review before any operational use. The project has not undergone a deployment, fairness, or prospective decision-impact validation.

## Environment

`requirements.txt` lists direct notebook dependencies and Jupyter tooling. The original saved output explicitly identifies scikit-learn 1.8.0; the other package versions are not completely recorded. Version ranges are suggested compatibility constraints, not an exact or execution-tested lockfile. Python 3.11 or 3.12 is a suggested starting point. LightGBM may additionally require the platform's OpenMP runtime.

Run all cells in their displayed order in a fresh kernel. Later model sections reuse variables created by earlier sections; running LightGBM in isolation will not work without recreating those dependencies. The notebook creates summary CSVs and PNG figures in the working directory; `.gitignore` excludes those generated files except the two curated figures already included in this repository.
