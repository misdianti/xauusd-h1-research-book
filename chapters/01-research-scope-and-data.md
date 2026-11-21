# 01 — Research scope and data

## Objective

The project investigates a rule-based N-Method interpretation on XAUUSD H1 and tests whether a simple market-regime filter and different swing-pivot definitions change its historical performance.

The experimentation is organized sequentially:

1. Establish an unfiltered N-Method backtest.
2. Define an interpretable EMA/ADX regime baseline.
3. Test an alternative three-state Gaussian HMM.
4. Compare swing-pivot confirmation widths.
5. Enforce per-strategy position capacity and estimate a simplified fixed-lot account path.

## Dataset

The research notebooks use a supplied, headerless XAUUSD H1 OHLCV file interpreted as:

`Date, Open, High, Low, Close, Volume`

| Attribute | Value |
|---|---|
| Instrument | XAUUSD |
| Timeframe | H1 |
| Parsed bars | 100,000 |
| First timestamp | 2010-02-22 18:00:00 |
| Last timestamp | 2026-11-12 06:00:00 |
| Duplicate timestamps removed | 0 |
| Invalid OHLC bars | 0 |
| Timezone | Not specified in source CSV |

Volume is retained in the data but is not used by the strategy or regime model. The source vendor and meaning of the volume field have not been independently established.

## Data handling and publication

The raw CSV is not included in this GitBook package because the source vendor and redistribution permission are unknown. Keep an authorized local copy at `data/XAUUSD_H1.csv` when running the research notebooks. Do not publish third-party market data unless its licence permits redistribution.

The timestamp timezone matters for matching candles with a broker feed and for any later MT5 execution. Confirm the timezone convention before attempting live signal parity.

## Scope boundaries

This book documents the H1 experiments. An earlier D1 experiment provided the initial rule settings, but the detailed sequence here is the H1 research. H1 results are not directly comparable to D1 results merely because numeric bar counts were held constant: 20 H1 bars and 20 D1 bars represent very different elapsed durations.
