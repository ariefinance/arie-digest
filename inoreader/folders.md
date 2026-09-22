# inoreader/folders.md — Backend organisation (Gate 2A)

Internal Inoreader structure. **Management never sees this** — it only shapes what
the single Automated Intelligence Report ingests. Keep minimal (D1: no unnecessary
structure).

## Folders (one per visible section + Must-Catch + Discovery)

```text
01-Mauritius      → feeds/queries for the Mauritius section
02-Global-Finance → international regulators + payments trade press (Worldwide)
03-Film           → Film & TV sources
04-Oil-Gas        → Oil & Gas sources
05-Must-Catch     → cross-cutting monitoring queries (sanctions, AML/CFT,
                    correspondent banking, de-risking, rails, outages, etc.)
90-Discovery      → broad Tier-3 discovery queries (must not dominate output)
```

This mirrors the mandate's proposed scheme. Challenged and kept: it is the minimum
that lets the report be scoped cleanly. **Must-Catch is a folder, not a management
tab** — its items map into the four visible sections at summarisation time.

## Tags (cross-folder labels the prompt can use)
- `competitor` — competitor-watch hits (map to Worldwide).
- `commercial-signal-candidate` — optional; items that may meet the D4 test (the
  report still applies the strict 3-condition guardrail; the tag is only a hint).
- `mauritius-tier1` — FSC/BoM/MoF/EDB/FIU items (elevate for "zero regulator misses").

## Report ingestion scope
The Automated Intelligence Report reads a **parent scope covering 01–05** (not 90
by default — Discovery is pulled in only when a query there is promoted). One
combined report over 01–05 is the default (D2, token economy); split per-folder
only if Gate 3 quality requires it.

## Rules (Inoreader "Rules", ≤ plan limit)
- Route each feed/query into its folder on ingest (where not already foldered).
- Apply `competitor` / `mauritius-tier1` tags by matching the curated entity lists.
- No webhook/Teams rules (D6 — email only). No rule that deletes items (the report
  does rejection, not the collection layer — keep hygiene reversible).
