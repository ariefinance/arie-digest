# A1 — Inoreader Platform Findings (Gate 0)

> **Superseded note (2026-09-22):** this is the original Gate 0 evidence record,
> kept intact. Delivery is now **email only** (see `DECISIONS.md` D6 — Teams
> removed from v1). Any Teams discussion below (§2, §4) is historical context, not
> a live recommendation; do not act on it.

Access date for all items: **2026-09-22**.

> **Evidence caveat (read first).** The egress proxy blocks `www.inoreader.com`,
> `help.inoreader.com`, `alternativeto.net`, `en.wikipedia.org` and the review
> aggregators (org policy 403 — not routed around). I could **not open any
> official Inoreader page first-hand.** Every official detail below comes from
> WebSearch result snippets that *quote* those official pages (blog + pricing),
> not a fetched document. Confidence is capped accordingly; anything numeric
> (prices, quotas) should be re-verified against the live page before money is
> spent. No account was created, logged into, or modified.

---

## 1. Plan & cost (required tier for AI / Intelligence / monitoring / rules / team)

**Finding.** Inoreader tiers: Free · **Pro** · Custom · **Team** · **Team
Intelligence** · Enterprise. AI features (Inoreader Intelligence: summaries,
custom prompts, Intelligence reports) require **Pro or above**. Monitoring feeds,
rules, and web feeds are **Pro** features. A distinct multi-user tier exists:
**Team** (collaboration, shared channels) and **Team Intelligence** (Team + the
AI/Intelligence quota bundled). Automated *Intelligence reports* are stated as an
**add-on for Pro and Custom**, and **included in Team Intelligence**.
- **Pro:** ~US$7.50/mo billed annually (~US$90/yr) or US$9.99/mo billed monthly;
  includes 1,000,000 Intelligence tokens/month.
- **Team / Team Intelligence:** priced by team-size bracket, not per-seat. Quoted
  brackets: 3 members US$44.99 (Team) / US$64.99 (Team Intelligence); 5 members
  ~US$74.99 / US$99.99; 10 members US$119.99 / US$169.99; 20 members US$224.99 /
  US$319.99; 50 members US$374.99 / US$629.99 per month. Team Intelligence bundles
  **6,000,000 tokens per member/month**.
- **BYOAI (Apr 2026):** you may connect your own OpenAI / Mistral / Anthropic API
  key and pick the model, bypassing the bundled token quota.

**Evidence.** inoreader.com/blog/2025/04/new-intelligence-reports-and-team-intelligence-plan.html;
inoreader.com/pricing; inoreader.com/pricing/enterprise;
inoreader.com/blog/2026/03/automated-intelligence-reports-for-insights-delivered-to-you.html
(all via WebSearch snippet, 2026-09-22).
**Confidence: PARTIAL** (currency assumed USD; exact figures snippet-sourced, page not openable).
**Recommendation.** For a single management briefing, **Pro + Intelligence
report add-on** is the minimum viable tier; a multi-recipient / shared-channel
setup points to **Team Intelligence**. Confirm live prices + currency before purchase.

## 2. Automated, scheduled AI Intelligence reports

**Finding.** Yes — native feature under **Automate > Automated reports**.
Reports run **on a schedule you set (daily or weekly)**, run a predefined or
custom prompt over a chosen source, and are **delivered automatically** (email;
see §4). You choose "how often the report should run and when it should be
delivered." A preview is available before saving. The effective usage cap is the
**monthly token quota** (1M Pro / 6M per member Team Intelligence); exhausting it
blocks new reports until reset, and extra tokens are purchasable.
- **Fixed daily weekday time (08:15 UTC+4):** cadence is daily/weekly and a
  delivery time is selectable. The *redesigned email digest* (Aug 2025)
  explicitly supports "start date, time, and a custom schedule across multiple
  days of the week" — i.e. Mon–Fri is achievable on the digest path. Whether the
  **automated Intelligence report** scheduler itself exposes weekday-only +
  minute-precision + timezone (UTC+4) is **not confirmed** in official snippets.
- **Article-count / report-length limits:** you select a max article count per
  report ("a smaller focused set produces more accurate insights"), but the
  **numeric max articles and any max output length are UNCONFIRMED.**

**Evidence.** inoreader.com/blog/2026/03/automated-intelligence-reports-for-insights-delivered-to-you.html;
inoreader.com/blog/2025/08/redesigned-email-digests-for-more-control-and-flexibility.html;
inoreader.com/blog/2026/01/get-insights-with-inoreader-intelligence.html (WebSearch snippets, 2026-09-22).
**Confidence: CONFIRMED** (scheduled recurring reports exist) / **PARTIAL** (exact
time granularity, weekday-only, per-report article & length limits).
**Limitation.** Validate at Gate 2 that the report scheduler can pin Mon–Fri 08:15
UTC+4; if not, fall back to the email-digest scheduler (weekday+time confirmed)
fed by the report output, or run daily and accept weekend suppression via source emptiness.

