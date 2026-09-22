# STATUS.md — ARIE Digest

**Current gate:** Gate 1 — Repository / Governance (governance scaffolding complete); Gate 0 PR #1 open for review
**Last updated:** 2026-09-22

## Gate progress
- [x] **Gate 0** Architecture Review — APPROVE. See `ARCHITECTURE_REVIEW.md`. No external changes made.
- [x] **Gate 1** Repository / Governance — README, ARCHITECTURE, DECISIONS, EXECUTION_PLAN, STATUS, SECURITY, qa/ scaffolding in place. Public-exposure safe.
- [ ] Gate 2 Inoreader Engine Design
- [ ] Gate 3 AI Digest Quality (prompt + adversarial QA)
- [ ] Gate 4 Email Delivery Proof (email only — Teams removed from v1)
- [ ] Gate 5 Live Pilot (5–10 business days)
- [ ] Gate 6 Production Lock (finalise, archive legacy, repo → private)

## Active workstreams
- (none active — A1 and A2 completed and terminated)

## Gate 0 outcome (summary)
- Inoreader validated as viable (Pro + Intelligence / Team Intelligence).
- Delivery (D6 LOCKED): **Email is the sole v1 surface.** Microsoft Teams removed entirely (no challenger, no Team-plan dependency). Pipeline: Curated Sources → Inoreader → One Automated Intelligence Report → ARIE Digest → Management Email. Target plan: Pro + Intelligence add-on.
- Zero-stale must be **prompt-enforced** (C1); dedupe on **event identity** (C2).
- Headline risk: **FSC Mauritius + MoF/EDB/FIU have no feed** → monitoring query + page-watch + Gate-2 acceptance test.
- Numeric items (prices/quotas/limits) snippet-sourced — confirm on live pages at purchase/Gate 2.

## Open blockers
- (none — Gate 0 complete)

## Open items carried forward
- O1 confirm live prices (payment step) · O2 schedule precision (Gate 2) · O3 verify `RSS ~` + page-watch (Gate 2) · O4 report/token caps (Gate 2) · O5 FSC injection test (Gate 2/3)

## Approval gates for user
- Gate 0 review (upcoming) · Gate 4 delivery confirmation · Gate 6 production lock.
- Between Gate 0 approval and Gate 4, execution is autonomous except for:
  architecture deviation, payment/account auth, MS/Inoreader tenant action,
  genuine blocker, or an imminent external production change.
