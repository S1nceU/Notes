A machine learning pipeline for identifying and validating **DNA methylation biomarkers** that distinguish tumor from normal tissue. The pipeline performs multi-model stability-based feature selection from high-dimensional methylation array data (~450K CpG probes) and evaluates selected biomarkers with rigorous cross-validation.

## Overview

DNA methylation arrays produce measurements for hundreds of thousands of CpG probes per sample. Most probes carry no discriminative signal. This pipeline addresses two key challenges:

1. **Feature selection** — Identify a compact set of CpG probes that reliably separate tumor from normal tissue across multiple machine learning models and cross-validation folds.
2. **Evaluation** — Validate the discriminative power of selected probes on held-out data that was never used during selection or training.

### Pipeline Architecture

```
┌────────────────────────────────────────────────────────────────────────┐
│                     Workflow 1: kfold_fs                               │
│  K-Fold × Multi-Model Feature Selection                                │
│  Input: ~450K probes → Output: selected_features.txt (stable subset)   │
└──────────────────────────────┬─────────────────────────────────────────┘
                               │ selected probes
                               ▼
┌────────────────────────────────────────────────────────────────────────┐
│                     Workflow 2: train                                  │
│  Train final classifier on selected features                           │
│  Output: model weights, ROC curves, metrics on holdout test set        │
└────────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────┐
│                     Workflow 3: nested_cv                              │
│  Nested CV (outer: evaluation, inner: feature selection)               │
│  Output: unbiased performance estimate + SHAP importance + consensus   │
└────────────────────────────────────────────────────────────────────────┘
```

## Methodology

### Feature Selection Strategy (kfold_fs)

The selection strategy is designed to find probes that are **stable across data splits** and **agreed upon by multiple model families**:

1. **Data isolation** — 20% of samples are held out as a test set before any analysis (stratified by class label).
2. **K-Fold cross-validation** — The remaining 80% training set is split into K folds (default K=5) using stratified sampling.
3. **Per-fold preprocessing** — In each fold, the training portion undergoes:
   - Outlier filtering (IQR-based, values outside 1.5×IQR replaced with NaN)
   - Median imputation for missing values
   - Low-variance feature removal (VarianceThreshold)
   - Standard scaling (z-score normalization)
   - Preprocessing is **fit only on fold-training data** and applied to fold-validation data, preventing data leakage.
4. **Multi-model ranking** — Each of 4+ model families independently ranks features by importance:
   - **Lasso** (L1 logistic regression) — absolute coefficient magnitude
   - **Ridge** (L2 logistic regression) — absolute coefficient magnitude
   - **ElasticNet** (L1+L2 logistic regression) — absolute coefficient magnitude
   - **XGBoost** (gradient boosted trees) — split gain
   - Optional: **SVM** (L1-LinearSVC) — absolute coefficient, **ReliefF** / **MultiSURF** — feature weights
5. **Stability aggregation** — A probe is selected if:
   - It appears in the top-M (default 50) across ≥ freq_threshold (default 70%) of all fold×model trials
   - It is selected by ≥ model_coverage_threshold (default 2) distinct model families

### Optional: Delta-Beta Pre-filtering

When `--delta_beta_threshold` is specified, low-effect probes are removed before feature selection. Delta-beta (mean methylation difference between tumor and normal) is computed **per-fold from training data only** to prevent leakage.

### Model Training (train)

After feature selection, the `train` workflow trains and evaluates a final classifier:

1. **Two-phase training** — Hyperparameters (e.g., regularization alpha) are selected on an inner train/validation split; the final model is refit on the full training set with selected hyperparameters.
2. **Holdout evaluation** — The 20% test set (shared via `split/splits.json`) provides an unbiased performance estimate.
3. **Outputs** — ROC curves, classification metrics (AUC, accuracy, sensitivity, specificity), and feature importance rankings.

### Nested Cross-Validation (nested_cv)

For unbiased performance estimation independent of any single train/test split:

1. **Outer CV** (default 5-fold) — Each fold provides an independent test set.
2. **Inner CV** — Within each outer fold's training set, the full feature selection pipeline runs to select probes.
3. **Evaluation** — A classifier trained on selected features is evaluated on the outer test fold.
4. **Outputs** — Per-fold metrics, SHAP feature importance, and consensus feature lists across outer folds.

## Data Leakage Prevention

The pipeline implements several safeguards against information leakage:

| Mechanism               | Implementation                                                                                               |
| ----------------------- | ------------------------------------------------------------------------------------------------------------ |
| Test set isolation      | `build_or_load_splits()` creates a fixed 80/20 stratified split persisted in `split/splits.json`             |
| Preprocessing isolation | `fit_preprocess()` is fit only on fold-training data; `transform_preprocess()` is applied to validation/test |
| Delta-beta isolation    | `compute_delta_beta_values_from_training()` uses only fold-training samples                                  |
| Split persistence       | All workflows share the same `splits.json`, ensuring identical train/test boundaries                         |


## Project Structure

