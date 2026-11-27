# Method Decisions

One row per decision about the method itself. Each has its own file, `MD-NNN-{slug}.md`.

| ID | Date | Decision | Status |
|---|---|---|---|
| [MD-001](MD-001-extract-the-method.md) | 2026-11-25 | Extract the method proven in ASCENDRA-PAY-004 into its own repository; freeze `ascendra-framework` | Agreed |
| [MD-002](MD-002-lighter-build-and-review.md) | 2026-11-25 | Plans keep length in check; easy gaps decided by the AI; build to the chunk's end; chunk pull requests into the unit's branch; sync shows only decisions; reading trimmed; closed plans not re-read | Agreed |
| [MD-003](MD-003-build-chunk-and-unit-branch.md) | 2026-11-25 | `/build-checkpoint` becomes `/build-chunk`; the builder merges a chunk into the unit's branch, the PO merges the unit into the main line; the hold passes with the chunk when turns are sequential | Agreed |
| [MD-004](MD-004-manual-testing.md) | 2026-11-27 | Manual testing as a separate activity: a registry of test cases built from each plan, rounds, per-person results, issues from triage, a local-only test page; nothing in the method reads it | Agreed |
