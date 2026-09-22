# STATUS.md — ARIE Digest

**Current gate:** Gate 2A — Inoreader Engine Design (docs complete, in review)
**Last updated:** 2026-09-22

## Gate progress
- [x] **Gate 0** Architecture Review — APPROVED at independent review. Closed (PR #1 squash-merged `6b34797`).
- [x] **Gate 1** Repository / Governance — governance set in place. Closed (PR #1).
- [x] **Gate 2A** Inoreader Engine Design — `inoreader/SOURCES.md`, `folders.md`, `monitoring-queries.md`, `noise-filters.md`, `report-config.md`. Docs/config only, no external systems touched.
- [ ] Gate 2B Live Setup & Verification (requires subscription/auth)
- [ ] Gate 3 AI Digest Quality (prompt + adversarial QA)
- [ ] Gate 4 Email Delivery Proof (email only — Teams removed from v1)
- [ ] Gate 5 Live Pilot (5–10 business days)
- [ ] Gate 6 Production Lock (finalise, archive legacy, repo → private)

## Active workstreams
- (none active — Gate 2A synthesised from A2 research, no new sub-agents; A1/A2 terminated)

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
