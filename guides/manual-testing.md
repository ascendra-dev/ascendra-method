# Guide: Manual testing

How a unit is tested by hand: the registry built from its plan, rounds, results, triage. The rules are METHOD.md section 8.5 (MD-004); this guide holds the file formats and the procedure. It is read by the AI when the PO asks for a registry, a round or a triage, and by no command.

Examples come from a fee collection product and are illustrations only.

---

## 1. The one-way rule

Testing reads the plan, Requirements and the design. Nothing reads `delivery/testing/`: no command, no template, no build step, no plan. What testing finds comes back only as:

- a fix branch, for a bug (into the unit's branch while the unit is open; into the main line for a closed unit, by the project's rule for a change outside a unit);
- a PO decision that corrects a current document, for a gap or a suggestion the PO accepts;
- a finding for a later unit, in that unit's plan or the build index.

A plan never cites an issue; a fix's commit message does (`ISS-004`).

---

## 2. Files

```
delivery/testing/
├── README.md               what these files are (from the template)
├── people.json             who tests
├── issues.json             every issue in the project
└── {unit}/                 lower case: b02, b-spine
    ├── registry.json
    └── rounds/
        └── R{n}/
            ├── round.json
            └── {person}.json
```

One writer per file:

| File | Kind | Written by | Committed |
|---|---|---|---|
| `people.json` | Living | The PO | When it changes |
| `{unit}/registry.json` | Living until the unit's close, then frozen | The AI, from the plan | When built or rebuilt |
| `rounds/R{n}/round.json` | Frozen once the round starts | Whoever starts the round, with the AI | At the round's start |
| `rounds/R{n}/{person}.json` | Log | That person, through the test page | By that person, at the round's end |
| `issues.json` | Living | Triage only: the PO with the AI | At triage |

All files are JSON, two-space indented, keys in the order shown below, so diffs stay readable.

### 2.1 `people.json`

```json
[
  { "id": "zaka", "name": "Zaka" },
  { "id": "ali", "name": "Ali" }
]
```

`id` is lower case, letters only; it names the person's results file.

### 2.2 `registry.json`

```json
{
  "unit": "B02",
  "plan": "delivery/units/b02-school-structure.md",
  "planVersion": "1.3",
  "built": "2026-11-28",
  "frozen": null,
  "cases": [
    {
      "id": "B02.3-04",
      "chunk": "B02.3",
      "type": "rule",
      "title": "A class name can't be used twice in a year",
      "as": "Nadia Rahman",
      "setup": ["Greenfield has 2026–27 with Grade 5"],
      "steps": ["Open Academic year and classes", "Add class", "Type \" grade 5 \"", "Save"],
      "expected": ["The dialog stays open", "Under the name: \"A class named Grade 5 already exists\""],
      "compareWith": ["SCR-091 Academic year and classes · Add class: name taken"],
      "covers": ["CLASS_NAME_TAKEN"],
      "surfaces": ["dialog.class.add"],
      "from": null
    }
  ],
  "coverage": [
    { "item": "CLASS_NAME_TAKEN", "cases": ["B02.3-04"] },
    { "item": "SCR-091 · Classes didn't load, with Retry", "cases": ["B02.1-07"] },
    { "item": "CLASS_ORDER_STALE", "cases": [], "why": "Automated only: an API refusal with no screen state" }
  ]
}
```

| Field | Holds |
|---|---|
| `id` | The chunk and a two-digit number, in the chunk's order. Never reused: a case removed leaves its number unused |
| `chunk` | The chunk the case belongs to; for a checkpoint's `check` case, that checkpoint's chunk |
| `type` | `flow`, `state`, `rule`, `role`, `lifecycle`, `effect` or `check` (section 3) |
| `title` | What the case proves, as a sentence a tester understands |
| `as` | The person to sign in as, from the sample data; `null` for a page with no sign-in |
| `setup` | What must be true before the steps: data, a setting, a development flag. Empty when the seed is enough |
| `steps` | What the tester does, one action each |
| `expected` | What the tester sees, with the exact texts the plan gives |
| `compareWith` | Artboard titles, as the plan's parity lines give them; empty when none is drawn |
| `covers` | The plan items this case covers: rule codes, artboard states, role actions (`role:action`), transitions (`from→to`), command effects (`effect:command`), checkpoint IDs for `check` |
| `surfaces` | The plan's surface IDs the case touches |
| `from` | `null`, or the issue a case was added for (`ISS-004`) |

`coverage` lists every plan item section 3 names, with its cases, or no cases and a reason. `frozen` is the date the unit closed.

### 2.3 `round.json`

```json
{
  "round": "B02-R2",
  "scope": ["B02.3"],
  "types": ["flow", "state", "rule", "role"],
  "commit": "a1b2c3d",
  "branch": "b02",
  "started": "2026-11-28",
  "ended": null,
  "cases": ["B02.3-04", "B02.3-07", "B02.3-01"],
  "carried": ["B02.3-02", "B02.3-03"]
}
```

`cases` are run in this round; `carried` keep their latest result from earlier rounds. `ended` is set at triage.

### 2.4 `{person}.json`

```json
{
  "round": "B02-R2",
  "person": "zaka",
  "results": [
    {
      "case": "B02.3-04",
      "status": "fail",
      "at": "2026-11-28T10:12",
      "note": "The message shows only after a second Save",
      "feedback": [
        { "kind": "bug", "text": "The first Save closes the dialog; the list shows two Grade 5 until a refresh" },
        { "kind": "suggestion", "text": "Check the name while typing" }
      ]
    }
  ]
}
```

