# QA — ARIE Digest

Adversarial quality assurance for the Digest (Gate 3) and pilot tracking (Gate 5).

## Structure

- `fixtures/` — accept/reject test items (real, historical, and harvested from the
  legacy repo). Each fixture states the item and the expected outcome + reason.
- `QA_RESULTS.md` — pass/fail results of running the master prompt against fixtures
  (created at Gate 3).

## What QA must break

Irrelevant stories · stale/resurfaced old news · syndicated duplicates · weak
commercial signals · generic fintech/crypto fundraising · routine Mauritius
notices (T-bill/repo-rate/reserves) · film entertainment noise (casting/box-office)
· oil-price chatter · procurement/tenders · SEO/jurisdiction guides.

## Must-pass positives

- A new **FSC Mauritius** licensee / communiqué surfaces ("zero regulator misses").
- Same event across 3–4 publishers collapses to **one** story (event-identity dedupe, C2).
- No item with an unverifiable underlying event date is presented as new (C1).

## Acceptance targets (pilot-tracked, see ARCHITECTURE_REVIEW.md §6)

High relevance (≥95% useful) · zero unsupported facts · zero material Tier-1
regulator misses · <5% duplicates · zero stale-as-new · ≤5 min read · ≤10 normal
daily volume.
