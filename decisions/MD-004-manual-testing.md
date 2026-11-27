# MD-004: Manual testing, a separate activity beside the build

| Field | Value |
|---|---|
| Date | 2026-11-27 |
| Decided by | PO |
| Status | Agreed by the PO, 2026-11-27 |

## Decision

1. **Manual testing is a separate activity, and nothing in the method reads it.** People run test cases by hand, record results and raise issues. Testing reads the method's documents (build plans, Requirements, the design). No command, template or build step reads testing's files. Testing feeds the method back only through channels that already exist: a bug becomes a fix branch, and a gap or suggestion becomes a PO decision that corrects a current document in place (section 9). Its files are outside worklog mode and the hold (decision 9).

2. **Each unit has one registry of test cases, built from its plan.** The AI builds it; nobody edits it by hand. Each case has a type, by the part of the plan it comes from:

   | Type | From the plan's | Checks |
   |---|---|---|
   | `flow` | Chunk acceptance (PO) | The main path, start to end |
   | `state` | Design parity: every artboard state | Empty, loading, error, phone width and the other drawn states |
   | `rule` | Rules: every rule code with a screen state | The refusal as the screen shows it, once per rule code. The refusal itself is covered by automated tests |
   | `role` | Actions and roles | Each role can, or can't, do each action |
   | `lifecycle` | Lifecycle and allowed actions | Each allowed and refused transition |
   | `effect` | What each command causes | Audit entries, emails, what a repeat does |
   | `check` | Each checkpoint's developer's check | The developer's own steps |

   The registry ends with a **coverage list**: every rule code, artboard state, role action and transition in the plan, and the case that covers it, or the reason none does ("automated only: no screen state"). A plan item that doesn't appear in the list is a gap.

3. **Each case carries its own steps.** Each case has: ID, chunk, type, title, the person to sign in as, setup, steps, expected result, the artboard to compare with, the plan items it covers, and the surfaces it touches. The case ID is the chunk and a number (`B02.3-04`). The surfaces are what pick the cases to retest after a fix (decision 5).

4. **When the registry is built and updated:**

   | Moment | Registry |
   |---|---|
   | The plan is approved | Built in full, before any code. A question the plan leaves open while writing the cases goes to the PO, as in planning |
   | During the build | Unchanged |
   | A round starts | Compared with the plan's current version; only the cases the plan's changes affect are rebuilt |
   | An issue no case covered | A case added, marked with the issue it came from: the only cases not taken from the plan |
   | The unit closes | Frozen with the plan; it joins the regression set |
   | A later decision changes a closed unit | The decision names the affected cases, and they change with it |

5. **Testing runs in rounds.** A round (`B02-R1`) is one pass over a scope (one chunk, several chunks or the whole unit), against one commit, over the case types it names. Its case list is fixed when it starts. Round 1 runs every case in its scope. Each later round runs:
   - the cases that failed or were blocked;
   - the cases touching a surface a fix changed;
   - the cases not yet run.

   Every other case keeps its earlier result. A chunk's round ends with no Fail and no open issue, and that is the PO's acceptance of the chunk. The close's regression is a round over every case of every closed unit.

