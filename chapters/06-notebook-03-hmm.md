# 06 — Notebook 03: HMM regime detection

**Source notebook:** [`03_HMM_Regime_Detection_H1.ipynb`](../notebooks/03_HMM_Regime_Detection_H1.ipynb)

## Research question

Can a three-state Gaussian Hidden Markov Model identify useful latent market regimes, and does selecting Bullish N in HMM Bullish states plus Ranging N in HMM Sideways states improve the strategy compared with the deterministic EMA/ADX baseline?

## Model design

The HMM uses four causal features calculated from completed H1 bars:

1. `log_return_1h` — one-bar log return.
2. `momentum_24h` — 24-bar cumulative log return.
3. `realized_volatility_24h` — rolling standard deviation of one-bar log returns across 24 bars.
4. `efficiency_24h` — absolute 24-bar price change divided by the sum of absolute one-bar changes over the same window.

A three-state `GaussianHMM` with diagonal covariance is used as a simple baseline. Features are standardized using training-period statistics only.

## Train / holdout design

- First 60% of usable feature observations: training.
- Remaining 40%: chronological holdout.
- Several fixed random seeds are tested; the fit with the highest training log-likelihood is retained.
- The scaler, model parameters, and state-to-regime mapping are frozen during the holdout.
- Holdout observations are processed one at a time using a forward-only recursive filter. Future holdout candles are not used to classify an earlier candle.

Numeric HMM states have no inherent economic names. The training states are ranked by posterior-weighted mean 24-bar momentum:

- Lowest → Bearish.
- Middle → Sideways.
- Highest → Bullish.

This is a naming heuristic, not ground truth. The model is detecting statistical states, not directly predicting price direction.

## Transition persistence diagnostic

The training-sample transition matrix reported in the experiment was:

| Current state | Next Bullish | Next Bearish | Next Sideways |
|---|---:|---:|---:|
| Bullish | 0.973 | 0.007 | 0.020 |
| Bearish | 0.022 | 0.948 | 0.029 |
| Sideways | 0.013 | 0.047 | 0.941 |

The diagonal probabilities indicate high estimated one-bar persistence in the fitted training model. Under a simple geometric-duration interpretation, the corresponding expected durations are approximately 37, 19, and 17 H1 observations. Rolling input features overlap and are autocorrelated, so persistence should not be interpreted as proof of a stable economic regime.

## Agreement with EMA/ADX

The reported raw label agreement with the EMA/ADX baseline was **22.7%**. This is descriptive agreement, not model accuracy: neither classifier provides ground-truth labels, and their definitions differ.

## HMM-selected performance on the common holdout

The comparison used the same HMM holdout signal window for each strategy selection:

| Selection | Trades | Win rate | Net R | Profit factor | Average R |
|---|---:|---:|---:|---:|---:|
| Unfiltered — all strategies | 1,143 | 32.283% | -48.229R | 0.939 | -0.042R |
| EMA/ADX selected — Bullish N + Ranging N | 261 | 36.782% | **+24.461R** | **1.146** | **+0.094R** |
| HMM selected — Bullish N + Ranging N | 187 | 33.155% | -1.490R | 0.988 | -0.008R |

The selected HMM groups were:

| Strategy and HMM state | Trades | Net R | Profit factor |
|---|---:|---:|---:|
| Bullish N + HMM Bullish | 131 | +9.510R | 1.113 |
| Ranging N + HMM Sideways | 56 | -11.000R | 0.732 |

**Interpretation:** EMA/ADX was the stronger selected configuration in this inspected common-period comparison. The HMM selected combination was slightly negative before costs. This does not prove that HMM is universally inferior; it shows that this particular HMM design and state mapping did not outperform the baseline in this experiment.

## Holdout integrity warning

Although the split was chronological and the HMM used forward-only filtering, the holdout results were inspected and used in subsequent research discussion. The holdout is therefore no longer a pristine final test set for further model selection. A new future period or nested walk-forward evaluation is required for a stronger final assessment.

## Limitations

- One fixed chronological split, not repeated walk-forward validation.
- Three latent states are a modelling choice, not an established market taxonomy.
- State labels depend on training-sample momentum ranking.
- Gaussian diagonal emissions simplify financial return distributions.
- Trading costs are excluded.
- Trade-level cumulative R is not a shared-capital portfolio simulation.
