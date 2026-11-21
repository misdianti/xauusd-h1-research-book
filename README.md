# XAUUSD H1 Quantitative Research Lab

This GitBook documents a sequence of Python notebook experiments on **XAUUSD H1**. The project starts with a rule-based N-Method interpretation, adds a deterministic EMA/ADX regime filter, evaluates a three-state Gaussian Hidden Markov Model (HMM), then studies swing confirmation width, per-strategy position limits, and a simplified fixed-lot account simulation.

> **Research status:** The findings in this book are historical exploratory backtest results, not a validated trading system. They are not evidence of live profitability and should not be used as a standalone basis for risking capital.

## What this research asks

**Does market-regime classification and swing-structure definition improve the N-Method's risk-adjusted performance on data that was not used to design the rules?**

A second practical question follows from the latest experiment: **How do different swing-confirmation widths behave when each strategy is limited to one active position, while Bullish N and Ranging N are allowed to overlap?**

## Research map

| Notebook | Experiment | Main purpose |
|---|---|---|
| 01 | N-Method backtest | Establish historical performance of Bullish N, Bearish N, and Ranging N without regime selection. |
| 02 | EMA/ADX baseline | Classify Bullish, Bearish, and Sideways regimes; measure conditional performance and matched-regime filtering. |
| 03 | HMM regime detection | Compare a three-state Gaussian HMM with EMA/ADX on a chronological holdout. |
| 04 | Swing-window sensitivity | Compare 2-, 3-, 4-, and 5-bar pivots under selected EMA/ADX regimes. This is the earlier sensitivity experiment. |
| 05 | Position capacity and account simulation | Re-run swing-width experiments with strict Bullish N geometry, regime filtering before position allocation, one position per strategy, and an illustrative fixed-lot account model. |

## Key findings at a glance

### Baseline and regime filtering

In Notebook 02, the unfiltered H1 strategies generated **2,946 valid trades** and **-121.39R** overall (profit factor 0.940). The pre-defined matched-regime subset retained 977 trades and returned **+3.13R** (profit factor 1.005), which is close to breakeven before costs.

### HMM versus EMA/ADX

On the common 40% chronological HMM holdout, the reported aggregate comparison was:

| Selection | Trades | Net R | Profit factor | Average R |
|---|---:|---:|---:|---:|
| Unfiltered — all strategies | 1,143 | -48.229 | 0.939 | -0.042 |
| EMA/ADX selected — Bullish N + Ranging N | 261 | **+24.461** | **1.146** | **+0.094** |
| HMM selected — Bullish N + Ranging N | 187 | -1.490 | 0.988 | -0.008 |

EMA/ADX was the stronger selected configuration in this comparison. The holdout was inspected during research, so it is no longer a pristine, never-viewed final test set.

### Swing-width and position-capacity experiment (Notebook 05)

| Swing bars on each side | Trades | Net R | Profit factor | Max drawdown (R) |
|---:|---:|---:|---:|---:|
| 2 | 1,088 | **+127.35** | 1.185 | -33.02 |
| 3 | 758 | +88.78 | 1.185 | -25.43 |
| 4 | 544 | +13.08 | 1.036 | -35.45 |
| 5 | 426 | +64.98 | **1.247** | **-19.00** |

Every swing-width experiment passed the position-capacity audit: at most one Bullish N position, at most one Ranging N position, and at most two combined positions. The 2-bar width had the highest historical Net R; the 5-bar width had the highest profit factor and shallowest R drawdown. Neither should be declared the future winner based on this full-sample comparison.

### Illustrative fixed-lot account simulation

Using USD 1,000 starting balance, fixed 0.01 lot per position, and an assumed contract size of 100 troy ounces per 1.00 lot:

| Swing | Net P/L | Ending equity | Max consecutive losses | Max drawdown |
|---:|---:|---:|---:|---:|
| 2 bars | +$1,757.94 | $2,757.94 | 18 | -$681.10 |
| 3 bars | +$1,401.98 | $2,401.98 | 13 | -$328.04 |
| 4 bars | +$546.63 | $1,546.63 | 12 | -$557.96 |
| 5 bars | +$908.74 | $1,908.74 | 10 | -$579.12 |

This is a simplified price-difference simulation, not a broker-exact or margin-aware account model. It excludes spread, commission, swap, slippage, leverage, margin, stop-out and floating drawdown.

## Current research configuration

The latest strategy-selection hypothesis is:

- **Bullish N:** eligible only when EMA/ADX classifies the setup-confirmation candle as Bullish.
- **Ranging N:** eligible only when EMA/ADX classifies the setup-confirmation candle as Sideways.
- **Bearish N:** excluded from the selected configuration.
- **Position capacity:** one active position per strategy; Bullish N and Ranging N may overlap, giving a maximum of two strategy positions.

The latest Notebook 05 Bullish N structure uses the stricter condition `P1 < P3 < P2`.

## Read the book

Start with [Research Scope and Data](chapters/01-research-scope-and-data.md), then follow the notebook sequence in the navigation. The notebooks and selected CSV outputs are bundled with this GitBook package under `notebooks/` and `outputs/`.

## Important publishing note

The supplied OHLCV CSV's vendor and timestamp timezone were not specified. The raw price dataset is intentionally **not included** in this GitBook package. Before publishing any raw data, verify the data provider's redistribution licence. To reproduce the notebooks, place your authorized source file at `data/XAUUSD_H1.csv` in the research repository.

## Disclaimer

This project is for educational and quantitative research purposes only. It is not investment advice, a signal service, or a recommendation to trade. Hypothetical historical performance does not guarantee future results.
