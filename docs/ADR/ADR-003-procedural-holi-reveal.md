# ADR-003 — The Holi reveal is procedural, not video

Status: Accepted
Date: 2026-07-20

## Context

The find moment is a "Holi" colour burst. Implementing it as an alpha-channel video
overlay is the obvious approach and the wrong one on the target device: alpha video
playback in iOS Safari is unreliable, and iPhone Safari is the primary test target.

## Decision

> **ADR-003** Holi reveal is procedural (canvas particles), not video: iOS Safari
> alpha-video is unreliable; a `playsinline` video skin is optional later.

## Consequences

- The burst is drawn with canvas particles at runtime. It has no media asset
  dependency, no decode cost, and degrades with the frame budget rather than failing
  outright.
- A `playsinline` video skin may be layered on later as an enhancement. It may never
  become the only path to the reveal.
- The reveal must hold its frame budget on an A15-class iPhone alongside the map;
  particle counts are a tuning knob, not a fixed constant.
