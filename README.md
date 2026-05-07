# Investment Strategy Agent

Browser-based tools for finding high-quality stocks — fast-growing revenue, low relative valuations, and wide margins.

**Live site:** https://ebfeinstein.github.io/Investment-Strategy-Agent/

---

## Tools

### 📊 Stock Screener
[stock-screener.html](https://ebfeinstein.github.io/Investment-Strategy-Agent/stock-screener.html)

Screens ~300 large-cap US stocks across 11 sectors using the Financial Modeling Prep API.

**Universe filters:**
- Min Revenue — $1B / $5B / $10B / $25B / $50B / $100B+ (defaults to $5B+)
- Sector — narrow to a single sector or scan all

**Screening criteria** (all optional — missing data passes, only present-but-failing data excludes):
- Revenue growth (year-over-year) — default ≥ 5%
- P/E ratio vs. sector average (sector averages computed live from scanned data) — default ≤ 2.0×
- Gross margin — default ≥ 20%
- Net margin — default ≥ 3%

**Features:**
- Composite score (0–100) ranking matched stocks across all criteria
- Fallback display: if 0 stocks match, shows all scanned stocks ranked by score with a warning banner
- Watchlist with localStorage persistence across sessions
- Stock detail drawer — key metrics (ROE, ROA, debt/equity), revenue history chart
- 4-year revenue trend sparkline per stock
- Live price feed — price + day change %, polls every 60 seconds
- Export results to CSV
- API key saved in browser (localStorage, never server-side)
- Sidebar shows stocks-to-scan count and estimated API call usage before running
- Results bar shows "X matched of Y fetched" for transparency

**Requirements:** Free API key from [financialmodelingprep.com](https://financialmodelingprep.com/developer/docs) (250 req/day on free tier)

---

## Running locally

Open any `.html` file directly in a browser. No build step, no server required.

## Tech

Single-file HTML apps — CSS, markup, and JavaScript all inline. No frameworks, no dependencies, no bundler.