`status`: `pass`, `fail`, `blocked` (couldn't be run: say why in the note), `skipped`. A case with no entry is not run. A person runs a case again by replacing its entry; the page keeps one entry per case.

`kind`: `bug` (it's wrong), `gap` (the plan or design misses something), `suggestion` (it works, it could be better).

### 2.5 `issues.json`

```json
[
  {
    "id": "ISS-004",
    "unit": "B02",
    "from": "B02-R2 zaka B02.3-04",
    "kind": "bug",
    "text": "A duplicate class name is accepted on the first Save",
    "status": "fixed",
    "fix": "b02.3-fix-class-name, d4e5f6a",
    "verifyIn": "B02-R3",
    "decision": null,
    "laterUnit": null
  }
]
```

`status`:

| Status | Means | Set with |
|---|---|---|
| `open` | Settled by the PO as needing work; not done | — |
| `fixed` | A fix is merged | `fix`: the branch and commit; `verifyIn`: the next round |
| `verified` | A later round passed every case it touches | — |
| `decided` | Settled by a PO decision that corrected a current document | `decision`: `DEC-NNN` |
| `later` | Goes to a later unit | `laterUnit`, and the finding written in that unit's plan or the build index |
| `wontfix` | The PO settled that nothing changes | One line in `text` saying why |

A `fixed` issue whose case fails again goes back to `open`.

---

## 3. Building a registry

**When:** the unit's plan is approved, before its first chunk is built. For a unit closed before testing began, once, from its frozen plan (section 6).

**Reads:** the plan, in full. Nothing else, unless a case needs a screen text the plan doesn't give: then the artboard named in its parity line.

Go through the plan section by section:

| Plan section | Case type | One case per |
|---|---|---|
| Each chunk's acceptance (PO) | `flow` | Acceptance table, its steps kept together as one case; a long table split where the sign-in person changes |
| Each checkpoint's design parity | `state` | Artboard state not already shown by a `flow` case |
| Rules | `rule` | Rule code with a screen state; a code with none goes to coverage as "Automated only" |
| Actions and roles | `role` | Role that may not take an action it would see, and each role whose allowed view differs |
| Lifecycle and allowed actions | `lifecycle` | Transition, allowed and refused, not already covered |
| What each command causes | `effect` | Command with an effect a person can see (an audit entry, an email, a repeat's answer) |
| Each checkpoint's developer's check | `check` | Checkpoint |

Then:

1. Write each case's steps and expected result with the plan's exact texts. A case says what to do and see, never how the code works.
2. Merge duplicates: one case may cover several items (`covers` lists them all).
3. Write the coverage list: every item from the table above, with its cases or its reason.
4. A question the plan leaves open (a text it doesn't give, a state with no way to reach it) is asked of the PO in chat, as in planning. The answer goes into the plan by the plan's own route, then into the case.
5. Report to the PO: counts by chunk and type, the items with no case and why, any question asked.

**Rebuild at a round's start:** compare the plan's version with `planVersion`. Read the plan's change log entries since then; rebuild only the cases whose items or surfaces they touch; keep every other case and its ID.

---

## 4. A round

1. **Start.** The PO names the scope (chunks, or the unit) and the types. The AI rebuilds the registry if the plan moved (section 3), then writes `round.json`:
   - round 1 of a scope: every case of those chunks and types;
   - a later round: cases whose latest result is `fail` or `blocked`; cases whose `surfaces` include a surface a fix changed since the last round (read from the fix commits' changed files and the plan's surface IDs); cases not yet run; cases with a `fixed` issue to verify. Every other case is `carried`.
   - `commit` and `branch`: what the testers run.
2. **Run.** Each tester checks out that commit, starts the web app locally and opens the test page. Testers may split the cases between them or each run all; the page shows everyone's latest result per case.
3. **End.** Each tester commits their own results file.

---

## 5. Triage

After the round, with the PO.

1. Read every results file of the round.
2. For each piece of feedback, and for each `fail` with none, propose one issue: kind, one line of text, and a recommendation (fix in this unit, decide, later unit, won't fix). Several testers reporting the same thing make one issue.
3. The PO settles each, one at a time in chat. Write `issues.json`.
4. For each `verifyIn` of this round: every case the issue touches passed → `verified`; one failed → `open`.
5. A bug no case covers gets a new case in the registry, `from` the issue.
6. Set the round's `ended`. Report: results by status, issues opened and closed, what the next round runs.

The chunk is ready for the PO's acceptance when its cases have no `fail` and its issues none `open` or `fixed`.

---

## 6. A unit closed before testing began

1. Build the registry once from the frozen plan, wherever it is kept, and the Built blocks: where they record a text or behaviour that differs from the plan, the case follows the Built block. Set `frozen` to the unit's close date.
2. Round 1 runs every case against the main line.
3. Triage as section 5. A bug's fix is a small branch into the main line; a gap or suggestion is decided or goes to a later unit. The frozen plan is never edited.

---

## 7. The page

The product's web app has one page, running only in local development, that reads the files above and saves the current tester's results file. How it is built for a technology is in the stack profile (`stacks/{name}/testing-page.md`). It shows:

- a header: tester, unit, round, the round's commit and branch;
- **Cases:** the units and chunks with their pass, fail and not-run counts; filters by type, by "this round", "failing", "not run", and by feedback (a bug, a gap or a suggestion); for each case its setup, steps, expected result and artboards, earlier rounds' results by person, and Pass, Fail, Blocked, Skip (or Clear result), a note and feedback, saved with Save;
- **Issues:** read only, filtered to the unit;
- **Coverage:** each plan item and its cases, those with none in red.
