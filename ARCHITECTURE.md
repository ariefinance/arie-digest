# ARCHITECTURE.md — ARIE Digest (v1)

Authoritative architecture. See `DECISIONS.md` for the locked decisions behind it
and `ARCHITECTURE_REVIEW.md` for the Gate 0 evidence.

## Production pipeline

```text
Curated Sources
      ↓
Inoreader                (collection · dedupe hygiene · folders/tags · monitoring feeds · web-feeds/page-watch)
      ↓
One Automated Intelligence Report   (single AI pass: reject → dedupe/group → classify → summarise → ARIE relevance → prioritise)
      ↓
ARIE Digest              (the formatted management briefing)
      ↓
Management Email         (automated Inoreader email to management recipients)
```

**Delivery is email only (D6, locked).** Microsoft Teams is out of scope for v1.

## Components

| Layer | Platform | Responsibility |
|-------|----------|----------------|
| Sources | Curated Tier 1/2/3 (see `context/a2-sources.md`) | Feeds, monitoring queries, web-feed page-watch for feedless Mauritius Tier-1 bodies |
| Collection & hygiene | **Inoreader** (Pro + Intelligence add-on) | Subscribe/organise (folders/tags per section), monitoring feeds, basic dedupe, scheduling |
| Intelligence | **Inoreader Automated Intelligence Report** | One scheduled report, custom ARIE prompt, folder/tag-scoped inputs, article cap |
| Digest | Report output | The management-facing briefing content |
| Delivery | **Inoreader automated email** | Weekday email to management recipients (recipients accept once) |

## Content model (see `context/agent-brief.md`)

Sections: **What Matters Today** (≤3) · **Mauritius** · **Worldwide** ·
**Film & TV** · **Oil & Gas**. Must-Catch topics map into these sections (no new
tabs). Item format: Headline · Summary (1–2 sentences) · Why it matters to ARIE
(1 sentence) · Source · date · link · optional `COMMERCIAL SIGNAL`.

## Two enforced design conditions (from Gate 0)

- **C1 — Zero-stale is prompt-enforced.** Inoreader does no date verification and
  web-feed change-tracking can resurface old pages; the AI pass rejects any item
  whose underlying event date is not evidenced in-item.
- **C2 — Dedupe on event identity** (named body + specific event word + date
  proximity), never on a generic regulator name alone.

## What is deliberately NOT built (D1/D9)

No backend, database, API, web app, dashboard, crawler, custom scheduler, or
custom AI service. No Make.com / Power Automate / Zapier / Railway / Vercel /
Supabase / SharePoint / Microsoft Lists / custom connectors. **No Microsoft Teams
path** (D6). No separate Claude/OpenAI API in production (Inoreader bundled tokens;
BYOAI not used by default).

## Scheduling

Weekdays **08:15 Mauritius time (UTC+4)**; Monday includes weekend developments
(D8). Exact scheduler precision confirmed at Gate 2; email-digest scheduler is the
confirmed weekday+time mechanism.

## Governance

Repo temporarily public → everything committed must be safe for permanent public
exposure. No secrets, private feed URLs, management emails, tenant info, or client
data — placeholders only.
