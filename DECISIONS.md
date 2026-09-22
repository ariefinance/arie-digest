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

### D6 — Delivery surface: Teams preferred, Email fallback/control (proven at Gate 4)
- **Decision:** **Microsoft Teams is the preferred delivery surface**, because all
  intended ARIE management/team users already have and use Teams internally, so it
  introduces **no new management application or user behaviour**. **Email is the
  fallback/control path.** Teams is approved ONLY if the **native Inoreader
  Team-channel → Microsoft Teams** integration (Automated Intelligence Report →
  Team channel → Teams) works cleanly, requires **no additional orchestration
  platform**, and any **Microsoft admin requirement is acceptable**. If any of
  those fail at Gate 4, fall back to Email. Not pre-locked — Gate 4 proves it.
- **Constraints:** (a) Do **not** introduce Power Automate / Make / Zapier /
  custom connectors merely to force Teams. (b) Existing Teams *access* does **not**
  imply ARIE holds the required Inoreader **Team / Team Intelligence** subscription
  (Teams delivery is a Team-plan feature) — validate the Inoreader plan requirement
  and cost **separately** (payment gate).
- **Reason:** Deliver where management already works; avoid orchestration creep;
  keep the plan-cost dependency explicit.
- **Date:** 2026-09-22 (amended per user business context) · **Status:** LOCKED
  (process) · **Reopen only if:** Gate 4 evidence.

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

### D10 — Preferred production platform: Inoreader (pending Gate 0 validation)
- **Decision:** Inoreader Team Intelligence is the assumed collection + AI-report
  + distribution platform. Assumption MUST be validated against current official
  documentation in Gate 0 before any external configuration.
- **Date:** 2026-09-22 · **Status:** PROVISIONAL (confirm at Gate 0)
- **Reopen only if:** Gate 0 evidence shows it cannot meet requirements simply.
