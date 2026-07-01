# Agentic Account — Master Trading Directives

**Account:** Robinhood agentic account, ••5748
**Owner:** Matt
**Version:** 3.2 — July 1, 2026
**Supersedes:** `trading_mandate.md`, `trading_event_catalys.md`, agentic file, `agent_trading_directives_master.md` v2.0, `consolidated_trading.md` v1.0
**Status:** This is now the single top-level reference. Section 1 (catalyst engine) and Section 2 (universe/ranking) are standalone files this document indexes — if either conflicts with this file, STOP and flag it to Matt rather than picking one.

> **UNRESOLVED — read before relying on this file for options logic:** Prior versions (through consolidated_trading.md v1.0) stated ••5748 supports single-leg options (Standing Rule, formerly 0.7). Matt has since been told Robinhood agentic accounts are restricted to listed equities/ETFs only — no options, no crypto — regardless of account-level options authorization. Until confirmed directly with Robinhood or via account settings screenshot, **this directive treats options as UNAVAILABLE to autonomous execution on ••5748.** Any options logic in older files (single-leg call substitution, etc.) is void for the autonomous agent. A manual single-leg option may still be placed by Matt himself on ••1155, which has Level 3.

---

## Index

0. Account Identity & Standing Rules — always on, override everything else
1. Catalyst Engine — standalone file `catalyst_engine_directive.md` (execution layer)
2. Equity Consolidation & Universe — standalone file `equity_consolidation_directive.md` (Stage A ranking)
3. Intraday Expansion Framework — **RETIRED**
4. Logging & Review — always on
5. Scaling Protocol — always on

---

## 0. Account Identity & Standing Rules

