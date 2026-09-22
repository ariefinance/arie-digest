# qa/QA_RESULTS.md — ARIE Digest Gate 3 QA / Adversarial Results

**Under test:** `digest/PROMPT.md` (master prompt) against `digest/format.md`,
`qa/fixtures/fixtures.md` (F01–F14), and `inoreader/noise-filters.md`.
**Method:** For each fixture, reason about how a competent LLM following
`digest/PROMPT.md` **verbatim** (seeing only the supplied article text, no browsing)
would actually behave, then compare to the fixture's Expected outcome. Then hunt for
weaknesses beyond the fixtures. **Locked decisions respected in all proposed edits:**
no scores/owners/actions/outreach, email-only, single AI pass, four sections + WMT,
strict 3-condition Commercial Signal (D2/D3/D4/D6).

**Headline:** 1 clear FAIL (F12 — over-eager Commercial Signal). Several PASSes carry
material risk that only survives because a *competent* model resolves an ambiguity the
prompt does not close. The prompt is basically sound on accept/reject/dedupe, but its
freshness rule, grounding rule, and Commercial-Signal gate have exploitable gaps.

---

## 1. Per-fixture results

| Fixture | Expected | Predicted prompt behaviour (verbatim) | PASS/FAIL | Note |
|---|---|---|---|---|
| **F01** FSC revises licensing | ACCEPT → Mauritius, likely WMT | Kept (genuine regulatory change; Step 1 carve-out "genuine policy/regulatory change IS in scope"); event date 22 Sep evidenced in body → passes C1; Mauritius; likely WMT | PASS | "Why it matters" cites *ARIE holds an FSC licence* — a fact **not** in the article, only in the persona. Survives only because W2 is read charitably. |
| **F02** BoM T-bill auction | REJECT | Matches Step 1 "treasury bill … auction … weekly … results:" → rejected | PASS | Clean. |
| **F03** Procurement EoI | REJECT | Matches "expression of interest / registration of suppliers / invitation to bid / prequalification" → rejected | PASS | Clean. |
| **F04** FATF delist, event Oct 2024 | REJECT (C1) | Event date Oct 2024 evidenced in body; ~2 yr old → "clearly happened long ago" → rejected | PASS | Passes only because 2 yr is obviously old. A **weeks-old** evidenced event would slip (W1). |
| **F05a/b** AMLA Chair (2 publishers) | MERGE → one | Same event, within 3 days, shared anchor "AMLA appointment" → merged; keep primary link | PASS* | Relies on the model equating "AMLA" with "EU anti-money-laundering authority" (different tokens, low overlap). Literal "named body" wording + noise-filters' "modest token overlap" clause would **split** it (W6). |
| **F06a/b** FSC → firm A vs firm B | DO NOT MERGE | Explicit override: "two different firms each getting an FSC licence are two stories" → kept separate | PASS | Saved *only* by the licence-specific override; the general anchor rule ("body + event word") would otherwise merge (W5). |
| **F07a** Company C African expansion + treasury | ACCEPT + COMMERCIAL SIGNAL | Named entity + new foreign operations + text cites cross-border payment & multi-currency treasury → all 3 met → accept + signal; Worldwide | PASS | Press-release date = event = pub date; a pedantic C1 reading could over-reject (W3). |
| **F07b** Series B for AI product | REJECT, no signal | Matches "generic fundraising" + no specific trigger → rejected; no signal | PASS | Clean. |
| **F08** Agentic AI + hardware opinion | REJECT | Topic-keyword only, no specific trigger → rejected | PASS | Clean. |
| **F09** Actor cast in series | REJECT | Step 1 "Film: casting, celebrity" → rejected | PASS | Clean. |
| **F10** Ireland Section 481 cap rise | ACCEPT → Film & TV | Step 4 Film "INCLUDE incentive/rebate changes even if payments not mentioned" → accepted; not casting/box-office | PASS | "Why it matters" (incentives drive cross-border production payments) is inferential — persona reasoning, technically outside literal grounding (W2). |
| **F11** Brent +0.4% | REJECT | Step 1 "Oil & Gas: bare oil-price movement" → rejected | PASS | Clean. |
| **F12** Trader D LNG supply, West Africa | ACCEPT → Oil & Gas; signal only if all 3 evidenced | Accepted (material supply agreement + cross-border corridor). **But condition 3 ("credible cross-border/payments angle") is met by "cross-border LNG supply deal" on its face → model likely ADDS a COMMERCIAL SIGNAL**, although no payment/treasury/FX element is stated in the text | **FAIL** | Accept is correct; the **signal is unwarranted** — the gate is too soft (W4). This is the "over-eager signal" the mandate flagged. |
| **F13** ARIE launches MU–India corridor | ACCEPT, likely WMT | Direct ARIE mention; event date evidenced → accept + WMT | PASS | Section is ambiguous (Mauritius vs Worldwide — W9); press-release date risk (W3). |
| **F14** 2026 index / 2025 event | REJECT (C1 trap) | Body says launch Mar 2025; index date non-probative; ~18 mo old → rejected | PASS | Handled — the explicit trap works. |