```text
.
├── src/
│   ├── cmd/
│   │   ├── train.py          # Single model training & evaluation
│   │   ├── kfold_fs.py       # K-fold multi-model feature selection
│   │   └── nested_cv.py      # Nested cross-validation evaluation
│   ├── data/
│   │   ├── io.py             # CSV reading, label parsing, split I/O
│   │   └── splits.py         # Train/test split creation & loading
│   ├── models/
│   │   ├── linear.py         # Lasso / Ridge / ElasticNet (PyTorch GPU + sklearn)
│   │   ├── xgb.py            # XGBoost (GPU-accelerated)
│   │   ├── svm.py            # L1-SVM / RFE-SVM feature selection
│   │   └── relieff.py        # ReliefF / MultiSURF feature selection
│   ├── preprocess/
│   │   ├── pipeline.py       # Outlier handling, imputation, scaling
│   │   └── delta_beta.py     # Per-probe delta-beta computation
│   └── utils/
│       ├── importance.py     # Unified feature importance extraction
│       ├── shap_utils.py     # SHAP value computation
│       ├── metrics.py        # Classification metrics
│       ├── paths.py          # Output directory management
│       └── plots.py          # ROC / MSE curve plotting
├── data/                     # (gitignored) Input data matrices
├── split/                    # (gitignored) Persistent split metadata
├── weights/                  # (gitignored) Saved model artifacts
├── output/                   # (gitignored) Metrics, figures, reports
├── config/                   # (gitignored) Local run configurations
├── environment.yml
├── requirements.txt
├── LICENSE
└── README.md
```

## Environment Setup

```powershell
conda env create -f environment.yml
conda activate dl310
```

Optional packages used by some workflows:

```powershell
pip install shap skrebate
```

### Key Dependencies

- Python 3.10, PyTorch 2.8 (CUDA 12.6), XGBoost 3.0, scikit-learn 1.7
- GPU required for linear model training (`train_linear_gpu`) and XGBoost

## Input Data Format

| File | Format | Description |
|------|--------|-------------|
| Normal matrix CSV | rows = probes, columns = samples | Methylation values for normal tissue samples |
| Tumor matrix CSV | rows = probes, columns = samples | Methylation values for tumor tissue samples |
| Label CSV | single column, binary | Sample labels; accepted values: `0/1`, `true/false`, `yes/no`, `tumor/normal` |

The pipeline supports both **M-values** (`--type M`) and **beta-values** (`--type B`).

## Usage

### 1) K-Fold Feature Selection

Select stable biomarkers from high-dimensional data:

```powershell
python -m src.cmd.kfold_fs `
  --type M `
  --subset demo `
  --models svm relieff xgb `
  --n_splits 5 `
  --shuffle `
  --seed 42 `
  --top_m 50 `
  --freq_threshold 0.7 `
  --model_coverage_threshold 2 `
  --normal_csv <path_to_normal_csv> `
  --tumor_csv <path_to_tumor_csv> `
  --label_csv <path_to_label_csv>
```


**Outputs** (saved to `weights/kfold_fs_*`):

| File | Description |
|------|-------------|
| `selected_features.txt` | Final selected probe IDs (one per line) |
| `feature_stability.csv` | Per-probe statistics: selection frequency, average rank, model coverage |
| `per_fold/*.csv` | Per-fold per-model importance rankings (for auditability) |
| `config.json` | Run configuration for reproducibility |
| `summary.json` | Summary: original feature count → selected count |

### 2) Train Single Model

Train and evaluate a classifier using selected features:

```powershell
python -m src.cmd.train `
  --model elasticnet `
  --type M `
  --subset demo `
  --features_txt selected_features.txt `
  --normal_csv <path_to_normal_csv> `
  --tumor_csv <path_to_tumor_csv> `
  --label_csv <path_to_label_csv>
```

Supported models: `lasso`, `ridge`, `elasticnet`, `xgb`

**Outputs**:

| Directory | File | Description |
|-----------|------|-------------|
| `weights/train_*` | `model.pt` / `model.joblib` | Trained model weights |
| `weights/train_*` | `preproc.joblib` | Fitted preprocessor (imputer + scaler) |
| `output/train_*` | `roc.png` | ROC curves for train / validation / test splits |
| `output/train_*` | `metrics.json` | AUC, accuracy, sensitivity, specificity per split |
| `output/train_*` | `config.json` | Full run configuration |
| `output/train_*` | `*_topk_coef.csv` | Top-K features by importance (interpretability) |

### 3) Nested CV Evaluation

Obtain an unbiased performance estimate with embedded feature selection:

```powershell
python -m src.cmd.nested_cv `
  --type M `
  --model elasticnet `
  --fs-models lasso ridge elasticnet xgb `
  --outer_splits 5 `
  --inner_splits 5 `
  --shuffle `
  --seed 42 `
  --top_m 50 `
  --freq_threshold 0.6 `
  --model_coverage_threshold 2 `
  --normal_csv <path_to_normal_csv> `
  --tumor_csv <path_to_tumor_csv> `
  --label_csv <path_to_label_csv>
```

**Outputs** (saved to `output/nested_*`):

| File | Description |
|------|-------------|
| `nested_results.json` | Per-fold metrics, selected features, SHAP importance |
| `nested_predictions.csv` | Per-sample predictions across all outer folds |
| `nested_shap_summary.csv` | Aggregated SHAP importance across folds |
| `nested_consensus.csv` | Feature consensus: how many outer folds selected each probe |
| `nested_fold_features.csv` | Features selected in each outer fold |
| `nested_preprocess_report.csv` | Per-fold preprocessing statistics |

## Configuration

All command modules support `--config` to load parameters from a JSON or YAML file. CLI arguments override config file values. Config files are typically stored under the gitignored `config/` directory.

## Reproducibility

- All random splits are seeded and persisted in `split/splits.json`.
- Run configurations are saved as `config.json` in each output directory.
- Per-fold feature rankings are saved for full auditability.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
