---
name: day-trading-advisor
description: Generate a day trading buying report for any stock ticker. Use when the user wants a stock analysis, buying report, trading report, stock research, or day trading advice for a specific ticker symbol.
argument-hint: "[ticker-symbol]"
---

# Day Trading Advisor

Generate a comprehensive day trading buying report for the stock ticker provided in `$ARGUMENTS`.

## Steps

1. **Research the stock** using web search to gather current data:
   - Current price, day range, 52-week range, volume
   - Recent price action and percentage moves
   - Key financial metrics (P/E, EPS, market cap, margins)
   - Recent earnings results vs expectations
   - Forward guidance if available

2. **Gather catalysts and news:**
   - Search for recent news articles (last 7 days)
   - Identify positive catalysts (product launches, approvals, partnerships, buybacks)
   - Identify negative catalysts (guidance cuts, competition, regulatory risk)
   - Note upcoming events (earnings dates, trial readouts, conferences)

3. **Analyze technicals:**
   - Key moving averages (5, 10, 20, 50, 100, 200-day)
   - RSI, MACD signals
   - Support and resistance levels
   - Volume trends

4. **Compile analyst sentiment:**
   - Consensus rating (Buy/Hold/Sell)
   - Average price target and range
   - Recent upgrades or downgrades

5. **Produce the report** in the output format below

6. **Save the report** to `.claude/skills/day-trading-advisor/reports/<TICKER>-<YYYY-MM-DD>.md`

## Rules

- ALWAYS include a disclaimer that this is not financial advice
- NEVER recommend specific position sizes or dollar amounts
- Present both bull and bear cases objectively
- Use current data only — do not fabricate prices or metrics
- Flag when data may be delayed or from a previous trading session
- Include sources for all data points

## Output Format

```markdown
# [TICKER] — Day Trading Buying Report
**Date:** YYYY-MM-DD
**Last Price:** $X.XX (±X.XX / ±X.XX%)

## Snapshot
| Metric | Value |
|---|---|
| Market Cap | ... |
| P/E (TTM) | ... |
| EPS (TTM) | ... |
| 52-Week Range | ... |
| Avg Volume | ... |
| Beta | ... |

## Recent Price Action
<Summary of recent moves, context for current price level>

## Catalysts
### Bullish
1. ...
### Bearish
1. ...

## Technical Levels
| Level | Price | Notes |
|---|---|---|
| Resistance 3 | ... | ... |
| Resistance 2 | ... | ... |
| Resistance 1 | ... | ... |
| **Current** | ... | ... |
| Support 1 | ... | ... |
| Support 2 | ... | ... |
| Support 3 | ... | ... |

## Analyst Consensus
| Source | Rating | Target |
|---|---|---|
| ... | ... | ... |

## Bull vs Bear Case
**Bull:** ...
**Bear:** ...

## Day Trading Outlook
<Short-term directional bias with key levels to watch>

---
*Disclaimer: This report is for informational purposes only and does not
constitute financial advice. Always do your own due diligence.*
```
