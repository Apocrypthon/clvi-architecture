# CLVI GAME SPEC — working title: GUARDIAN (Paradise)
Version 1.0 · 2026-07-18 · Canon owner: Claudius
Commit this file into `clvi-architecture` as `SPEC.md`. All five tracks build against it.

## 0. Canon & precedence

Order of authority: `docs/LOOP.md` protocol → this SPEC → `contracts/` → session judgment. One section below is marked **[STRATA PLACEHOLDER]**: the source document (Strata) failed to upload and its canon is pending. Sessions build to the stated defaults there and must not invent beyond them.

## 1. Product frame

A location-based game for the Pick It Up LV / Clean Las Vegas Initiative universe. Players ("guardians") find artifacts in the real world within the service area, prove the find cryptographically, and light up the map. Real-world frame: artifacts map naturally to cleanup finds; the bloom map doubles as a public record of coverage in Paradise. The game ledger is an **internal game ledger** — guardian tokens are not a financial instrument, not tradable, not sold. (Default canon; deliberate, given nonprofit ambitions. Change only by editing this section.)

## 2. Flow map

- **Title screen.** Full-bleed, holds until input. On input: **horizontal pan** to the Gate.
- **Gate.** Three choices: `New User` · `Returning` · `Settings / Wallet log`.
- **New User → vertical pan-down → Character Creation.** Name + archetype (3 placeholder archetypes until art canon exists). Completing creation initializes save slot 1 and generates the wallet keypair if none exists.
- **Returning → vertical pan-down → Save Initialization.** 3 save slots. localStorage primary; Supabase profile sync once backend contract is merged.
- **Settings / Wallet log.** Wallet = locally generated Ed25519 keypair (`@noble/curves`), never leaves the device unencrypted. Shows: short pubkey, guardian token balance, energy audit (per §9), and a plain-language line stating what the wallet is and is not (per §1).
- All pans are CSS-transform animations. 60fps target on an A15-class iPhone. No route change may cause layout thrash mid-pan.

## 3. Service area — Paradise, NV only

The playable map is clipped to the boundary of the unincorporated town of **Paradise, Clark County, Nevada**. Source of truth for the polygon: Clark County GIS open data (townships/unincorporated boundaries layer) — Track 1 fetches the GeoJSON, commits a snapshot into the repo (`/data/paradise-boundary.geojson`) so builds are deterministic, and records the retrieval URL + date in that file's header. Everything outside the polygon renders desaturated and locked. No gameplay events spawn or resolve outside it. Expansion beyond Paradise is out of scope for this loop.

## 4. Artifacts & the Holi Special Video

Finding an artifact at its location triggers a **Holi Special Video** capture: a short clip (≤ 6 s) through the device camera with a color-burst ("holi") overlay rendered client-side. The capture produces:

