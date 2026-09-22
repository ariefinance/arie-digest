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
- **Film:** reject casting/celebrity/box-office. **Include material production /
  incentive / finance / jurisdiction developments** — new international productions,
  incentive/rebate changes, production-finance changes, major studio/production-company
  or shooting-jurisdiction moves — **even when the article does not literally mention
  payments** (these are in the approved Film scope). A payment/treasury/FX/escrow/
  settlement/repatriation/rebate-documentation/payroll/multi-currency angle raises an
  item toward "What Matters" and is required for a `COMMERCIAL SIGNAL`, but is **not**
  a precondition for inclusion. Cap film at **≤25%** of output.
- **Oil & Gas:** reject bare oil-price chatter and generic exploration. **Include**
  material refinery/LNG/project developments, acquisitions, supply agreements,
  trading-infrastructure changes, and cross-border corridor developments **where
  there is credible ARIE transaction/commercial relevance** — alongside the core
  physical trading + trade finance + sanctions/payment restriction + shipping.

## F. Freshness & duplication (C1/C2 — load-bearing)
- **C1 event-date rejection:** the report sees only the articles in the selected
  input — it cannot fetch an external primary source. It may treat an underlying
  **event date as established only when that date is evidenced within the selected
  input articles** (the item itself, or a corroborating article in the same batch).
  Feed/index/syndication dates are non-probative; old news republished today is not
  new. If no in-batch evidence fixes the event date, **reject** (do not present as
  new). Do not infer a date, and do not imply the report can verify it externally.
- **C2 event-identity dedupe:** same event across publishers = one story. Merge only
  when items share a **specific event anchor** — a named body (an abbreviation and its
  full name are the same body, e.g. "AMLA" = "EU anti-money-laundering authority") + a
  specific event word (e.g. "FATF plenary", "AMLA appointment", "X acquires Y") **AND**
  date proximity (~3 days). The **specific subject entity must also match** — never
  merge on a regulator/body name plus a generic event word alone (two firms each
  getting an FSC licence, or each facing an FSC action, are separate stories). Token
  overlap is a supporting hint only, never a requirement (differently-worded reports of
  the same event must still merge). This mirrors `digest/PROMPT.md` Step 3 — keep them
  in sync.

## G. Grounding (zero unsupported claims)
Entity names and facts must appear **literally** in the item title/summary — do not
infer. If a prose field can't be grounded in the provided data, leave it empty. No
forced actions; "monitor" is not an action. Empty sections beat weak content.

## Not doing (per D1/D4)
No giant deterministic keyword scoring system, no confidence scores, no owner
routing, no prospect engine. These lists are prompt guidance, not a scoring engine.