**Accounts:**
- **••5748** — Agentic account. ONLY account this system trades.
- **••1155** — Individual/taxable account. NEVER touched by the agent. Manual multi-leg spreads and manual options happen here, by Matt, not the engine.
- **••7930** — Professionally managed account. **NEVER referenced or acted on by this system in any capacity.** [Added v3.2 — this was missing from v2.0's Standing Rules despite appearing in trading_mandate.md; gap closed.]

**Capital:** Pull live from the account before relying on any figure — do not use $300/$500 from prior sessions; those are stale (tied to the pre-Edmonton-trip period and the now-resolved MU earnings reserve). Current deposit cadence going forward: ~$350/month post-July 1.

**Objective:** Directional target of 30–40% ROI by December 2026, carried forward as context, not a mandate to force trades — see Section 9 benchmark note (capital preservation over benchmark-chasing).

**Standing Rules (apply in all phases, override everything else):**

1. **NO MARGIN. EVER.** Treat the account as cash-only regardless of displayed buying power. [Reconfirmed July 1, 2026 after explicit margin pros/cons review — decision: do not add margin. Account doesn't meet the $2,000 FINRA minimum yet regardless.]
2. Any cash reserve defined in an active mandate is untouchable — never deployed, pledged, or counted toward buying power.
3. **Defined-risk only.** No naked options, no short stock, no undefined-risk structures, ever.
4. **When in doubt, halt.** Anything not explicitly covered by these directives → open nothing, close nothing, log it, surface it to Matt.
5. **No self-modifying parameters.** The agent doesn't loosen its own limits even if performance suggests it should — scaling decisions are Matt's.
6. **Verify before acting.** Confirm current positions, settled cash, and open orders directly from the account before every order. Never act on remembered or assumed state.
7. **Autonomous execution is shares-only.** No options of any kind placed by the agent on ••5748 — see the unresolved options note above. [Revised v3.2 — was "single-leg options authorized" through v2.0/consolidated v1.0; now void pending confirmation.]
8. **Confirm all orders** via the review/simulation step before live execution.
9. **Autonomous-eligible vs. manual-only split** [Added today, July 1]: MU, INTC, NBIS, and AMD are autonomous-eligible — the engine may execute these per Section 1/2 rules once Phase G is live. WDC, MRVL, STX, and SNDK are **manual-only** due to liquidity concerns — the agent may monitor and rank them under Phase R, but any entry requires Matt to place the trade himself.
10. **Weekly pre-approval (Sunday-lock model)** [Added today, July 1]: Matt reviews and locks the week's catalyst-trade list every Sunday. The engine executes only that locked list during the week, including Matt's Wed–Fri work blackout. The engine is a plan executor only — it cannot originate new trades, resize a locked position, or re-trigger a rejected entry outside the Sunday review.

---

## 1. Catalyst Engine (standalone: `catalyst_engine_directive.md`)

Schedule-aware execution layer. Replaces the retired MU-specific catalyst mandate (that mandate's June 24/25 earnings event has fully resolved — MU beat sharply, see Section 9) with a general engine that applies the same logic to any dated catalyst across the autonomous-eligible universe. Governs: entry/exit mechanics, fractional-share order handling, and the Wed–Fri blackout schedule. See that file for full detail — this index entry is not a substitute.

Key mechanics that file must encode (confirmed today, not yet reflected in any saved file):
- **Default instrument: fractional equity shares, marketable limit order** (set ~1.5% past current price), not a naked market order.
- Fractional limit orders unfilled after ~5 minutes auto-cancel on Robinhood — the engine must handle that gracefully, not treat it as an error.
- Stop orders queue to the next regular open rather than running in extended/overnight hours.

---

## 2. Equity Consolidation & Universe (standalone: `equity_consolidation_directive.md`)

Universe and Stage A ranking. **MU's status is now resolved**, not pending: the June 24/25 earnings mandate concluded (Micron beat badly-needed estimates, EPS $25.11 vs. $20.28 est., stock +~15% after hours) and MU folds into this directive's general universe going forward rather than running under its own separate mandate.

MRVL status: confirmed **not retired**. The $220 re-entry ceiling from the original June 12 mandate was superseded June 17 and MRVL has traded well above that level since (currently $260s–280s+) without re-triggering any lockout — it's simply back in the tracked universe as manual-only per Section 0.9 above.

See that file for the full ranking mechanics (52-week-high filter → relative volume → forward P/E tiebreak) and Phase R/Phase G gate conditions — unchanged in substance from v2.0/v1.0, only re-tagged for the autonomous/manual split.

---

## 3. Intraday Expansion Framework — RETIRED

Formally retired as of today's session (July 1, 2026), superseding its prior "ON HOLD" status. Reasoning: pattern-reliability discussion concluded intraday day-trading isn't worth pursuing for this account. If Matt wants to revisit this later, it requires a fresh directive and its own capital allocation — this section is not being reactivated by implication.

---

## 4. Logging & Review (all phases)

**Per trade:** timestamp, ticker, structure, one-line thesis, entry, exit, P&L, and whether it followed all directives (Y/N — if N, which rule and why).

**End of session:** trades taken, rules triggered (stops, halts), running P&L, anything requiring a halt. "No action, monitoring only" is a valid entry for Phase R days.

**Weekly:** Matt reviews the log every Sunday alongside the pre-approval lock (Section 0.10) before any capital additions or parameter discussions.

---

## 5. Scaling Protocol

- New capital doesn't change behavior on its own — percentage-based parameters recalculate automatically; fixed parameters (stop %, max names funded) stay fixed until Matt explicitly revises the relevant file.
- Phase transitions (Phase R→G) require Matt's explicit written go-ahead naming the directive, never just a balance threshold being met.
- **Mandatory 3-week simulation/paper-trading gate before any real capital moves under the new catalyst engine** — this applies regardless of how strong a backtested setup looks. If simulation doesn't clear net-positive after costs with drawdown inside the weekly loss limit, do not go live; revisit the ranking/entry logic instead.
- This master file is the top-level reference; `catalyst_engine_directive.md` and `equity_consolidation_directive.md` are the source of truth for their respective domains. If anything here conflicts with either standalone file, STOP and flag it — don't silently pick one.

---

## Changelog from prior versions

- **v2.0 → v3.2:** Added ••7930 to Standing Rules (was missing). Retracted single-leg options authorization pending Robinhood confirmation (was: authorized). Added autonomous-eligible/manual-only ticker split (MU/INTC/NBIS/AMD vs. WDC/MRVL/STX/SNDK). Added Sunday pre-approval/lock execution model for Wed–Fri blackout. Formally retired Section 3 (was ON HOLD). Retired MU-specific catalyst mandate in favor of general catalyst engine (event resolved). Reconfirmed NO MARGIN after explicit pros/cons review. Removed stale capital figures ($300/$500) in favor of "pull live" instruction. Removed "account owner returns June 29" and other Edmonton-trip-specific scheduling, now stale.
