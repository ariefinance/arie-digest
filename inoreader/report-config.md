# inoreader/report-config.md — Automated Intelligence Report input design (Gate 2A)

Design of the single Automated Intelligence Report's inputs and settings. **Values
here are the intended configuration to apply at Gate 2B** — nothing is verified
against a live account yet (article caps, token headroom, scheduler precision are
UNCONFIRMED until Gate 2B).

## One report, single-source input (D2)
- **Ingestion source:** the single folder **`00-Digest-Input`**. An Inoreader
  automated report selects ONE source (feed, folder, tag, or channel) — there is no
  parent/multi-folder scope — so every eligible feed and monitoring feed from 01–05
  is also added to `00-Digest-Input` (see `folders.md`). Discovery (`90`) is not in it.
- **Pre-AI native duplicate filter:** a conservative duplicate filter runs on
  `00-Digest-Input` before the report (same URL / same-or-near-identical title,
  multi-day lookback ~3 days, up to Inoreader's max ~1 month) to cut token waste and
  cross-day syndication. Semantic event-identity dedupe for rewritten headlines stays
  in the AI prompt (C2). Filter is *safe* (suppress, not delete).
- **Rationale:** one combined pass is simplest and most token-efficient (D2). Split
  into per-section reports (each pointed at a section folder) only if Gate 3 demands it.

## Schedule (intended — confirm mechanism at Gate 2B)
- **Weekdays, 08:15 Mauritius time (UTC+4)** (D8); Monday's window includes the
  weekend. Preferred: the automated-report scheduler pins weekday + time. **Fallback
  if it can't:** run the report daily and deliver via the email-digest scheduler
  (confirmed to support weekday + time), or accept weekend suppression via source
  emptiness. Decide at Gate 2B against live capability.

## Input volume
- **Article cap per run:** start at **~30–40** most-recent items (Inoreader
  recommends smaller focused sets for more reliable reports, and this matches the
  ≤10-story output). Increase only if Gate 3/2B shows material misses. Exact max and
  any output-length cap are UNCONFIRMED — tune at Gate 2B against the token quota
  (Pro ~1M tokens/month; ~22 weekday runs/month).
- **Time window:** last ~24h weekdays; ~72h for Monday. (Event-date discipline C1
  still governs freshness regardless of window.)

## Prompt responsibilities (the report's single AI pass — full prompt at Gate 3)
The custom prompt must implement, in order:
1. **Reject** — apply noise-filters.md A–E, the relevance test (D), and C1 event-date rejection.
2. **Dedupe/group** — C2 event-identity clustering.
3. **Classify** — into the four visible sections (Must-Catch items map in, not a new tab).
4. **Summarise** — 1–2 factual sentences per item.
5. **Explain ARIE relevance** — one specific sentence.
6. **Prioritise** — pick "What Matters Today" (≤3); order sections; respect volume
   (5–10 normal, 2–4 quiet, 0 acceptable, ~12 max) and film ≤25%.
7. **Commercial Signal** — annotate only when all 3 D4 conditions hold; no scores/contacts/outreach.
8. **Grounding** — noise-filters.md G (literal entities; empty if ungroundable).

## Output shape (management-facing)
Sections: What Matters Today · Mauritius · Worldwide · Film & TV · Oil & Gas.
Per item: Headline · **Summary** (1–2 sentences) · **Why it matters to ARIE** (1
sentence) · Source · date · link · optional `COMMERCIAL SIGNAL — [reason]`. Nothing
else. Target read time ≤5 min. Full format spec + prompt text: Gate 3 (`digest/`).

## Delivery (D6)
Email only, to management recipients (list kept OUT of this public repo). Recipients
may need to accept a subscription once — confirm at Gate 2B / Gate 4.

## Gate 2B verification hooks (not yet done)
- [ ] Scheduler pins Mon–Fri 08:15 UTC+4 (or fallback chosen).
- [ ] Actual max-article and output-length caps recorded; token headroom checked.
- [ ] Combined-scope report runs and respects the prompt ordering.
