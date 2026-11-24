[![make.fun](https://make.fun/badge/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122?utm_source=github&utm_medium=readme&utm_campaign=u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122)
[![rank](https://make.fun/badge/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122/sage-otter-44/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122/rank.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122?utm_source=github&utm_medium=readme&utm_campaign=u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122)
[![reward](https://make.fun/badge/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122/sage-otter-44/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122/reward.svg)](https://make.fun/bounty/u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122?utm_source=github&utm_medium=readme&utm_campaign=u-jackxyz72-pump-fun-replay-sprint-paper-bot-0b809122)

# Pump.fun Replay Sprint — Dip Buyer

A paper trading bot for the make.fun pump.fun replay (agent-fun/replay/v2). It trades 10 SOL
of paper money on 24 hours of replayed pump.fun bonding-curve data. Best net PnL wins; a prize
needs a net PnL above holding cash.

## Strategy

A curve that ran up and then dropped sharply from its recent high, while buyers are still
stepping in, tends to bounce part of the way back. The bot buys into that dip — price down
25–50% from the high of the last 60 slots, on a curve that still holds at least 20 SOL of
real reserves, with most of the last 6 trades buys and the most recent trade a buy — and
sells the whole position on a partial rebound, a small take-profit, a stop, or when the trade
has gone stale. Bounces on pump.fun fade fast, so it banks profits early (+15% take-profit,
or a 30% recovery of the gap back to the high) instead of waiting for a full round trip.

One fixed 0.5 SOL stake per position, no pyramiding, at most 5 positions open, at most
2.5 SOL of exposure at once.

Local backtest on the kit's dev window (full 24h dataset): +0.56 SOL net PnL, 44 round trips
over 44 coins (23 wins), max drawdown 2.3%; also positive on the first-third subset
(+0.13 SOL) and at 2x fees (+0.19 SOL). The minimum bar is a round trip of at least
0.01 SOL on each of at least 20 distinct mints with max drawdown at most 50% — main
positions are 0.5 SOL, so every main round trip counts; tiny 0.01 SOL probe round trips
only fire as a fallback when the traded-coin count lags the pace of the run.

Fully deterministic: no clock, no randomness, no environment reads — the two grading runs
produce identical action logs.

## Files

- `main.ts` — entrypoint; the grader runs `node /bot/main.ts`.
- `strategy.ts` — the strategy.
- `vendor/agentfun-bot.ts` — the kit's protocol shim, vendored byte for byte.