## 3. Custom prompt / input control

**Finding.** Yes — reports accept a **user-defined custom prompt** (or a
predefined one). Inputs are scoped by **source selection: a single feed, a
folder, a tag, or a Team channel**, plus a selectable **max article count**.
Date-window scoping is implied by "new/recent articles in the source" but an
explicit date-range control is **not confirmed**. Prompt-length limits are
**UNCONFIRMED**.

**Evidence.** inoreader.com/blog/2026/03/automated-intelligence-reports-for-insights-delivered-to-you.html;
inoreader.com/blog/2025/04/new-intelligence-reports-and-team-intelligence-plan.html (snippets, 2026-09-22).
**Confidence: CONFIRMED** (custom prompt + folder/tag scoping + article cap) /
**UNCONFIRMED** (explicit date window, prompt max length).
**Recommendation.** The single-pass ARIE prompt (reject→dedupe→classify→
summarise→relevance→prioritise, per D2) fits the custom-prompt + folder/tag-scoped
model. Structure sources as folders/tags per section (Mauritius / Worldwide /
Film&TV / Oil&Gas) so one report can be scoped or several run per section.

## 4. Delivery options actually supported (decisive for D6)

**Finding.**
- **Email digest — NATIVE, first-party.** Automated email summaries on a chosen
  schedule/time to chosen **recipients** (recipients get a subscription request
  they must **accept** once). Automated Intelligence reports are "delivered to
  you" via this email path. This is the only fully first-party automated
  delivery surface.
- **Webhooks — NATIVE (Pro), via Rules.** Rule action "Trigger webhook" posts to
  any URL. This is the native hook for pushing elsewhere, but it fires on
  **rule/article** events, not as the packaged Intelligence-report deliverable.
- **Microsoft Teams — NO first-party path.** There is **no native
  Inoreader→Teams integration** for the AI report. Teams delivery requires an
  **external connector**: the webhook rule action into a Teams incoming webhook,
  or a third-party automation (Make, Zapier, Power Automate, Integrately, Albato,
  Pabbly). Any of these adds an orchestration dependency and typically Teams
  admin action to provision the connector/flow.
- **Slack / RSS-of-report / IFTTT:** Slack only via the same third-party
  automations (no native). No evidence of a native "RSS feed of the report
  output." In-app viewing of reports is native.

**Evidence.** inoreader.com/blog/2019/03/introducing-a-new-rule-action-webhooks.html;
inoreader.com/blog/2025/08/redesigned-email-digests-for-more-control-and-flexibility.html;
inoreader.com/blog/2020/02/send-daily-email-digests-to-friends-colleagues-or-even-to-yourself.html;
make.com/en/integrations/inoreader/microsoft-teams; integrately.com/integrations/inoreader/microsoft-teams (snippets, 2026-09-22).
**Confidence: CONFIRMED.**
**Recommendation → D6.** Evidence favours **Email as the native baseline**:
zero extra orchestration, first-party, weekday+time scheduling confirmed. **Teams
cannot win D6 on "equal or lower operational complexity"** because every Teams
path requires an external connector and likely admin action — contrary to D6/D9's
no-orchestration-creep constraint. Flag: email-digest recipients must *accept* a
subscription (one-time operational step).

## 5. Monitoring feeds / discovery

**Finding.** **Monitoring feeds** turn a saved search into a followable feed
(Search → build query → "+" → name → create). Supports **Boolean AND/OR/NOT,
parentheses, quoted phrases, wildcards** (e.g. `(coronavirus OR covid) AND "new
case*"`), language filters, and global search across millions of articles beyond
subscribed feeds. **Pro allows 30 monitoring feeds.** Refresh: Pro feeds update
**at least hourly** by default; **Boosted feeds every 10 minutes.**

**Evidence.** inoreader.com/blog/2024/01/stay-in-the-know-with-monitoring-feeds.html;
inoreader.com/blog/2023/04/making-complex-searches-easy-with-our-new-query-builder.html;
inoreader.com/blog/2026/01/discover-and-monitor-content.html (snippets, 2026-09-22).
**Confidence: CONFIRMED** (capability, syntax, 30-cap, refresh) — exact 2026
per-tier monitoring-feed counts PARTIAL.
**Recommendation.** 30 monitoring feeds is ample for the five ARIE sections; use
Boolean queries per Must-Catch topic.

## 6. Source limits (Pro)

