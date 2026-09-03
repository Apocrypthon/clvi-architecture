# ADR-002 — "Wallet" means a custodial Guardian account

Status: Accepted
Date: 2026-07-20

## Context

The product surfaces a "wallet" in Settings, and the word invites the assumption of a
self-custodied crypto wallet. The MVP needs an identity that survives a Safari
restart on a phone, nothing more.

## Decision

> **ADR-002** "Wallet" = custodial Guardian account (Supabase email OTP, display id
> GRD-xxxxxx). No crypto-wallet integration in MVP.

## Consequences

- Identity is a Supabase account reached by email OTP. The user-visible handle is a
  display id of the form `GRD-xxxxxx` — this is what `Account.displayId` in
  Contracts v1 carries.
- No key material is generated, stored, or asked for on the device in the MVP. No
  seed phrases, no signing prompts, no wallet-connect flows.
- Any UI that says "wallet" must also state in plain language what the wallet is and
  is not, so the word does not over-promise.
- Adding real key custody later is a superseding ADR and, because `Account` is a
  contract type, a `CONTRACTS.md` amendment.
