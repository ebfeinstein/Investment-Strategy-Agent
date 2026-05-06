# Investment Strategy Agent

Browser-based tools for finding high-quality stocks — fast-growing revenue, low relative valuations, and wide margins.

**Live site:** https://ebfeinstein.github.io/Investment-Strategy-Agent/

---

## Tools

### 📊 Stock Screener
[stock-screener.html](https://ebfeinstein.github.io/Investment-Strategy-Agent/stock-screener.html)

Screens 120 large-cap US stocks across 11 sectors using the Financial Modeling Prep API.

**Screening criteria:**
- Revenue growth (year-over-year)
- P/E ratio vs. sector average
- Gross margin
- Net margin

**Features:**
- Composite score (0–100) ranking stocks across all criteria
- Watchlist with localStorage persistence
- Stock detail drawer — key metrics, revenue history chart
- Revenue trend sparkline per stock
- Live price feed (price + day change %, updates every 60s)
- Export results to CSV
- API key saved in browser

**Requirements:** Free API key from [financialmodelingprep.com](https://financialmodelingprep.com/developer/docs)

---

## Running locally

Open any `.html` file directly in a browser. No build step, no server required.

## Tech

Single-file HTML apps — CSS, markup, and JavaScript all inline. No frameworks, no dependencies, no bundler.
