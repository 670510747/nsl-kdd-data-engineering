# NSL-KDD · Data Engineering for Intrusion Classification

A course project that turns network-connection records into validated, model-ready data and measures how preprocessing choices affect five-class intrusion classification.

**Focus:** data quality, train/test separation, feature preparation, and transparent evaluation. The downstream model is a Random Forest, not a deployed intrusion-detection service.

## What the project does

- Loads the 43-column NSL-KDD schema: 41 predictors, attack label, and difficulty.
- Maps attack subtypes to Normal, DoS, Probe, R2L, and U2R; unknown labels raise an error.
- Checks missing values, schema alignment, row counts, constant columns, and feature-only duplicates.
- Deduplicates training predictors, removes constant and highly correlated features using training data, and explores engineered features.
- Uses a `ColumnTransformer` and `Pipeline` for one-hot encoding and model fitting.
- Reports macro metrics, a confusion matrix, and class-level results for a strongly imbalanced benchmark.

## Recorded results

Source: saved outputs in my notebook. These experiments were **not retrained while preparing this repository**. Metrics below are rounded to three decimals.

| Setup | Accuracy | Macro F1 |
|---|---:|---:|
| Raw predictors | 0.785 | 0.660 |
| Deduplicate + remove constants | 0.783 | 0.661 |
| Also remove correlated features | 0.791 | 0.680 |
| Add engineered features | 0.775 | 0.627 |

Original split sizes: **125,973 training records / 22,544 test records**. Training contains only 52 U2R and 995 R2L examples, so accuracy alone is insufficient. Feature-only duplicate counts differ from full-record duplicate counts because labels and difficulty are excluded from that check.

Model: 100 trees, maximum depth 8, minimum leaf size 20, balanced class weights, random seed 42.

## Run locally

Use Python 3.11 or 3.12. From the repository root:

```bash
python -m venv .venv
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS / Linux: source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyterlab
```

Open `notebooks/nsl_kdd_data_engineering.ipynb`, choose the installed Python kernel, and run cells in order. The extraction cell downloads the two input files into `data/`; this requires internet access. Set `NSLKDD_DATA_DIR` before launching Jupyter to change that location. Generated pipeline imagery goes into `artifacts/`.

The dataset URL is a third-party GitHub mirror retained from the original notebook. The official UNB page currently states the dataset is no longer available there. If the mirror stops working, obtain the same KDDTrain+ / KDDTest+ files from an attributable source and update the extraction cell; do not substitute a different split silently.

## Evaluation limits

The active ablation measures **test-set** performance. Setup 2 was retained after inspecting those results; its result is not an unbiased final estimate after model selection. Cross-validation is not part of the active ablation; the notebook keeps the CV evaluator under a separate function name, and this explanation reflects that.

The optional CV diagnostic is not executed by the ablation and does not isolate all feature-selection steps inside each fold. Future work should fit all learned preprocessing inside cross-validation and reserve an untouched final test set. This older benchmark is not a substitute for evaluation on modern network traffic.

## Repository contents

- `notebooks/`: cleaned runnable notebook, saved outputs cleared.
- `results/`: historical rounded results extracted from my notebook.
- `docs/reproducibility.md`: provenance, edits, and verification scope.
- `scripts/check_notebooks.py`: schema and syntax checks without training.

```bash
python -m pip install -r requirements-dev.txt
python scripts/check_notebooks.py
```

## Data source and citation

[UNB / Canadian Institute for Cybersecurity: NSL-KDD](https://www.unb.ca/cic/datasets/nsl.html)

M. Tavallaee, E. Bagheri, W. Lu, and A. A. Ghorbani. *A Detailed Analysis of the KDD CUP 99 Data Set*. IEEE CISDA, 2009.

[Download mirror used by the notebook](https://github.com/defcom17/NSL_KDD). Raw dataset files are not bundled. No new license is assigned to third-party data or dependencies.
