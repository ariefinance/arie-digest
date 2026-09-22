# A2 — Noise, Exclusion & Prompt Lessons (harvested from legacy repo)

Author: ismael@ariefinance.com · Date: 2026-09-22 · Agent: A2

**Legacy clone: SUCCEEDED.** `git clone --depth 1
https://github.com/ariefinance/arie-intelligence-command-centre.git`
→ `/tmp/legacy-acc` (public repo, exit 0). Everything below is extracted from
that clone with file paths. Per D7, this is harvest-only: we reuse *lessons*,
not the architecture or the giant keyword config. The legacy system was a
Railway/FastAPI app with a scraping+enrichment+scoring pipeline — exactly the
over-engineering DECISIONS D1/D2 rule out. Take the noise knowledge, drop the
machinery.

---

## (a) Validated / high-value sources (proven working in legacy)
From `scraper.py` `RSS_SOURCES` (native feeds that worked with a **browser
User-Agent** — datacenter IPs get 0 results without it):
- Bank of Mauritius `latest-news.xml` (source weight 0.90 — highest)
- ECB Press + ECB Publications RSS (0.85)
- Finextra `rss/headlines.aspx` (0.80); PYMNTS cross-border feed; FinTech Global
- FCA: RSS-first with HTML fallback (`_try_fca_rss()`, multiple candidate feed
  URLs tried, `scraper.py` ~L96-99)
- The Paypers: **no feed** — HTML-scraped article list (`scraper.py` ~L412)

**Lesson:** feeds need `agent=BROWSER_UA` in feedparser (`scraper.py` L138-143,
L367); FATF + all Google-News topic feeds return 0 from server IPs otherwise
(`CLAUDE.md` Known Quirks). Native publisher RSS > Google-News aggregator >
Reddit for trust (`config.py` `SOURCE_WEIGHTS`).

## (b) Exclusion & false-positive lessons
From `config.py` + `enrich.py` + `tests/`:
- **Procurement hard-drop** (`config.py` `PROCUREMENT_HARD_DROP_PATTERNS`,
  L306): "expression of interest", "registration of suppliers/vendors",
  "invitation to bid", "request for quotation/tender", "call for
  bids/tenders/proposals", "prequalification". These are *never* intelligence —
  absolute filter regardless of source. Maps to brief's "procurement/tenders".
- **Operational-noise demotion** (`config.py` `OPERATIONAL_NOISE_PATTERNS`,
  L335): treasury bill/certificate, auction, "advance notice", "issue of",
  prospectus, "dissemination of", "exchange rate indices", weekly, "results:",
  repo rate, "open market operations", "secondary market transactions", "gross
  official international reserves", "gross tourism earnings", "central bank
  survey". **Critical for Bank of Mauritius** — its feed is dominated by these
  routine bulletins; without demotion they drown real regulatory news. (Legacy
  demoted rather than dropped, ×0.25.)
- **Mauritius jurisdiction-guide exclusion** (`config.py`
  `MAURITIUS_GUIDE_EXCLUSION_PATTERNS`, L324): "fintech laws and regulations
  2024/25/26", "country guide", "iclg", "jurisdiction guide", "regulatory
  guide" — evergreen SEO guides that masquerade as Mauritius news. Drop unless
  "mauritius" is genuinely the subject. Maps to brief's "SEO guides".
- **CX/pain false positives** (`tests/test_cx_relevance.py`): real live
  false-positives that shipped and had to be gated out —
  - "Where is the next profitable frontier for Agentic AI + Everyday Hardware?"
  - "Fintech firm raises Series B funding round to expand AI product"
  Lesson: generic AI / SaaS / funding / crypto / hardware chatter reads as
  relevant on keyword match alone. Legacy added a strict gate (`is_cx_relevant`,
  `enrich.py` L660): require a money-term **AND** a problem-term, or a strong
  pain phrase. Our single AI pass (D2) must apply the same "specific trigger,
  not just topic keyword" test.
