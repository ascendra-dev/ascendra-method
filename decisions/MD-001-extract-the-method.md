# MD-001: Extract the method into its own repository

| Field | Value |
|---|---|
| Date | 2026-11-25 |
| Decided by | PO |
| Status | Agreed |

## Decision

The way ASCENDRA-PAY-004 is delivered (a milestone Blueprint, a screen map in dependency order, every design group designed and approved, then build plans with chunks and checkpoints, and worklog mode for parallel work) becomes its own method in this repository, `ascendra-method`. `ascendra-framework` is frozen as it is and no longer maintained.

## Why

`ascendra-framework` was structured but sequential: whole-scope documents (BRD, epics, stories, a Locked architecture) had to exist before any code, and the PAY-004 attempt became unmanageable under them. The approach that replaced it inside PAY-004 works, but grew without structure: one handoff of 46,000 words, plans of 28,000 to 40,000 words, every document an append-only log. Maintaining both was not affordable for one person.

## What carries over

| From `ascendra-framework` | Here |
|---|---|
| `/plan-build-group`, `/build-checkpoint`, `/sync-worklogs`, the build plan and worklog templates (FW-053) | Reworked into `/plan-unit`, `/build-checkpoint`, `/sync-worklogs` and their templates, under METHOD.md's rules |
| The screen design process run in PAY-004 | `/map-screens`, `/design-group` |
| `reference/` standards used by the build commands (UI library, API contract, validation messages, layering, commons adoption) | `stacks/ascendra/` |
| Everything else (intake, discovery, BRD, epics, stories, sprints, UAT, release notes, estimation) | Not carried over; stays in the frozen framework |

## Alternatives rejected

- **Update `ascendra-framework` in place:** keeps the clutter of two methods in one history and one lifecycle document.
- **Keep both tracks:** doubles what one person maintains.
