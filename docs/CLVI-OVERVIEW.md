# CLVI Overview — archived concept document

Status: **placeholder — source text not yet archived.**

This file is reserved for the verbatim archive of the original CLVI concept
document (the "archived CLVI overview" the Strata Seeds v2 runbook assigns to this
repo). **The body below is not that document.** The source text was not available to
the session that created this file: it is not in this repository, and it is not in
the CLVI Drive folder, which holds only the derived documents (the orchestration
plan, the game spec, and the five repo seeds). Until someone pastes it in, this page
records what the project has already fixed about the source so that nothing depends
on memory.

Do not reconstruct the overview from the derived documents. The point of an archive
is that it is the original; a plausible rewrite would be worse than an empty section,
because it would be indistinguishable from the real thing later.

## What the record already fixes about the source

- **It truncates.** The source file ends mid-sentence at `directly contri—`.
- **The completion is author-confirmed.** The act of picking up litter
  "…directly contributes to the mint of a guardian token." This sentence is canon;
  the truncation is an artifact of the file, not an open question about intent.
- **It names a north-star stack.** [ADR-001](ADR/ADR-001-web-native-stack.md) records
  Godot/WebGL2 + Rust/PostgreSQL + Nakama as coming from this overview, and defers it
  as the graduation path behind today's web-native stack. That deferral is the only
  load-bearing dependency any current decision has on this document.

Nothing else in the system reads from this file. Contracts v1 is self-contained in
[`../CONTRACTS.md`](../CONTRACTS.md), and the ADR log states its own reasoning, so
the missing body blocks no work — it is a gap in the archive, not in the canon.

## How to complete this file

1. Paste the original document verbatim under a `## Source text` heading. Do not
   copy-edit it, do not fix its typos, and do not reflow it.
2. Leave the truncation visible where it occurs, and apply the author-confirmed
   completion as a marked editorial insertion rather than silently:
   `directly contri[butes to the mint of a guardian token.]` — with a footnote
   pointing back to this section.
3. Change `Status:` at the top of this file to `archived`, and date it.
4. Note the provenance of the copy you pasted: where it came from and when.

Filling this in is a documentation change, not a contract change. It needs no ADR,
and it must not touch `CONTRACTS.md`.
