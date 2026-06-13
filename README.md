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

### `de-stock-screener` (primary)

Discovery screener for the **German market (Xetra / Börse Frankfurt)** — DAX 40,
MDAX, SDAX, TecDAX. Finds candidate stocks by valuation, growth, momentum,
dividend yield, or analyst-consensus upside and returns a ranked shortlist.
Use it to *find* German stocks, not to analyze a named one. Web-search based,
no API key. Ships with `references/german-indices.md` (index universe + sector
quick-filters). Built in-repo to cover a gap no off-the-shelf skill fills.

Example prompts:

```
find undervalued DAX stocks
German dividend stocks yielding over 3%
Xetra semiconductor stocks with momentum
best MDAX small-caps by analyst upside
```

### `binance-crypto-screener` (primary)

Discovery screener for crypto, hard-filtered to coins **listed on Binance**.
Screens by market cap, momentum, sector/category, or volume and confirms each
coin trades on Binance before listing it. Uses the CoinPaprika MCP data (with
web-search fallback) and hands winners to `crypto-market-search` / the
`crypto-analyst` agent for deep dives.

Example prompts:

```
find Binance altcoins with strong momentum
top Binance coins by market cap
Binance DeFi tokens to watch
which Binance coins are oversold right now?
```

## Layout

```
.claude/
  skills/
    de-stock-screener/      # PRIMARY: find German (Xetra) stocks by criteria
    binance-crypto-screener/# PRIMARY: find Binance-listed crypto by criteria
    us-stock-analysis/      # deep analysis of a named stock (any ticker)
    crypto-market-search/   # crypto market lookup/analysis (SKILL.md)
  agents/
    crypto-analyst.md       # crypto analyst subagent
.mcp.json                   # CoinPaprika MCP server (SSE, no key needed)
```

Skills load automatically when you open this repo in Claude Code. On first use, approve the `coinpaprika` MCP server when prompted.

## Disclaimer

For educational and research purposes only. Not financial advice.