**Finding.** Pro: **2,500 feed subscriptions**, **30 monitoring feeds**, **30
rules**, **50 filters** (filter quotas pooled to 50 as of Apr 2026). **Folder
count limit and active-search count limit: UNCONFIRMED** (not in official
snippets). Team tiers scale limits per plan/member.

**Evidence.** inoreader.com/pricing; inoreader.com/blog/2019/02/increased-limits-on-rules-in-the-new-pro-plan.html;
readless.app/blog/inoreader-pricing-2026 (snippets, 2026-09-22).
**Confidence: PARTIAL** (core numbers corroborated; folders/active-searches unconfirmed).
**Recommendation.** Limits comfortably exceed ARIE's needs; confirm folder cap
at Gate 2 if a folder-per-section structure is chosen.

## 7. Mauritius Tier-1 feasibility (sources lacking RSS)

**Finding.** Inoreader natively supports **Web feeds** — "convert almost any
webpage into an RSS feed" and monitor whole pages or page elements **without
RSS**, incl. **visual and textual change tracking** (screenshot-diff + text
diff). Setup: Add feed → Web feed → paste URL → pick suggested selection →
follow. This is the native mechanism for FSC Mauritius, Bank of Mauritius,
Ministry of Finance, EDB if they lack RSS. Native fallback if a page resists
selection: a **monitoring feed on a site-scoped or keyword search query**.

**Evidence.** inoreader.com/blog/2020/04/convert-almost-any-webpage-into-rss-feed-with-inoreaders-web-feeds.html;
inoreader.com/blog/2025/02/improved-web-feeds-track-changes.html;
inoreader.com/blog/2021/04/monitor-web-pages-for-changes-with-web-feeds.html (snippets, 2026-09-22).
**Confidence: CONFIRMED** (native capability exists). **Per-regulator
reliability UNCONFIRMED** — not tested (constraint honoured; no regulator feeds
visited/created).
**Recommendation.** Native capability satisfies the requirement in principle;
prove each Tier-1 regulator page individually at Gate 2 (some gov pages defeat
selector/JS extraction — web feed then falls back to change-tracking or a search-based monitoring feed).

## 8. Zero-stale feasibility (underlying-event-date governs)

**Finding.** Inoreader Intelligence operates on the **article text/content it has
ingested** (full text where the feed provides it, else the snippet). It
summarises/analyses that in-item text and can answer questions about it, but
there is **no evidence it independently fetches or verifies the primary source to
establish the true event date.** Feed/ingestion/index dates are what the platform
knows — exactly the dates the brief says do NOT prove event date. Inoreader also
performs **no editorial staleness rejection** ("feeds and articles remain
untouched, no AI filtering").

**Verdict.** Inoreader **alone cannot guarantee** "underlying-event-date governs,
zero stale-as-new." The requirement must be enforced at the **prompt level**: the
single AI pass must **reject any item whose event date is not evidenced within
the item text**, treat feed/syndication dates as non-probative, and collapse
same-event duplicates. Web-feed change-tracking further risks surfacing
re-published/edited old pages as "new," reinforcing the need for prompt-level
event-date gating rather than trusting ingestion recency.
**Confidence: CONFIRMED** (reasoned from how Intelligence + ingestion work).
**Recommendation.** Bake an explicit event-date-evidence rejection rule into the
D2 prompt; do not rely on Inoreader recency signals for freshness.

---

## Blockers & risks to the no-code architecture

- **RISK (not a blocker) — No first-party Teams delivery.** Any Teams surface
  needs an external connector/admin action; this constrains D6 toward Email and
  runs against D9's no-orchestration rule.
- **RISK — Zero-stale is not platform-native.** Must be solved in the prompt
  (event-date-evidence rejection). Architecturally fine, but a hard prompt requirement.
- **RISK — Schedule precision unproven.** Weekday-only + 08:15 + UTC+4 on the
  *automated report* scheduler is unconfirmed; email-digest scheduler is the
  confirmed fallback for weekday+time. Verify at Gate 2 (bears on D8).
- **RISK — Unconfirmed limits.** Max articles/length per report, prompt-length,
  folder/active-search caps, and exact current prices/currency are snippet-only.
- **EVIDENCE GAP (process) — Official pages not directly fetchable** from this
  environment (egress policy). All official detail is snippet-quoted, not
  first-hand. A follow-up with direct page access (or the user pasting the pages)
  should confirm the PARTIAL/UNCONFIRMED items before final Gate 0 sign-off.

## Verdict (D10)

**PROVISIONALLY CONFIRMED** — Inoreader (Pro + Intelligence, or Team
Intelligence) can carry collection + scheduled AI report + **email** distribution
as assumed; the design holds with two mandatory conditions — **prompt-level
event-date rejection for zero-stale**, and **Email (not Teams) as the native
delivery baseline** — plus Gate-2 verification of schedule precision and the
unconfirmed limits.
