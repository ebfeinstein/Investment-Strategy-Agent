# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project overview

A collection of browser-based investment analysis tools. Each tool is a fully self-contained single HTML file — no build step, no dependencies, no bundler. Hosted on GitHub Pages.

## Running locally

Open any `.html` file directly in a browser. No server required.

## Architecture

Each tool is a single `.html` file with three inline sections: CSS in `<style>`, markup in `<body>`, and JavaScript in a `<script>` block at the bottom.

### Tool index

**index.html** — Landing page linking to all tools. Dark-themed card grid. Add a new card here whenever a new tool is created.

**stock-screener.html** — Stock screener using the Financial Modeling Prep (FMP) API. Screens stocks by revenue growth, P/E ratio relative to sector average, gross margin, net margin, and market cap. Requires a free FMP API key entered at runtime (not stored anywhere server-side).

Key internals:
- `rows[]` — result set from current scan; each entry holds all metrics plus `score`, `sparkline`, `priceHistory`
- `watchlist` — `Set` of symbols, persisted to `localStorage`
- `computeScores(rows)` — min-max normalizes each metric across the result set, then computes a weighted composite 0–100 score
- `fetchSparklines()` — runs in the background after a scan; patches sparkline `<td>` cells in the DOM as data arrives
- `openDetail(symbol)` — fetches `/v3/profile` and `/v3/income-statement` for the clicked stock and renders a slide-in drawer
- FMP endpoints used: `/v3/stock-screener`, `/v3/sector_price_earning_ratio`, `/v3/ratios/{sym}`, `/v3/income-statement-growth/{sym}`, `/v3/historical-price-full/{sym}`, `/v3/profile/{sym}`, `/v3/income-statement/{sym}`

## Style conventions

- Dark color themes using CSS custom properties (`--bg`, `--panel`, `--panel2`, `--text`, `--accent`, `--dim`, `--border`, etc.)
- No external fonts, icons, or libraries — everything inline
- Responsive via CSS Grid and `@media` breakpoints
- Score badges, sector tags, and good/ok/bad color classes (`var(--green)`, `var(--yellow)`, `var(--red)`) used consistently across tools

## Adding a new tool

1. Create a new `.html` file following the single-file pattern above
2. Add a card for it in `index.html`
3. Use the same CSS custom properties for visual consistency
