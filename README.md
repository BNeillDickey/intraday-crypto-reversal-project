# Intraday Reversal and Momentum Around the US Equity Session in Crypto Markets

Do the US equity open (09:30 ET) and close (16:00 ET) leave a tradeable footprint in crypto returns? This notebook tests that idea, which comes from intraday-periodicity results in equities (Heston, Korajczyk & Sadka 2010; Bogousslavsky 2016; Gao, Han, Li & Zhou 2018).

**Tests**
1. Cross-sectional reversal across 25 spot crypto pairs around the US close (signal 15:30–16:00 ET, hold 16:00–17:00).
2. The same design around the US open (signal 09:30–10:00, hold 10:00–11:00).
3. Spot BTC/ETH ETFs: BTC-vs-ETH relative momentum into the close, Gao et al. intraday time-series momentum, and a twin-ETF placebo (IBIT vs FBTC).

**Result.** No tradeable effect. The 25-pair crypto universe does show a small gross reversal (about 3.8 bps a day, Sharpe ≈ 1.6 in 2020–24). All of it sits in the first 15-minute bar after the signal, and it vanishes when that bar is skipped. The same pattern appears at every hour of the day, not just around the US open and close. That makes it bid-ask bounce, which costs more to capture than it pays. The spot BTC/ETH ETFs show no intraday momentum once the windows are non-overlapping and the books are genuinely dollar-neutral. The notebook's Findings section has the full numbers.

**Methodology**
- Signal and hold windows never share a bar; the code asserts this.
- Long/short books are dollar-neutral, with equal names per leg (asserted in code).
- Primary specifications are fixed in advance. Window sweeps select on the early sample, evaluate on the most recent two years, and report a Deflated Sharpe Ratio (Bailey & López de Prado 2014).
- Every Sharpe ratio is reported with its standard error (Lo 2002). Crypto annualises over 365 days, ETFs over 252.
- Costs are charged on every round trip: commission, half-spread and square-root market impact. Spread assumptions are cross-checked against Roll (1984).
- Bid-ask bounce is tested directly: first-bar vs remaining-hold P&L, Roll spreads, and the same strategy run at every half-hour of the day.

**Running it**
```bash
pip install -r requirements.txt
jupyter notebook Intraday_Crypto_Reversal_Project.ipynb
```
The first run downloads about six years of 15-minute klines from the public Binance archive (data.binance.vision) plus two years of hourly ETF bars from Yahoo Finance, and caches them in `data_cache/`. To use Binance.US instead, set `DATA_SOURCE = "binance_us"` in the first code cell.
