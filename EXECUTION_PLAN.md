# EXECUTION_PLAN.md — ARIE Digest

Gate-by-gate plan. See `DECISIONS.md` (locked decisions), `ARCHITECTURE.md`
(pipeline), `STATUS.md` (live progress). Delivery is **email only** (D6).

## Intervention model (agreed)
After Gate 0 approval/merge, execution is autonomous through internal
repository/governance/design/QA work. Stop for the user only on: a
`PROPOSED ARCHITECTURE DEVIATION`; payment/subscription or account authentication;
a Microsoft/Inoreader tenant action that requires the user; a genuine blocker; or
an imminent external production change.

---

## Gate 0 — Independent Architecture Review ✅
Validated Inoreader capability, cost shape, source availability, delivery, zero-
stale strategy. Output: `ARCHITECTURE_REVIEW.md`. No external systems touched.
**Outcome:** APPROVE. Delivery locked to email (Teams removed, D6).

## Gate 1 — Repository / Governance (in progress)
Deliverables: `README.md`, `ARCHITECTURE.md`, `DECISIONS.md`, `EXECUTION_PLAN.md`,
`STATUS.md`, `SECURITY.md`, QA structure (`qa/`). Acceptance: repo is a clear
source of truth; public-exposure sign-off. No external systems.

## Gate 2 — Inoreader Engine Design
Deliverables (documented config, ready to apply — not yet applied):
- Source set (`context/a2-sources.md` → `inoreader/SOURCES.md`), folder/tag scheme
  (one per section), monitoring queries for Must-Catch + feedless Mauritius Tier-1.
- Verify each `RSS ~` URL; prove FSC/EDB/MoF/FIU web-feed page-watch works.
- Confirm the automated-report scheduler can pin Mon–Fri 08:15 UTC+4 (else use the
  email-digest scheduler); confirm per-report article/length caps, folder cap,
  monthly token headroom.
- **FSC-communiqué injection acceptance test** ("zero regulator misses" positive).
Acceptance: configuration validated and ready for live Inoreader setup.

## Gate 3 — AI Digest Quality
Deliverables: single master prompt (`digest/PROMPT.md`) implementing
reject→dedupe/group→classify→summarise→ARIE-relevance→prioritise, with C1
(event-date rejection) and C2 (event-identity dedupe), Commercial Signal guardrail,
and the harvested exclusions (procurement hard-drop, operational-noise demotion,
SEO/guide exclusion, film ≤25% + payment-angle test, grounding rule, no forced
actions). Adversarial QA (`qa/`) against real/historical + harvested fixtures.
Acceptance: high relevance; zero unsupported facts; zero stale-as-new; low
duplicate rate; Commercial Signal holds.

## Gate 4 — Email Delivery Proof (email only)
Test **only** the email path:
- automated Inoreader Intelligence report **email delivery**;
- **multiple management recipients**;
- **formatting / readability**;
- **links**;
- **scheduling** (weekday 08:15 UTC+4);
- **reliability**;
- whether **recipient acceptance/activation** is required (and the one-time steps).
Acceptance: clean automatic management consumption by email with no orchestration.
(Microsoft Teams is out of scope for v1 — not tested.)

## Gate 5 — Live Pilot (5–10 business days)
Track in `STATUS.md`: important story missed; irrelevant story included; duplicate;
factual/date issue; management usefulness. Targets are pilot-tracked (see
`ARCHITECTURE_REVIEW.md` §6).

## Gate 6 — Production Lock
Finalise config/prompt/email delivery; retire/archive the legacy repo; make this
repo **private**; remove temporary public-execution exposure.

---

## User intervention points (minimised)
- Inoreader subscription purchase/auth (Pro + Intelligence add-on) — payment gate.
- Provide Inoreader account access **or** apply the exported config prepared here.
- Confirm management recipient list at Gate 4 (kept out of the public repo).
- Gate approvals: Gate 0 (done), Gate 4 confirmation, Gate 6 production lock.

## What Claude Code completes autonomously
Governance files, source curation, folder/query design docs, the master prompt,
QA fixtures and adversarial testing, public-exposure review, and all planning —
without touching external production systems until the payment/setup gate.
