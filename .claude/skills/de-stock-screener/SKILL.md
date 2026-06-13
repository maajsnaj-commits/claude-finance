---
name: de-stock-screener
description: Discover German (Xetra-listed) stocks matching criteria — DAX 40, MDAX, SDAX, TecDAX. Screen by valuation, growth, momentum, dividend yield, and analyst-consensus upside, then return a ranked candidate list. Use when the user wants to FIND German stocks (e.g. "find undervalued DAX stocks", "German dividend ideas", "Xetra momentum breakouts", "cheap German small caps"), not analyze one named ticker. Web-search based — no API key required.
---

# German (Xetra) Stock Screener

## Overview

Discovery tool for the German market. Translate a natural-language screen
into criteria, scan the relevant German index universe via web search, and
return a ranked shortlist of candidate tickers with the metrics that matter.
This skill FINDS stocks; hand the winners to `us-stock-analysis`-style deep
analysis (it works for any ticker) for a full write-up.

Scope is restricted to **German exchanges (Xetra / Börse Frankfurt)** — DAX
40, MDAX, SDAX, TecDAX constituents. Report the Xetra ticker (e.g. `SAP`,
`AIXA`, `SZG`) and ISIN where possible; quote prices in **EUR**.

## When to Use

- "Find undervalued German stocks" / "günstige DAX-Aktien finden"
- "German dividend stocks yielding over 3%"
- "Xetra momentum / breakout candidates"
- "Best MDAX small-caps for 2026"
- "Which German semiconductor / defense / green-energy stocks look strong?"
- Any request to *discover/screen/scan* German equities by criteria

Do NOT use for deep analysis of a single named German ticker — use the
stock-analysis workflow for that.

## Screening Criteria Mapping

Translate the user's words into measurable filters:

| User phrase | Filter |
|---|---|
| "undervalued" / "cheap" / "günstig" | P/E below sector median, P/B < 2, EV/EBITDA below 5-yr avg |
| "growth" | Revenue & EPS growth > 10% YoY, positive forward estimates |
| "dividend" / "income" | Yield ≥ 3%, payout sustainable, ≥ 3-yr dividend track record |
| "momentum" / "breakout" | Price above 50- & 200-day MA, near 52-week high, RS strong |
| "oversold" / "dip" | RSI < 35, price below 20/50-day MA but uptrend intact |
| "quality" | ROE > 15%, net cash or low debt, stable margins |
| "high upside" | Largest gap to average analyst consensus target |
| sector words (semis, defense, autos, green energy, banks) | Filter universe to that sector |

If the user gives no criteria, default to **consensus-upside ranking**:
sort candidates by average analyst 12-month target vs current price.

## Workflow

### Step 1: Define universe
Pick the index set implied by the request (default: DAX 40 + MDAX):
- **DAX 40** — large caps (SAP, Siemens, Allianz, Deutsche Telekom, …)
- **MDAX** — mid caps
- **SDAX** — small caps
- **TecDAX** — tech-focused
Read `references/german-indices.md` for constituents and sector tags.

### Step 2: Gather data via web search
For each candidate gather, from quality German/EU sources:
- Current EUR price, 52-week range, YTD performance
- Valuation: P/E, P/B, EV/EBITDA, dividend yield
- Growth: revenue/EPS trend
- Analyst consensus: average target price + Buy/Hold/Sell count
- Recent catalysts/news

**Preferred sources:** boerse-frankfurt.de, finanzen.net, marketscreener.com,
stockanalysis.com, Yahoo Finance (`.DE` suffix, e.g. `SAP.DE`), onvista.de,
Kepler Cheuvreux / broker research roundups.

**Search tips:** combine ticker/ISIN + metric (`"Aixtron Aktie Kursziel 2026"`,
`"MDAX undervalued stocks 2026"`, `"DAX dividend yield screener"`). For a
sector scan, search the sector + "DAX MDAX 2026 analyst top picks".

### Step 3: Filter and rank
- Apply the mapped filters; drop non-matches
- Rank by the primary objective (upside, yield, momentum score, etc.)
- Keep the top 5-10

### Step 4: Output
Present a ranked table, most attractive first:

| Rank | Ticker (Xetra) | Company | Sector | Price (€) | Key metric(s) | Consensus target / upside | Rating | Why it screened |
|---|---|---|---|---|---|---|---|---|

Then add:
- **2-3 line rationale** for the top pick
- **Data date** and sources
- Note any candidate failing a secondary check (e.g. high debt)
- Offer to run a full deep-dive on any shortlisted name

## Output Guidelines
- EUR formatting, Xetra tickers + ISIN where available
- Bold the metric that earned each stock its place
- State the screen date and that data is web-sourced
- Be explicit about filters applied and universe scanned
- End with: *Educational/research purposes only — not financial advice.*
