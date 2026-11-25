# Build Checkpoint

Builds the checkpoints of an approved unit plan, one at a time, verifying and recording each (METHOD.md section 8.3). Started on a chunk's checkpoint, it goes on through the rest of the chunk without waiting, and stops only for something the PO must answer, a failure it can't fix within the plan, or the chunk's end (MD-002). A checkpoint is one behaviour a reviewer can see work, from the surface down to the database, with its own tests: small enough for the developer to review as one commit, and a chunk small enough to review as one pull request. Run for the unit's close (`B{UU}.X`), it also promotes what the unit settled into the current documents and freezes the plan (section 8.4).

It runs in the plan's order, after `/plan-unit` has written the plan and the PO and the developer have approved it. It reads the plan and the code, not the wider documents: the plan already carries every fact and decision. A gap with one clear answer in the conventions, patterns or code it decides itself and lists; a gap that changes the design, scope, Requirements, Architecture, money or security behaviour, or needs a dependency, a secret or a product answer, it asks. It produces code on the chunk's branch, one commit per checkpoint with its Built block. It never pushes or merges, except that the close merges the unit's branch and deploys only when the PO says so.

In worklog mode (METHOD.md section 10), the Built block, status changes and findings are entries in the builder's own worklog, unless the builder holds the hold; `/sync-worklogs` applies them. The code is built the same either way.

---

## Arguments received

`$ARGUMENTS`

---

## Step 1: Parse arguments

Required: a project code and a checkpoint ID. Optional: `--check-tests`, `--one`.

- **Project code:** the folder under `projects/`.
- **Checkpoint ID:** from the unit's plan, e.g. `B03.1.2`, or `B03.X` for the close.
- **`--check-tests`:** after the tests pass, break the code on purpose to prove they can fail (Step 7). Always done for a checkpoint with a money command, given or not.
- **`--one`:** build only this checkpoint, then stop, instead of going on to the chunk's end.

If anything is missing, ask for all of it in one message:

> "To build a checkpoint I need:
> 1. **Project code:** the folder under `projects/`
> 2. **Checkpoint ID:** from the unit's plan (e.g. `B03.1.2`); `delivery/index.md` lists the plans"

Wait for the PO's response.

---

## Step 2: Gate check

| Check | How | If it fails |
|---|---|---|
| Writing mode | The project's `CLAUDE.md` has a Worklog mode section or not. If it has: `git config user.name` is one of its people, and `worklogs/hold.md` names who holds the hold. **Direct:** no worklog mode, or the hold is yours. **Worklog:** otherwise | > "Your git user, {NAME}, isn't one of `{CODE}`'s people in worklog mode, so I don't know whose worklog to write." |
| Close needs the hold | For `B{UU}.X` in worklog mode: the hold is yours, because promotion changes several current documents | > "The close promotes into Requirements, Architecture, Standards and Experience, so it needs the hold. Take it first (`worklogs/hold.md`)." |
| Plan exists | `delivery/units/b{UU}-*.md` exists | > "There's no plan for B{UU} in `delivery/units/`. Run `/plan-unit {CODE} {UU}` first." |
| Plan approved | Its Status is `Approved` or `In progress`, counting any unapplied Status entry in any worklog | > "B{UU}'s plan is still {STATUS}. The PO and the developer approve it first." |
| Checkpoint exists | The ID is a row in the plan's B0, or `B{UU}.X` | > "{ID} isn't in B{UU}'s plan." |
| Not already built | Its Status is `Planned`, and no worklog holds a Built entry for it | > "{ID} is already {STATUS}. Rebuilding it replaces its Built block. Rebuild it?" |
| Earlier checkpoints built | Every checkpoint above it in B0 is `Built` (in the plan or a Built entry); for the close, all of them | > "{ID} comes after {IDS}, which aren't built. Build them first, or tell me to continue anyway." |
| Earlier chunk merged | For a chunk's first checkpoint: the previous chunk's pull request is merged into the unit's branch | > "Chunk {PREVIOUS} isn't merged into `b{UU}` yet, so this chunk wouldn't start from it. Continue anyway?" |
| Units it depends on closed | Every unit in the plan's Depends on is `Closed` in `delivery/index.md` | > "B{UU} depends on {UNITS}, which aren't closed. Continue anyway?" |
| Clean start | Each code repository the checkpoint touches is on the chunk's branch (or the unit's branch, for a chunk's first checkpoint) with no uncommitted changes | > "{REPO} has uncommitted changes on {BRANCH}. The previous checkpoint's commit comes first, so this one stands on its own." |
| Plan still matches the code | Commits since the plan's Code base commit, other than this unit's own, change no module, table, endpoint or shared part Part A names | > "Since the plan was reconciled, {COMMITS} changed {WHAT}, which the plan relies on in {SECTIONS}. My recommendation: [the plan change / no change, because …]. Go on?" |