- **Promo-vs-complaint exclusion** (`config.py` `PAIN_EXCLUDE_KEYWORDS`, L467):
  "press release", "raises $/€", "funding round", "series a/b/c", "launches",
  "announces", "acquires", "case study", "webinar", "whitepaper", "report
  finds", "market size" — strip these from genuine-complaint detection.
- **HTML-entity hygiene** (`tests/test_esc_entities.py`): feed titles arrive
  with literal `&nbsp;&nbsp;` / `&amp;`; decode-then-escape once (never
  double-escape) or the Digest renders visible `&nbsp;`.

## (c) Stale-story / syndication lessons
From `enrich.py`:
- **Stale cutoff** (`_is_stale`, L562): drop items > 60 days old by
  `published_at`; items with **no date pass through** (unknown age ≠ stale).
  Note: our brief is stricter (daily/weekend) — use a much tighter window, but
  keep the "missing date ≠ auto-reject, but must be corroborated" principle.
- **Near-duplicate dedup** (`_dedup`, L435): Jaccard > 0.70 on title tokens;
  survivor accumulates a supporting-source count.
- **Soft cross-publisher cluster dedup** (`_soft_dedup`, L504): the real lesson
  for "same event across publishers = one story" (brief). Merge when items share
  ≥1 **event-specific anchor word** (`_CLUSTER_EVENT_ANCHORS`, L476: fatf, imf,
  bis, amla, mica, greylist, plenary, "vice"/"presidency"/"appointed" …) AND
  Jaccard ≥ 0.25 AND published within 3 days. Deliberately **excludes** generic
  regulator names (fsc, sec) as anchors — two different companies getting an FSC
  licence must NOT merge. Worked example in code: "FATF India vice-presidency"
  covered by 4 outlets with different phrasing → one story.
  **Lesson for D2 prompt:** dedupe on *event identity* (named body + specific
  event word + date proximity), not on title similarity alone.

## (d) Film & oil relevance lessons
From `config.py`:
- **Film cap** (`FILM_MAX_SHARE = 0.25`, L126): cap film at ≤25% of output so it
  never crowds core payments news — matches brief's Film section being one of
  four, not the headline. Legacy also *reduced* the film priority boost after
  generic film items over-ranked (L114 comment).
- **Film must have a payment angle** (`FILM_PAYMENT_ANGLE_KEYWORDS`, L130): a
  film item only earns prominence if it also mentions payment/treasury/fx/
  escrow/settlement/repatriation/"rebate documentation"/"production
  accounting"/payroll/multi-currency. **Directly reusable** as the brief's Film
  test ("cross-border production payments"), and as the Commercial-Signal ARIE
  angle. Reject casting/celebrity/box-office (brief).
- **Oil/Oil-relevance:** legacy had *no* dedicated Oil & Gas stream — this is a
  **new** Digest section with no legacy precedent, so build its relevance test
  fresh: physical trading + trade finance + sanctions/payment restriction +
  shipping, reject bare oil-price chatter and generic exploration (brief).
  Reuse the sanctions/OFAC/OFSI feeds (a2-sources Tier 1) as the spine.

## (e) Competitor names (harvest — curate, don't copy wholesale)
`config.py` `COMPETITOR_NAMES` (L618) + `WATCHLIST` (L161):
Wise, Revolut, Airwallex, Payoneer, Mercury, WorldFirst, OFX, Nium, Rapyd,
Stripe, Adyen, PayPal, TransferMate, Currencies Direct, Brex, Monzo; contractor
platforms Deel, Remote, Multiplier; Mauritius banks AfrAsia, Bank One, Absa
Mauritius. Stablecoin watch: Circle, Ripple, Tether, Coinbase.
**Lesson:** keep a *short* curated competitor list for the Worldwide "important
competitor moves" must-catch; don't reproduce the full 40-entry watchlist —
that volume of named-entity matching is exactly the over-fit the brief warns
against ("Never fill a quota").

## (f) ARIE relevance concepts (reusable framing)
From `intelligence.py` `DIGEST_SYSTEM_PROMPT` (L527) + `config.py`
`COMMERCIAL_SIGNALS` (L384):
- ARIE = FSC-Mauritius-licensed **payment intermediary** (licensed Feb 2026,
  launched May 2026); primary channel = **introducers** (CSPs, management
  companies, trust/fiduciary, accountants, law firms). "Never position as a
  bank." (Matches brief.)
