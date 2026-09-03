# ADR-001 — Web-native stack now, Godot/Rust/Nakama as the graduation path

Status: Accepted
Date: 2026-07-20

## Context

The CLVI overview document (archived at [`../CLVI-OVERVIEW.md`](../CLVI-OVERVIEW.md))
describes a Godot/WebGL2 client with a Rust/PostgreSQL backend and Nakama for
realtime multiplayer. The system as actually being built is operated from an iPhone,
deployed as static builds on Netlify, and advanced by unattended loop sessions on
ephemeral runners.

## Decision

> **ADR-001** Web-native TypeScript/Vite/Canvas now; Godot/WebGL2 + Rust/PostgreSQL
> + Nakama (per CLVI overview) recorded as the graduation path. Reason: iPhone
> Safari + Netlify static + unattended loop sessions favor web-native today.

## Consequences

- Every repo builds TypeScript/Vite with a `dist/` published to Netlify. Canvas is
  the rendering surface; there is no game engine dependency.
- The Godot/Rust/Nakama stack is not rejected — it is deferred and recorded. Moving
  to it requires a superseding ADR, not a session's judgment call.
- Loop sessions must be able to verify their work with `npm run build` plus browser
  tests on an iPhone-profile viewport. A stack that cannot be checked that way from a
  phone is out of contract with how this project is operated.
