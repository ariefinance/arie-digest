# inoreader/folders.md — Backend organisation (Gate 2A)

Internal Inoreader structure. **Management never sees this** — it only shapes what
the single Automated Intelligence Report ingests. Keep minimal (D1: no unnecessary
structure).

## Folders

```text
00-Digest-Input   → THE single source the automated report reads. Every eligible
                    feed / monitoring feed from 01–05 is also added here
                    (Inoreader feeds can belong to multiple folders).
01-Mauritius      → feeds/queries for the Mauritius section
02-Global-Finance → international regulators + payments trade press (Worldwide)
03-Film           → Film & TV sources
04-Oil-Gas        → Oil & Gas sources
05-Must-Catch     → cross-cutting queries (sanctions, AML/CFT, correspondent
                    banking, de-risking, rails, outages) + commercial-signal
                    queries + direct ARIE-mention watch
90-Discovery      → broad Tier-3 discovery queries (NOT in the report input)
```

01–05 remain the human-facing organisation for curation/maintenance. **`00-Digest-Input`
exists solely because an Inoreader automated report selects ONE source (a single
feed, folder, tag, or channel) — there is no parent-folder/multi-folder scope.** So
every feed and monitoring feed that should reach the report is added to its section
folder (01–05) **and** to `00-Digest-Input`; the report points at `00-Digest-Input`.
**Must-Catch is a folder, not a management tab** — its items map into the four
visible sections at summarisation time. `90-Discovery` is deliberately excluded from
`00-Digest-Input` (promote a query into the input only if it proves its worth).

## Tags (cross-folder labels the prompt can use)
- `competitor` — competitor-watch hits (map to Worldwide).
- `commercial-signal-candidate` — optional; items that may meet the D4 test (the
  report still applies the strict 3-condition guardrail; the tag is only a hint).
- `mauritius-tier1` — FSC/BoM/MoF/EDB/FIU items (elevate for "zero regulator misses").
- `arie-direct-mention` — direct "ARIE Finance" / "Arie Capital Investment" mentions (MQ-MC-ARIE).

## Report ingestion scope
The Automated Intelligence Report reads the single folder **`00-Digest-Input`**.
One combined report is the default (D2, token economy); split into per-section
reports (each pointed at a section folder) only if Gate 3 quality requires it.

## Native duplicate filter (before AI — reduces token waste & syndication)
Apply Inoreader's built-in **duplicate filter** on `00-Digest-Input`: drop items
with the **same URL** or **same/near-identical title**, using a **multi-day
lookback** (~3 days) to catch cross-day syndication. This is a *safe* filter — no
aggressive content deletion; it only suppresses obvious duplicates so fewer tokens
are spent. Semantic **event-identity dedupe for rewritten headlines stays in the AI
prompt** (C2) — the two layers are complementary. Exact filter options/lookback
confirmed at Gate 2B.

## Rules (Inoreader "Rules", ≤ plan limit)
- Add each eligible feed/query to its section folder (01–05) **and** to `00-Digest-Input`.
- Apply `competitor` / `mauritius-tier1` / `commercial-signal-candidate` / `arie-direct-mention` tags by matching the curated entity lists.
- No webhook/Teams rules (D6 — email only). No rule that deletes items (the report
  does rejection, not the collection layer — keep hygiene reversible; the duplicate
  filter suppresses, it does not delete).
