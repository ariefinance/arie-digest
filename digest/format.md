# digest/format.md — ARIE Digest output format (Gate 3)

Exact management-facing structure the master prompt (`digest/PROMPT.md`) must emit.
Optimised for a ≤5-minute email read. Plain, factual, institutional tone. No scores,
badges, owners, trend labels, confidence indicators, or generated outreach.

## Structure

```text
ARIE Digest — {Weekday}, {DD Month YYYY}

WHAT MATTERS TODAY
• {one-line pointer to the most material development}         (≤3 bullets; omit if none)
• {…}

MAURITIUS
{Headline}
Summary: {1–2 factual sentences.}
Why it matters to ARIE: {one specific sentence.}
{Source} · {event date} · {link}
[COMMERCIAL SIGNAL — {brief evidence-based reason}]           (only if all 3 conditions met)

{next item…}

WORLDWIDE
{items…}

FILM & TV
{items…}

OIL & GAS
{items…}
```

## Rules
- **Omit any section with no qualifying items.** Do not print an empty heading.
- If nothing qualifies anywhere, output exactly:
  `ARIE Digest — {date}` then `No material developments today.`
- **What Matters Today:** ≤3 bullets; each points to an item detailed below. Omit the
  whole block on a quiet day if nothing rises to it.
- **Per item:** Headline line; `Summary:`; `Why it matters to ARIE:`; then
  `Source · date · link`. The optional `COMMERCIAL SIGNAL` line is last.
- **Date shown = underlying event date** (per C1). If only a publication date is
  evidenced and the event date cannot be established, the item should have been
  rejected upstream — do not display it.
- **Volume:** normally ≤10 items total; ~12 max on a busy day. Film should not dominate:
  aim for ≤25% of items on multi-item days; if Film would exceed that, keep only the
  most material Film items rather than dropping stronger non-Film items or padding
  other sections.
- **Links:** one canonical/most-primary link per story (deduped per C2).
- **No** prospect scores, suggested contacts, owners, actions, or "monitor".

## Worked micro-example (illustrative only — not live data)

```text
ARIE Digest — Monday, 22 September 2026

WHAT MATTERS TODAY
• FSC Mauritius issues a new payment-intermediary licensing communiqué.
• OFAC designates additional shadow-fleet tankers affecting oil-payment corridors.

MAURITIUS
FSC Mauritius updates payment-intermediary licensing conditions
Summary: The FSC issued a communiqué revising conditions for payment intermediary
licences, effective next quarter.
Why it matters to ARIE: ARIE holds an FSC payment-intermediary licence, so revised
conditions may change its compliance obligations directly.
FSC Mauritius · 22 Sep 2026 · {link}

WORLDWIDE
Company C opens treasury operations across three African markets
Summary: Company C announced new foreign operations in three African markets, stating
it will run multi-currency treasury and cross-border settlement for the new offices.
Why it matters to ARIE: A named company standing up foreign multi-currency treasury is
the kind of cross-border payments need ARIE serves.
Company C newsroom · 22 Sep 2026 · {link}
COMMERCIAL SIGNAL — Company C (named) is establishing new foreign operations and the
release explicitly cites multi-currency treasury and cross-border settlement.

OIL & GAS
OFAC designates additional tankers in shadow-fleet action
Summary: OFAC added several tankers to its SDN list over sanctioned-oil transport.
Why it matters to ARIE: Tightened tanker sanctions raise payment-screening and
counterparty risk on oil-trade corridors relevant to ARIE's cross-border payments work.
OFAC · 22 Sep 2026 · {link}
```

The OFAC item carries **no** COMMERCIAL SIGNAL — a sanctions action is not a named
company with an explicit payments element, so it fails the strict 3-condition gate.
The Company C item shows a compliant signal (named entity + new foreign operation +
explicit multi-currency-treasury/settlement element stated in the source).