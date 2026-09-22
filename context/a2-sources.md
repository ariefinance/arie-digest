# A2 — Curated Source Set (ARIE Digest, Gate 0)

Author: ismael@ariefinance.com · Date: 2026-09-22 · Agent: A2 (Source Intelligence)
Scope: curated, **not exhaustive**, aligned to the four visible sections +
"What Matters Today" (see brief §The product / DECISIONS D3).

**Feed-status legend**
- `RSS ✅` — feed confirmed (legacy repo `RSS_SOURCES` and/or web search).
- `RSS ~` — feed very likely exists but URL not verified this session (do not
  assume in production; verify the exact URL before wiring).
- `RSS ✖` — no discoverable native feed → use a monitoring/search query (Tier 3)
  or lightweight page-watch.
- Feed-existence checks were done via search + legacy config only. Direct fetch
  of external sites is blocked by this session's egress proxy, so URLs marked
  `~` need a one-time live confirmation at Gate 2.

> Egress note: `www.bom.mu`, `www.fscmauritius.org`, `edbmauritius.org`,
> `mof.govmu.org` etc. returned 403 on CONNECT from the agent proxy (org egress
> policy). Availability below is from Anthropic-side WebSearch + the legacy
> repo's proven config, not from a direct fetch here.

---

## ⚠️ Mauritius Tier-1 RSS availability call-out (KNOWN PROJECT RISK)

Goal is **zero regulator misses**. Only **one** Mauritius Tier-1 body exposes a
usable native feed. The rest must be caught by monitoring queries or a
page-watch — this is the single biggest collection risk in the whole Digest.

