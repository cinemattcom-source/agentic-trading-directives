# Catalyst Engine Directive

**Parent document:** agent_trading_directives_master.md (Section 1)
**Account:** Robinhood agentic account, ••5748
**Owner:** Matt
**Version:** 1.4 — July 2, 2026 (v1.2 merged the opportunistic approval window + 15-min timeout into v1.1; v1.3 set profit targets; v1.4 sets the stop at 15%. All v1.1 content — calendar, macro monitoring, session protocol — preserved)
**Status:** Active reference for the execution layer. Simulation-gated per master Section 5.

## 0. Purpose

Generalizes the original MU-specific earnings catalyst mandate (June 2026, now resolved) into a repeatable engine that applies the same dated-catalyst logic to any name in the autonomous-eligible universe (MU, INTC, NBIS, AMD — see master Section 0.9). Defers to the master file's Standing Rules rather than restating them.

## 1. Schedule Awareness — Matt's Blackout

Matt works six-day weeks, ten-hour shifts, starting July 1, 2026. Full-session phone blackout Wednesday through Friday. Available before ~6 AM and after ~6–7 PM on workdays; full availability Sunday through Tuesday.

**Mechanism:** Every Sunday, Matt reviews the ranked shortlist from `equity_consolidation_directive.md` and locks a specific catalyst-trade list for the week — which names, what triggers, what size. The engine executes **only** that locked list during Wed–Fri blackout. It cannot:
- Originate a trade not on the locked list.
- Resize a position beyond what was locked.
- Re-trigger an entry that was already rejected or stopped out earlier in the week.

This moves the human decision point earlier (Sunday) instead of removing it.

### 1a. Opportunistic Approval Window [added v1.2]

The blackout schedule is a *floor of unavailability, not a ceiling.* If Matt becomes reachable intraday on a blackout day — sent home early, on break with phone access, etc. — the engine may treat him as available for that window and push a trade suggestion for live one-tap approval. This does not change the Sunday-locked list or grant the engine new authority; it only lets Matt opt *in* to a live decision he'd otherwise miss. Two firm rules govern it:

- **A suggested trade pushed for approval carries a response deadline of 15 minutes** (or tighter if the catalyst is unusually time-sensitive).
- **On timeout — no response within the window — the request EXPIRES and the engine does nothing.** No position opens. Ignoring the notification is a valid, safe response: silence means "don't." The engine never fires a trade Matt didn't actively approve. A timeout is logged so Matt can see it happened, but nothing executes. Losing an unapproved trade is acceptable; taking an unseen one is not. **This is a hard safety inversion and is not overridable by any convenience setting.**

Notifications the engine may push whenever Matt appears reachable (any day) are informational and never require a response: end-of-day/intraday log summary, account value and open-position status, stop-loss proximity alerts, and newly-surfaced catalysts or news not on the tracked calendar.

## 2. Entry Mechanics

- **Instrument: fractional equity shares only.** No options — see master Section 0.7/unresolved-options note.
- **Order type: marketable limit order**, set approximately 1.5% past the current price — not a naked market order. Robinhood collars naked market orders to roughly 5% slippage anyway; a tighter marketable limit caps worst-case fill price while still filling promptly.
- **Session: regular hours only.** No pre-market, no after-hours entries.
- **Robinhood fractional-order quirks the engine must handle, not treat as errors:**
  - A fractional limit order unfilled after ~5 minutes auto-cancels. If this happens mid-blackout with no re-trigger authorized on the locked list, the engine logs it and stands down — it does not chase the fill with a new order.
  - Stop orders queue to the next regular-hours open; they do not execute in extended hours even if triggered after-hours.

## 3. Exit Rules

**Profit targets (per trade, shares in the agentic account):**
- **Minimum acceptable target: 30–40% ROI** on the position. Below this band, a setup generally isn't worth the risk given the 10% stop.
- **Upper target: 80–100% ROI** — scale out / take profit as a position runs into this band; don't round-trip a large gain waiting for more.
- These are targets to exit *into* when reached, not guarantees every trade achieves them. Scale out across the band rather than all-or-nothing at one price.

**Stop loss: 15% of entry value.** [Set v1.4 — tightened from the 40% carried over from the original options-based mandate, which was far too wide for a shares-only account.] Paired with the 30–40% minimum profit target, this gives a minimum reward:risk of roughly 2:1 to 2.7:1.

- No averaging down without a new, confirmed catalyst.
- Never carry a full position through an earnings print uncovered — trim ahead of the print; hold a partial position only if the thesis is still intact going in.
- No same-name re-entry the same week after a stop-out.

## 4. Catalyst Calendar — 2026 (carried forward from trading_event_catalys.md, refreshed for what's now historical)

> **Note:** Several dates below were marked "estimated" as of the June 12 source file. Before relying on July–December entries, re-verify AMD and MRVL earnings dates against current confirmed schedules — they've likely firmed up since.

