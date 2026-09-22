# SECURITY.md — Public-exposure & secrets rules

This repository is **temporarily PUBLIC** during execution and becomes private at
Gate 6. Treat every commit as **permanently public**.

## The test (apply before every commit / PR / merge)

> Would ARIE be comfortable if every line of this change were indexed publicly
> forever? If no — do not commit it.

## Never commit

- API keys, tokens, passwords, OAuth secrets, production credentials
- Private/authenticated feed URLs
- Microsoft tenant identifiers, admin details
- Management email addresses or recipient lists
- Client / KYC / KYB data, confidential counterparties, real prospects
- Internal correspondence, confidential commercial arrangements
- Any infrastructure IDs (e.g. the legacy repo's Railway IDs / prod URL — never copied here)

Use **placeholders** for anything private (e.g. `sk-...`, `manager@example.com`,
`https://REDACTED-feed-url`).

## Practices

- Secrets belong in the operator's Inoreader/Microsoft account settings, never in
  this repo. There is no application backend and no credential store here (D1/D9).
- Config committed here is **documentation of settings to apply**, with private
  values redacted — not live credentials.
- Every PR gets a public-exposure review before merge (Security/Hygiene pass).
- Report any accidental exposure immediately; rotate the affected secret and purge.

## Reporting

For anything sensitive discovered in this repo, contact the repository owner
directly rather than opening a public issue.
