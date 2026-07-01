# Equity Consolidation & Multi-Ticker Monitoring Directive

**Parent document:** agent_trading_directives_master.md (Section 2)
**Account:** Robinhood agentic account, ••5748
**Owner:** Matt
**Version:** 2.1 — July 1, 2026
**Status:** Phase R (Research & Monitoring) ACTIVE for the full universe. Phase G (Graduation) PENDING for autonomous-eligible names — see Section 3.

## 0. Purpose

Matt's individual taxable account (••1155) holds the same AI-infrastructure/memory/storage thesis across several tickers, built manually over time. His work schedule (six-day weeks, ten-hour shifts, starting July 1, 2026) removes his ability to watch the market intraday. This directive moves that thesis under agentic monitoring — and, for eligible names, eventual agentic execution — deliberately and on a defined schedule, not all at once.

This is **not** an instruction to copy the taxable-account positions directly. The agentic account builds its own entries under its own risk rules.

## 1. Universe

**Autonomous-eligible** (may be executed by the engine once Phase G opens, per master Section 0.9):
- **MU** — Micron. Structural AI memory/HBM demand. Formerly ran under its own separate catalyst mandate; that mandate resolved (June 24/25 earnings, strong beat) and MU now folds into this directive's general universe.
- **INTC** — Intel.
- **NBIS** — Nebius Group. AI cloud/GPU infrastructure pure-play.
- **AMD** — added to the autonomous set July 1, 2026 (was not in the original June 17 universe).

**Manual-only** (Phase R monitoring/ranking applies; any entry must be placed by Matt himself — liquidity concerns):
- **WDC** — Western Digital. Storage.
- **MRVL** — Marvell. Custom AI silicon/networking ASICs. Confirmed **not** subject to the old $220 re-entry ceiling — that condition was superseded and MRVL has traded well above it since without issue.
- **STX** — Seagate. Storage.
- **SNDK** — SanDisk. NAND flash; smallest dollar weight, highest per-share price of the group.

**Explicitly excluded — do not add, do not monitor under this directive:**
- **ASTS** — flagged for Matt's own manual review of the taxable-account position only. Agent takes no action.
- **SPCX** — excluded per Matt's instruction. Not tracked.

## 2. Phase R — Research & Monitoring (ACTIVE, all names, zero capital risk regardless of eligibility tier)

The agent may:
- Pull quotes and fundamentals each session for every ticker in the universe.
- Track distance from 52-week high/low and day-over-day relative volume.
- Note confirmed catalyst dates and add them to `catalyst_engine_directive.md`'s calendar.
- Maintain a ranked shortlist per Section 4 below.

The agent may **not**, regardless of tier:
- Place any buy or sell order in a manual-only name, ever — that's Matt's trade to place.
- Treat "the price looks good" as authorization to trade an autonomous-eligible name outside a Sunday-locked plan (see `catalyst_engine_directive.md` Section 1).

Output format: one ranked update per session, plus anything materially changed. Log even on no-action days.

## 3. Phase G — Graduation (autonomous-eligible names only; manual-only names never graduate to autonomous execution)

Requires **all three**:
1. New capital deposited beyond whatever's earmarked for an active catalyst trade.
2. Matt's explicit written go-ahead naming this directive — a balance appearing is not consent.
3. The relevant Sunday-lock plan (per `catalyst_engine_directive.md`) has been reviewed and approved for the week.
4. The mandatory 3-week simulation gate (master Section 5) has cleared net-positive.

Once open:
- Fund at most 2 names from the autonomous-eligible ranked shortlist at a time.
- Position sizing: percentage-based against newly added capital only; hard reserve untouched; no margin; defined-risk only.
- Default instrument: fractional equity shares, marketable limit order (~1.5% past price), regular hours only — see `catalyst_engine_directive.md` Section 2 for full mechanics.
- No averaging down. No same-week re-entry after a stop-out.

## 4. Ranking Criteria (mechanical)

1. Within 5% of its own 52-week high — pass/fail filter.
2. Of those passing, highest day's relative volume vs. its own 30-day average.
3. Tiebreak: lower forward P/E wins.

Fund only names that pass filter 1, even if fewer than 2 pass — don't lower the bar to fill slots. Applies to autonomous-eligible names for actual funding; manual-only names still get ranked for Matt's reference even though the agent can't act on them.

## 5. Review Trigger

Revisit this universe (add/remove names, change eligibility tier) only when Matt explicitly updates this file. The agent does not add new tickers or change a name's tier on its own initiative.