**FAIL count: 1** (F12 signal). **PASS-with-material-risk: F05, F06, F07a, F10, F13.**

---

## 2. Weaknesses found

### W1 — C1 has no recency window; evidenced-but-weeks-old events pass as "new" (SEVERITY: HIGH)
**Scenario.** C1 rejects an item only if the event "clearly happened long ago" or the
date cannot be established. An event with an evidenced date of, say, 20 Aug 2026
(5 weeks before "today") is *established* and is not *clearly* long ago, so a
rule-following model keeps it and presents it as current in a **daily** briefing.
F04/F14 are caught only because they are years old; the actual stale-as-new leak is
the middle band (days-to-weeks). This is the precise failure mode the project cares
most about, left with no numeric edge.
**Minimal edit — Step 2.**
- Current: `If the event clearly happened long ago and is merely being republished/resurfaced, REJECT.`
- Replace: `Treat an event as current only if evidenced as occurring within roughly the last few days (since the previous weekday digest; Monday covers the weekend). If it is older than that, or is merely being republished/resurfaced, REJECT.`
(Respects D8 cadence; no code, no new pass.)

### W2 — Grounding rule contradicts the required "Why it matters to ARIE" sentence (SEVERITY: HIGH)
**Scenario.** Step 5 requires a "Why it matters to ARIE" sentence for every item, and
Step 4 asks for "credible ARIE transaction/commercial relevance" (Oil & Gas) and Film
relevance "even if payments not mentioned." But the same Step 5 says "Entity names and
facts must appear literally in the supplied text — do NOT infer … leave it empty rather
than guess." A strictly rule-following model faces a contradiction: the ARIE-relevance
sentence *inherently* connects the article to ARIE's business (persona knowledge, not
article text), so the model must either leave the mandatory field empty (breaking the
format) or write an ungrounded sentence (breaking the rule). This is where the grounding
rule is **weakest**, and the format's own worked example already exhibits it
("oil-trade corridors ARIE **may serve**" — an unevidenced claim ARIE operates there).
Affects F01, F10, F12, F13.
**Minimal edit — Step 5.**
- Current: `Entity names and facts must appear literally in the supplied text — do NOT infer.`
- Replace: `Entity names and facts about the news event must appear literally in the supplied text — do NOT invent article facts. The "Why it matters" sentence may connect those stated facts to ARIE's known business (the persona above), but must not assert unstated facts about ARIE's own corridors, clients, or operations (e.g., do not imply ARIE already serves a market unless stated).`

### W3 — Same-day announcement vs publication date: C1 can over-reject legitimate news (SEVERITY: HIGH)
**Scenario.** C1 says feed/index/**publication**/syndication dates are non-probative and
"if you cannot establish the event is recent, REJECT." For a press release, communiqué,
or launch, the event date **equals** the publication date (F01, F05, F07a, F10, F12, F13
are all 22 Sep 2026). A conservative model can read "22 Sep 2026" as merely a publication
date, decide the event date "cannot be established," and reject genuine same-day news —
i.e., C1 becomes a false-negative machine for exactly the fresh items the digest exists
to surface. Competent models resolve it; the prompt gives no rule to distinguish.
**Minimal edit — Step 2.**
- Current: `The underlying EVENT date governs freshness, not the feed/index/publication/syndication date.`
- Replace: `The underlying EVENT date governs freshness, not the feed/index/publication/syndication date. An action, issuance, launch, appointment, or announcement dated within the article body IS an evidenced event date even when it coincides with the publication date; only a bare feed/index timestamp with no in-body event is non-probative.`

