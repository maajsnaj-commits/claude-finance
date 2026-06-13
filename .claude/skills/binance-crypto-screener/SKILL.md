---
name: binance-crypto-screener
description: Discover cryptocurrencies that trade on Binance, screened by market cap, momentum/price change, category/sector, or volume. Returns a ranked candidate list and CONFIRMS each coin is listed on Binance before including it. Use when the user wants to FIND crypto to watch/buy (e.g. "find Binance altcoins with momentum", "top Binance coins by market cap", "Binance DeFi tokens", "which Binance coins are oversold"), not analyze one named coin. Uses CoinPaprika MCP data with web-search fallback.
---

# Binance Crypto Screener

## Overview

Discovery tool for cryptocurrencies, hard-filtered to assets **listed on
Binance**. Screen the market by the user's criteria, verify Binance listing,
and return a ranked shortlist. This skill FINDS coins; pass winners to the
`crypto-market-search` skill / `crypto-analyst` agent for a deep dive.

The **Binance-listed constraint is mandatory** — never include a coin in the
final list without confirming it trades on Binance.

## Data Source

Primary: **CoinPaprika MCP** (server `coinpaprika`, configured in `.mcp.json`).
Key tools:
- `getTickers(limit, quotes)` — top coins by market cap (the screening pool)
- `getTickersById(coinId, quotes)` — price, % change 1h/24h/7d/30d, volume, mcap
- `getTags()` / `getTagById(tagId, additionalFields:"coins")` — sector/category screen (DeFi, L1, AI, memes…)
- `getCoinExchanges(coinId)` — **listing check: confirm `binance` appears**
- `getCoinMarkets(coinId, quotes)` — Binance trading pairs (e.g. BTC/USDT) + volume
- `getCoinOHLCVHistorical(coinId, start)` — candles for momentum/trend
- `getGlobal()` — total mcap, BTC dominance (market regime context)

If the MCP server is not loaded (it activates on session restart after
`.mcp.json` is added), **fall back to web search** of CoinPaprika /
CoinMarketCap / CoinGecko plus Binance's own markets page to verify listings.

## When to Use

- "Find Binance altcoins with strong momentum"
- "Top Binance coins by market cap" / "biggest Binance gainers this week"
- "Binance DeFi / AI / L1 tokens to watch"
- "Which Binance coins are oversold right now?"
- Any request to *discover/screen/scan* crypto subject to a Binance listing

Do NOT use for deep analysis of one named coin — use `crypto-market-search`.

## Screening Criteria Mapping

| User phrase | Filter |
|---|---|
| "momentum" / "gainers" / "strong" | Positive 7d & 30d % change, rising volume |
| "oversold" / "dip" / "beaten down" | Negative 30d change but above key support; low RSI from OHLCV |
| "large" / "blue chip" / "safe" | Top 20 by market cap |
| "small cap" / "moonshot" | Lower market-cap band (higher risk — flag it) |
| sector words (DeFi, L1, AI, gaming, memes, RWA) | `getTagById` for that category |
| "high volume" / "liquid" | Sort by 24h volume; check Binance pair depth |
| "by market cap" (default) | Rank by market cap |

If no criteria given, default to **top coins by market cap that are
Binance-listed**.

## Workflow

### Step 1: Build the screening pool
- Broad scan: `getTickers(limit: 50-100)` for the market-cap-ranked pool, OR
- Sector scan: `getTags()` → `getTagById(tagId, additionalFields:"coins")`

### Step 2: Apply criteria
Filter the pool by the mapped metrics (% change, volume, mcap band). Use
`getCoinOHLCVHistorical` for trend/momentum or oversold checks when needed.
Capture market regime from `getGlobal()` (BTC dominance, total mcap trend).

### Step 3: Verify Binance listing (mandatory)
For every surviving candidate, call `getCoinExchanges(coinId)` and keep it
**only if `binance` is present**. Optionally `getCoinMarkets` to record the
specific Binance pair (e.g. `BTC/USDT`) and its volume. Drop anything not on
Binance.

### Step 4: Rank and output
Keep the top 5-10. Present a ranked table:

| Rank | Symbol | Name | Binance pair | Price | 7d % | 30d % | Market cap | Why it screened |
|---|---|---|---|---|---|---|---|---|

Then add:
- **Market regime note** (BTC dominance / risk-on vs risk-off from `getGlobal`)
- **2-3 line rationale** for the top pick
- **Data timestamp** and source (MCP vs web fallback)
- Risk flag if the market is in a downtrend or picks are small-cap
- Offer a deep-dive via `crypto-market-search` / `crypto-analyst` on any name

## Output Guidelines
- Always show the confirmed Binance pair — this proves the constraint is met
- Bold the metric that earned each coin its spot
- State data timestamp and whether MCP or web fallback was used
- Flag elevated risk plainly in bear-market conditions
- End with: *Educational/research purposes only — not financial advice.*
