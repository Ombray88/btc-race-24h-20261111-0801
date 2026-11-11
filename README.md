# btc-race-24h-20261111-0801

[![make.fun · btc-race-24h-20261111-0801](https://make.fun/badge/btc-race-24h-20261111-0801.svg)](https://make.fun/bounty/btc-race-24h-20261111-0801?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261111-0801)
[![rank](https://make.fun/badge/btc-race-24h-20261111-0801/Ombray88/btc-race-24h-20261111-0801/rank.svg)](https://make.fun/bounty/btc-race-24h-20261111-0801/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261111-0801)
[![reward](https://make.fun/badge/btc-race-24h-20261111-0801/Ombray88/btc-race-24h-20261111-0801/reward.svg)](https://make.fun/bounty/btc-race-24h-20261111-0801?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261111-0801)

A dip-bounce contrarian bot that trades Coinbase Wrapped BTC (cbBTC) on 1-minute on-chain prices with $1,000 of paper money: it buys the deeper hourly flushes only once the price has started to bounce off the trailing 60-minute low, and sells on a +60 bps target, a −80 bps stop, or after 240 bars.

## Run it

```sh
node main.ts
```

The bot speaks `agent-fun/bars-replay/v1` on stdin/stdout (see the competition kit): one JSON line per minute in, one reply with its orders out. It needs no install step — only the Node 22 standard library plus the vendored `vendor/makefun-bars.ts` shim.

## Strategy

Hourly returns of cbBTC mean-revert (autocorrelation about −0.37 on the dev window), while a full round trip costs about 26 bps in fees, slippage and price impact. The bot therefore trades rarely and only the deeper flushes:

- **Entry:** rolling 60-minute return ≤ −40 bps **and** the close ≥ +10 bps above the trailing 60-minute low → buy with all cash.
- **Exit:** close ≥ +60 bps over the entry fill, or ≤ −80 bps under it, or 240 bars held → sell the whole position.

All decisions are exact bigint arithmetic on the bar closes, so two runs on the same bars produce the same orders byte for byte. Local dev-window grade: net PnL about +$12.71 on 5 round trips (win rate 0.80), max drawdown about 1.0%. Every baseline loses on the same window (buy-and-hold −$13.44, momentum −$12.03, ema-cross −$89.29).

---

<sub>Entered on [make.fun](https://make.fun/?ref=9w5z28cm&utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261111-0801) · AI agents build software for cash rewards</sub>
