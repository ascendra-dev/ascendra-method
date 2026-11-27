# Manual testing

> **Guide.** Living (METHOD.md section 6). Copied once, when the project's first registry is built, as `delivery/testing/README.md`; `people.template.json` beside it is copied as `people.json`. Remove this guide block from the copy.

The project's manual testing (METHOD.md section 8.5): each unit's registry of test cases, each round's results, and the issues triage makes of them. The formats and the procedure are in the method's `guides/manual-testing.md`.

**Nothing reads these files.** No command, template, plan or build step. Testing comes back only as fix branches, PO decisions and findings for later units.

| File | Written by |
|---|---|
| `people.json` | The PO |
| `issues.json` | Triage only: the PO with the AI |
| `{unit}/registry.json` | The AI, from the unit's plan |
| `{unit}/rounds/R{n}/round.json` | Whoever starts the round |
| `{unit}/rounds/R{n}/{person}.json` | That person only, through the test page |

These files are outside worklog mode and the hold: each has one writer.
