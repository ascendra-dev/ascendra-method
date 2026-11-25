# MD-003: `/build-chunk`, merging into the unit's branch, the hold with the chunk

| Field | Value |
|---|---|
| Date | 2026-11-25 |
| Decided by | PO |
| Status | Agreed by the PO, 2026-11-25 |

## Decision

1. **`/build-checkpoint` becomes `/build-chunk`.** Since MD-002 it builds a whole chunk, one checkpoint at a time, each verified and committed. `/build-chunk {CODE} B03.2` builds the chunk; `/build-chunk {CODE} B03.2.3` resumes from that checkpoint; `--one` builds only the named checkpoint. The close stays in the same command, `/build-chunk {CODE} B03.X`, until it grows enough to need its own.
2. **The developer merges a chunk into the unit's branch; the PO merges the unit into the main line.** A chunk's pull request into `b{UU}` is merged by whoever built it once CI passes. The PO accepts each chunk when it suits: reads its report ("Decided myself", differences from the plan) and runs its acceptance steps on `b{UU}`; anything found is a fix branch into `b{UU}`. Only the PO merges `b{UU}` into the main line, at the close, after every chunk is accepted and the regression passes (DEC-099 holds: only the PO merges to the main line and deploys production).
3. **The hold passes with the chunk when turns are sequential.** People may share a unit's chunks in any order (one builds chunks 1 and 2, the other chunk 3), each chunk branching from `b{UU}` after the one before has merged there. When nobody else is working on the project's documents at the time, whoever builds the chunk holds the hold and records directly in the plan. Worklogs and `/sync-worklogs` are used only when work overlaps in time: two chunks at once, or building beside planning.

## Why

Merging each chunk into the main line as soon as it was built tied the PO's review to the developer's pace. The unit's branch isn't deployed, so a chunk can merge there without waiting, and the PO's control moves to where it protects the main line: the unit's close. The hold and worklogs exist for the project's documents, not the code; when turns are sequential the hold alone keeps them consistent with no sync to run. The command's name follows what it does.

## What changes

| File | Change |
|---|---|
| `METHOD.md` | §4 Chunk: merged into the unit's branch by its builder, accepted by the PO before the unit merges; §8.3 accordingly; §10 worklog mode: worklogs only when work overlaps, the hold passing with the chunk otherwise; §13 the command's name; change log 1.5 |
| `.claude/commands/build-checkpoint.md` | Renamed `build-chunk.md`; arguments (a chunk ID, or a checkpoint to resume from); the gate "Earlier chunk merged" into `b{UU}`; the close's gate: every chunk accepted by the PO |
| `.claude/commands/plan-unit.md`, `sync-worklogs.md`, `projects/TEMPLATE/` | Every mention of `/build-checkpoint` |
| `projects/TEMPLATE/delivery/units/unit.template.md` | Each chunk's "Accepted by the PO" line |

## Alternatives rejected

- **The PO merges every chunk:** keeps the PO on the developer's timing for no added safety, since the unit's branch isn't deployed.
- **A separate `/close-unit` command now:** the close fits in the same command today; split it only if it grows.
- **Worklogs always on with two people:** a sync for work that never overlapped is cost without benefit.
