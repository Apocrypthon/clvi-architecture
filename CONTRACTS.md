# CONTRACTS v1 (frozen)

Status: **frozen**. Version: **v1**. Ratified: 2026-07-20.

These are the shared interfaces of the CLVI / STRATA system. They change ONLY in
this repository, by a human-merged PR (see [Amendment procedure](#amendment-procedure)).
Every other repo copies the block below **verbatim** into its own seed/state docs and
treats it as frozen. Loops never negotiate contracts among themselves.

```
Account     { playerId, displayId, palette, name, createdAt }
Challenge   { challengeId, salt, difficultyBits, expiresAt }
Submission  { challengeId, playerId, cellId, artifactId, nonce, hashes, ms, deviceClass }
LedgerEntry { entryId, prevHash, ts, playerId, cellId, artifactId, estKwh, tokenId }
MapEvent    { cellId, ts, kind:"restored" }
AuditReport { rangeStart, rangeEnd, entryCount, totalEstKwh, chainOk, tokenCount, signature }

Solve rule    sha256(salt + ":" + nonce + ":" + playerId) leading zero bits >= difficultyBits
Difficulty    start 18; server auto-tunes ±1 toward median solve 3–6 s; floor 12, ceiling 24
Caps          30 s per attempt (then reissue at −4 bits); 60 solves/player/day (client AND server)
Cell id       "P-<col>-<row>", 150 m grid from Paradise boundary bbox NW corner
Energy        estKwh = (ms / 3.6e9) × WATTS[deviceClass]; WATTS {phone 3, tablet 5, laptop 15, desktop 45};
              server recomputes from ms clamped to [0, 30 s]; estimates, assumptions documented
Integrity     ledger append-only (DB triggers); prev_hash chain (genesis 64 zeros);
              per-row HMAC immutable_check; reports HMAC-signed; /verify re-checks — persist-ant pattern
```

## Copying rule

Copy the fenced block above **character for character** — including the en dashes in
`3–6 s`, the minus sign in `−4 bits`, and the `×` in the energy formula. Do not
reformat it, do not "tidy" the alignment, do not translate the prose rules into your
own words in the copy. A repo whose copy differs from this block is out of contract,
and reconciling it is that repo's next loop item.

Each consuming repo carries the block under a heading of the form
`## Contracts v1 (frozen; change only via clvi-architecture)`, and may carry only the
subset of type definitions it actually uses (as `clvi-infrastructure` does with
`MapEvent` and `AuditReport`) — but any line it does carry is verbatim.

## Amendment procedure

- **Propose.** Open a PR against this repo editing `CONTRACTS.md` **and** adding a
  new ADR under `docs/ADR/` explaining why. A contract change without an ADR is
  incomplete; an ADR without the contract edit is a proposal, not an amendment.
- **Merge.** Human only. No loop session, cron run, or agent merges this file.
- **Propagate.** The next loop session in each repo copies the new block verbatim as
  a `STATE.md` Next item, bumping "Contracts v1" to the new version everywhere in the
  same day. Loops never negotiate contracts among themselves.

Version bumps are whole numbers (`v1` → `v2`). Every occurrence of the version string
moves together; a system running mixed versions is a bug, not a migration.