### W4 — Commercial Signal condition 3 too soft → over-eager signals (SEVERITY: MEDIUM) [drives the F12 FAIL]
**Scenario.** Conditions 1 (named entity) and 2 (specific trigger) are objective;
condition 3 — "a credible ARIE cross-border/payments angle supported by the evidence" —
uses "credible," which invites inference. Any "cross-border" deal reads as a "credible
payments angle," so the model attaches a COMMERCIAL SIGNAL to F12 (LNG supply deal) with
no stated payment/treasury/FX element — an inferred banking angle D4 forbids. The gate
does not actually hold.
**Minimal edit — Step 6, condition (3).**
- Current: `(3) a credible ARIE cross-border/payments angle supported by the evidence.`
- Replace: `(3) an explicit payment, treasury, FX, settlement, correspondent-banking, or documented banking-difficulty element stated in the supplied text (the activity merely being cross-border does NOT satisfy this).`
(Still exactly 3 conditions, annotation-only — D4 intact.)

### W5 — C2 anchor rule over-merges distinct events beyond the licence case (SEVERITY: MEDIUM)
**Scenario.** The anchor is "named body + a specific event word." Two FSC enforcement
actions against different firms, or two BoM circulars, share "FSC/BoM" + an event word
("enforcement", "circular") and would merge. Only the *licence* case is explicitly
carved out; the general defect (subject entity not part of the anchor) remains, so
F06-shaped collisions outside licensing still merge.
**Minimal edit — Step 3.**
- Current: `NEVER merge on a generic regulator name alone — two different firms each getting an FSC licence are two stories.`
- Replace: `NEVER merge on a regulator/body name plus a generic event word alone — the specific subject entity must also match. Two different firms each getting an FSC licence, or each facing an FSC action, are separate stories.`

