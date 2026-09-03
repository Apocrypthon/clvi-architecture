# ADR log

Architecture Decision Records for CLVI / STRATA. One decision per file, numbered in
order, never renumbered. An ADR is append-only in spirit: to change a decision, add a
new ADR that supersedes the old one and mark the old one `Superseded by ADR-NNN` —
do not rewrite history in place.

A PR that edits `CONTRACTS.md` must add an ADR here explaining why. That is the
whole amendment procedure; see [`../../CONTRACTS.md`](../../CONTRACTS.md).

| ADR | Decision | Status |
|---|---|---|
| [ADR-001](ADR-001-web-native-stack.md) | Web-native TypeScript/Vite/Canvas now; Godot/Rust/Nakama is the graduation path | Accepted |
| [ADR-002](ADR-002-custodial-guardian-account.md) | "Wallet" means a custodial Guardian account, not a crypto wallet | Accepted |
| [ADR-003](ADR-003-procedural-holi-reveal.md) | Holi reveal is procedural canvas particles, not video | Accepted |
| [ADR-004](ADR-004-application-level-tokens.md) | Guardian tokens are application-level Postgres accounting, not on-chain | Accepted |
| [ADR-005](ADR-005-cron-on-ephemeral-runners.md) | The improvement cron runs on ephemeral GitHub Actions runners | Accepted |
| [ADR-006](ADR-006-scope-lock-paradise.md) | Scope lock: Paradise, NV only until acceptance passes on prod | Accepted |

All six were seeded together with Contracts v1 on 2026-07-20, verbatim from
`clvi-architecture.CONTRACTS.md` (Strata Seeds v2). Each file below quotes its seed
text unaltered under **Decision**; surrounding sections add only context already on
the record.

## Format

```
# ADR-NNN — <short title>

Status: Accepted | Superseded by ADR-NNN | Proposed
Date: YYYY-MM-DD

## Context     what forced a decision
## Decision    what was decided, in the imperative
## Consequences what this costs and forecloses
```