6. **Each person records only their own results.** Each person writes one results file per round. For each case they record a status (not run, pass, fail, blocked, skipped), a note, and any number of feedback items, each a **bug** (it's wrong), a **gap** (the plan or design misses something) or a **suggestion**. A passing case can still carry a suggestion.

7. **Triage turns feedback into issues.** After each round the AI reads every results file and proposes one **issue** (`ISS-001`, numbered across the project) for each piece of feedback. The PO settles each one:

   | Issue | Becomes | Its status |
   |---|---|---|
   | A bug | A fix branch into the unit's branch, or into the main line for a closed unit; the commit cites the issue | open → fixed (with the commit) → verified by a later round, or reopened |
   | A gap or a suggestion | A PO decision that corrects a current document, or a finding for a later unit, or nothing | decided (`DEC-NNN`), later (the unit), or won't fix |

   The issue list is the only tracking. Triage writes no report document.

8. **Units already closed get a regression baseline.** The registry is built once from the closed plan, wherever it is kept, and frozen. The first round runs against the main line as it is. A bug is fixed on a small branch into the main line, by the project's rule for changes outside a unit. Anything else is decided, or goes to a later unit. The closed plan is never reopened.

9. **The files are the only storage.** There is no database, no API and no report. The files live in the project folder, beside the plans:

   ```
   delivery/testing/
   ├── people.json             who tests: id and name
   ├── issues.json             every issue, written at triage
   └── b02/
       ├── registry.json       the unit's cases and coverage; built from the plan
       └── rounds/
           └── R1/
               ├── round.json  scope, commit, case types, the case list
               └── {person}.json   that person's results
   ```

   | File | Kind | Written by |
   |---|---|---|
   | `registry.json` | Living until the unit's close, then frozen | The AI, from the plan |
   | `round.json` | Frozen once the round starts | Whoever starts the round |
   | `{person}.json` | Log | That person only, through the page |
   | `issues.json` | Living | Triage only (the PO with the AI) |
   | `people.json` | Living | The PO |

   One writer per file means nothing to merge. Each tester commits their own results file at the round's end. The registry and the issues are committed at triage.

10. **A local page runs the tests.** The product's own web app has one test page that works only in local development; it is never built for staging or production. It reads the registry, the round and the results from the project folder through a setting that names its path, and saves only the current tester's results file. It has three tabs:
    - **Cases:** units and chunks with their counts; filters by type and by this round, failing or not run; each case's setup, steps and expected result, the artboard to compare with, earlier rounds' results, and the buttons Pass, Fail, Blocked and Skip, a note and feedback.
    - **Issues:** read only.
    - **Coverage:** plan items with no case shown in red.

    How the page is built for a technology lives in its stack profile.

11. **No command yet.** Building the registry, starting a round and triage are done by hand with the AI, from a guide, until real rounds prove them (principle 9). ASCENDRA-PAY-004A is the first project: B01 and B03 as regression baselines, then B02 as its acceptance.

## Why

A chunk's PO acceptance is a five-step demo of its main path. Every state, refusal, role and effect a unit's plan settles goes unchecked by hand, and what testers notice (gaps, suggestions) has no place to go. The plan already holds every item worth testing, so the cases are built from it instead of written a second time (principle 7). Keeping testing out of everything the method reads means it adds no reading to planning or building. Per-person results files keep several testers from ever editing the same file, so there is nothing to sync and nothing to store but files.

## What changes

| File | Change |
|---|---|
| `METHOD.md` | §4: Test case, Registry, Round, Issue (`ISS-001`); §5.3: Testing under Operations; §8.5 Manual testing (decisions 1, 2, 4, 5, 7, 8, in short); §10: testing's files outside the hold; §12: `delivery/testing/` in the project layout; change log 1.6 |
| `guides/manual-testing.md` | New: the file formats, how a registry is built from a plan section by section, how a round is started and its cases chosen, how triage runs |
| `projects/TEMPLATE/delivery/testing/` | New: `people.template.json`, and `README.template.md` naming the files and the one-way rule |
| `projects/TEMPLATE/CLAUDE.template.md` | A Manual testing section: the testers, the test page's address, its path setting |
| `stacks/ascendra/README.md` | The profile's table lists `testing-page.md` |
| `stacks/ascendra/testing-page.md` | New: the page in the web app (a local-only route and the route handler that reads and writes the files), how it is kept out of every build but local development |

No command, and no command's reading list, changes.

## Alternatives rejected

- **Test cases from the PO acceptance steps only:** five steps per chunk test the main path and nothing else.
- **Test cases written by the AI apart from the plan:** the same steps in two places, drifting apart.
- **Results in a database, through the API:** storage, migrations and an endpoint for what a few small files do.
- **The registry and results in the web repository:** test results in the code's history, and testing files away from the plans they come from.
- **One shared results file per round:** two testers editing one file at once means merges.
- **Testing on staging:** the page couldn't save there, so every tester would export and commit a file by hand.
- **Commands now:** not yet proven in a real round.
