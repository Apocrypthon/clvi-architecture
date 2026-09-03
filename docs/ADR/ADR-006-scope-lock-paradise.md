# ADR-006 — Scope lock: Paradise, NV only

Status: Accepted
Date: 2026-07-20

## Context

The service-area map is the most tempting thing in the system to widen — every new
neighbourhood looks like a small change. Widening it before the core loop works on
production turns one unfinished game into several.

## Decision

> **ADR-006** Scope lock: Paradise, NV service area only until every acceptance
> item passes on prod.

## Consequences

- The playable area is clipped to the boundary of the unincorporated town of
  Paradise, Clark County, Nevada. Cell ids are `"P-<col>-<row>"` on a 150 m grid
  measured from that boundary's bbox NW corner, per Contracts v1.
- Nothing outside the polygon spawns or resolves. Outside is rendered locked.
- Expansion is not a loop decision. It unlocks only when every acceptance item passes
  on production, and it lands as a superseding ADR.
