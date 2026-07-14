# Intraday Limit Directive
**Version: 1.1 — 2026-07-14**
**Supersedes: v1.0 (drafted 2026-07-03, never pushed to `main`)**
**Status: Backtested. Ready for activation once ••5748 is refunded.**

## 0. Purpose

Defines pre-staged, price-triggered buy-the-dip and sell-into-spike logic for the
autonomous-eligible universe (MU, AMD, INTC, NBIS), so entries and exits execute
on price action without requiring Matt to react in real time. This directive does
NOT authorize trading outside the weekly Sunday-locked catalyst list — it defines
*how* a locked symbol is worked intraday, not *whether* to trade it.

## 1. Why v1.0 was replaced

Backtested against 1 year of real daily data (2025-07-14 to 2026-07-13) and 1 month
of real 5-minute intraday data:
- v1.0's fixed -5% / -15% / +8% / +20% thresholds applied the same trigger to every
  symbol regardless of that symbol's actual volatility. NBIS (higher volatility)
  and AMD (lower volatility) were treated identically, which is wrong.
- On $100-per-signal sizing over the trailing year, v1.0 generated positive but
  modest P&L (MU +$158, AMD +$103, INTC +$109, NBIS +$123) — while simple buy-and-hold
  returned 265%-690% over the same period. Fixed-threshold dip-buying sells winners
  too early in a trending market.
- A trend-filtered variant (skip dip-buys when price is above its 20-day MA; widen
  targets to +15%/+35%) roughly doubled profit-per-trade but traded far less often.
- True 5-minute intraday backtesting confirmed dip signals cluster heavily in the
  first 30-60 minutes after open (9:30-10:30 ET) and are comparatively rare during
  midday, including the 1-2pm ET lunch window.

v1.1 addresses the first problem (fixed thresholds) directly via ATR-adaptive
triggers. The trend-filter question (v2) is left as a future refinement — see §6.

## 2. Adaptive Trigger Formula (replaces fixed percentages)

Each trading day, before the market-open routine runs, recompute ATR(14) (14-day
Average True Range, daily bars) for every symbol on the active Sunday-locked list.
Derive that day's triggers from ATR, not from a static percentage:

| Trigger | Formula |
|---|---|
| Dip-buy trigger | Reference high − (0.35 × ATR14) |
| Falling-knife guard (do not buy beyond this) | Reference high − (1.00 × ATR14) |
| Sell 50% of position | Entry price + (0.35 × ATR14) |
| Sell remaining 50% | Entry price + (0.75 × ATR14) |

"Reference high" = the highest close in the trailing 10 trading days (unchanged
from v1.0's reference-high logic).

Rationale: ATR scales the trigger to what's actually normal for that stock right
now. A 3% move in a stock with 6.5% average daily range is noise; the same 3% move
in a stock with 12.6% average daily range is barely a blip. Fixed percentages
can't tell the difference; ATR-relative triggers can.

### 2.1 Reference table (2026-07-14, for illustration — recompute live daily)

| Symbol | Price | ATR14 | Dip-buy | Falling-knife guard | Sell 50% | Sell rest |
|---|---|---|---|---|---|---|
| MU | $985.07 | $86.30 | −$30.20 (3.1%) | −$86.30 (8.8%) | +$30.20 (3.1%) | +$64.72 (6.6%) |
| AMD | $555.56 | $35.91 | −$12.57 (2.3%) | −$35.91 (6.5%) | +$12.57 (2.3%) | +$26.93 (4.8%) |
| INTC | $107.18 | $9.58 | −$3.35 (3.1%) | −$9.58 (8.9%) | +$3.35 (3.1%) | +$7.19 (6.7%) |
| NBIS | $195.52 | $24.60 | −$8.61 (4.4%) | −$24.60 (12.6%) | +$8.61 (4.4%) | +$18.45 (9.4%) |

## 3. Authorization Model — what "automatic" actually means here

This directive does **not** grant the routine authority to trade any symbol at
any time. Authorization flows from the existing weekly ritual:

1. **Sunday**: Matt reviews the coming week's catalyst calendar and locks a symbol
   list (unchanged from `catalyst_engine_directive.md`). Only locked symbols are
   eligible for this directive's logic during that week.
2. **Weekday execution**: For locked symbols only, the routine may act on the §2
   triggers without a fresh per-trade approval, because Sunday's lock *is* the
   approval. This is what allows the routine to catch the 9:30-10:30 ET open-hour
   volatility window Matt is usually unavailable for.
3. **Stop-loss breaches remain flag-only** (Standing Rule 0.10, unchanged): a 15%
   stop breach is logged and surfaced immediately, never auto-exited. This directive
   governs entries and profit-taking exits only, not loss-side exits.
4. **Every executed trade is logged same-day** to `logs/YYYY-MM-DD.md` per the
   logging directive already in the master file, so nothing executes silently.
5. **Nothing in this directive authorizes trading an unlocked symbol**, increasing
   position size beyond the Sunday-locked allocation, or overriding the Catalyst
   Calendar Reserve rule in the master directive.

## 4. Cash-account settlement constraint

••5748 is a cash account (confirmed via account snapshot, no margin). It is not
subject to the Pattern Day Trader rule, but it IS subject to trade settlement
(T+1). The routine must track settled vs. unsettled cash before firing a buy —
using proceeds from a same-day sale to fund a new same-day buy risks a good-faith
violation. This caps realistic same-day round-trip frequency regardless of how
many valid triggers fire.

## 5. Position sizing

Sized against that week's Sunday-locked allocation for the symbol (per the master
directive's capital allocation rules), not against the illustrative $100/signal
used for backtesting comparability. No new tranche opens on a symbol that already
has an open position from this directive — one active tranche per symbol at a time.

## 6. Open question for future revision

The backtest also tested a trend-filtered variant (v2): skip dip-buys when price
is above its 20-day moving average, and widen profit targets to +15%/+35% of ATR
instead of +0.35/+0.75. This produced roughly 2x the profit-per-trade but traded
40-60% less often, since it correctly declines to "buy the dip" during strong
uptrends. Whether to adopt the trend filter is an open decision — not included in
v1.1 so it can be evaluated separately once live results from the ATR-adaptive
baseline exist to compare against.

## 7. Activation

This directive is written and backtested but **not active** — ••5748 has no
capital to deploy until refunding (~August 2026 per current plan). On refunding:
1. Confirm this file is on `main`.
2. Confirm `agent_trading_directives_master.md` references it.
3. First Sunday after refunding, lock the initial symbol list and confirm the
   routine picks up §2's ATR recompute step in its pre-open sequence.
