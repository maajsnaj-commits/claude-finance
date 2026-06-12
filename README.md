# claude-finance

Claude Code skills for analyzing stock prices, price predictions, and cryptocurrency markets.

## Installed skills

### `us-stock-analysis`

Comprehensive US stock analysis — invoked automatically when you ask Claude to analyze a ticker.

- **Fundamental analysis**: financial metrics, business quality, valuation (P/E, PEG, EV/EBITDA, fair-value range)
- **Technical analysis**: trend, support/resistance, RSI, MACD, Bollinger Bands, chart patterns
- **Predictions**: analyst ratings and price targets, probabilistic scenarios, Buy/Hold/Sell recommendation with target price and timeframe
- **Comparisons & reports**: side-by-side stock comparisons and full investment reports

No API key required — data is gathered via web search (Yahoo Finance, MarketWatch, SEC filings, etc.).

Example prompts:

```
analyze AAPL
compare TSLA vs NVDA
give me a full investment report on Microsoft
what's the technical outlook for AMD?
```

Source: [tradermonty/claude-trading-skills](https://github.com/tradermonty/claude-trading-skills) (MIT)

### `crypto-market-search`

Cryptocurrency market search and analysis covering 59,000+ coins and 1,100+ exchanges via the CoinPaprika MCP server (configured in `.mcp.json`, hosted, free tier, no API key).

- Prices, market cap, volume, OHLCV history for any coin
- Global market overview (total cap, BTC dominance)
- Coin fundamentals, exchange listings, token lookup by contract address
- Category/tag exploration and currency conversion

A `crypto-analyst` agent (`.claude/agents/crypto-analyst.md`) is also included for deeper data-driven crypto analysis and risk assessment.

Example prompts:

```
what's the price of bitcoin?
analyze ethereum's market position
show me the top 10 coins by market cap
get OHLCV history for solana over the last month
```

Source: [coinpaprika/claude-marketplace](https://github.com/coinpaprika/claude-marketplace) (MIT)

## Layout

```
.claude/
  skills/
    us-stock-analysis/      # stock analysis skill (SKILL.md + reference docs)
    crypto-market-search/   # crypto market skill (SKILL.md)
  agents/
    crypto-analyst.md       # crypto analyst subagent
.mcp.json                   # CoinPaprika MCP server (SSE, no key needed)
```

Skills load automatically when you open this repo in Claude Code. On first use, approve the `coinpaprika` MCP server when prompted.

## Disclaimer

For educational and research purposes only. Not financial advice.
