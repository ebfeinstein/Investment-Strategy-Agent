# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project overview

A collection of browser-based investment analysis tools. Each tool is a fully self-contained single HTML file — no build step, no dependencies, no bundler. Hosted on GitHub Pages at https://ebfeinstein.github.io/Investment-Strategy-Agent/

## Running locally

Open any `.html` file directly in a browser. No server required.

## Architecture

Each tool is a single `.html` file with three inline sections: CSS in `<style>`, markup in `<body>`, and JavaScript in a `<script>` block at the bottom.

### Tool index

**index.html** — Landing page linking to all tools. Dark-themed card grid. Add a new card here whenever a new tool is created.

**stock-screener.html** — Stock screener using the Financial Modeling Prep (FMP) stable API. Screens ~300 large-cap US stocks filtered by revenue, sector, and fundamental criteria. Requires a free FMP API key (saved to `localStorage`, never server-side).

Key internals:
- `UNIVERSE[]` — hardcoded list of ~300 large-cap US stocks, each with `{s, n, sec, rev}` (symbol, name, sector, revenue in $B). Revenue is used to pre-filter before API calls.
- `getUniverse()` — applies the Min Revenue and Sector filters to `UNIVERSE` before scanning
- `rows[]` — result set from current scan; each entry holds all metrics plus `score`, `sparkline`, `revenues`, `incomeStmts`, `price`, `dayChange`
- `watchlist` — `Set` of symbols, persisted to `localStorage`
- `computeScores(rows)` — min-max normalizes rev growth (30%), PE ratio (20%), gross margin (25%), net margin (25%) across the result set into a 0–100 composite score
- `fetchRevSpark(key)` — background fetch of `/stable/income-statement` for matched stocks; renders 4-year revenue sparklines
- `pollQuotes()` — live feed; fetches `/stable/quote` per matched stock individually (free tier doesn't support batch), 300ms apart, 60s interval
- `openDetail(symbol)` — fetches `/stable/income-statement` + `/stable/key-metrics`, renders a slide-in drawer with metrics, key ratios, and revenue history chart
- Sector P/E is computed dynamically from fetched P/E values across the scanned universe — no dedicated endpoint needed

### Resilience / filter logic

- A stock is only skipped if **both** `/stable/ratios` and `/stable/income-statement-growth` return empty — partial data is used.
- Field name fallbacks handle stable API variations: `priceEarningsRatio ?? peRatio`, `growthRevenue ?? revenueGrowth ?? growthRevenueRatio`.
- Filter uses null-tolerant logic: a missing metric **passes** the filter (it is not penalized). Only a present metric that fails the threshold excludes the stock.
- Fallback: if 0 stocks pass all criteria, all fetched stocks are shown ranked by composite score with a yellow warning banner.
- Default thresholds are intentionally lenient: rev growth ≥ 5%, P/E vs sector ≤ 2.0×, gross margin ≥ 20%, net margin ≥ 3%.

FMP stable API endpoints used:
- `GET /stable/ratios?symbol={sym}&limit=1` — P/E, gross/net margins
- `GET /stable/income-statement-growth?symbol={sym}&limit=1` — revenue growth
- `GET /stable/income-statement?symbol={sym}&limit=4` — revenue history (sparkline + detail chart)
- `GET /stable/quote?symbol={sym}` — live price and day change
- `GET /stable/key-metrics?symbol={sym}&limit=1` — market cap, ROE, ROA, debt/equity

API budget: ~2 calls per stock scanned + ~1 per match (sparkline) + ~2 per detail panel open. Free tier = 250 req/day.

## Style conventions

- Dark color themes using CSS custom properties (`--bg`, `--panel`, `--panel2`, `--text`, `--accent`, `--dim`, `--border`, `--green`, `--yellow`, `--red`)
- No external fonts, icons, or libraries — everything inline
- Responsive via CSS Grid and `@media` breakpoints
- Score badges, sector tags, and `.good` / `.ok` / `.bad` color classes used consistently

## Adding a new tool

1. Create a new `.html` file following the single-file pattern above
2. Add a card for it in `index.html`
3. Update `README.md` with a description of the new tool
4. Use the same CSS custom properties for visual consistency