On any failure, show the message and wait.

---

## Step 3: Read

Record the start time now; it goes in the Built block. Read, in this order:

1. `projects/{CODE}/CLAUDE.md`: project rules, its stack profile, and its row for `/build-checkpoint`.
2. **The plan, in parts:** the header, Part A in full (A0 to A14), the checkpoint's section (or Part C for the close), B0, the Built blocks of earlier checkpoints in the same chunk, C2 and C3. **The checkpoint's blocks are binding, and so is Part A.**
3. **In worklog mode:** every worklog's unapplied entries on this plan (Built, Status, Change, Finding). With the plan, they are its current state: an earlier Built block may still be an entry, and a Change entry may already correct a block this checkpoint uses. Where an entry in another person's worklog disagrees with the plan, say so in Step 4.
4. `standards/conventions.md`, in full, and the entries of `standards/patterns.md` the blocks name.
5. **When the checkpoint copies or changes a shared module:** the log of changes to copied code that Architecture §2 names, so no changed copy is copied again, and the stack profile's `commons.md`.
6. **For a checkpoint with endpoints:** the rules of the stack profile's `api-contract.md` its blocks cite ("Rule 8"); the plan's A0 carries the standard behaviour.
7. **For a checkpoint with screens:** each surface's artboard source, found from its exact title as the design runbook (`runbooks/design.md`) says; the sections of `experience/design-rules.md` for the parts they use; the stack profile's `ui/usage-rules.md` and `ui/code-style.md`; the UI library's own reference docs, and its showcase only to find a specific pattern.
8. **For the close:** the plan in full; `delivery/index.md`; and each document a C2 row targets.
9. **The code it builds on:** the files earlier Built blocks list, the modules and screens it changes, and nearby code showing the repository's conventions.

Don't read the sources, Requirements, the Charter, the screen map or the decision log, except as item 8 says for the close. The plan carries what they decided; anything the build needs that isn't in the plan is a plan gap (Step 4).

---

## Step 4: Check before building

Work out three things for the checkpoint:

1. **Needs first:** what it relies on (earlier code, a running database, the seed, a provider account), and whether each is met. Check it yourself where you can; mark it "PO to confirm" where you can't.
2. **Needed from the PO:** secrets, accounts or a choice, each with exactly where it goes and what it looks like.
3. **Plan gaps:** anything the blocks and Part A don't state, anything wrong against the code, anything that would take the checkpoint past its size or change its scope. Sort each one:
   - **Asked:** it changes the design, the scope, Requirements, Architecture, money or security behaviour, or needs a dependency, a secret or a product answer; or more than one answer is reasonable. Give the plan change you recommend.
   - **Decided myself:** everything else with one clear answer in the conventions, the patterns, the plan's own decisions or the existing code. Decide it, follow it, and list it with its one-line reason; it goes in the Built block and the report.

**At a chunk's first checkpoint**, show all three in one message and wait for the PO's go, which covers the rest of the chunk:

> "Before building chunk {CHUNK} ({N} checkpoints), starting with {ID} ({goal}):
>
> **Needs first:** [each: met / not met / PO to confirm]
> **Needed from you:** [each and where it goes, or 'nothing']
> **Asked:** [each with the change I recommend, or 'nothing']
> **Decided myself:** [each with its reason, or 'nothing']
>
> Shall I build the chunk?"

**At a later checkpoint of the chunk**, go straight on to Step 5 when nothing is asked, nothing is needed from the PO and every need is met; the decided gaps are listed in its Built block. Otherwise stop and ask as above, for this checkpoint only.

Ask in plain chat, never through a selection widget; a choice that needs its own answer is asked on its own, with a recommendation. Write no code for an asked gap until the PO answers.

**A plan change**, agreed or decided, is made in the plan itself (METHOD.md section 8.2): the block corrected in place, a choice added or changed in A14 with its why, a design difference added to A12, and one line in the header's Versions naming them. A change that also alters a current document (Requirements, the design, Architecture) is always asked; it gets a C2 row, or, if it can't wait for the close, a `DEC` entry and the document changed now. In worklog mode, each is a Change entry in the builder's worklog (an asked one with the PO's agreement in its body), written before building, and the build follows it; one that alters another person's current work is marked Urgent.

