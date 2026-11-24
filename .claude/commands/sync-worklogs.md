# Sync Worklogs

Brings every person's worklog into the project's current documents in one session, for a project in worklog mode (METHOD.md section 10), where two or more people work at once and each writes only their own `worklogs/{name}.md`. It reads every unapplied entry, finds what clashes (the same section edited twice, two answers to one question, a decision that undoes someone's built work, an entry written against a document that has since moved), puts every conflict to the PO at once with a recommendation, then applies everything: decisions into the decision log as `DEC` entries, Built blocks and statuses into plans, changes, findings and new files into their documents, the build index recounted and the status page rewritten.

Run it at each chunk's acceptance, and early for any entry marked Urgent. Only the PO runs it to apply; anyone may run it as `check`, which reports and writes nothing. It commits nothing: it proposes the commit, and after it is pushed every person pulls before writing again.

---

## Arguments received

`$ARGUMENTS`

---

## Step 1: Parse arguments

Required: a project code. Optional: the mode, `apply` (default) or `check` (report what would apply and every conflict, write nothing).

If the project code is missing:

> "Which project are these worklogs for? Give its folder under `projects/`."

Wait for the PO's response.

---

## Step 2: Gate check

| Check | How | If it fails |
|---|---|---|
| Worklog mode is on | The project's `CLAUDE.md` has a Worklog mode section naming each person, git user, worklog, ID prefix and who runs the sync | > "`{CODE}` isn't in worklog mode, so every document is edited directly and there's nothing to sync." |
| Up to date | `git fetch`; the project repository has no uncommitted changes outside the worklogs and isn't behind its remote | > "The project repository isn't up to date: {what}. Pull, and commit or set aside other changes, so the sync sees every person's latest worklog." |
| The sync owner, for `apply` | `git config user.name` is the person who runs the sync | > "Only {owner} applies a sync, so the current documents keep one author. Run `/sync-worklogs {CODE} check` to see the conflicts without writing anything." |
| Something to sync | At least one worklog has an entry | > "Every worklog is empty: nothing to sync." |
| The hold, for `apply` | `worklogs/hold.md` is empty or names the sync owner | > "{PERSON} holds the hold since {WHEN} ({WHY}), so they may be editing the documents now. Ask them to release it, then run the sync." |

On any failure, show the message and wait.

For `apply`, if the hold is free, take it before reading: write the sync owner, the time and "sync" in `worklogs/hold.md`, and ask:

> "I've taken the hold for the sync. Shall I commit and push `worklogs/hold.md` now, so nobody edits the documents meanwhile?"

Commit and push only that file, and only if the PO says so. Edits made directly by whoever held the hold since the last sync are part of the current documents: the stale-base check compares every entry against them.

---

## Step 3: Read

1. `projects/{CODE}/CLAUDE.md`: its Worklog mode section, and its project rules.
2. `projects/TEMPLATE/worklogs/worklog.template.md`: the entry shape and the closed list of kinds.
3. **Every worklog**, in full.
4. **Each target, in parts:** the section each entry names, as it is now. For a plan, also its header (Status, Versions) and B0; for `decisions.md`, its last number and any rows a Decision entry touches.
5. **What moved:** `git log` on each target since the commit in the entry's "Based on", and the diff of the named section.
6. **Each Built entry:** the commit it names exists in the code repository (`git cat-file -e`), and the checkpoint's Status and Depends on in the plan.
7. `delivery/index.md` and `STATUS.md`, which the sync refreshes.

Don't read whole documents no entry targets.

---

## Step 4: Check every entry

Give each entry one result:

- **Applies:** well formed, its target unchanged since "Based on" (or changed elsewhere in the document only), and no check below fails.
- **Needs the PO:** it changes an approved document (a plan after approval, an approved design, Requirements, Architecture), or it is a Question.
- **Conflict:** any check below.
- **Back to the author:** it can't be applied as written (no target, no base, no exact text, a kind outside the list). Never guessed at.

Run every check on every entry; Urgent entries first.

1. **Same section, two people:** entries from different worklogs change the same section of the same document.
2. **Same question, two answers:** two Decision entries answer one question differently, or one's answer makes another's untrue. Read for meaning, not wording.
3. **Against what is settled:** it contradicts a `DEC`, an approved design, Requirements or Architecture without saying it replaces it.
4. **Against built work:** it changes something a Built entry or a Built block relies on (an endpoint, a table, a rule, a text). Name the code that would have to change.
5. **Stale base:** the target section changed since "Based on". Say whether the entry still makes sense on the new text.
6. **Order:** a Built entry whose checkpoint depends on one not yet built; a commit that doesn't exist; a checkpoint already Built.
7. **Dangling reference:** it depends on an entry that is in conflict or returned.

---

## Step 5: Ask the PO, at once

In one message: every conflict, every entry needing the PO, every entry going back to its author, each numbered with a recommendation; then what applies cleanly, by document.

> "Sync of {CODE}: {N} entries from {people}.
>
> **Conflicts**
> 1. [Z-3 and A-5, `delivery/units/b03-staff-and-roles.md` › A14: Z-3 answers '…', A-5 answers '…'. B03.1.2 is built on A-5.] My recommendation: [keep A-5; Z-3 becomes a Change for the next chunk].
>
> **Needs your decision**
> 2. [A-7 changes the approved plan's B03.2.2 form: …] My recommendation: [approve].
>
> **Back to the author**
> 3. [A-9: no exact text to replace.] It stays in the developer's worklog to fix.
>
> **Applies cleanly:** [N] entries
> - `decisions.md`: Z-1, A-2 (become DEC-200, DEC-201)
> - `delivery/units/b03-staff-and-roles.md`: Built B03.1.1, B03.1.2 (A-1, A-3)
> - `delivery/index.md`: recount
>
> Answer by number, or 'yes to all'. Where a conflict touches another person's work, they should be here, or agree first."

Ask in plain chat. A resolution can turn an entry into another (a Decision into a Change, two into one): note it and apply the result. An answer that opens a new question is its own numbered item.

In `check` mode, show the message without its last line, then go to Step 8's report and write nothing.

Then confirm:

> "I'll apply {N} entries to {M} documents and record {K} decisions as DEC-{first} to DEC-{last}, then empty the applied entries from each worklog. Right?"

Write nothing until the PO confirms.

---

## Step 6: Apply

In this order, so each step can refer to what earlier ones wrote:

1. **Decisions:** in worklog order, oldest first across people, from the log's next `DEC` number, each in the log's two-line form, "By" naming the person and the provisional ID ("the developer, A-14, through the sync"). Keep the map of provisional IDs to `DEC` numbers.
2. **Rewrite references:** in every entry still to apply, each provisional ID becomes its `DEC`.
3. **New files:** each added to the index it names.
4. **Status:** plans approved, chunks accepted, units closed, in each document's own place (a plan's header and B0, the build index).
5. **Built:** each Built block directly above its checkpoint's Status line, set to Built, in checkpoint order.
6. **Changes:** each exact edit, in place. A change to an approved plan adds one line to its header's Versions, with the reason in A14 or A12. A change to another current document adds a row to its change log naming the `DEC`.
7. **Findings:** into the target unit's C3, or the document named.
8. **Recount:** `delivery/index.md` (checkpoints built, statuses), then rewrite `STATUS.md` as a fresh one-page snapshot (Now, Next, Loose ends, Code).
9. **Record the sync:** one `DEC` entry: the date, who took part, the number of entries applied, the `DEC` range, and the entries returned.

Then **empty each worklog**: remove exactly the entries applied or turned into others; keep any added after Step 2's pull and any returned to its author; set Last sync to today; leave Next ID as it is.

Never apply an entry in conflict or returned. Never edit text outside an entry's target.

---

## Step 7: Release and commit

Empty `worklogs/hold.md`, unless the PO keeps the hold to go on working directly. Commit nothing; propose:

> "The sync is applied and the hold released. Proposed commit: `Sync worklogs {date}: {N} entries, DEC-{first} to DEC-{last}`. Commit and push it; then each person pulls before writing to their worklog again."

Commit and push only if the PO says so.

---

## Step 8: Report

```
WORKLOGS SYNCED: {CODE}
─────────────────────────────────────────
Mode:          {apply | check}
Entries:       {N} read from {people}
Applied:       {N} ({by kind})
Decisions:     DEC-{first} to DEC-{last}; the sync is DEC-{n}
Conflicts:     {N}, resolved: {list, or none}
Returned:      {entry IDs and authors, or none}
Documents:     {each document changed}
Worklogs:      emptied of {N}; {N} kept
─────────────────────────────────────────
Next: commit and push, then every person pulls. The next sync is at
the next chunk's acceptance, or sooner for an Urgent entry.
```

In `check` mode, "Applied" reads "Would apply", "Documents" lists what would change, and nothing is written.
