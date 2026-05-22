# Major Index Forecasting With Classical, Deep, and Quantum Models

This repository studies one-step-ahead index-level forecasting for major US equity indices using econometric baselines, classical machine learning, deep learning, and quantum-enhanced variants.

## Study Design
- Indices: `^IXIC`, `^GSPC`, `^DJI`, `^RUA`
- Data source: Yahoo Finance
- Sample: `2022-01-01` to `2026-03-31`
- Hyperparameter-tuning segment: windows whose test-point date falls in calendar year `2022`
- Pure-test segment: windows whose test-point date falls in `2023-01-01` to `2026-03-31`
- Forecast target: one-step-ahead index level
- Sliding-window settings: `10` and `20`
- Window rule: the last observation in each window is the test point

## Inputs
- The modeled input set is fixed to 8 technical indicators:
  `rsi14`, `macd_hist`, `bb_z20`, `roc10`, `vol20`, `sma10_gap`, `atr14_pct`, `williams_r14`
- `close` is not part of `X`. It is retained only as the target level and as reference metadata for plotting and error measurement.
- The quantum notebooks use the 8 inputs directly with no PCA, implying 8 qubits at the feature-encoding stage.

## Notebook Map
- [`notebooks/00_major_index_data_collection_and_eda.ipynb`](notebooks/00_major_index_data_collection_and_eda.ipynb)
  Collects data, validates coverage, and produces descriptive statistics and exploratory figures.
- [`notebooks/01_major_index_sliding_window_pipeline.ipynb`](notebooks/01_major_index_sliding_window_pipeline.ipynb)
  Builds the shared rolling-window bundle used by all downstream experiments.
- [`notebooks/02_major_index_econometric_benchmarks.ipynb`](notebooks/02_major_index_econometric_benchmarks.ipynb)
  Runs linear, ARMA, ARIMA, ARCH, GARCH, and GJR-GARCH baselines using a fit-on-2022, one-step walk-forward design in index-level space.
- [`notebooks/03_major_index_classical_ml_rolling.ipynb`](notebooks/03_major_index_classical_ml_rolling.ipynb)
  Runs rolling-window tuning and evaluation for `SVR`, `RandomForest`, `GradientBoosting`, and `XGBoost`.
- [`notebooks/04_major_index_deep_learning_rolling.ipynb`](notebooks/04_major_index_deep_learning_rolling.ipynb)
  Runs rolling-window tuning and evaluation for `MLP`, `RNN`, `LSTM`, and `GRU`.
- [`notebooks/05_major_index_quantum_ml_rolling.ipynb`](notebooks/05_major_index_quantum_ml_rolling.ipynb)
  Runs quantum-enhanced classical ML experiments, applying the quantum feature pipeline before `SVR`, `RandomForest`, `GradientBoosting`, and `XGBoost`.
- [`notebooks/06_major_index_quantum_deep_learning_rolling.ipynb`](notebooks/06_major_index_quantum_deep_learning_rolling.ipynb)
  Runs quantum-enhanced deep-learning experiments, applying the quantum feature pipeline before `MLP`, `RNN`, `LSTM`, and `GRU`.

## Execution Order
1. `notebooks/00_major_index_data_collection_and_eda.ipynb`
2. `notebooks/01_major_index_sliding_window_pipeline.ipynb`
3. `notebooks/02_major_index_econometric_benchmarks.ipynb`
4. `notebooks/03_major_index_classical_ml_rolling.ipynb`
5. `notebooks/04_major_index_deep_learning_rolling.ipynb`
6. `notebooks/05_major_index_quantum_ml_rolling.ipynb`
7. `notebooks/06_major_index_quantum_deep_learning_rolling.ipynb`

## Outputs
- Each notebook writes to its own folder under `outputs/`.
- Tables, prediction logs, summary metrics, and figures are written incrementally during execution.
- Forecast plots separate the `2022` tuning segment from the `2023-2026` pure-test segment.
- Saved prediction files include direct index-level forecasts together with daily signed and absolute percent-error traces.

## Resume Behavior
- Notebooks `02` to `06` default to resume mode.
- Existing saved results are read at startup, and completed units are skipped automatically.
- To force a clean restart for a notebook, set `FORCE_FRESH_RUN = True` inside that notebook before execution.

## Compute Expectations
- `00`: about `1-3` minutes from cache
- `01`: about `1-2` minutes
- `02`: about `10-30` minutes on a laptop CPU
- `03`: roughly `2-6` hours
- `04`: roughly `18-48` hours
- `05`: long-running; often several hours to multiple days depending on mitigation modes
- `06`: potentially multiple days with the full quantum mitigation set

## Environment
- Install dependencies from [`requirements.txt`](requirements.txt).
- Python `3.11` is the intended environment for the current dependency stack.