### W6 — C2 under-merge (synonym gap) + divergence from noise-filters.md (SEVERITY: MEDIUM)
**Scenario.** F05 names the same body two ways ("AMLA" vs "EU anti-money-laundering
authority") with low token overlap. A literal "named body" reading splits them.
Worse, `noise-filters.md` §F states C2 as **three** conditions (anchor + ~3 days +
"modest token overlap"), whereas `digest/PROMPT.md` Step 3 has **two**. For F05 the
token-overlap condition points the *wrong* way (overlap is low → would block the correct
merge). The two specs disagree, and syncing PROMPT.md to noise-filters would break F05.
**Minimal edit — Step 3 (prompt).**
- Current: `they share a specific event anchor (a named body + a specific event word, e.g. "FATF plenary", "AMLA appointment", "X acquires Y") AND are within ~3 days.`
- Replace: `they share a specific event anchor — a named body (treat an abbreviation and its full name as the same body, e.g. "AMLA" = "EU anti-money-laundering authority") + a specific event word, e.g. "FATF plenary", "AMLA appointment", "X acquires Y" — AND are within ~3 days.`
- **Also (spec hygiene, not the prompt):** align `noise-filters.md` §F to the two-condition C2, or drop "modest token overlap," so the spec cannot be used to "fix" the prompt into splitting F05.

### W7 — Film ≤25% cap conflicts with "keep all relevant / never fill a quota" and is unspecified for small N (SEVERITY: MEDIUM)
**Scenario.** On a light non-film day (e.g., 2 non-film + 3 strong film = 60% film), the
cap forces the model to either drop genuinely relevant Film items (violating "keep an
item … empty sections better than weak content") or ignore the cap. No drop rule is given,
and 25% of small N is degenerate (25% of 5 = 1). The cap is unenforceable as written.
**Minimal edit — Step 7.**
- Current: `Total volume: … Film ≤25% of items.`
- Replace: `Total volume: … Film should not dominate: aim for ≤25% of items on a multi-item day; if Film would exceed that, keep only the most material Film items rather than dropping stronger non-film items or padding other sections.`

### W8 — Judgment carve-outs invite over-acceptance (SEVERITY: LOW–MEDIUM)
**Scenario.** "repo rate (unless a *change* with policy signalling)" and evergreen
"unless a genuinely dated development" are subjective; a model can re-label routine BoM
or SEO-guide content as "a policy change"/"dated development" and surface it. Acceptable
residual, but worth a tighter phrasing if BoM noise recurs in live testing. No edit
proposed now (would risk suppressing genuine policy news); flagged for Gate-4 monitoring.

### W9 — "What Matters Today" ↔ section coupling under-specified (SEVERITY: LOW)
**Scenario.** format.md says each WMT bullet "points to an item detailed below," but the
prompt says WMT "may repeat items covered below" — a model could place a development
*only* in WMT with no section entry (F13 is a candidate, given its Mauritius/Worldwide
ambiguity). Minor, but worth one clause.
**Minimal edit — Step 7, WMT bullet.**
- Current: `"What Matters Today": at most 3 bullets — the developments management would most regret not knowing. May repeat items covered below.`
- Replace: `"What Matters Today": at most 3 bullets — the developments management would most regret not knowing. Each must also appear as a full item in one of the four sections below.`

### W10 — Step 1 Film reject can drop mixed production+casting stories (SEVERITY: LOW)
**Scenario.** "Film: casting, celebrity, box-office/ratings" as a bare reject can catch a
material production-finance/incentive story that *also* mentions a star, contradicting
Step 4's include list.
**Minimal edit — Step 1.**
- Current: `Film: casting, celebrity, box-office/ratings.`
- Replace: `Film: items that are ONLY casting, celebrity, or box-office/ratings (keep if a material production/incentive/finance/jurisdiction development is also present).`

---

## 3. Residual limitations (genuinely unguaranteeable in a single, no-browsing pass)

1. **True freshness cannot be verified.** With no browsing, "recency" rests entirely on
   dates the article chooses to state. A resurfaced piece that *fabricates or omits* an
   event date, or restates an old event in present tense with a fresh in-body date, can
   still read as current. W1/W3 narrow but cannot close this; C1 is a text-trust rule,
   not a verification.
2. **Deduplication is event-recognition, not identity proof.** Merging/splitting depends
   on the model recognising two write-ups as the same event from wording alone
   (W6). Genuinely ambiguous pairs (same regulator, same day, different subject entities
   only implied) will sometimes merge or split wrongly; no anchor rule fully removes this.
3. **ARIE relevance is inherently a judgement.** Even with W2, the "Why it matters"
   sentence and the Oil & Gas / Film "credible relevance" tests require connecting facts
   to ARIE's business — a reasoned inference, not something literally in the article.
   Quality here is bounded by model judgement, not by the prompt.
4. **Commercial Signal condition 3 still needs judgement.** Even tightened (W4), deciding
   whether a stated element is a *real* ARIE payments angle vs incidental is a call the
   single pass makes without external context.
5. **Volume/section balance is advisory.** "~5–10", "≤12", "Film ≤25%", "WMT ≤3" are soft
   targets a single pass approximates; it cannot guarantee global optimality across a
   batch it processes once (D2 single-pass, by design).
6. **No cross-day memory.** The pass cannot know what yesterday's digest already carried,
   so a slow-developing story can recur on consecutive days despite C1 — acceptable under
   V1 scope (D5), but a real limit.

---

## 4. Remediation applied (editor response — Gate 3 revision)

All findings reviewed and accepted. Prompt/spec revised in the same Gate 3 branch:

| # | Weakness | Action |
|---|----------|--------|
| W1 | C1 no recency window | **Fixed** — Step 2 now bounds "current" to ~last few days since prior weekday digest (Mon covers weekend, D8). |
| W2 | Grounding vs required relevance sentence | **Fixed** — Step 5 scopes literal grounding to article event facts; permits connecting stated facts to ARIE persona, forbids unstated ARIE-operations claims. |
| W3 | Same-day announcement over-rejection | **Fixed** — Step 2 states an in-body action/issuance/launch/appointment date IS an evidenced event date even when it equals publication date. |
| W4 | Commercial Signal cond. 3 too soft (F12 FAIL) | **Fixed** — Step 6(3) now requires an explicit payment/treasury/FX/settlement/correspondent-banking/banking-difficulty element in text; "cross-border" alone does not qualify. F12 signal now correctly withheld. |
| W5 | C2 over-merge beyond licence case | **Fixed** — Step 3 requires the specific subject entity to match; generalised beyond licences. |
| W6 | C2 under-merge (synonym) + spec divergence | **Fixed** — Step 3 treats abbreviation = full name; `noise-filters.md` §F aligned to the two-condition rule with token overlap as a hint only. |
| W7 | Film ≤25% unenforceable for small N | **Fixed** — Step 7 reworded to "aim ≤25% on multi-item days; keep most material Film rather than drop stronger non-film or pad." |
| W8 | Subjective carve-outs | **No edit** (accepted residual) — flagged for Gate-4/pilot monitoring; tightening now risks suppressing genuine policy news. |
| W9 | WMT↔section coupling | **Fixed** — Step 7 requires each WMT bullet to also appear as a full section item. |
| W10 | Mixed film-reject | **Fixed** — Step 1 rejects Film only when ONLY casting/celebrity/box-office. |

**Post-revision fixture status:** all 14 fixtures expected to PASS (F12 signal now
withheld; F04/F14 remain rejected and the middle-band stale case is now closed by the
recency window; F05 merge and F06 split both explicitly supported; F01/F10/F12/F13
relevance sentences now permitted without breaking grounding). The **residual
limitations in §3 remain true** — they are inherent to a single, no-browsing pass and
are accepted for V1, to be watched in the Gate 5 pilot.
