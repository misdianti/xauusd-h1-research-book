# Reproducing the experiments

## Package contents

This GitBook package includes copies of the five H1 notebooks, selected result CSVs, and chart assets. It intentionally excludes the raw OHLCV CSV because the data vendor and redistribution licence were not specified.

## Expected local research layout

Place your authorized source data at `data/XAUUSD_H1.csv` in the research project. The notebooks expect a headerless CSV interpreted as:

`Date, Open, High, Low, Close, Volume`

Keep the GitBook documentation separate from the licensed source data if publishing publicly.

## Python environment

A typical environment for the notebooks uses Python 3.10 or later and the following packages:

```bash
python -m venv .venv
source .venv/bin/activate  # macOS / Linux
python -m pip install --upgrade pip
python -m pip install pandas numpy matplotlib jupyter ipykernel hmmlearn
```

`hmmlearn` is required for Notebook 03. The other experiments primarily use pandas, NumPy, and Matplotlib.

## Notebook sequence

1. `notebooks/01_N_Method_Backtest_H1.ipynb`
2. `notebooks/02_Baseline_Regime_Analysis_H1.ipynb`
3. `notebooks/03_HMM_Regime_Detection_H1.ipynb` (alternative regime experiment)
4. `notebooks/04_Swing_Window_Experiment_H1.ipynb` (earlier swing sensitivity experiment)
5. `notebooks/05_Per_Strategy_Position_Capacity_H1.ipynb` (current swing-width, position-capacity, and fixed-lot account experiment)

Run Notebook 01 before Notebook 02 and Notebook 03 because they use trade-level outputs created by the earlier analysis. Notebook 05 is the current reference for position capacity and the account simulation.

## Notes on reproducibility

- The dataset timestamp timezone is unspecified; do not assume it matches a broker's MT5 server time.
- Outputs can differ if the source CSV differs, rows are reordered, indicators are implemented differently, or notebook code is edited.
- The HMM may fit different local optima across library versions or random seeds; the notebook tests fixed seeds and selects based on training likelihood.
- Notebooks produce historical research outputs; they do not place live orders.
