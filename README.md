# Pine Script Portfolio — Asad Kurbonov

TradingView indicators and strategies I've built for MNQ (Micro Nasdaq futures) and QQQ, written in **Pine Script v6**. About **22,000 lines** across 12 scripts.

These are research tools and engineering work, not trading advice or a promise of profit. Backtest results depend on the market regime, and I test my own ideas hard. Many of them fail out of sample, and the comments in the code say so.

> **Available for custom work:** indicators, strategies, alerts/webhooks, converting a setup into code, backtesting, and fixing broken scripts.

---

## What's here

### `mnq-market-reasoning/`: a multi-script "eyes + brain" system

The largest project. One script maps the market and another makes decisions from that map. They're connected through hidden **BUS plots** (`input.source`) so each part can be tested on its own.

| Script | Type | What it does |
|---|---|---|
| **Bot1 Market Reasoning Map v0.6.1** | Indicator | Maps support/resistance and decision zones. Clusters nearby levels into one zone, keeps a bounded registry and retires old zones. Never votes long/short. |
| **Universal Market Eyes v1.5** | Indicator | A factual context layer: levels, footprint (delta, POC/value migration, imbalances), multi-timeframe context and cross-market lead/lag. It exposes 23 BUS channels and keeps them backward-compatible across versions. |
| **Bot2 Market Reasoning Strategy v0.30.4** | Strategy | Reads the BUS facts and trades auction behavior at mapped levels (failed auctions, acceptance) with a thesis/invalidation model, a live trade panel and visible plans. About 4,100 lines. |
| **Auction Edge v1.0** | Strategy | A distilled, standalone version of Bot2 that keeps only the parts the research supported (fading failed auctions at external levels, retests), with TP1 + runner exits. |

### `qqq-options-engine/`: QQQ signals for an options workflow

| Script | Type | What it does |
|---|---|---|
| **Option1 QQQ Market Eyes v2.0** | Indicator | Facts-only map for QQQ: zones, footprint, value and VWAP. It feeds Option2 through BUS plots. |
| **Option2 Evolving Auction Interpreter v1.9.1** | Strategy | Interprets Option1's auction context into CALL/PUT/WAIT decisions with a session and risk framework. |
| **QQQ Precision v2.17** | Strategy | A rule-based QQQ engine with an options trading window and an adaptive stop-day window (stops trading after a bad day). |
| **QQQ Reaction Scalper v1.10A** | Strategy | A 5-minute reaction scalper that exits when the expected reaction fails to happen. |

### `ny-session-tools/`: NY-open tools and research labs

| Script | Type | What it does |
|---|---|---|
| **NY Session Reasoning Scalper v0.3** | Strategy | Builds a premarket plan, classifies the market (trend/range), picks matching setups, and reads the 1-minute "candle story" inside each 5-minute signal candle. Includes a research mode that tags every trade for export. |
| **Move Radar v1.1** | Indicator | Predicts **when** a big move is likely and **how big**, not which way. It scores itself walk-forward with no lookahead and shows its real hit rate. |
| **Probability Assist v2.4.6D** | Strategy | A LONG/SHORT/WAIT engine on 5-minute bars with confirmed 15-minute context and quality gates, including a London low-volume pullback filter. |
| **Concepts Lab v0.1** | Strategy | Research tool. Trades textbook concepts (EMAs, VWAP, ICT ideas, round numbers, opening range, RSI) one at a time with a fixed bracket and writes the concept into each trade name, so the exported trade list can rank which concepts actually work. |

---

## How I build

- **No lookahead.** Higher-timeframe data uses completed bars only. Levels appear only once they would have been knowable live.
- **Separation of concerns.** "Eyes" scripts report facts, "brain" scripts decide, and they connect through versioned BUS plots.
- **Honest testing.** Commission and slippage are set, bar magnifier is on, I compare against random-direction baselines, and I test out of sample: discovery on one period, validation on another, and a final holdout.
- **Readable code.** Every script has a header explaining what it does, why, and what changed in each version.

## Using a script

1. Open TradingView → **Pine Editor** → paste the file → **Add to chart**.
2. Most scripts are tuned for **MNQ1! or QQQ on 5-minute charts** with Pacific/New York session times.
3. For the two-script systems, add the "eyes" indicator first, then connect the strategy's BUS inputs to the matching plots.

---

**Contact:** open an issue here, or message me on Fiverr/Upwork.

*© Asad Kurbonov. Shared for viewing and portfolio purposes. Please ask before reusing or reselling.*
