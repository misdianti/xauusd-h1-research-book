# 09 — Results, decisions, and caveats

## Current evidence-based working hypothesis

The research currently uses this selected H1 rule set as the working candidate:

- EMA/ADX regime classifier.
- Bullish N only in Bullish regime.
- Ranging N only in Sideways regime.
- Bearish N excluded.
- One active position maximum per strategy.
- Bullish N and Ranging N may overlap; maximum two active strategy positions.
- Three-bar swing was the original H1 baseline; Notebook 05 compares 2/3/4/5-bar alternatives using the stricter Bullish N geometry.

This is a research configuration, not a recommendation to trade live.

## What the experiments suggest

1. **The unfiltered three-strategy combination was historically weak.** In Notebook 02 it produced -121.39R and a 0.940 profit factor.
2. **The EMA/ADX selection was more promising than HMM in the inspected holdout.** The EMA/ADX-selected pair returned +24.461R versus -1.490R for the HMM-selected pair on the common holdout period.
3. **The regime mapping interacts with pattern choice.** Bullish N and Ranging N showed different conditional results; a single universal setup rule was not supported by the exploratory analysis.
4. **Swing width changes the trade distribution and outcomes.** In Notebook 05 the 2-bar width had the highest Net R; the 5-bar width had the highest PF and shallowest R drawdown.
5. **Position constraints change the interpretation.** Notebook 05 enforces a maximum of one open position per strategy and permits the two strategies to overlap.
6. **Historical USD profit is highly assumption-dependent.** The fixed-lot account simulation assumes a standard 100 oz contract per lot and ignores execution costs and margin.

## What cannot yet be claimed

- That EMA/ADX or HMM is predictive in a stable, future-proof sense.
- That the best full-sample swing width will remain best in new market data.
- That a USD 1,000 account can safely sustain the reported historical drawdowns.
- That the combined R or fixed-lot equity curves represent a fully realistic portfolio account.
- That any of the current results demonstrate live profitability.

## Why Notebook 04 and Notebook 05 differ

Notebook 05 makes three important methodological changes compared with the earlier swing experiment:

- Strict Bullish N geometry: `P1 < P3 < P2`.
- Regime eligibility before the position slot, so ineligible setups cannot block eligible trades.
- Separate one-position locks for Bullish N and Ranging N, with overlap permitted.

Consequently, the swing-3 full-sample output changed from +60.54R across 614 trades in Notebook 04 to +88.78R across 758 trades in Notebook 05. These results are not a like-for-like test of swing width alone.

## Main conclusion

The current evidence supports retaining the simple EMA/ADX framework as the baseline and treating HMM as an alternative experiment. It does not yet justify selecting a swing width for live deployment. The most important next step is an untouched, chronological validation with broker-specific execution costs and portfolio-level risk controls.
