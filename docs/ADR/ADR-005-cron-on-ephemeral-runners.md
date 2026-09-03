# ADR-005 — The improvement cron runs on ephemeral runners

Status: Accepted
Date: 2026-07-20

## Context

The loop advances each repo by running unattended agent sessions on a schedule. Those
sessions need to work without a human approving each tool call, which means
`--dangerously-skip-permissions`. Granting that to a machine anyone keeps is not an
acceptable trade; granting it to a container that is destroyed after each tick is.

## Decision

> **ADR-005** The improvement cron runs in GitHub Actions on ephemeral runners;
> `--dangerously-skip-permissions` is permitted there and only there. Local runs
> use the scoped allowlist relay.

## Consequences

- The flag is scoped to GitHub-hosted runners, which are isolated and die after each
  tick. "There and only there" is the whole point of the ADR; a local or long-lived
  machine running with it is a violation, not a shortcut.
- Local and interactive runs go through the scoped allowlist relay instead.
- Every invocation carries `--max-turns`; headless has no default limit, so an
  unbounded session is a runaway session.
- Secrets never enter a repo. They exist as GitHub and Netlify environment variables,
  and this repo documents their names only, never their values.
