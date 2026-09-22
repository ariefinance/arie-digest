# STATUS.md — ARIE Digest

**Current gate:** Gate 3 — AI Digest Quality (final QA complete, in review)
**Last updated:** 2026-09-22

## Gate progress
- [x] **Gate 0** Architecture Review — APPROVED at independent review. Closed (PR #1 squash-merged `6b34797`).
- [x] **Gate 1** Repository / Governance — governance set in place. Closed (PR #1).
- [x] **Gate 2A** Inoreader Engine Design — sources/folders/queries/noise/report design. Closed (PR #2 squash-merged `ad55884`).
- [x] **Gate 3** AI Digest Quality — `digest/PROMPT.md`, `digest/format.md`, `qa/fixtures/`, `qa/QA_RESULTS.md`. Initial adversarial QA found 1 FAIL + 9 weaknesses; remediated. Final independent re-test of the self-contained runtime prompt: **14/14 PASS, self-containment PASS, no remaining FAILs**. Docs/QA only.
- [ ] Gate 2B Live Setup & Verification (requires subscription/auth — deferred until account exists)
- [ ] Gate 4 Email Delivery Proof (email only — Teams removed from v1)
- [ ] Gate 5 Live Pilot (5–10 business days)
- [ ] Gate 6 Production Lock (finalise, archive legacy, repo → private)

## Gate 3 outcome (summary)
- Master prompt (`digest/PROMPT.md`) implements the single pass and is fully self-contained for direct paste into Inoreader; `digest/format.md` mirrors the runtime format for human reference.
- 14 adversarial fixtures. Initial QA found 1 FAIL (over-eager Commercial Signal) + 9 weaknesses; remediation completed. Final independent re-test: **14/14 PASS**, self-containment PASS, no FAILs.
- Key hardening: C1 recency window + same-day-event rule; grounding vs ARIE-relevance contradiction resolved; Commercial Signal condition 3 requires an explicit payments element; C2 subject-entity match + abbreviation=full-name; Film-cap and WMT coupling clarified.
- Four new LOW residuals (N1–N4) from the final re-test are documented in `qa/QA_RESULTS.md`; no prompt change made pre-pilot. N1 (recency-window false-negative risk) is specifically deferred to Gate 5 evidence to avoid reopening the stale-as-new failure mode.
- Residual limits (single, no-browsing pass) documented in `qa/QA_RESULTS.md` §3 — for Gate 5 pilot tracking.

## Active workstreams
- (none active — Gate 3 QA agents terminated after final re-test)

## Gate 0 outcome (summary)
- Inoreader validated as viable. Target plan: **Pro + Intelligence add-on** (D10); Team Intelligence only if Pro can't meet a demonstrated need.
- Delivery (D6 LOCKED): **Email is the sole v1 surface.** Microsoft Teams removed entirely (no challenger, no Team-plan dependency). Pipeline: Curated Sources → Inoreader → One Automated Intelligence Report → ARIE Digest → Management Email. Target plan: Pro + Intelligence add-on.
- Zero-stale must be **prompt-enforced** (C1); dedupe on **event identity** (C2).
- Headline risk: **FSC Mauritius + MoF/EDB/FIU have no feed** → monitoring query + page-watch + FSC-capture test (designed in 2A, proven in 2B).
- Numeric items (prices/quotas/limits) snippet-sourced — confirmed live at purchase/Gate 2B.

## Open blockers
- (none)

## Open items carried forward
- O1 confirm live prices (payment step) · O2 schedule precision (Gate 2B) · O3 verify `RSS ~` + page-watch (Gate 2B) · O4 report/token caps (Gate 2B) · O5 FSC injection test (Gate 2B)

## Approval gates for user
- Gate 0–1 (approved) · payment/auth for Inoreader (before Gate 2B) · Gate 4 delivery confirmation · Gate 6 production lock.
- Execution is autonomous except for: architecture deviation, payment/account auth,
  MS/Inoreader tenant action, genuine blocker, or an imminent external production change.