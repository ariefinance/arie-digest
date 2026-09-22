# inoreader/SOURCES.md — Apply-ready source set (Gate 2A)

Operator configuration for Inoreader. **Gate 2A = design only.** Each `RSS ~` /
page-watch entry is verified live at **Gate 2B** (needs the account). Feed-status
legend: `RSS ✅` confirmed · `RSS ~` likely, verify URL · `RSS ✖` no feed → use a
monitoring query and/or Web-feed page-watch.

Source detail and provenance: `context/a2-sources.md`. Noise handling (encoded in
the report prompt, not here): `context/a2-noise-lessons.md`.

---

## Tier 1 — Authoritative (regulators / primary issuers)

### Mauritius → folder `01-Mauritius`
| Source | URL | Feed | Setup |
|---|---|---|---|
| Bank of Mauritius | https://www.bom.mu/latest-news.xml | RSS ✅ | Subscribe feed. Demote routine ops in prompt (see noise). |
| FSC Mauritius (ARIE's regulator) | https://www.fscmauritius.org/en/media-corner/communiques | RSS ✖ | **Web-feed page-watch** on communiqués + press-releases + `others/news`, **plus** monitoring query MQ-MU-FSC. Highest miss-risk. |
| Ministry of Finance | https://mof.govmu.org/ | RSS ✖ | Monitoring query MQ-MU-MOF (+ page-watch if selectable). |
| Economic Development Board (EDB) | https://edbmauritius.org/newsroom | RSS ✖ | Web-feed page-watch on newsroom + MQ-MU-EDB. |
| FIU Mauritius | https://www.fiumauritius.org/ | RSS ✖ | Monitoring query MQ-MU-FIU. |

### International regulators / standard-setters → folder `02-Global-Finance`
| Source | URL | Feed |
|---|---|---|
| ECB press + publications | https://www.ecb.europa.eu/rss/press.html · /rss/pub.html | RSS ✅ |
| OFAC recent actions | https://ofac.treasury.gov/recent-actions | RSS ✅ |
| OFSI (UK) | https://ofsi.blog.gov.uk/feed/ | RSS ✅ |
| FATF | https://www.fatf-gafi.org/en/publications.html | RSS ~ (GNews-proxy backup w/ browser UA) |
| FSB | https://www.fsb.org/ | RSS ~ |
| BIS + CPMI | https://www.bis.org/ | RSS ~ (topic feeds incl. payments/CPMI) |
| FCA (UK) | https://www.fca.org.uk/news | RSS ~ |
| EBA | https://www.eba.europa.eu/ | RSS ~ |
| EU AMLA | https://www.amla.europa.eu/news-media/news-articles_en | RSS ~ (else query) |
| SWIFT newsroom | https://www.swift.com/news-events | RSS ✖ → MQ-WW-RAILS |

## Tier 2 — Specialist trade press
### Payments → `02-Global-Finance`
| Source | URL | Feed |
|---|---|---|
| Finextra | https://www.finextra.com/rss/headlines.aspx | RSS ✅ |
| PYMNTS cross-border | https://www.pymnts.com/category/cross-border-commerce/feed/ | RSS ✅ |
| FinTech Global | https://fintech.global/feed/ | RSS ✅ |
| The Paypers | https://thepaypers.com/news | RSS ✖ → query |
| The Banker / Global Finance / Central Banking | resp. sites | RSS ~ (correspondent banking / de-risking) |

### Film & TV → folder `03-Film`
| Source | URL | Feed |
|---|---|---|
| Screen Daily | https://www.screendaily.com/ | RSS ~ (GNews proxy backup) |
| Variety / THR (business & finance only) | resp. sites | RSS ~ (filter hard: reject casting/box-office) |
| KFTV / incentive trackers | https://www.kftv.com/ | RSS ✖ → query |
| Mauritius film incentive (EDB) | edbmauritius.org/newsroom | RSS ✖ → MQ-FILM-MU |

### Oil & Gas → folder `04-Oil-Gas`
| Source | URL | Feed |
|---|---|---|
| Reuters Energy/Commodities | https://www.reuters.com/business/energy/ | RSS ~ |
| S&P Global Commodity Insights (Platts) | https://www.spglobal.com/commodityinsights/ | RSS ~ → query |
| Global Trade Review (trade finance) | https://www.gtreview.com/ | RSS ~ |
| TradeWinds / Lloyd's List (shipping/sanctions) | resp. sites | RSS ~ / paywalled → headline query |
| OFAC/OFSI energy designations | (reuse Tier-1 sanctions feeds) | RSS ✅ |

## Competitor watch (short curated list) → tag `competitor`
Wise, Revolut, Airwallex, Payoneer, Nium, Rapyd, Stripe, Adyen; Mauritius banks
AfrAsia, Bank One, Absa Mauritius. (Deliberately short — see D-notes; not the
legacy 40-entry list.) Applied via monitoring query MQ-WW-COMPET, mapped to Worldwide.

---

## Gate 2B verification checklist (per source)
- [ ] Feed URL resolves and returns items (browser UA where needed).
- [ ] FSC/EDB/MoF/FIU: Web-feed selection captures the right page region; page-watch fires on change.
- [ ] Known-FSC-communiqué capture test passes.
- [ ] No source silently returns 0 items from datacenter IPs.
