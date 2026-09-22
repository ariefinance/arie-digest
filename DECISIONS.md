# DECISIONS.md — ARIE Digest locked decisions

Authoritative drift-control record. Sub-agents may **challenge** a decision only
via a `PROPOSED ARCHITECTURE DEVIATION` (state: current decision · new evidence ·
why current fails · proposed replacement · added complexity/cost · user approval
needed?). They may **not** silently override.

Format per entry: Decision · Reason · Date · Status · Reopen only if.

---

### D1 — No custom production code by default
- **Decision:** No backend, database, API, web app, dashboard, crawler, custom
  scheduler, or custom AI service unless a component proves Inoreader + M365
  cannot solve a demonstrated requirement simply and reliably.
- **Reason:** Prior project failed through over-engineering; goal is a briefing, not an app.
- **Date:** 2026-09-22 · **Status:** LOCKED
- **Reopen only if:** Gate 0 proves a specific requirement is unmet without code.

### D2 — Single AI intelligence report
- **Decision:** One AI pass: reject → dedupe/group → classify → summarise →
  explain ARIE relevance → prioritise. No multi-agent voting, no second-model
  verification, no confidence scoring, no complex ranking formulas.
- **Reason:** Simplicity and reliability over sophistication.
- **Date:** 2026-09-22 · **Status:** LOCKED
- **Reopen only if:** QA (Gate 3) shows the single pass cannot meet quality targets.

### D3 — Four visible content sections + "What Matters Today"
- **Decision:** What Matters Today (≤3) · Mauritius · Worldwide · Film & TV ·
  Oil & Gas. Must-Catch topics map into these; no new management tabs.
- **Date:** 2026-09-22 · **Status:** LOCKED · **Reopen only if:** user changes scope.

### D4 — Strict Commercial Signal (annotation only)
- **Decision:** Annotation requiring all 3 conditions (named entity + observable
  trigger + evidenced ARIE payments angle). No prospect engine, scores, contacts,
  or outreach.
- **Date:** 2026-09-22 · **Status:** LOCKED · **Reopen only if:** user changes scope.

### D5 — Urgent alerts deferred from V1
- **Decision:** No real-time/urgent alerts in V1. Reconsider only after the daily
  Digest proves reliable.
- **Date:** 2026-09-22 · **Status:** LOCKED · **Reopen only if:** post-pilot decision.

### D6 — Email is the sole management delivery surface (v1); Teams removed
- **Decision:** **Email is the ONLY management delivery surface for ARIE Digest
  v1.** The automated Inoreader Intelligence report is emailed to management
  recipients. **Microsoft Teams is removed entirely from v1** — architecture,
  delivery tests, acceptance gates, and future execution tasks. There is no
  challenger comparison.
- **Do NOT research, configure, or test:** Inoreader→Teams integration; Teams
  channels; Teams webhooks; Teams Workflows; Power Automate (or Make/Zapier/custom
  connectors) for Digest delivery.
- **Production pipeline:** `Curated Sources → Inoreader → One Automated
  Intelligence Report → ARIE Digest → Management Email`.
- **Gate 4 = Email Delivery Proof** (tests only: automated report email delivery;
  multiple management recipients; formatting/readability; links; scheduling;
  reliability; whether recipient acceptance/activation is required).
- **Reason:** Direct user decision. Removes an entire branch of complexity,
  reducing setup effort and ongoing failure points.
- **Date:** 2026-09-22 (locked by user; supersedes prior Teams-preferred framing)
  · **Status:** LOCKED · **Reopen only if:** the user explicitly requests Teams later.

### D7 — Legacy repo: harvest then archive
- **Decision:** `ariefinance/arie-intelligence-command-centre` (public) is a
  reference only. Harvest lessons (validated sources, exclusions, false positives,
  QA fixtures, useful prompt concepts). Do not refactor, migrate, or recreate it.
- **Date:** 2026-09-22 · **Status:** LOCKED · **Reopen only if:** never (retire target).

### D8 — Update cadence
- **Decision:** Weekdays 08:15 Mauritius time (UTC+4). Monday includes weekend.
- **Date:** 2026-09-22 · **Status:** LOCKED · **Reopen only if:** platform constraint (Gate 0/2).

### D9 — Tools out of scope by default
- **Decision:** No Make.com, Power Automate, Railway, Vercel, Supabase, SharePoint
  Digest site, Microsoft Lists, custom website/backend/DB/scraper, Activepieces,
  changedetection.io, ArchiveBox, Cowork/ChatGPT Work in production, or separate
  Claude/OpenAI API, unless Gate 0 proves a genuine blocker. Cowork/ChatGPT Work
  allowed for ad-hoc deeper research only, not routine production.
- **Date:** 2026-09-22 · **Status:** LOCKED · **Reopen only if:** proven blocker.

### D10 — Preferred production platform: Inoreader Pro + Intelligence capability
- **Decision:** The preferred v1 platform is **Inoreader Pro + the Intelligence
  capability/add-on** (collection + one automated Intelligence report + native
  email delivery), subject to **live purchase/setup verification** (Gate 2B).
  **Team Intelligence only if Pro cannot satisfy a demonstrated requirement.**
- **Reason:** Email-only architecture (D6) removes any need for a Team plan; Pro
  supports monitoring feeds, web-feeds/page-watch, custom-prompt automated reports,
  and multi-recipient email.
- **Date:** 2026-09-22 (updated post-Gate-0; supersedes the earlier Team
  Intelligence assumption) · **Status:** LOCKED (plan target)
- **Reopen only if:** Gate 2B/live verification shows Pro cannot meet a
  demonstrated requirement (then evaluate Team Intelligence).
