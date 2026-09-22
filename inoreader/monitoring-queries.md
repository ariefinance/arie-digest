# inoreader/monitoring-queries.md — Monitoring feed queries (Gate 2A)

Boolean monitoring feeds fill gaps where no reliable native feed exists — above all
the feedless Mauritius Tier-1 regulators. Inoreader query syntax: `AND OR NOT`,
parentheses, "quoted phrases", wildcards `*`. Keep tightly scoped; the report does
final reject/dedupe (D2). Underlying-event-date discipline applies (C1): a search
hit's index date is not the event date.

Verify counts/refresh at Gate 2B (Pro allows ~30 monitoring feeds — ample).

## Mauritius Tier-1 gap-fillers → `01-Mauritius` (highest priority)
- **MQ-MU-FSC:** `"FSC Mauritius" OR "Financial Services Commission" Mauritius` `AND (communiqué OR licence OR licensee OR revoked OR sanction OR enforcement OR authorised OR "financial services")`
- **MQ-MU-MOF:** `"Ministry of Finance" Mauritius AND (budget OR tax OR levy OR "financial services" OR "money laundering")`
- **MQ-MU-EDB:** `"Economic Development Board" Mauritius AND (investment OR incentive OR licence OR film OR rebate)`
- **MQ-MU-FIU:** `Mauritius (FIU OR "Financial Intelligence Unit") AND (AML OR CFT OR guidance OR typology OR enforcement)`
- **MQ-MU-BOM (backstop to RSS):** `"Bank of Mauritius" AND (guideline OR licence OR AML OR payment OR fintech OR "national payment")`
- **MQ-MU-FATF:** `Mauritius AND (FATF OR greylist OR "grey list" OR "mutual evaluation")`  ← reputation risk

## Must-Catch (cross-cutting) → `05-Must-Catch`
- **MQ-WW-CORRBANK:** `"correspondent banking" AND (de-risking OR "de-risking" OR "account closure" OR nostro OR exit OR withdrawal)`
- **MQ-WW-RAILS:** `(SWIFT OR SEPA OR "ISO 20022" OR "instant payments" OR "real-time payments") AND (change OR outage OR migration OR mandate OR deadline)`
- **MQ-WW-CORRIDOR:** `"cross-border payments" AND (corridor OR restriction OR "capital controls" OR convertibility) AND (Africa OR India OR MENA OR "Middle East")`
- **MQ-WW-SANCTIONS:** `(sanctions OR OFAC OR OFSI OR EU) AND (payment OR bank OR "de-listing" OR designation)`
- **MQ-WW-AMLA:** `AMLA EU AND (supervision OR "selected obliged entities" OR guidance)`
- **MQ-WW-OUTAGE:** `(bank OR payment OR fintech) AND (outage OR "service disruption" OR failure OR insolvency OR "wind down")`

## Competitor watch → `05-Must-Catch`, tag `competitor`
- **MQ-WW-COMPET:** `(Wise OR Revolut OR Airwallex OR Payoneer OR Nium OR Rapyd OR Stripe OR Adyen OR "AfrAsia" OR "Bank One" OR "Absa Mauritius") AND (licence OR expansion OR acquisition OR partnership OR corridor OR "cross-border" OR outage OR fine)`

## Commercial-signal candidates → `05-Must-Catch`, tag `commercial-signal-candidate`
Narrowly scoped discovery of a **named company + observable international trigger**
(D4 condition 1+2). These only *surface candidates*; the report still applies the
strict 3-condition D4 guardrail (named entity + specific trigger + evidenced ARIE
payments angle) and rejects anything unproven. Keep to two queries (avoid a prospect
engine, D4).
- **MQ-CS-EXPANSION:** `(company OR firm OR group OR "Ltd" OR "Limited") AND ("expands into" OR "enters the" OR "new market" OR "opens office" OR "sets up operations" OR "international expansion" OR "cross-border") AND (Africa OR India OR "Middle East" OR Mauritius OR Europe OR Asia)`
- **MQ-CS-DEALS:** `(acquires OR acquisition OR "major contract" OR "wins contract" OR "secures licence" OR "raises" ) AND (international OR "cross-border" OR overseas OR foreign) AND (payment OR treasury OR FX OR trade)`

## Direct ARIE mention watch → `05-Must-Catch`, tag `mauritius-tier1`
- **MQ-MC-ARIE:** `"ARIE Finance" OR "Arie Capital Investment"`  ← direct mention watch (moved out of Discovery so it always reaches the report). Bare `ACBM` dropped as a standalone term — too ambiguous.

## Film & TV → `03-Film`
- **MQ-FILM-INCENTIVE:** `film AND (rebate OR "tax incentive" OR "cash rebate") AND (change OR increase OR cap OR launch) `
- **MQ-FILM-FINANCE:** `"production finance" OR "completion bond" OR "gap financing" OR "co-production treaty"`
- **MQ-FILM-MU:** `Mauritius film AND (rebate OR production OR "film commission")`

## Oil & Gas → `04-Oil-Gas`
- **MQ-OG-SANCTIONS:** `(oil OR LNG OR crude) AND (sanctions OR "payment restriction" OR "trade finance") AND (Russia OR Iran OR Venezuela)`
- **MQ-OG-SHIPPING:** `(tanker OR "shadow fleet" OR "dark fleet") AND (sanction OR designation OR insurance)`
- **MQ-OG-PROJECT:** `(Africa OR "Middle East") AND (refinery OR LNG OR "oil terminal") AND (project OR financing OR payment)`

## Discovery (broad) → `90-Discovery` (NOT in report input by default)
- **MQ-DISC-CBPAY:** `"cross-border payment intermediary" Mauritius OR "payment service provider" Mauritius licence`  ← broad sector discovery
- Kept intentionally few. `90-Discovery` is excluded from `00-Digest-Input`; promote
  a query into the input only if it proves its worth. (Direct ARIE-mention watch was
  moved to Must-Catch above so it always reaches the report.)
