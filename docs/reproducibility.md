# Provenance and publication changes

Source: `attack_type_nslkdd_DE(2).ipynb` supplied by the project owner; accompanying course report reviewed for context. The notebook's active code and saved outputs take precedence where its prose conflicts.

Publication edits: English project overview and result interpretation; unique evaluator function names; portable data/artifact directories; explicit display import; cleared outputs and execution metadata; dependency manifest, validation script, and CI. No model hyperparameter, attack mapping, split, or metric formula was changed.

The unused CV helper is named `evaluate_cv_diagnostic`; the active ablation uses `evaluate_test_metrics`. Statements about CV-based setup selection and conclusive overfitting were removed. Setup 2 remains the detailed-report setup as in the original code.

All CSV/report values in `results/` are transcribed from existing saved output with its original rounding. They are not results from executing the cleaned notebook. Original dependency versions and dataset hashes were not captured, so bit-for-bit reproduction is not promised.

Validation performed during packaging: notebook JSON/schema and Python syntax checks. No dataset download or full model training was performed. CI also runs static validation only.

Recommended next experiment: put feature selection and learned transformations inside the model-selection folds; choose the setup on training validation data; evaluate once on a new untouched holdout. Report class-level metrics alongside macro F1.
