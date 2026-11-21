# 02 — N-Method rule definitions

## Status of the definition

The notebooks use an **operational research interpretation** of the N-Method patterns. It is not a verified reproduction of a proprietary Traders Family rulebook. The patterns, swing confirmation, stop placement, and execution logic are specified explicitly so the experiment can be reproduced.

## Confirmed swing pivots

A swing width `N` means `N` candles on the left and `N` candles on the right of the candidate pivot. The pivot is the center candle in a `2N + 1` window and becomes available only after all right-side confirmation candles have closed.

| Width | Full window | Confirmation delay |
|---:|---:|---:|
| 2 | 5 candles | 2 H1 bars |
| 3 | 7 candles | 3 H1 bars |
| 4 | 9 candles | 4 H1 bars |
| 5 | 11 candles | 5 H1 bars |

The pivot detector uses the same tie-handling convention as Notebook 05: an equal extreme to the right disqualifies the center candidate. The signal is not known at the candidate pivot time; it is known at the confirmation time.

## Bullish N

The latest Notebook 05 definition uses three confirmed pivots:

- P1: swing low.
- P2: swing high after P1.
- P3: a higher swing low after P2.

The strict geometry is:

`P1 < P3 < P2`

The condition means P3 is above the first low but remains below the intervening high, so it is a retracement low rather than a new high.

Trade management:

- Buy-stop entry: `P2 + 0.05 × ATR(14)`.
- Stop-loss: `P3 − 0.05 × ATR(14)`.
- Take-profit: fixed at 2R above entry.

The setup is eligible only if the EMA/ADX regime at the P3 confirmation candle is Bullish in Notebooks 04 and 05.

**Version note:** earlier N-Method notebooks required P3 > P1 but did not explicitly enforce P3 < P2. Notebook 05 tightened the condition to `P1 < P3 < P2`. This changes the candidate population and means Notebook 05 results should not be treated as a direct reproduction of earlier 3-bar results.

## Bearish N

Notebook 01 and Notebook 02 also evaluate the inverse structure:

- P1: swing high.
- P2: swing low.
- P3: lower swing high.
- Sell-stop below P2, stop above P3, target at 2R.

Bearish N is excluded from the selected strategy configuration in Notebooks 04 and 05 because the research focus shifted to Bullish N and Ranging N.

## Ranging N

Ranging N uses four alternating confirmed pivots:

- `H-L-H-L`, or
- `L-H-L-H`.

The two swing highs must be within `0.50 × ATR(14)` of one another, and the two swing lows must also be within `0.50 × ATR(14)`.

The algorithm defines the upper and lower range boundaries and places OCO breakout orders:

- Buy stop above the range high with a `0.05 × ATR(14)` buffer; stop-loss beyond the opposite range boundary.
- Sell stop below the range low with a `0.05 × ATR(14)` buffer; stop-loss beyond the opposite range boundary.

The target is 2R. If both OCO sides are touched in the same H1 candle, the setup is treated as ambiguous and excluded from valid trade statistics.

Ranging N is an operational consolidation-breakout interpretation, not an externally verified proprietary definition. It is eligible only in the EMA/ADX Sideways regime in the selected strategy experiments.

## Shared trade parameters

| Parameter | Value |
|---|---:|
| ATR period | 14 H1 bars |
| Entry buffer | 0.05 ATR |
| Stop buffer | 0.05 ATR |
| Range tolerance | 0.50 ATR |
| Reward target | 2R |
| Pending-order expiry | 20 H1 bars |
| Signal activation | Next H1 bar after confirmation |

## Execution assumptions

- Pivots are only usable after right-side confirmation candles close.
- Pending orders become active on the bar after setup confirmation.
- Entry and exit fills are gap-aware using the OHLC bar open when price gaps through a stop level.
- If SL and TP are both touched in the same candle, SL is assumed to execute first.
- `OPEN_AT_END` trades are marked to the final close, not counted as realized closed trades.
- Spread, commission, swap, and additional slippage are excluded.
