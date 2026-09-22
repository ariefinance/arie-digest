# ARCHITECTURE REVIEW — Gate 0 (ARIE Digest)

**Date:** 2026-09-22 · **Author:** Lead Execution Agent (Claude Code)
**Inputs:** A1 (Inoreader platform validation) · A2 (source curation + legacy
harvest). Full evidence: `context/a1-inoreader-findings.md`,
`context/a2-sources.md`, `context/a2-noise-lessons.md`.
**Status:** For independent review. **No external system was configured, logged
into, or modified. No production changes made.**

---

## Verdict

**APPROVE the architecture — proceed to Gate 1.** The Inoreader + Microsoft 365
(email) no-code design is validated as viable, with **two mandatory design
conditions** and **one headline collection risk** to manage. No
`PROPOSED ARCHITECTURE DEVIATION` is required — the findings refine the plan
inside the existing locked decisions; they do not break them.

**Confidence caveat (honest):** the session egress proxy blocked every official
Inoreader page and every external regulator site (403 on CONNECT). A1's
capability findings are sound (feature existence is well-corroborated) but all
**numeric** items — prices, token quotas, per-report article/length caps, exact
schedule granularity — are WebSearch-snippet-sourced, not fetched first-hand, and
must be confirmed on the live pages before money is spent (this coincides with
the payment gate anyway).

---

## 1. Does Inoreader carry the design? — YES (Pro + Intelligence, or Team Intelligence)

Confirmed capabilities (feature existence, high confidence):
- **Scheduled automated AI reports** over a chosen folder/tag, native, daily/weekly.
- **Custom prompt** per report + **folder/tag-scoped inputs** + selectable max
  article count → fits the single-pass D2 prompt exactly.
- **Native email delivery** to multiple recipients (one-time accept per recipient).
- **Monitoring feeds** (Boolean search-to-feed, ~30 on Pro, ≥hourly refresh) and
  **Web feeds** (turn any page into a feed / change-tracking) → the mechanism for
  Mauritius regulators that lack RSS.
- Ample source limits (2,500 feeds / 30 monitoring / 30 rules on Pro).

→ **D10 (Inoreader as platform): CONFIRMED** (provisional on numeric verification).
→ **D1/D9 (no custom code, no extra tools): HOLD.** Nothing found requires a
backend, DB, scraper, or orchestration platform.

## 2. Delivery — Email baseline strongly supported; Teams unlikely to win Gate 4

**Decisive finding: there is NO first-party Inoreader→Teams path for the AI
report.** Every Teams route requires an external connector (webhook rule into a
Teams incoming webhook, or Make/Zapier/Power Automate) **plus likely Teams admin
action** — precisely the orchestration/admin dependency D6 and D9 rule against.

Per your amendment #1, D6 is **not pre-locked**: Email is the baseline/control,
Teams remains the challenger, and Gate 4 still runs the empirical proof. But on
current evidence Teams **cannot beat email on "equal-or-lower operational
complexity with no unnecessary admin/orchestration"** — so Gate 4 is expected to
confirm email. We test rather than assume; we do not introduce a connector merely
to keep Teams alive.

## 3. Two mandatory design conditions

**C1 — Zero-stale must be enforced at the prompt level (not the platform).**
Inoreader's AI operates only on ingested article text and does **no** date
verification or staleness rejection; Web-feed change-tracking can actively
resurface edited/old pages as "new." Therefore the single AI pass (D2) must:
reject any item whose **underlying event date is not evidenced in the item
text**, treat feed/index/syndication dates as non-probative, and collapse
same-event duplicates. This is within D2 — it defines *how* the one pass works.

**C2 — Cross-publisher dedupe on event identity, not title similarity.** Reuse
the legacy `_soft_dedup` lesson: merge only when items share a **specific event
anchor** (named body + specific event word) + date proximity (~3 days) + modest
token overlap; **never** merge on a generic regulator name alone (two firms both
getting an FSC licence are two stories). Prevents both duplicate bloat and
wrongful merges.

## 4. Headline risk — Mauritius Tier-1 coverage ("zero regulator misses")

