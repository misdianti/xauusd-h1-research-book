# 10 — Validation and next steps

## 1. Freeze the current rules

Before the next evaluation, document and freeze:

- EMA(50), EMA(200), ADX(14), threshold 20.
- ATR(14), entry and stop buffers of 0.05 ATR.
- Range tolerance of 0.50 ATR.
- 2R target and 20 H1-bar pending-order expiry.
- Bullish N strict geometry `P1 < P3 < P2`.
- Ranging N four-pivot structure and OCO execution.
- The chosen swing width for the test (do not pick using the final test outcome).
- One active position per strategy, with Bullish N and Ranging N allowed to overlap.

## 2. Separate development, validation, and final test periods

A chronological split can be used to reserve a final unseen segment. For example, a development/validation/final-test split may be 60% / 20% / 20%, but the exact split should be decided before inspecting final-period results.

Because all four swing widths have already been evaluated on the full historical sample, selecting among them using the same sample creates selection bias. The last segment should remain unopened until the candidate is frozen.

## 3. Evaluate stability, not just a single Net R total

Report at minimum:

- Trade count and win rate.
- Net R, average R, and profit factor.
- Maximum drawdown and recovery behaviour.
- Maximum consecutive losses.
- Bullish N and Ranging N contributions separately.
- Position concurrency and overlap frequency.
- Performance by subperiod and market regime.

## 4. Add broker-specific costs and execution details

The OHLC backtest excludes spread, commission, swap, and additional slippage. Add actual broker specifications before evaluating whether a result could survive trading costs. Confirm the source timestamp timezone, symbol naming, contract size, tick size, tick value, stop levels, filling policy, account currency, and hedging/netting mode.

## 5. Portfolio and account risk

The current Notebook 05 account simulation does not model shared-capital margin usage or floating exposure. Before live trading, add:

- Simultaneous open-position P/L and floating drawdown.
- Total risk-at-stop across both strategies.
- Margin used and available margin.
- Broker stop-out and forced liquidation rules.
- Account-level loss limits and daily/weekly stop conditions.

## 6. Forward test

After historical rules are frozen and tested, record new signals prospectively in MT5 demo or another controlled forward-testing environment. Compare actual signal timestamps, entry prices, SL/TP, spread and slippage against the Python research logic.

## Decision rule

Do not choose a parameter solely because it has the highest full-sample Net R. Prefer a configuration that is understandable, consistent across periods, survives realistic costs, has tolerable drawdown, and can be implemented consistently between Python and MT5.