**Secrets** go in a gitignored `.env` in the code repository, never in chat if avoidable, never in a committed file. Give the PO the keys to add, blank. When the PO says it's ready, check each key is set and isn't a placeholder, without printing any value.

---

## Step 5: Build

**Branches:** the unit's branch, `b{UU}` (`b03`), created from the main line at the unit's first checkpoint, in each repository the unit touches; and one per chunk, `b{UU}.{chunk}-{slug}` (`b03.1-see-the-staff`), created at the chunk's first checkpoint from the unit's branch. A chunk's pull request targets the unit's branch; the unit's branch merges into the main line at the close. Code from any other unmerged branch is used only where the plan's A13 lists it.

Build what the checkpoint's blocks say, and nothing more.

- **The plan's words are the specification:** text, IDs, rule codes, messages, types and query keys exactly as written.
- **IDs in code:** each surface's root carries its surface ID as its test ID; a client function is named after its action, a query key after its query ID; each rule's refusal uses its code; tests are named as the plan names them.
- **Folders:** every file in the folder of the module that owns it, by Architecture §3 and the plan's module block. A new module is registered in the project's boundary check with its kind, layer and table prefixes.
- **Effects** travel as A9 and Architecture §4 say: no module calls audit or notification directly; what must commit with the change happens inside the transaction; what follows it is sent after the commit.
- **This checkpoint only.** Code for a later checkpoint isn't written now, even when it's obvious. If this one can't work without it, stop and ask.
- **Stay within Part A:** modules, calls between them, tables and patterns as Part A decides. If the checkpoint can't be built that way, stop and propose the plan change.
- **Copied shared code:** copy a module only as the stack profile says, only the parts named. A change to a copy is the smallest that works, logged in the log of changes to copied code that Architecture §2 names. Never copy again a module the log lists.
- **Screens:** start from the artboard source and keep its composition, text and states, with the real API in place of sample data. A library part used in the artboard is used here, whole, at its defaults. A part the plan extracts is built once, where A11 says. A link to a screen not built yet is left out. A deferred part shows exactly what A2 says. Errors show as A0 and the response tables say.
- **Seed:** A10's records for this checkpoint, through the modules' contracts, never by inserting rows.
- **Small choices** the plan doesn't state and no reviewer would question (a private helper's name, a file split) are yours; record each for the Built block. A gap found while building is sorted as Step 4 says: decided and listed, or asked before going on.
- **Size:** if the change grows past the checkpoint's limit, stop and propose a split.
- **Terms:** use the domain's terms in names, as the plan does.

---

## Step 6: Verify

Every gate runs, in this order. A failed gate is fixed in the code, not the test, before the next runs. Change a test only when the test is wrong, and say so in the Built block.