- `media_hash` = SHA-256 of the raw video bytes (computed client-side before upload)
- geotag (must fall inside §3's polygon) + timestamp
- upload to the `holi-videos` Supabase storage bucket

The video is the human-verifiable proof-of-find; the hash is what enters the ledger. **Assumption flag:** "holi special video" is interpreted here as color-burst-overlay capture footage that drives the map update. If the intent was different (e.g., a specific format or existing footage), correct this section and the tracks inherit it.

## 5. Regenerative map

- Grid: **geohash precision 7** cells (~150 m) over the Paradise polygon.
- A verified ledger entry (§7) blooms its cell: desaturated → full color gradient, intensity stacking with entry count.
- **Decay:** bloom intensity halves every **14 days** without a new verified find in the cell. Cells regenerate to grey; the map is alive and demands re-finding. This is the "regenerative update": the world state is a pure function of the ledger + time, recomputable by anyone.
- Frontend (T3) may render a public read-only version of the same bloom state.

## 6. The hash problem (proof-of-find)

On a find, the client solves, in a web worker (wasm SHA-256, e.g. hash-wasm):

```
find nonce such that
sha256(prev_entry_hash || artifact_id || media_hash || nonce)
has >= k leading zero bits
```

- `k` initial value: **20 bits** (~1.05M hashes expected). Calibrate so the median solve is **2–5 s** on an A15 iPhone; store `k` in a config the backend also reads (contract), never hardcoded twice.
- The client reports `hashes_attempted` alongside the solution. The backend re-verifies the target before accepting (§7) — the puzzle is cheap to check, expensive-ish to forge, and the energy cost is bounded and audited (§9).

## 7. Ledger

Append-only hash chain in Supabase Postgres:

```
ledger_entries(
  id, prev_hash, artifact_id, media_hash, nonce,
  solver_pubkey, hashes_attempted, kwh_est,
  entry_hash, created_at
)
entry_hash = sha256(prev_hash || artifact_id || media_hash || nonce || solver_pubkey)
```

- Trigger-enforced: no UPDATE, no DELETE, `entry_hash` recomputed server-side.
- Writes only via the `verify-and-append` edge function: checks PoW target, recomputes hashes, verifies the Ed25519 signature over the submission, enforces the §9 energy cap, appends atomically.
- Public read. Anyone can replay the chain and recompute every hash — Track 5 ships exactly that replayer as a test.

## 8. Guardian token mint — **[STRATA PLACEHOLDER]**

> The Strata document is the canonical source for mint mechanics ("directly contributes to the mint of a guardian token"). It failed to upload and is not yet integrated. **Sessions: do not invent mint canon.** Build only the default rule below, structured so Strata's rules can replace it in one file.

**Default rule (until Strata lands):** every **10** verified ledger entries by one `solver_pubkey` → one mint event:

```
guardian_tokens(token_id, entry_ids[10], kwh_total, audit_hash, minted_at)
audit_hash = sha256(concat(entry_hashes) || kwh_total)
```

Mint logic lives in one edge function (`mint-guardian`) behind one config object, so replacing the default with Strata canon is a single-surface change. When Strata is integrated, this section gets rewritten and versioned.

## 9. kWh self-audit

Guardian tokens actively self-audit their energy cost:

- Per entry: `kwh_est = hashes_attempted × J_PER_HASH / 3.6e6`, with `J_PER_HASH = 6e-7` J (device-class constant for wasm SHA-256 on a ~3 W mobile SoC at ~5 MH/s; tunable, versioned in config). Expected per solve at k=20: ~0.6 J ≈ 1.7e-7 kWh.
- Caps, enforced at verify time: **5 J per entry**, **100 J (~2.8e-5 kWh) cumulative per token**. Submissions exceeding caps are rejected — the proof-of-find stays provably tiny.
- `energy_audit`: a public view/endpoint that **recomputes** `kwh_total` for any token from its chain entries rather than trusting the stored value. A token whose stored total diverges from its recomputed total fails audit. Track 5 tests this recomputation.

## 10. Wallet & saves

- Keypair: Ed25519 via `@noble/curves`, generated on device at first wallet log or character creation, stored locally (localStorage now; consider WebCrypto non-extractable or passkey-wrapped later — NEXT.md material, not cycle-1).
- Saves: 3 slots, localStorage primary, Supabase `profiles` sync keyed by pubkey once ledger-v1 contract is merged.
- Losing the device = losing the key, for now. State that honestly in Settings. Recovery design is out of scope for this 6-cycle loop.

## 11. Build, deploy, test constraints

- `clvi-game-client` and `clvi-frontend` deploy to Netlify from `main`, auto-deploy on push. A red build never reaches `main` (LOOP.md gate).
- Primary test device: iPhone Safari, 390×844. Playwright iPhone 13 profile (T5) is the automated proxy.
- Budgets: Lighthouse mobile performance ≥ 85 on the game client; initial JS payload lean enough that the title screen is interactive on LTE fast.

## 12. Open questions for Claudius (answer by editing this file — tracks inherit)

1. **Strata:** re-upload it; §8 gets rewritten from it verbatim-in-spirit.
2. **Holi:** confirm or correct the §4 interpretation.
3. **Name:** "GUARDIAN (Paradise)" is a placeholder title. Rename here once; Title screen follows.
