# Glossary

| Term | Meaning in this project |
|---|---|
| ADX | Average Directional Index; used to estimate trend strength, not direction. |
| ATR | Average True Range; used to scale entry/stop buffers and range tolerance. |
| Bullish N | Low → High → Higher Low structure followed by a buy-stop breakout. Latest strict rule: `P1 < P3 < P2`. |
| Bearish N | High → Low → Lower High structure followed by a sell-stop breakdown. Tested in Notebooks 01–02, excluded from the selected configuration. |
| HMM | Hidden Markov Model; a probabilistic latent-state model. It is used here for regime classification, not direct price forecasting. |
| OCO | One-Cancels-the-Other; one pending order is cancelled after the opposite order triggers. |
| P1 / P2 / P3 | Sequential confirmed pivot points in an N pattern. |
| PF / Profit factor | Gross winning P/L divided by absolute gross losing P/L. It is not the target risk-to-reward ratio. |
| Regime | A descriptive market-state label: Bullish, Bearish, Sideways, or Unclassified. |
| R / Net R | Return expressed in units of initial trade risk. It is not dollars or percentage account return. |
| Swing width | Number of candles on each side of a candidate pivot used to confirm the local extreme. |
| Sideways | In the EMA/ADX baseline, ADX(14) < 20. |
| Signal date | Timestamp at which the full pattern has been confirmed and the strategy can act on the setup. |
| `OPEN_AT_END` | A trade still open at the final data bar; marked to final close rather than treated as a realized closed trade. |
