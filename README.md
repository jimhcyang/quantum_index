# Major Index Forecasting With Classical, Deep, and Quantum Models

This repository studies one-step-ahead daily return forecasting for major US equity indices using econometric baselines, classical machine learning, deep learning, and quantum-enhanced variants.

## Research Scope
- Indices: `^IXIC`, `^GSPC`, `^DJI`, `^RUA`
- Data source: Yahoo Finance
- Sample: `2022-01-01` to `2026-03-31`
- Hyperparameter tuning period: calendar year `2022`
- Evaluation period: `2023-01-01` to `2026-03-31`
- Forecast target: one-step-ahead daily return
- Sliding-window settings: `10` and `20`

## Feature Set
- Inputs are fixed to 9 direct features:
  `close`, `rsi14`, `macd_hist`, `bb_z20`, `roc10`, `vol20`, `sma10_gap`, `atr14_pct`, `williams_r14`
- The quantum workflow uses these 9 inputs directly with no PCA, implying 9 qubits at the feature-encoding stage.

## Notebook Map
- [00_major_index_data_collection_and_eda.ipynb](/Users/jimyhc/Desktop/research/quantum_index/root/notebooks/00_major_index_data_collection_and_eda.ipynb)
  Collects data, validates coverage, and produces descriptive statistics and exploratory figures.
- [01_major_index_sliding_window_pipeline.ipynb](/Users/jimyhc/Desktop/research/quantum_index/root/notebooks/01_major_index_sliding_window_pipeline.ipynb)
  Builds the shared rolling-window bundle used by all downstream experiments.
- [02_major_index_econometric_benchmarks.ipynb](/Users/jimyhc/Desktop/research/quantum_index/root/notebooks/02_major_index_econometric_benchmarks.ipynb)
  Runs linear, ARMA, ARIMA, ARCH-family, and GARCH-family benchmarks with a single chronological fit.
- [03_major_index_classical_ml_rolling.ipynb](/Users/jimyhc/Desktop/research/quantum_index/root/notebooks/03_major_index_classical_ml_rolling.ipynb)
  Runs rolling-window tuning and evaluation for `SVR`, `RandomForest`, `GradientBoosting`, and `XGBoost`.
- [04_major_index_deep_learning_rolling.ipynb](/Users/jimyhc/Desktop/research/quantum_index/root/notebooks/04_major_index_deep_learning_rolling.ipynb)
  Runs rolling-window tuning and evaluation for `MLP`, `RNN`, `LSTM`, and `GRU`.
- [05_major_index_quantum_rolling.ipynb](/Users/jimyhc/Desktop/research/quantum_index/root/notebooks/05_major_index_quantum_rolling.ipynb)
  Runs the quantum pipeline after the classical deep-learning setup, including `ideal`, `aer_noisy`, `mthree_readout`, and `zne` modes when available.

## Recommended Execution Order
1. `notebooks/00_major_index_data_collection_and_eda.ipynb`
2. `notebooks/01_major_index_sliding_window_pipeline.ipynb`
3. `notebooks/02_major_index_econometric_benchmarks.ipynb`
4. `notebooks/03_major_index_classical_ml_rolling.ipynb`
5. `notebooks/04_major_index_deep_learning_rolling.ipynb`
6. `notebooks/05_major_index_quantum_rolling.ipynb`

## Outputs
- Each notebook writes to its own folder under `outputs/`.
- Tables, prediction logs, summary metrics, and figures are written incrementally during execution.
- Rolling notebooks write per-window results immediately so interrupted runs retain completed work.

## Resume Behavior
- Notebooks `02` to `05` default to resume mode.
- Existing saved results are read at startup, and completed units are skipped automatically.
- To force a clean restart for a notebook, set `FORCE_FRESH_RUN = True` inside that notebook before execution.
- `tqdm` progress bars report remaining work rather than total historical work.

## Compute Expectations
- `00`: about `1-3` minutes from cache
- `01`: about `1-2` minutes
- `02`: about `10-30` minutes on a laptop CPU
- `03`: roughly `2-6` hours
- `04`: roughly `18-48` hours
- `05`: potentially multiple days with the full quantum mitigation set

## Environment
- Install dependencies from [requirements.txt](/Users/jimyhc/Desktop/research/quantum_index/root/requirements.txt).
- Python `3.11` is the intended environment for the current dependency stack.
