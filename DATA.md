# Data requirements

The notebook expects **`data/lending_analytics.csv`**, or an alternative path supplied through the `CREDIT_RISK_DATA_PATH` environment variable.

The original input was a course-provided, already aggregated lending dataset with **307,511 rows and 105 columns**. It includes application information plus aggregated bureau, prior-application, instalment, and point-of-sale history. It is not interchangeable with a raw Home Credit application table: the notebook does not contain the upstream aggregation that produces this exact input.

No input data is distributed in this repository. Obtain the original dataset only through a source you are authorized to use; no verified public download location is claimed here. The original data provider's conditions continue to apply.

## Expected schema

[`expected_columns.json`](expected_columns.json) lists the 105 original column names, extracted from the notebook's saved schema. It contains no applicant records.

Key fields include:

| Field or family | Purpose |
|---|---|
| `TARGET` | Binary target: `1` for payment difficulty; `0` otherwise |
| `AMT_INCOME_TOTAL`, `AMT_CREDIT`, `AMT_ANNUITY`, `AMT_GOODS_PRICE` | Income, exposure, and repayment amounts |
| `NAME_CONTRACT_TYPE`, income/education/employment categories | Application and borrower descriptors |
| `DAYS_BIRTH`, `DAYS_EMPLOYED`, other `DAYS_*` fields | Age and recorded durations, including sentinel values |
| `EXT_SOURCE_1`, `EXT_SOURCE_2`, `EXT_SOURCE_3` | External risk-score variables |
| `BUREAU_*`, `PREV_*`, `INST_*`, `POS_*` | Pre-aggregated historical credit and repayment information |

Provide a CSV with a header row, numeric target values `0` / `1`, the original column names, and missing values represented consistently with pandas CSV parsing. Input row order, dataset transformations, software versions, and CPU/GPU execution can affect reproducibility.

The dataset is excluded by `.gitignore`. Do not commit raw applications, record previews, identifiable borrower-level predictions, or unauthorized copies of the data.
