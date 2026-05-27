# Major Index Forecasting Workflow

This repository studies one-step-ahead index-level forecasting for major US equity indices using econometric baselines, classical machine learning, deep learning, and a reduced quantum comparison workflow.

## Study Design
- Indices: `^IXIC`, `^GSPC`, `^DJI`, `^RUA`
- Data source: Yahoo Finance
- Sample: `2022-01-01` to `2026-03-31`
- Forecast target: one-step-ahead index level
- Sliding-window settings: `10` and `20`
- Window rule: the last observation in each window is the test point
- Modeled inputs: `rsi14`, `macd_hist`, `bb_z20`, `roc10`, `vol20`, `sma10_gap`, `atr14_pct`, `williams_r14`

## Active Workflow
The active workflow is now split into two phases.

### Phase 1: Full Tuning On Classical And Deep Models
- `2022` windows are used for hyperparameter tuning.
- Notebook `03` tunes `SVR`, `RandomForest`, `GradientBoosting`, and `XGBoost`.
- Notebook `04` tunes `MLP`, `RNN`, `LSTM`, and `GRU`.
- Tuning logs all tested configurations on the `2022` windows.
- The original `2023-2026` evaluation in `03` and `04` is retained as the first benchmark pass, but it is no longer the final model-selection stage for the full study.

### Phase 2: Top-5 Full-Span Reruns
- Notebook `10` aggregates the completed tuning results from `03` and `04`.
- For each `model x horizon` setup, the top 5 configurations are selected using the average `abs_error_pct` across the four indices during the `2022` tuning span.
- Only these top-5 configurations are rerun on the full rolling span.
- The reruns compare:
  - `conventional`
  - `quantum_ideal`
  - `quantum_aer_noisy`
  - `quantum_mthree_readout`
  - `quantum_zne`
- The deep-learning branch then adds a focused one-at-a-time sensitivity pass around the final selected setup-level winners.

## Notebook Map
- [`notebooks/00_major_index_data_collection_and_eda.ipynb`](notebooks/00_major_index_data_collection_and_eda.ipynb)
  Collects data, validates coverage, and produces descriptive statistics and exploratory figures.
- [`notebooks/01_major_index_sliding_window_pipeline.ipynb`](notebooks/01_major_index_sliding_window_pipeline.ipynb)
  Builds the shared rolling-window bundle used by downstream notebooks.
- [`notebooks/02_major_index_econometric_benchmarks.ipynb`](notebooks/02_major_index_econometric_benchmarks.ipynb)
  Runs linear, ARMA, ARIMA, ARCH, GARCH, and GJR-GARCH baselines in index-level space.
- [`notebooks/03_major_index_classical_ml_rolling.ipynb`](notebooks/03_major_index_classical_ml_rolling.ipynb)
  Tunes the full classical ML grid on the `2022` window set and evaluates the selected winners on `2023-2026`.
- [`notebooks/04_major_index_deep_learning_rolling.ipynb`](notebooks/04_major_index_deep_learning_rolling.ipynb)
  Tunes the full deep-learning grid on the `2022` window set and evaluates the selected winners on `2023-2026`.
- [`notebooks/10_major_index_tuning_selection_summary.ipynb`](notebooks/10_major_index_tuning_selection_summary.ipynb)
  Summarizes the completed tuning results, ranks setups by average `abs_error_pct` across the four indices, and writes the top-5 manifests used by the remaining notebooks.
- [`notebooks/11_major_index_ml_top5_full_branch_runs.ipynb`](notebooks/11_major_index_ml_top5_full_branch_runs.ipynb)
  Reruns the top-5 ML configurations on the full rolling span under the conventional and quantum branches.
- [`notebooks/12_major_index_dl_top5_full_branch_runs.ipynb`](notebooks/12_major_index_dl_top5_full_branch_runs.ipynb)
  Reruns the top-5 DL configurations on the full rolling span under the conventional and quantum branches.
- [`notebooks/13_major_index_ml_top5_branch_comparison.ipynb`](notebooks/13_major_index_ml_top5_branch_comparison.ipynb)
  Summarizes ML top-5 rerun results, including tuning-versus-pure-test scatterplots and branch deltas relative to the conventional baseline.
- [`notebooks/14_major_index_dl_top5_branch_comparison.ipynb`](notebooks/14_major_index_dl_top5_branch_comparison.ipynb)
  Summarizes DL top-5 rerun results, including tuning-versus-pure-test scatterplots and branch deltas relative to the conventional baseline.
- [`notebooks/15_major_index_dl_branch_sensitivity.ipynb`](notebooks/15_major_index_dl_branch_sensitivity.ipynb)
  Runs one-at-a-time deep-learning sensitivity checks around the final branch winners selected from notebook `14`.

## Recommended Execution Order
1. `notebooks/00_major_index_data_collection_and_eda.ipynb`
2. `notebooks/01_major_index_sliding_window_pipeline.ipynb`
3. `notebooks/02_major_index_econometric_benchmarks.ipynb`
4. `notebooks/03_major_index_classical_ml_rolling.ipynb`
5. `notebooks/04_major_index_deep_learning_rolling.ipynb`
6. `notebooks/10_major_index_tuning_selection_summary.ipynb`
7. `notebooks/11_major_index_ml_top5_full_branch_runs.ipynb`
8. `notebooks/12_major_index_dl_top5_full_branch_runs.ipynb`
9. `notebooks/13_major_index_ml_top5_branch_comparison.ipynb`
10. `notebooks/14_major_index_dl_top5_branch_comparison.ipynb`
11. `notebooks/15_major_index_dl_branch_sensitivity.ipynb`

## Outputs
- Each notebook writes to its own folder under `outputs/`.
- Tuning summary artifacts from notebook `10` are written to:
  - `outputs/10_major_index_tuning_selection_summary`
- Full-span rerun artifacts are written to:
  - `outputs/11_major_index_ml_top5_full_branch_runs`
  - `outputs/12_major_index_dl_top5_full_branch_runs`
- Comparison artifacts are written to:
  - `outputs/13_major_index_ml_top5_branch_comparison`
  - `outputs/14_major_index_dl_top5_branch_comparison`
- Sensitivity artifacts are written to:
  - `outputs/15_major_index_dl_branch_sensitivity`

## Resume Behavior
- Notebooks `11`, `12`, and `15` are written as resumable rolling workflows.
- Large master logs are flushed incrementally and can be resumed from disk.
- If a clean rerun is required, set `FORCE_FRESH_RUN = True` inside the notebook before execution.

## Legacy Notebooks
- [`notebooks/05_major_index_quantum_ml_rolling.ipynb`](notebooks/05_major_index_quantum_ml_rolling.ipynb)
- [`notebooks/06_major_index_quantum_deep_learning_rolling.ipynb`](notebooks/06_major_index_quantum_deep_learning_rolling.ipynb)

These notebooks are retained as legacy exploratory full-grid quantum tuning notebooks. They are no longer part of the active workflow and are not required for the current study design.

## Environment
- Install dependencies from [`requirements.txt`](requirements.txt).
- Python `3.11` is the intended environment for the current dependency stack.