1. **Static:** type check, lint, and the boundary check, in each repository touched.
2. **Tests:** every test the checkpoint names, against the real thing (the real database as the app's own role, not a mock) unless the plan says otherwise. At least one fails without this change: confirm it. Then every earlier test of the unit.
3. **Financial checks:** for a money command, every check its block lists.
4. **Design parity (screens):** render each artboard the parity line names and the built surface at the same size, with the seed and the clock the plan's header gives. Look at both. List every difference in layout, text, parts and states. Fix what's wrong; a difference you keep goes in the Built block with its reason, and must be in A12 or proposed for it.
5. **Developer's check:** run its steps yourself where you can and record the result. The developer runs them again with the commit.
6. **A chunk's last checkpoint:** run the chunk's acceptance steps on the running product and record the result. List the commits the chunk's pull request into `b{UU}` will hold.
7. **The close:** the demo as a browser test, the browser tests of every closed unit, the financial checks. Then stop:

   > "B{UU} passes its demo and the regression of {N} closed units. Every chunk's pull request is merged into `b{UU}`. Shall I open the pull request merging `b{UU}` into the main line, and deploy to staging once you've merged it?"

   Merge and deploy only as the PO says. The close's manual steps (settings, seeds, provider changes) are listed for the PO in order, each confirmed before the next.

If a gate can't pass without breaking something the plan made binding, stop:

> "{GATE} can't pass without [what would break]. My recommendation: [option]. How do you want to handle it?"

---

## Step 7: Check the tests can fail

For a checkpoint with a money command, and any run with `--check-tests`; otherwise skip it, and the Built block says "Tests not checked by breaking the code."

For each break the money command block lists, or each rule the checkpoint enforces (at most three):

1. Back up the file, then break the rule in the code.
2. Run the tests with a short timeout; record which failed.
3. Restore the file from the backup and confirm it matches byte for byte.

Every break must make at least one test fail. If one doesn't, a test is missing: add it and say so. After the last break, run the full suite and confirm everything passes.

---

## Step 8: Promote and freeze (the close only)

For `B{UU}.X`, after the PO confirms the deploy:

1. **Apply every C2 row** to its target, in place (METHOD.md section 8.4), each with a row in that document's change log:
   - A business rule clarified, corrected or first settled → `requirements/requirements.md`, with a new `RULE` (or `VAL`) where it had none.
   - An architecture decision, a module, an event, a consistency row → `architecture/architecture.md`.
   - A foundation proved or found wanting → Architecture §2.
   - A convention → `standards/conventions.md`; a pattern extracted → `standards/patterns.md`, its status `Exists`, with where it lives.
   - A design change → the screen map, design rules or sample data.
   - Capabilities delivered → `delivery/index.md` coverage rows set to Built.
2. **Findings:** each C3 row aimed at a later unit is checked to be in that unit's plan or the build index.
3. **Freeze the plan:** fill Part C's Built block and sign-off; set its Status to `Closed` and its index row to `Closed` with the date. From now on it is frozen: a later correction lives in the current documents.
4. **Decision log:** one `DEC` entry for the close, listing what was promoted where.

If a C2 row turns out wrong or conflicts with a current document, ask the PO before applying it.

---

## Step 9: Record

Record the end time.

**Direct** (no worklog mode, or you hold the hold): in the plan,

1. under the checkpoint, its Built block, at most ten lines, as the template gives: the result in one sentence; files by area; decided myself (each gap decided in Step 4 or while building, with its reason, or "none"); differences from the plan (every small choice, every change to a copy, every PO decision taken during the build, or "none"); tests, where they ran, the parity result, breaks caught if Step 7 ran; the commit per repository;
2. the checkpoint's Status `Built`, in its section and in B0; the plan's Status `In progress` if it was `Approved`;
3. new findings appended to C3, new promotions to C2;
4. the unit's row in `delivery/index.md`: checkpoints built.

**Worklog:** the same content as entries in your own worklog, in the shape of `projects/TEMPLATE/worklogs/worklog.template.md`, each with the plan's version and the project commit you read as "Based on": a **Built** entry with the Built block exactly as it would go into the plan ("Planned → Built", and "Approved → In progress" if so); a **Finding** entry per C3 addition; a **Change** entry per C2 addition. Leave the build index to the sync. Never edit the plan, the build index or the decision log in worklog mode.

**Commit the checkpoint** on the chunk's branch in each repository it touched, one commit with its message, after every gate passed: the PO's go for the chunk is the instruction. Never push. The project's documents (the plan, the build index) are committed only when the PO asks. In worklog mode, commit the worklog with the checkpoint's code, so the other person sees its Built entry once the branch is pushed.

**Then go on:** unless `--one` was given or this was the chunk's last checkpoint, start the next checkpoint at Step 2. The developer reviews each commit and its Built block with the chunk's pull request.

---

## Step 10: Report

After each checkpoint, one line, then go on:

> "{ID} built: {goal}. {N} tests pass; decided myself: {N}; commit {SHA}. Going on to {NEXT_ID}."

At the chunk's end (or with `--one`, or when stopped), the report:

```
CHUNK BUILT: {CODE} {CHUNK}
─────────────────────────────────────────
Goal:           {what a reviewer can now see working}
Checkpoints:    {IDs}, each one commit on {repo} b{UU}.{chunk}-{slug} (not pushed)
Tests:          {N} passed, on {where}; each checkpoint's new test fails without it
Parity:         {no differences / N kept, listed / no screens}
Tests checked:  {yes, N breaks caught / no}
Decided myself: {every item, by checkpoint, with its reason / none}
Plan changes:   {asked and agreed, decided, or none}
Promoted:       {the close: N rows, to which documents / —}
Acceptance:     {the chunk's steps, run: result}
Time taken:     {minutes}, of which {minutes} waiting on the PO
Changed:        the plan, delivery/index.md (not committed)
                {or, in worklog mode: entries {IDs} in worklogs/{name}.md}
Needs the PO:   {anything left open, or none}
─────────────────────────────────────────
Next: the developer reviews the commits and their Built blocks; the PO
reads "Decided myself" and runs the acceptance steps; then the chunk's
branch is pushed and its pull request opened into b{UU}. Then
/build-checkpoint {CODE} {NEXT_CHUNK_FIRST_ID}
```
