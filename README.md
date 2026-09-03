# clvi-architecture — Doc-of-Record & Contracts v1

This repo is the constitution, not a loop target. Contracts change ONLY here, by a
human-merged PR; every other repo copies them verbatim and treats them as frozen.
This is the apex of the documentation mise en abyme: the document that defines how
the other documents may change.

No cron runs against this repository. No loop session commits to it. If an agent
session is editing this repo, a human asked it to, and a human merges the result.

## Contents

| Path | What it is |
|---|---|
| [`CONTRACTS.md`](CONTRACTS.md) | Contracts v1, frozen. The shared interfaces every other repo copies verbatim, plus the copying rule and the amendment procedure. |
| [`docs/ADR/`](docs/ADR/) | The decision log. ADR-001 … ADR-006, seeded with Contracts v1. |
| [`docs/CLVI-OVERVIEW.md`](docs/CLVI-OVERVIEW.md) | Archive slot for the original CLVI concept document. **Currently a placeholder** — the source text has not been archived yet; the file records what is known about it and how to complete it. |

## The system this governs

STRATA / CLVI is a location-based game for the Pick It Up LV / Clean Las Vegas
Initiative universe. Players ("guardians") find artifacts in the real world inside
the Paradise, NV service area, prove the find by solving a disclosed hash puzzle, and
light up a regenerative map. Each verified find appends to an append-only hash-chained
ledger and mints one Guardian token, and the ledger publishes HMAC-signed self-audits
of the energy it spent doing so. The token audits its own energy; picking up litter
directly contributes to the mint of a guardian token.

Five repos build it, each advanced by its own loop of unattended sessions that keep
their memory in `docs/` rather than in the model:

| Repo | Slice |
|---|---|
| `clvi-frontend` | Pick It Up LV public shell; title pan → new/returning/settings |
| `clvi-game-client` | Paradise map, regenerative bloom, the find: dig → hash solve → Holi burst → kWh estimate |
| `clvi-backend` | Netlify Functions + Supabase: verify solves, chained ledger, mint, signed audit |
| `clvi-infrastructure` | Status dashboard, environment doc-of-record, canonical mocks, smoke script |
| `clvi-testing` | iPhone acceptance checklist, contract and e2e smoke tests, solve-timing bench |

Each of those repos carries the Contracts v1 block verbatim. This repo is where the
block is authored; `clvi-architecture` is attached alongside the track repo in every
loop session so that no session has to guess at a shared shape.

## Amendment procedure

**Propose:** a PR editing `CONTRACTS.md` **and** adding a new ADR under `docs/ADR/`
explaining why. **Merge:** human only. **Propagate:** the next loop session in each
repo copies the new block verbatim (a `STATE.md` Next item), bumping "Contracts v1"
to the new version everywhere in the same day.

Loops never negotiate contracts among themselves. A session that finds a contract
inconvenient records the problem in its `STATE.md` and builds to the contract anyway;
it does not fix the contract locally, and it does not fix it here.

Full rules, including the character-for-character copying requirement, are in
[`CONTRACTS.md`](CONTRACTS.md). ADR conventions are in [`docs/ADR/README.md`](docs/ADR/README.md).

## License

MIT — see [LICENSE](LICENSE).