- Legacy commercial-signal axes worth reusing as *relevance reasoning* (not as
  scores — D2/D4 forbid scoring): introducers/CSP ecosystem; cross-border SME
  trade; payment friction / correspondent-banking / de-risking pain; onboarding/
  KYC burden; FX/treasury; priority corridors (Africa/India/MENA/Mauritius);
  stablecoin+trade-finance (only when paired with a payment term).
- Two BD angles baked into the legacy prompt (L533): (1) Mauritius ~30-40% film
  rebate → production treasury/repatriation; (2) firms expanding into
  Africa/Mauritius needing multi-currency treasury.

## (g) Reusable QA-fixture candidates
Turn these directly into Gate-3 QA test items (accept/reject expectations):
- REJECT: "Where is the next profitable frontier for Agentic AI + Everyday
  Hardware?" (topic-keyword false positive) — `tests/test_cx_relevance.py`
- REJECT: "Fintech firm raises Series B funding round to expand AI product"
  (generic funding) — same file
- REJECT: any "Expression of Interest" / "Registration of Suppliers" / "Request
  for Quotation" (procurement) — `config.py` L306
- REJECT / DEMOTE: BoM "Results: Treasury Bills", "Repo Rate", "Gross Official
  International Reserves", "Open Market Operations" (routine central-bank ops) —
  `config.py` L335
- REJECT: "Fintech Laws and Regulations 2026" / "ICLG country guide" (evergreen
  SEO) — `config.py` L324
- DEDUPE-TO-ONE: the same FATF/AMLA/BIS event carried by 3-4 outlets with
  different headlines within 3 days — `enrich.py` `_soft_dedup` worked example
- ENTITY-HYGIENE: title containing `&nbsp;&nbsp;` must render clean —
  `tests/test_esc_entities.py`
- FILM CAP: a day heavy on film news must not exceed ~25% film in output, and a
  film item with no payment angle must not reach "What Matters" — `config.py`
  L126-155
- ACCEPT: a new FSC Mauritius licensee / communiqué (the "zero regulator
  misses" positive test) — inject a known FSC item and confirm it surfaces.

## (h) Useful prompt concepts (adapt for the single AI pass, D2)
From `intelligence.py` `DIGEST_SYSTEM_PROMPT` (L527-660):
- **Grounding rule:** "Entity names must appear literally in the item title or
  summary — do not infer." + "If you cannot ground a prose field in the provided
  data, return an empty string." → directly supports brief's "zero unsupported
  claims".
- **One factual sentence + one why-it-matters sentence** per item — matches the
  brief's item format almost exactly; reuse the phrasing.
- **No forced action:** "Do NOT force an action if none is plausible" / "Do NOT
  use 'monitor' as a suggested_action alone." → aligns with brief's "Empty
  sections beat weak content" and "Never fill a quota."
- **Film framing directive:** "relevance must focus on the PAYMENT/TREASURY/
  REBATE-DOCUMENTATION angle" → reuse verbatim for the Film section.
- **Institutional tone / compliance caution** boilerplate (L545): "Do not
  promise onboarding outcomes, FX pricing, or returns. Do not position Arie as a
  bank." → good guardrail phrasing for any generated prose.
- **DROP for our project (over-engineering, per D1/D2/D4):** suggested_owner
  routing (BD/Ops/Compliance/Management), confidence levels, commercial scores,
  Opportunity Radar, trajectory/trend memory, LinkedIn post generation, prospect
  engine. The brief explicitly bans scores/badges/owners/actions/outreach.

---

### One safety note (governance, brief §Governance)
The legacy repo is public and its `.env.example` uses placeholder secrets only
(`sk-ant-...`), but its `CLAUDE.md` leaks Railway project/service IDs and a
production URL. Do **not** copy those into this repo. Nothing harvested above
carries secrets, private feed URLs, or client data.
