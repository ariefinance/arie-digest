# ARIE Digest

A short, accurate, high-signal daily briefing of the external developments most
worth knowing for ARIE Finance management — delivered automatically, consumable
in **≤5 minutes**, with almost no routine human intervention.

ARIE Digest is a **management intelligence briefing, not an application.** It
answers one question: *what does ARIE management need to know today?*

## How it works

```text
Curated Sources → Inoreader → One Automated Intelligence Report → ARIE Digest → Management Email
```

- **Sources:** curated Tier 1/2/3 (regulators, specialist trade press, discovery
  queries) — see `context/a2-sources.md`.
- **Inoreader** collects, organises (folders/tags per section), de-duplicates,
  monitors search queries, and watches feedless pages.
- **One Automated Intelligence Report** runs a single AI pass
  (reject → dedupe/group → classify → summarise → ARIE relevance → prioritise).
- **Delivery: email only** to management recipients (Microsoft Teams is out of
  scope for v1 — see `DECISIONS.md` D6).

## Content

**What Matters Today** (≤3) · **Mauritius** · **Worldwide** · **Film & TV** ·
**Oil & Gas**. Each item: headline · 1–2 sentence summary · one "why it matters to
ARIE" sentence · source · date · link · optional `COMMERCIAL SIGNAL`. Target
volume: 5–10/day normal, 2–4 quiet, 0 acceptable, ~12 max busy. Never fill a quota.

## Principles

Simple, reliable, autonomous, useful. **No custom production code by default** — no
backend, database, dashboard, scraper, or orchestration platform. A component is
built only if Inoreader + Microsoft 365 cannot solve a demonstrated requirement
simply and reliably.

## Repository map

| File | Purpose |
|------|---------|
| `DECISIONS.md` | Locked decisions (drift control) |
| `ARCHITECTURE.md` | Production architecture |
| `EXECUTION_PLAN.md` | Gate-by-gate execution plan |
| `STATUS.md` | Live progress |
| `SECURITY.md` | Public-exposure & secrets rules |
| `ARCHITECTURE_REVIEW.md` | Gate 0 independent review |
| `context/` | Shared agent brief + Gate 0 research evidence |
| `qa/` | QA fixtures + results (Gate 3) |

## Status

Execution is gated (see `STATUS.md`). This repository is **temporarily public** for
independent review during execution and will be made private at production lock
(Gate 6). Everything committed is safe for permanent public exposure — no secrets,
private feed URLs, management emails, tenant info, or client data.
