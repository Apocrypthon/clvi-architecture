# ADR-004 — Guardian tokens are application-level accounting

Status: Accepted
Date: 2026-07-20

## Context

One Guardian token is minted per verified find. "Token" reads as "blockchain", which
would bring a deployment, a chain dependency, key custody, and a financial-instrument
posture that a nonprofit cleanup initiative does not want. What the product actually
needs is an auditable count that can prove it has not been tampered with.

## Decision

> **ADR-004** Guardian tokens are application-level Postgres accounting with
> chain + HMAC integrity — not an on-chain token. On-chain is a future ADR, not a
> default.

## Consequences

- Tokens live in Postgres. Integrity comes from the mechanisms frozen in Contracts v1:
  an append-only ledger enforced by database triggers, a `prev_hash` chain from a
  genesis of 64 zeros, a per-row HMAC `immutable_check`, HMAC-signed reports, and a
  `/verify` endpoint that re-checks a pasted report — the persist-ant pattern.
- Tokens are not tradable, not sold, and not a financial instrument. The audit trail,
  not a market, is what gives them meaning.
- No repo in this system deploys a contract or takes a chain dependency. Doing so
  requires a superseding ADR that argues the case explicitly; the absence of one is a
  decision, not an oversight.
