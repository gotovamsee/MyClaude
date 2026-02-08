---
name: morning-briefing
description: Pre-market morning briefing that scans your watchlist, checks US futures, OMXC25 direction, and flags the best day trading setups before Copenhagen opens. Use when the user asks for a morning scan, pre-market check, or daily briefing.
---

# Morning Briefing

Deliver a compact pre-market scan of the watchlist with actionable setups.

## Trader Profile

- **Location**: Denmark | **Style**: Swing-leaning day trader
- **Watchlist**: NOVO-B, ZEAL, VWS, DANSKE (Nasdaq Copenhagen)
- **Also monitors**: US markets (NYSE/NASDAQ)
- **Account**: Saxo (real), Trading212 (practice)
- **Copenhagen open**: 09:00 CET | **US open**: 15:30 CET

## Steps

### 1. Market Pulse (2 searches)

Use **WebSearch only** — never WebFetch on finance sites.

Run in parallel:
1. `"S&P 500 futures" OR "NASDAQ futures" premarket today` — US direction
2. `"OMXC25" OR "Copenhagen" stock market today` — Danish index trend

Summarize in 2 sentences: where are US futures and OMXC25 pointing?

### 2. Watchlist Scan (1 search per ticker, run in parallel)

For each of NOVO-B.CO, ZEAL.CO, VWS.CO, DANSKE.CO:
- WebSearch `"{TICKER}" stock price news today`
- Extract: price, overnight move %, any news, key level to watch today

### 3. Rank Setups

From the scan, pick the **top 1–3 tickers** with the clearest setup.
For each, give a one-line thesis: direction, entry zone, stop, target.

### 4. Calendar Check (1 search)

WebSearch `Denmark economic calendar OR earnings today {date}`
Flag any ECB/Fed decisions, Danish data releases, or earnings from watchlist stocks.

## Rules

- Max 7 WebSearch calls total (2 market + 4 watchlist + 1 calendar)
- NEVER use WebFetch — finance sites return JavaScript, not data
- Keep it compact — this is a quick-read briefing, not full analysis
- If a ticker has a strong setup, suggest using `/day-trading-advisor {TICKER}` for deep analysis
- ALWAYS end with the disclaimer

## Output Format

```
## Morning Brief — {date}, {time} CET

### Market Pulse
- US Futures: {direction and %} | OMXC25: {direction}
- Vibe: {one-liner mood}

### Watchlist
| Ticker | Price | Overnight | News | Key Level | Setup? |
|--------|-------|-----------|------|-----------|--------|
| NOVO-B | | | | | |
| ZEAL   | | | | | |
| VWS    | | | | | |
| DANSKE | | | | | |

### Top Setups
1. **{TICKER}** — {direction} near {price}, stop {price}, target {price}
   Run `/day-trading-advisor {TICKER}` for full analysis.

### Calendar
{Events for today, or "Nothing major."}

*Not financial advice. Trade at your own risk.*
```
