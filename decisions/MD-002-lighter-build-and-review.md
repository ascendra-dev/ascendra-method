# MD-002: Lighter building, reviewing and reading

| Field | Value |
|---|---|
| Date | 2026-11-25 |
| Decided by | PO |
| Status | Agreed by the PO, 2026-11-25 |

## Decision

Seven changes, from the PO's review of the method before its commands' first real use:

1. **Plans keep their length in check.** A plan's Part A and each checkpoint section stay as short as their decisions allow. A part that runs long says why in one line, in the plan's header. No fixed word limit. `/plan-unit`'s plan checks look for repetition and for facts copied that the build doesn't need.
2. **Easy gaps are decided by the AI.** At a checkpoint's start, a gap is **asked** when it changes the design, the scope, Requirements, Architecture, money or security behaviour, or needs a dependency, a secret or a product answer. Every other gap with one clear answer in the conventions, patterns or existing code is **decided by the AI**, built, and listed under "Decided myself" in the Built block and the report.
3. **Build to the chunk's end.** A checkpoint is still built, verified and recorded on its own, one commit each. The AI then goes on to the next checkpoint of the chunk without waiting. It stops only for an asked gap, a failure it can't fix within the plan, or the chunk's end. At the chunk's end one report gives the acceptance steps, every "Decided myself" item and every difference from the plan.
4. **Pull requests per chunk into a group branch.** Each chunk's pull request targets the unit's branch (`b03`), and the PO reviews it when it suits. The unit's branch merges to `main` once, at the close, after the regression.
5. **A sync shows the PO only what needs reading.** In `/sync-worklogs`, every Decision entry that applies cleanly is listed one line each (question → answer, by whom), so the PO skims the decisions without reading entries. Built, Status, Change, Finding and New file entries that apply cleanly are counted only.
6. **Reading is trimmed where it repeats.**
   - `/build-checkpoint` reads the changed-copies log (named by Architecture §2) only when the checkpoint copies or changes a shared module.
   - It reads only the API contract rules its blocks cite, since the plan's A0 carries the standard behaviour.
   - `/plan-unit` reads Architecture in parts (§1, §3, §4, §6, §10), and the rest only where the unit touches it.
7. **A closed plan isn't read again** except its findings for later units (C3), because the close promotes everything else into the current documents. This keeps every unit's reading flat, however many units come before it.

## Why

ASCENDRA-PAY-004 grew a 46,000-word handoff and plans of 28,000 to 40,000 words. Most of it was history mixed into current documents, which the method already fixes, and facts repeated inside plans, which METHOD.md section 8.2 forbids but nothing checked. The PO also spent time answering small gaps before every checkpoint and reviewing work twice: at each chunk's pull request, then at the merge. These changes keep the PO on real decisions and keep each command's reading close to what it uses.

## What changes

| File | Change |
|---|---|
| `METHOD.md` | §4 Chunk: "one pull request into the unit's branch"; §6.3 Build plan: length kept in check, a stated reason when long; §8.2: the gap split; §8.3: building to the chunk's end, the unit's branch merging at the close; change log 1.4 |
| `.claude/commands/plan-unit.md` | Step 3 item 12: Architecture in parts; plan checks: repetition and length, the stated reason |
| `.claude/commands/build-checkpoint.md` | Step 3 items 5 and 6 trimmed; Step 4: asked gaps and AI-decided gaps; Steps 9 and 10: "Decided myself", and the chunk-end report; going on to the next checkpoint |
| `.claude/commands/sync-worklogs.md` | Step 5: clean Decision entries listed one line each, others counted |
| `projects/TEMPLATE/delivery/units/unit.template.md` | The header's line for a long part; the Built block's "Decided myself" |

## Alternatives rejected

- **Fixed word limits on plans:** a hard limit invites cutting a needed fact; a stated reason keeps the judgment visible.
- **One pull request per unit:** too large to review, and a long-lived branch; chunk pull requests into the unit's branch keep each review small with one merge to `main`.
- **The AI deciding every gap:** a design, scope, money or security answer stays the PO's.
