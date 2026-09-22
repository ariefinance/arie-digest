# qa/fixtures/fixtures.md — ARIE Digest adversarial fixtures (Gate 3)

**Synthetic test data** (not live news; illustrative for testing prompt behaviour).
Each fixture: input as the report would see it + the **expected** outcome. "Today"
= 22 Sep 2026. Used to adversarially test `digest/PROMPT.md`; results in
`qa/QA_RESULTS.md`.

---

### F01 — Relevant Tier-1 regulation (EXPECT: ACCEPT → Mauritius)
- Title: "FSC Mauritius revises payment intermediary licensing conditions"
- Summary: "The Financial Services Commission issued a communiqué on 22 Sep 2026 revising licensing conditions for payment intermediaries, effective Q4."
- Source: FSC Mauritius · event date in text: 22 Sep 2026
- Expect: ACCEPT, Mauritius, likely What-Matters. ARIE relevance grounded (ARIE holds FSC payment-intermediary licence).

### F02 — Routine BoM operational noise (EXPECT: REJECT/demote)
- Title: "Bank of Mauritius — Results: Auction of 91-day Treasury Bills"
- Summary: "Weekly T-bill auction results; total accepted amount and weighted yield published."
- Source: Bank of Mauritius · 22 Sep 2026
- Expect: REJECT (routine operations, no policy change).

### F03 — Procurement (EXPECT: REJECT)
- Title: "Expression of Interest: Registration of Suppliers for IT services"
- Summary: "Invitation to bid for prequalification of vendors."
- Source: gov portal · 22 Sep 2026
- Expect: REJECT (procurement hard-drop).

### F04 — Stale / resurfaced (EXPECT: REJECT — C1)
- Title: "FATF removes country X from grey list"
- Summary: "Article republished today; body states the delisting occurred at the FATF plenary in October 2024."
- Source: aggregator · feed date 22 Sep 2026; event date in text: Oct 2024
- Expect: REJECT (event date evidenced as 2024; feed date is not freshness).

### F05a / F05b — Syndicated duplicate coverage (EXPECT: MERGE to ONE — C2)
- F05a Title: "AMLA appoints new Chair" — Source: Finextra · 22 Sep 2026
- F05b Title: "EU anti-money-laundering authority names its first Chair" — Source: PYMNTS · 22 Sep 2026 (same event, different wording)
- Expect: ONE story (shared anchor "AMLA"/"Chair appointment", within 3 days). Keep most-primary link; note corroboration.

### F06a / F06b — Distinct events, same regulator (EXPECT: DO NOT MERGE — C2)
- F06a Title: "FSC Mauritius grants payment licence to Company A" — 22 Sep 2026
- F06b Title: "FSC Mauritius grants investment-dealer licence to Company B" — 22 Sep 2026
- Expect: TWO separate stories. Must NOT merge on "FSC" alone (different entities/events).

### F07a — Strong Commercial Signal (EXPECT: ACCEPT + COMMERCIAL SIGNAL)
- Title: "Company C (named) announces expansion into three African markets, opening treasury operations"
- Summary: "Company C will open foreign operations across Africa; press release cites cross-border payment and multi-currency treasury needs."
- Source: Company C newsroom · 22 Sep 2026
- Expect: ACCEPT (Worldwide) + COMMERCIAL SIGNAL (named entity + specific trigger [new foreign operations] + evidenced cross-border payments angle).

### F07b — Weak Commercial Signal (EXPECT: no signal; likely REJECT)
- Title: "Fintech startup raises Series B to expand AI product"
- Summary: "Generic funding round; no named international operation or payments angle."
- Source: trade blog · 22 Sep 2026
- Expect: NO COMMERCIAL SIGNAL; REJECT as generic fundraising (no specific ARIE-relevant trigger).

### F08 — Generic fintech/crypto noise (EXPECT: REJECT)
- Title: "Where is the next profitable frontier for Agentic AI and everyday hardware?"
- Summary: "Opinion piece on AI hardware trends."
- Source: blog · 22 Sep 2026
- Expect: REJECT (topic-keyword false positive; no specific ARIE trigger).

### F09 — Film casting/celebrity noise (EXPECT: REJECT)
- Title: "Star actor cast in new streaming series"
- Summary: "Casting announcement; box-office expectations."
- Source: entertainment site · 22 Sep 2026
- Expect: REJECT (casting/celebrity).

### F10 — Legitimate Film production/incentive/finance (EXPECT: ACCEPT → Film & TV)
- Title: "Ireland raises Section 481 film production incentive cap"
- Summary: "Screen Ireland confirms an increase to the production incentive ceiling for international productions, effective January."
- Source: Screen Ireland · 22 Sep 2026
- Expect: ACCEPT, Film & TV (material incentive change — no literal payments mention required). ARIE relevance: incentive changes drive cross-border production-payment/treasury activity.

### F11 — Generic oil-price/exploration noise (EXPECT: REJECT)
- Title: "Brent crude edges up 0.4% on inventory data"
- Summary: "Daily price movement; analyst commentary."
- Source: market wire · 22 Sep 2026
- Expect: REJECT (bare price chatter).

### F12 — Legitimate refinery/LNG/acquisition/supply/corridor (EXPECT: ACCEPT → Oil & Gas)
- Title: "Trader D signs multi-year LNG supply agreement for West Africa terminal"
- Summary: "Company D agrees a cross-border LNG supply deal serving a new West African import terminal."
- Source: energy trade press · 22 Sep 2026
- Expect: ACCEPT, Oil & Gas (material supply agreement + cross-border corridor with plausible ARIE payment relevance). Possible COMMERCIAL SIGNAL only if all 3 conditions evidenced.

### F13 — Direct ARIE mention (EXPECT: ACCEPT, high priority)
- Title: "ARIE Finance launches new corridor for Mauritius–India payments"
- Summary: "ARIE Finance (named) announces a cross-border payment corridor."
- Source: press · 22 Sep 2026
- Expect: ACCEPT, likely What-Matters (direct ARIE mention). Grounded strictly to text.

### F14 — Adversarial: 2026-dated page, 2025 event (EXPECT: REJECT — C1 trap)
- Title: "Central bank launches instant-payment system"
- Summary: "Page carries a 2026 index date, but the body says the launch took place in March 2025."
- Source: publisher index date 2026; event date in text: Mar 2025
- Expect: REJECT (the exact stale-as-new failure mode from the mandate — event date governs).