### RESOLVED / HISTORICAL (no longer actionable)
- Jun 16–17 FOMC (Warsh's first meeting) — occurred.
- Jun 24 MU earnings — occurred; blew out estimates (EPS $25.11 vs. $20.28 est.), stock +~15% after-hours.
- Jun 25 MU reaction assessment — occurred; MU catalyst mandate resolved and folded into the general engine (master Section 1).

### JULY 2026
| Date | Event | Trading Action |
|---|---|---|
| Jul 3 | Jobs Report | Monitor macro |
| Jul 29–30 | Fed Meeting | Dovish/rate cut = add to semiconductor positions |
| Mid-July | AMD earnings setup watch | Begin building toward August earnings window |

### AUGUST 2026
| Date | Event | Trading Action |
|---|---|---|
| Aug 7 | Jobs Report | Monitor macro |
| ~Aug 3 | AMD earnings (verify date) | Exit 80% before print; hold 20% if thesis intact |
| Late Aug | MRVL earnings (verify date) | Manual-only per Section 0.9 — Phase R monitoring, Matt places any trade |

### SEPTEMBER 2026
| Date | Event | Trading Action |
|---|---|---|
| Sep 4 | Jobs Report | Monitor macro |
| Sep 17 | Fed Meeting | Rate cut = deploy reserve more aggressively across best-performing name |
| Sep 18 | Quad Witching | Expect elevated volatility |
| Sep (TBA) | Apple iPhone launch | Historically bullish for semiconductor supply chain |

### OCTOBER 2026
| Date | Event | Trading Action |
|---|---|---|
| Oct 2 | Jobs Report | Monitor macro |
| Oct 10 | Tariff watch window | Monitor US-China trade; pause runs to June 2027 |
| Oct 21–25 | Earnings season (AMD, INTC, MU) | Stagger entries — never all three at full size simultaneously |

### NOVEMBER 2026
| Date | Event | Trading Action |
|---|---|---|
| Nov 5 | Fed Meeting | Final major FOMC of the year |
| Nov 6 | Jobs Report | Monitor macro |
| Nov (ongoing) | Holiday/AI capex announcements | MSFT/GOOG/AMZN/META spend bullish for the book |

### DECEMBER 2026
| Date | Event | Trading Action |
|---|---|---|
| Dec 4 | Jobs Report | Monitor macro |
| Dec 16–20 | Earnings season | Hold winners; consider tax-loss-harvesting dips in strong names |
| Dec 18 | Quad Witching | Expect elevated volatility |

### Recurring reference dates
- **Quad witching:** Mar 20 · Jun 19 · Sep 18 · Dec 18
- **Fed meetings:** Mar 19 · Apr 30 · Jun 16–17 · Jul 29–30 · Sep 17 · Nov 5
- **Jobs reports:** Feb 7 · Mar 7 · Apr 4 · May 2 · Jun 6 · Jul 3 · Aug 7 · Sep 4 · Oct 2 · Nov 6 · Dec 4

## 5. Macro & Geopolitical Monitoring (carried forward, still relevant)

| Factor | Status / Action |
|---|---|
| US-China trade relations | Tariff pause runs to June 23, 2027. Monitor for early termination. |
| Taiwan Strait | Volatility spike = buying opportunity on overreaction, not a sell signal. |
| AI regulation | Export control changes = short-term volatility only; doesn't change long-term thesis. |
| Federal AI infrastructure spending | Government contract announcements bullish for the book. |
| Middle East conflict | Feeds energy prices/inflation → Fed hawkishness risk. Monitor weekly. |

## 6. Quantum Watch (carried forward, unchanged)

Beginning Q3 2026: monitor QTUM ETF and IONQ. Do not deploy into quantum names until the semiconductor thesis has generated a minimum 20% return on deployed capital.

## 7. Session Protocol

Every session open:
1. Pull account balance and open positions directly (never assumed/remembered — master Standing Rule 6).
2. Quote live prices for the autonomous-eligible universe (MU, INTC, NBIS, AMD) plus anything on the current Sunday-locked list.
3. Sweep for breaking macro/geopolitical/sector news.
4. Deliver a situational briefing.
5. Compare against the locked weekly plan — execute only what's on it, during blackout days, per Section 1. Honor any live opportunistic approval per Section 1a.

Current market data overrides stale calendar assumptions. When they conflict, surface the conflict, follow the data, and explain the deviation — don't silently resolve it.

## 8. Simulation Gate

This engine must run in paper/simulation mode for 3 consecutive weeks before any real capital is committed under it, per master Section 5's Scaling Protocol. Net-positive after costs with drawdown inside the weekly loss limit is the bar; if it doesn't clear, revisit entry/exit logic rather than going live anyway.
