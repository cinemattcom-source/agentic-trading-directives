# Watch-Only / Notify Directive — v1.0 (DRAFT, pending backtest validation)

**Status:** DRAFT — same pipeline as `intraday_limit_directive.md` and `market_data_tools_directive.md`. No trading authority granted by this file, ever — see scope below.

## Purpose
Extend visibility (not authority) to holdings on ••1155 that sit outside the autonomous-eligible universe (MU, INTC, NBIS, AMD) and outside the manual-only expansion list (WDC, MRVL, STX, SNDK). As of this draft, that means: **TSM, SMH, DBO, USO, SPCX, XOM**. This list should be re-checked against actual ••1155 holdings before each Sunday cycle, since it will drift as positions change.

## Hard scope limit
This directive **never** calls `place_equity_order`, `place_option_order`, `review_equity_order`, or `review_option_order` on any symbol in its watch list. If a symbol needs trading authority, it must be promoted into the autonomous-eligible universe through the existing four-AI review process and get its own backtest — not granted authority implicitly by appearing on a watch list. This file only reads and reports.

## What it does
Once per Sunday pre-approval cycle, for each symbol on the watch list:
1. `get_equity_technical_indicators` — RSI(14) and MACD, daily interval, to flag notably overbought/oversold conditions or a fresh MACD cross.
2. `get_earnings_calendar` / `get_earnings_results` — flag if the symbol reports earnings in the coming week.
3. `get_equity_quotes` — flag any move >5% from prior close since last review (large-move flag).

Any symbol that trips one of the three flags gets a line in the dated log (`logs/YYYY-MM-DD.md`) and a push notification, worded as an FYI, not a recommendation: e.g. "TSM: RSI 78 (overbought), no action taken — watch-only." Symbols that trip nothing get a single rolled-up "no flags this week" line, not individual noise.

## Explicitly out of scope
- No rebalancing logic, no position sizing, no sells or buys of any kind.
- No promotion path that happens automatically — moving a symbol from watch-only to autonomous-eligible requires the same four-AI review + backtest as MU/INTC/NBIS/AMD went through, and a version bump on this file to remove it from the watch list.
- Does not apply to anything on ••7930 — that account is out of scope for this entire system, full stop.

## Next step before promotion to live
Same as the other two DRAFT directives: validate against real trade/flag history once there's enough of it, then bump to v1.0 live and note it in the master directive's changelog.