Only **Bank of Mauritius** exposes a native RSS (`bom.mu/latest-news.xml`,
proven). **FSC Mauritius (ARIE's own regulator), Ministry of Finance, EDB, and
FIU have no discoverable feed.** "Zero regulator misses" therefore rests entirely
on monitoring queries + a page-change watch for those bodies.

**Mitigation (Gate 2):** for each feedless Tier-1 body, configure a scoped
monitoring query **and** an Inoreader Web-feed page-watch; then run an
**acceptance test that injects a known FSC communiqué date and confirms it
surfaces** before go-live. Also: BoM's own feed is dominated by routine ops
(T-bill auctions, repo rate, reserves) — demote these in-prompt (legacy
`OPERATIONAL_NOISE_PATTERNS`), or real BoM regulatory news drowns.

## 5. Cost shape (verify live before purchase — payment gate)

- **Minimum viable:** **Pro** (~US$90/yr annual, snippet-sourced) **+ the
  Intelligence automated-report capability** (stated as a Pro/Custom add-on;
  add-on price UNCONFIRMED). A single Pro account can email the digest to several
  managers, so multi-recipient does **not** by itself force a team tier.
- **Team Intelligence** (team-size brackets, e.g. ~US$65–170/mo for 3–10 members;
  6M tokens/member) only if shared channels / collaborative curation is wanted.
- **Token quota watch:** Pro bundles ~1M Intelligence tokens/month; daily
  weekday reports over scoped article sets should fit but must be monitored.
  BYOAI (own API key) exists but reintroduces a "separate API" that D9 rules out
  of production — **not** recommended as default; keep bundled tokens.

**Recommendation:** start on **Pro + Intelligence add-on**; escalate to Team
Intelligence only if a demonstrated need appears. Confirm all figures on the live
pricing page at the purchase step.

## 6. Metrics realism (adjustment to §28 targets)

"Zero Tier-1 regulator misses" and "zero stale-as-new" are not platform-
guaranteed — they are enforced by monitoring+page-watch (C-risk 4) and prompt
discipline (C1/C2), and **verified by explicit acceptance tests** at Gate 2/3 and
tracked in the Gate 5 pilot. Recommend treating them as **pilot-tracked targets
with named test gates**, not binary V1 pass/fail guarantees. All other §28
targets stand.

---

## Design decisions this review sets up (to record at Gate 1)

- **Structure sources as one folder/tag per section** (Mauritius / Worldwide /
  Film&TV / Oil&Gas) + a Must-Catch set of monitoring queries mapping into them.
  Decide at Gate 2: one combined AI report over a parent folder vs. one report per
  section (cleaner but more token spend) — lean **one combined report** for
  simplicity/token economy, split only if quality needs it.
- **Reuse harvested guardrails** in the D2 prompt: procurement hard-drop,
  operational-noise demotion (esp. BoM), SEO/jurisdiction-guide exclusion,
  film ≤25% share + mandatory payment-angle test, "entities must appear
  literally / else empty string" grounding, no forced actions. Build the Oil & Gas
  relevance test fresh (no legacy precedent).
- **Keep a short curated competitor list** (Wise, Revolut, Airwallex, Payoneer,
  Nium, Rapyd, Stripe, Adyen, + Mauritius banks AfrAsia/Bank One/Absa) for the
  Worldwide must-catch — not the legacy 40-entry watchlist.

## Open items carried to later gates (none block Gate 1)

| # | Item | Resolve at | Needs user? |
|---|------|-----------|-------------|
| O1 | Confirm live Inoreader prices/currency + Intelligence add-on cost | Purchase step | Yes (payment) |
| O2 | Confirm report scheduler pins Mon–Fri 08:15 UTC+4 (else use email-digest scheduler) | Gate 2 | No |
| O3 | Verify each `RSS ~` feed URL + prove FSC/EDB/MoF page-watch works | Gate 2 | No |
| O4 | Confirm per-report article/length caps, folder cap, monthly token headroom | Gate 2 | No |
| O5 | FSC-communiqué injection acceptance test | Gate 2/3 | No |

## Governance / public-exposure check

This review and all Gate 0 outputs contain no secrets, private feed URLs,
management emails, tenant info, or client data. A2 noted the **legacy repo's
`CLAUDE.md` leaks Railway IDs + a prod URL**; these were **not** copied here.
Safe for permanent public exposure. ✅
