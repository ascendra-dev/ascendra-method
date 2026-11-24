# Worklog: [Person]

> **Guide.** Log (METHOD.md section 6), used only in worklog mode (section 10), when two or more people work on one project at once. Each person writes only their own worklog. Current documents (the decision log, the build index, approved plans, the design, Requirements, Architecture) change only through `/sync-worklogs`, which the PO runs at each chunk's acceptance, or by whoever holds the hold (`worklogs/hold.md`). The sync applies the entries and empties this file.
>
> **Writing:** append one entry per change, at the end of Entries. Never edit another person's worklog or a current document. Take the next ID from the header and raise Next ID. "Based on" names the version and commit of the target you read, so the sync can tell if it moved since. An entry that changes what another person is building right now is marked Urgent: tell them, and ask the PO for an early sync.
>
> **Reading:** a target's current state is the document plus every unapplied entry on it, in every worklog.
>
> Not the decision log (a decision here is provisional, `Z-3`, until the sync gives it a `DEC-NNN`), not a diary, not code (code goes through branches and pull requests).

| Field | Value |
|---|---|
| Person | [Name, role] |
| Git user | [`git config user.name`, exactly] |
| ID prefix | [One or two letters, e.g. `Z`] |
| Next ID | [e.g. `Z-1`. IDs never restart] |
| Last sync | [Date and project commit of the last sync, or "none yet"] |

---

## Entries

> **Guide.** One H3 per entry, oldest first: `### {ID} · {Kind} · {YYYY-MM-DD}`, with ` · Urgent` when it is. Then the fields table, then the body for its kind:
>
> | Kind | For | Body |
> |---|---|---|
> | **Decision** | A choice with a real alternative | Question, Answer, Why (naming the alternative rejected) |
> | **Built** | A checkpoint built | The Built block exactly as it goes into the plan, and `Planned → Built` |
> | **Status** | A plan approved, a chunk accepted, a unit closed | From, to, on whose say-so |
> | **Change** | Any other edit to a current document | The exact text to replace and the new text, or the text to add and where |
> | **Finding** | Something a later unit must act on | What, and where it goes |
> | **New file** | A file a current document should list | The path, and the rows to add to which index |
> | **Question** | Something only the PO or the other person can answer first | The question, a recommendation, and the entries waiting on it |
>
> Correct: `A-4 · Change` with Target `delivery/units/b03-staff-and-roles.md › B03.1.1 Endpoint GET /staff, Cost`, Based on `plan v1.0 at 8a65bc8`, and the exact old and new text.
> Incorrect: `A-4 — fixed the index thing in the plan` (no target, no base, no text: the sync can't apply or check it).

### [ID] · [Kind] · [YYYY-MM-DD]

| Field | Value |
|---|---|
| Target | [Document › section, or "new"] |
| Based on | [Target version and project commit read] |
| Summary | [One line] |
| Refs | [Code commits, entry IDs it depends on, the checkpoint or chunk] |
| Affects | [Work in progress it may change, or "none"] |

[Body, by kind]