| Mauritius Tier-1 source | Native RSS? | Fallback needed | Notes |
|---|---|---|---|
| **Bank of Mauritius (BoM)** | **RSS ✅** | — | `https://www.bom.mu/latest-news.xml` (proven in legacy, weight 0.90). BoM `/rss` page also lists **Latest News**, **Exchange Rate**, **Upcoming Events** feeds. Demote operational noise (T-bill auctions, repo rate, reserves) — see a2-noise-lessons. |
| **FSC Mauritius** (ARIE's own regulator) | **RSS ✖** | **Monitoring query + page-watch** | No discoverable feed. Highest-priority miss risk. Watch `media-corner/communiques`, `media-corner/press-releases`, `others/news`. Legacy relied on a Google-News proxy query (weight 0.60) — weak; supplement with a direct page-watch. |
| **Ministry of Finance (MoF)** `mof.govmu.org` | **RSS ✖** | Monitoring query | Budget, tax, financial-services policy. No feed found. |
| **Economic Development Board (EDB)** | **RSS ✖** | Monitoring query + page-watch | Newsroom exists (`edbmauritius.org/newsroom`) but no feed found. Investment/incentive/film announcements land here. |
| **FIU Mauritius** (AML/CFT authority) | **RSS ✖** | Monitoring query | Financial Intelligence Unit — AML/CFT guidance/enforcement. No feed found. |
| **NCPO / AML-CFT Core Group / MoFA sanctions notices** | **RSS ✖** | Monitoring query | Mauritius sanctions transposition notices; no feed. |

**Implication for platform choice (feeds into D10 / Gate 0):** any collection
platform (Inoreader) must combine RSS with **keyword/search monitoring** and
ideally a **page-change watch** for FSC + EDB + MoF, or those Tier-1 regulators
will be silently missed on quiet feed days. Recommend a Gate-2 acceptance test:
"inject a known FSC communiqué date and confirm it surfaces."

---

## Tier 1 — Authoritative (primary issuers / regulators)

### Mauritius (see call-out above)
| Source | URL | Section(s) | Feed |
|---|---|---|---|
| Bank of Mauritius | https://www.bom.mu/latest-news.xml | Mauritius; What Matters | RSS ✅ |
| FSC Mauritius | https://www.fscmauritius.org/en/media-corner/communiques | Mauritius; What Matters | RSS ✖ → query+watch |
| Ministry of Finance MU | https://mof.govmu.org/ | Mauritius | RSS ✖ → query |
| EDB Mauritius | https://edbmauritius.org/newsroom | Mauritius; Film&TV | RSS ✖ → query+watch |
| FIU Mauritius | https://www.fiumauritius.org/ | Mauritius; Worldwide(AML) | RSS ✖ → query |

### International regulators / standard-setters
| Source | URL | Section(s) | Feed |
|---|---|---|---|
| FATF (news + publications) | https://www.fatf-gafi.org/en/publications.html | Worldwide | RSS ~ (native inconsistent; legacy used GNews proxy w/ browser UA — keep as backup) |
| FSB (Financial Stability Board) | https://www.fsb.org/ | Worldwide | RSS ~ (press/publications feeds; verify) |
| BIS + CPMI | https://www.bis.org/ | Worldwide | RSS ~ (BIS offers topic feeds incl. payments/CPMI; verify) |
| SWIFT newsroom | https://www.swift.com/news-events | Worldwide | RSS ✖/~ → monitoring query (ISO 20022, gpi, rail changes) |
| FCA (UK) | https://www.fca.org.uk/news | Worldwide | RSS ~ (legacy: RSS-first, HTML fallback) |
| ECB (press + publications) | https://www.ecb.europa.eu/rss/press.html · /rss/pub.html | Worldwide | RSS ✅ (legacy, weight 0.85) |
| EBA (European Banking Authority) | https://www.eba.europa.eu/ | Worldwide | RSS ~ (verify) |
| EU AMLA | https://www.amla.europa.eu/news-media/news-articles_en | Worldwide | RSS ~ (news page may expose feed; unverified — treat as query until confirmed) |
| OFAC (US Treasury) recent actions | https://ofac.treasury.gov/recent-actions | Worldwide; Oil&Gas | RSS ✅ (OFAC Recent Actions feed, live since 2006) |
| OFSI (UK) | https://ofsi.blog.gov.uk/feed/ | Worldwide; Oil&Gas | RSS ✅ |

### Industry primary (company newsrooms / incentive authorities)
| Source | URL | Section(s) | Feed |
|---|---|---|---|
| Competitor newsrooms (Wise, Revolut, Airwallex, Payoneer, Nium, Rapyd) | each `/newsroom` or `/press` | Worldwide | RSS ~ mixed → per-company query where no feed |
| Film commission / incentive authorities (see Tier 2 film) | — | Film&TV | mostly RSS ✖ → query |

---

## Tier 2 — Specialist trade press

### Payments / cross-border finance
| Source | URL | Section(s) | Feed |
|---|---|---|---|
| Finextra | https://www.finextra.com/rss/headlines.aspx | Worldwide | RSS ✅ (legacy 0.80) |
| The Paypers | https://thepaypers.com/news | Worldwide | RSS ✖ (legacy scraped HTML) → HTML/query |
| PYMNTS (cross-border) | https://www.pymnts.com/category/cross-border-commerce/feed/ | Worldwide | RSS ✅ (legacy 0.75) |
| FinTech Global | https://fintech.global/feed/ | Worldwide | RSS ✅ (legacy 0.75) |
| Central Banking / Global Finance / The Banker | resp. sites | Worldwide | RSS ~ (verify; strong for correspondent banking/de-risking) |

### Film & TV (production finance / incentives)
| Source | URL | Section(s) | Feed |
|---|---|---|---|
| Screen Daily (production/finance/incentives) | https://www.screendaily.com/ | Film&TV | RSS ~ (legacy used GNews proxy `GNews/Screen_Daily`) |
| Variety / THR (business & production finance) | resp. sites | Film&TV | RSS ~ → filter hard for finance/incentive only (reject casting/box-office per brief) |
| KFTV / film-commission incentive trackers | https://www.kftv.com/ | Film&TV | RSS ✖ → query |
| Mauritius film incentive (via EDB) | edbmauritius.org/newsroom | Film&TV | RSS ✖ → query (`Mauritius film rebate`) |

### Oil & Gas (physical trading / trade finance / sanctions)
| Source | URL | Section(s) | Feed |
|---|---|---|---|
| Reuters Energy / Commodities | https://www.reuters.com/business/energy/ | Oil&Gas | RSS ~ (verify; core) |
| S&P Global Commodity Insights (Platts) | https://www.spglobal.com/commodityinsights/ | Oil&Gas | RSS ~ → query if no feed |
| TradeWinds / Lloyd's List (shipping/tankers/sanctions) | resp. sites | Oil&Gas | RSS ~ / paywalled → headline query |
| Global Trade Review (GTR — trade finance) | https://www.gtreview.com/ | Oil&Gas; Worldwide | RSS ~ (verify) |
| OFAC/OFSI energy designations | (see Tier 1 sanctions feeds) | Oil&Gas | RSS ✅ reuse |

---

## Tier 3 — Discovery (monitoring / search queries)

**Must not dominate output.** These fill gaps where no reliable native feed
exists — especially the Mauritius Tier-1 regulators. Keep tightly scoped and let
the AI reject/dedupe per DECISIONS D2. Underlying-event-date discipline applies
(brief §Date & evidence discipline): a search hit's index date ≠ event date.

**Mauritius Tier-1 gap-fillers (highest priority):**
- `"FSC Mauritius" (communiqué OR licence OR enforcement OR "financial services")`
- `"Financial Services Commission" Mauritius (revoked OR sanction OR authorised)`
- `"Bank of Mauritius" (guideline OR licence OR AML OR "payment")` (RSS backstop)
- `"Ministry of Finance" Mauritius (budget OR tax OR "financial services")`
- `"Economic Development Board" Mauritius (investment OR incentive OR film)`
- `Mauritius FIU (AML OR CFT OR guidance OR typology)`
- `Mauritius (FATF OR greylist OR "mutual evaluation")`  ← reputation risk

**Worldwide gap-fillers:**
- `correspondent banking (de-risking OR "account closure" OR nostro OR exit)`
- `(SWIFT OR SEPA OR ISO 20022 OR "instant payments") (change OR outage OR migration)`
- `cross-border payments (corridor OR restriction OR capital controls) Africa OR India OR MENA`
- `(sanctions OR OFAC OR OFSI) (payment OR bank OR de-listing) [named entity]`
- `AMLA EU (supervision OR "selected obliged entities" OR guidance)`

**Film & TV gap-fillers:**
- `film (rebate OR "tax incentive" OR "cash rebate") (change OR increase OR cap) [jurisdiction]`
- `"production finance" (completion bond OR "gap financing" OR co-production treaty)`
- `Mauritius film (rebate OR production OR "film commission")`

**Oil & Gas gap-fillers:**
- `(oil OR LNG OR crude) (sanctions OR "payment restriction" OR "trade finance") (Russia OR Iran OR Venezuela)`
- `(tanker OR "shadow fleet" OR "dark fleet") (sanction OR designation OR insurance)`
- `Africa OR "Middle East" (refinery OR LNG OR "oil terminal") (project OR financing OR payment)`

---

## Biggest source gaps / risks (summary)

1. **Mauritius Tier-1 feeds:** only **Bank of Mauritius** has a native RSS.
   **FSC Mauritius (ARIE's own regulator), MoF, EDB, FIU have no discoverable
   feed** → "zero regulator misses" depends entirely on monitoring queries +
   a page-watch. Highest risk. Add a Gate-2 acceptance test.
2. **Search-date ≠ event-date:** Tier-3 queries and Google-News proxies carry
   stale/syndicated items; the freshness discipline and dedupe (see
   a2-noise-lessons) are load-bearing, not optional.
3. **Egress:** external regulator sites are blocked from this session, so all
   `RSS ~` URLs need one live confirmation before production wiring (Gate 2).
