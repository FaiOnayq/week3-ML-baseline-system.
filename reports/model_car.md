# Model Card — Week 3 Baseline
## Problem
- Predict: <target> (e.g., is_high_value or amount_spent_next_7d) for unit of analysis (e.g., one row per user, transaction, or day per store).

- Decision enabled: <what action changes?> (e.g., determining who receives a retention offer).

- Constraints: CPU-only; offline-first (reads only from data/processed/); batch inference.

## Data (contract)
- Feature table: data/processed/features.<csv|parquet>.

- Unit of analysis: One row per <unit of analysis> representing a single prediction decision.

- Target column: <name> (e.g., is_* or *_usd), positive class: 1 (define what this represents, e.g., "high value user").

- Optional IDs (passthrough): <list> (e.g., user_id, order_id) used for joining results back but dropped from model features.

## Splits (draft for now)
- Holdout strategy: random stratified (default for balanced classes) / time (to prevent future leakage) / group (to avoid splitting records from the same user).

- Leakage risks:

Future-known features: Information known only after the outcome.

Target proxies: Columns that almost perfectly correlate with the label.

IDs: Unique identifiers that models might memorize.

Time leakage: Using random splits on time-ordered data.

## Metrics (draft for now)
- Primary: <metric> (e.g., Precision if false positives are costly, or Recall if false negatives are costly).

- Baseline: A dummy model (majority class or mean) must be reported to establish a performance floor.

## Shipping
- Artifacts: model + schema (JSON contract) + metrics + holdout tables + env snapshot.

- Known limitations: Where the model is expected to fail (e.g., missing features or shift in data distribution).

- Monitoring sketch: What to watch (e.g., input column presence, target distribution shifts, or prediction value drift).