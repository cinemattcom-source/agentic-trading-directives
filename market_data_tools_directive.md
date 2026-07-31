# Market Data & Scanner Tools Directive — v1.0 (DRAFT, pending backtest validation)

**Status:** Drafted from Robinhood's published agent tool list (Trading with your agent, reviewed 2026-07-27). Not yet backtested — hold at DRAFT status alongside `intraday_limit_directive.md` until validated, per existing pipeline convention.

## Purpose
Formalize how the agent uses Robinhood's market data, earnings, index, and scanner tools inside the existing catalyst engine (v1.4) and equity consolidation (v2.1) workflows. These tools were confirmed live on the connected Robinhood agent account as of this review; several (technical indicators, earnings calendar, index quotes, scanners) were not previously referenced in the live directive set.

## 1. Pre-trade technical filter (applies to all autonomous-eligible tickers: MU, INTC, NBIS, AMD)
Before `place_equity_order` or `place_option_order` fires on an autonomous-eligible ticker, the agent must call `get_equity_technical_indicators` and check:
- RSI(14): flag if >70 (overbought) or <30 (oversold) — does not block the trade, but must be logged in the dated log file as a caution note.
- 50/200-day moving average relationship: note whether price is above or below each, for trend context.
- MACD: log signal-line cross state.

This is a **logging and awareness requirement**, not a hard gate — it does not override the four-AI sell list or the 25% single-stock rebalance trigger.

## 2. Earnings-date collision check
Before any Sunday pre-approval lock, the agent must call `get_earnings_calendar` for the coming week and cross-reference against the autonomous-eligible universe and any manual-only tickers under active consideration (WDC, MRVL, STX, SNDK).
- If an eligible ticker reports earnings during the Wed–Sat blackout window, it must be flagged explicitly in the pre-approval summary — earnings-week volatility inside a blackout window (no manual override available) is a materially different risk than a normal week.
- `get_earnings_results` should be used for any ticker already flagged, to pull the current EPS estimate vs. prior actual, consistent with how the MU Q3 FY2026 earnings research was done manually in July.

## 3. Macro/index context
`get_indexes` and `get_index_quotes` (SPX, NDX at minimum) should be pulled once per Sunday pre-approval cycle and logged as a one-line macro snapshot. This is context only — no existing rule changes based on index level. Purpose is to have a same-day reference point in the log for after-the-fact review, given the semiconductor-heavy book's correlation to NDX.

## 4. Fundamentals sanity check on DCA changes
Any time the monthly DCA proportions (SOXX 30% / PLTR 25% / NBIS 20% / WDC 15% / AMD 10%) are revisited, pull `get_equity_fundamentals` for each name and log P/E, market cap, and 52-week range alongside the proposed weights. This doesn't change the allocation logic — it's a documentation step so future-you can see what valuation looked like at each rebalance decision.

## 5. Scanner tools — exploratory only, not wired into autonomous execution
`get_scans` / `create_scan` / `run_scan` / `update_scan_filters` / `update_scan_config` are available but are **not** part of the autonomous-eligible ticker pipeline. Treat as a manual research aid for expanding the watchlist of candidates (the NRTA/LLY/ABBV/FANG/NET/MRK list currently under consideration) — not for triggering trades. If this changes, it needs its own directive version bump and backtest pass, same as `intraday_limit_directive.md`.

## 6. Reporting
`get_realized_pnl` should be pulled at each Sunday cycle for ••5748 (agentic account only — never ••7930) and logged alongside the macro snapshot, so realized P&L trend is visible without opening the app.

## Explicitly out of scope for this version
- Any tool not listed in Robinhood's published tool table (subject to change as Robinhood adds more).
- `get_option_level_upgrade_info` — not relevant; options access is already established on ••1155.
- No change to the $250 max debit, one-active-position, 10%-notional options cap.
- No change to the 25% single-stock rebalance trigger or the four-AI sell list process.

## Next step before promotion to live
Backtest the technical-indicator logging and earnings-collision check against the last 3 months of catalyst engine decisions to confirm they would not have altered any trade that was actually placed, then bump to v1.0 live and update the master directive's changelog.
