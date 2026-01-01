# Machine learning — Week 3 Baseline  

## Problem  
- **Target:** `is_high_value` (binary classification)  
- **Unit of analysis:** Individual user (`user_id`)  
- **Decision enabled:** Identify high-value users for targeted marketing or promotions  

---

## Setup
1. Clone this repository:
```
git clone https://github.com/FaiOnayq/week3-ML-baseline.git
cd week3-ML-baseline
```
2. Install dependencies:

```
uv sync
```

### Quick Start
1. Generate Sample Data
```
uv run ml-baseline make-sample-data
```
This creates `data/processed/features.csv` with sample data for development.

2. Train a Model
```
uv run ml-baseline train --target is_high_value
```
- this loads features from `data/processed/features.csv`
- Creates holdout split (80/20, stratified)
- Trains baseline `DummyClassifier` and model `LogisticRegression` 
- Saves artifacts to `models/runs/<run_id>/`
- Records run metadata 


3. Make Predictions
```
uv run ml-baseline predict --run latest --input data/processed/features.csv --output outputs/preds.csv
```
Parameters:

- `--run latest`: Use most recent training run (or specify run ID)
- `--input`: Path to CSV with same schema as training data
- `--output`: Where to write predictions


---

## Training Artifacts
After training, inspect your run folder:
```
# Find latest run ID
models/registry/latest.txt

# View run metadata (dataset hash, git commit, timestamp)
models/runs/<run_id>/run_meta.json

# View holdout metrics
models/runs/<run_id>/metrics/holdout_metrics.json

# Examine predictions for error analysis
models/runs/<run_id>/tables/holdout_predictions.csv
```

## Data  
- **Feature table:** `data/processed/features.csv`  
- **Features used:**  
  - `country` (categorical)  
  - `n_orders` (integer)  
  - `avg_amount` (float)  
  - `total_amount` (float)  
- **Forbidden columns:** `is_high_value`  
- **Optional ID column:** `user_id`  

---

## Splits  
- **Holdout strategy:** Random stratified split  
- **Test size:** `0.2`  
- **Random seed:** `42`  

---

## Metrics (Holdout Set)

| Metric       | Baseline | Model |
|--------------|----------|-------|
| ROC AUC      | 0.5      | 1.0   |
| PR AUC       | 0.2      | 1.0   |
| Accuracy     | 0.8      | 1.0   |
| Precision    | 0.0      | 1.0   |
| Recall       | 0.0      | 1.0   |
| F1 Score     | 0.0      | 1.0   |
| Threshold    | 0.5      | 0.5   |

> **Critical Note**:  
> - Baseline performance using LogisticRegression classifier.  
> - **Model achieves perfect metrics**, indicating **strong evidence of data leakage or overfitting**.  


---

## Reproducibility  
- **Run ID:** `2026-01-01T17-27-09Z__classification__seed42`  
- **Environment:** `models/runs/2026-01-01T17-27-09Z__classification__seed42/env/pip_freeze.txt`  
- **Artifacts:**  
  - `model/model.joblib` — Trained model (joblib)  
  - `schema/input_schema.json` — Input spec (columns, dtypes, constraints)  
  - `tables/holdout_predictions.parquet` — Full holdout predictions  


Memo:

<img src="reports/ml.jpg" alt="Sample Image" width="400" height="300">

