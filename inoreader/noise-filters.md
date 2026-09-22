# inoreader/noise-filters.md — Noise & exclusion design (Gate 2A)

Where filtering lives: **the single Intelligence-report prompt does the rejection**
(D2), not the collection layer — collection stays reversible (folders.md). This doc
specifies the exclusion rules the prompt must encode. Harvested from
`context/a2-noise-lessons.md` (legacy lessons); **adapted, not copied wholesale.**

## A. Hard-drop (never intelligence — reject regardless of source)
Procurement/tender noise — any of:
`expression of interest`, `registration of suppliers/vendors`, `invitation to bid`,
`request for quotation`, `request for tender`, `call for bids/tenders/proposals`,
`prequalification`.

## B. Operational-noise demotion (esp. Bank of Mauritius feed)
Routine central-bank/market ops — demote hard (do not surface unless genuinely
material policy change): `treasury bill/certificate`, `auction`, `advance notice`,
`issue of`, `prospectus`, `dissemination of`, `exchange rate indices`, `weekly`,
`results:`, `repo rate` (unless a *change* with policy signalling), `open market
operations`, `secondary market transactions`, `gross official international
reserves`, `gross tourism earnings`, `central bank survey`.
→ Without this, BoM's feed drowns real regulatory news.

## C. Evergreen / SEO guide exclusion
Reject "guide" content masquerading as news: `fintech laws and regulations 20xx`,
`country guide`, `iclg`, `jurisdiction guide`, `regulatory guide` — unless the item
is genuinely a dated development with Mauritius (or a target sector) as the subject.

## D. Topic-keyword false-positive gate (relevance, not just keyword)
Generic AI / SaaS / funding / crypto / hardware chatter reads as relevant on a bare
keyword match. Require a **specific observable trigger**, not just a topic word.
Reject examples (QA fixtures): "next profitable frontier for Agentic AI + Everyday
Hardware"; "Fintech firm raises Series B to expand AI product". Generic fintech
fundraising and generic crypto speculation are rejected (brief).

## E. Sector-specific noise
- **Film:** reject casting/celebrity/box-office. A film item earns prominence only
  with a **payment/treasury/FX/escrow/settlement/repatriation/rebate-documentation/
  production-accounting/payroll/multi-currency** angle. Cap film at **≤25%** of output.
- **Oil & Gas:** reject bare oil-price chatter and generic exploration. Keep only
  physical trading + trade finance + sanctions/payment restriction + shipping.

## F. Freshness & duplication (C1/C2 — load-bearing)
- **C1 event-date rejection:** reject any item whose **underlying event date** is
  not evidenced in the item text; feed/index/syndication dates are non-probative;
  old news republished today is not new. Missing date ≠ auto-reject, but must be
  corroborated before use.
- **C2 event-identity dedupe:** same event across publishers = one story. Merge only
  when items share a **specific event anchor** (named body + specific event word,
  e.g. "FATF plenary", "AMLA appointment") **AND** date proximity (~3 days) **AND**
  modest token overlap. **Never** merge on a generic regulator name alone (two firms
  each getting an FSC licence are two stories).

## G. Grounding (zero unsupported claims)
Entity names and facts must appear **literally** in the item title/summary — do not
infer. If a prose field can't be grounded in the provided data, leave it empty. No
forced actions; "monitor" is not an action. Empty sections beat weak content.

## Not doing (per D1/D4)
No giant deterministic keyword scoring system, no confidence scores, no owner
routing, no prospect engine. These lists are prompt guidance, not a scoring engine.
