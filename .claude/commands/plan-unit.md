# Plan Unit

Plans the build of one unit (usually one approved design group) and writes it as one living document, `projects/{CODE}/delivery/units/b{UU}-{slug}.md`, from `projects/TEMPLATE/delivery/units/unit.template.md` (METHOD.md section 8.2). The plan holds every implementation decision and the build order, for the PO, the developer and the AI that builds it. `/build-checkpoint` builds it one checkpoint at a time.

The command takes every decision itself and writes each as question, answer and why, so the reviewers check decisions instead of filling gaps. It stops for the PO only on a change to the approved design, content for something the design implies but doesn't draw, and a product question the documents don't answer. It writes no code.

It is the only delivery step that reads the wider documents. Everything the build needs is copied into the plan (principle 7), so `/build-checkpoint` reads the plan and the code. Plans are written two or three units ahead, in build order, each reviewed before the next is planned; planning several in one session shares the common reads.

In worklog mode (METHOD.md section 10), the new plan is written directly, since it is a new Draft file; every other change (the build index, the decision log, another plan) is an entry in the planner's own worklog, unless the planner holds the hold.

---

## Arguments received

`$ARGUMENTS`

---

## Step 1: Parse arguments

Required: a project code and a unit.

- **Project code:** the folder under `projects/`, e.g. `ASCENDRA-PAY-004`.
- **Unit:** a design group number from the screen map (`3`, `G03`), or a named unit from the build index (`B-SPINE`).

If anything is missing, ask for all of it in one message:

> "To plan a unit I need:
> 1. **Project code:** the folder under `projects/`
> 2. **Unit:** a design group from the screen map (e.g. `G03`), or a named unit from the build index (e.g. `B-SPINE`)"

**A named unit** with no row in the build index yet: ask, in the same message, what it builds (one or two sentences), which Requirements areas and capabilities it covers, and whether it builds any screens.

Wait for the PO's response.

---

## Step 2: Gate check

| Check | How | If it fails |
|---|---|---|
| Project exists | `projects/{CODE}/` exists | > "There's no project `{CODE}` under `projects/`." |
| Writing mode | The project's `CLAUDE.md` has a Worklog mode section or not. If it has: `git config user.name` is one of its people, and `worklogs/hold.md` names who holds the hold. **Direct:** no worklog mode, or the hold is yours. **Worklog:** otherwise | > "Your git user, {NAME}, isn't one of `{CODE}`'s people in worklog mode, so I don't know whose worklog to write. Add yourself, or set `git config user.name`." |
| Design approved | For a design group: its status in the screen map's groups table is Approved | > "G{GG} isn't approved in the screen map. Plan it once its design is approved, or tell me to continue anyway." |
| Not already planned | No `delivery/units/b{UU}-*.md` exists | > "B{UU} already has a plan: `{path}` ({status}). Re-plan it from the start, or change it? A change to an Approved plan is a new version and needs approval again." In worklog mode, either needs the hold: > "That changes a shared document. Take the hold first, or I'll write the changes as entries in your worklog." |
| Earlier units planned | Every unit this one depends on (the screen map's group table, the build index) has a plan | > "B{UU} depends on {UNITS}, which have no plan yet, so this plan would have to guess what they build. Plan them first, or tell me to continue anyway." |
| Current documents exist | `architecture/architecture.md`, `requirements/requirements.md`, `experience/screen-map.md`, `experience/design-rules.md` exist; the stack profile the project's `CLAUDE.md` names exists under `stacks/` | > "A plan needs {MISSING}. Without it the plan would invent what it should copy." |

On any failure, show the message and wait.

---

## Step 3: Read

Read in this order. Where the project's `CLAUDE.md` names a different path or a narrower part, the project wins. Read parts wherever this list says so: a document read in full costs attention for little the unit uses. Note what you read, by name, for the plan's Sources line.

**Method and template**
1. `projects/TEMPLATE/delivery/units/unit.template.md`, in full: the structure, every guide, the block library.
2. The stack profile's `README.md`, in full: its "considered for every unit" rows and the standard behaviour a plan copies.

**The project**
3. `projects/{CODE}/CLAUDE.md`: project rules, sources, and its row for `/plan-unit`.
4. `STATUS.md`: what is in progress now.
5. `delivery/index.md`: the build order, this unit's row, what it carries from other groups, coverage.
6. `experience/screen-map.md`, in parts: the unit's group section, the groups table (depends on, size, parts), navigation, and coverage rows for the unit's capabilities.
7. `experience/design-rules.md`, in full: every rule the screens follow.
8. `experience/sample-data.md`, in parts: the moment, the customers, the people and the records the group's artboards show.
9. **The group's artboards**, in full, found as the design runbook (`runbooks/design.md`) says: every surface's parts, states, text and sample values. Quote each artboard by its exact title, `SCR-NNN Screen name · state`. If the group's artboards don't carry that form, stop and ask the PO to retitle them first.
10. `requirements/requirements.md`, in parts: the language in full (binding); the domain areas for this unit's records (capabilities, lifecycles, rules, validations); rules across areas; the variations, scenarios and acceptance rows naming its records; its open questions.
11. `charter/charter.md`, in parts: the unit's capability rows (target, stage) and the quality floor.
12. `architecture/architecture.md`, in parts: §1 (shape), §3 (modules, kinds, layers, folders: the only source of module names and file paths), §4 (each command's consistency row), §6 (data, tenancy, security) and §10 (not decided yet); §2, §5, §7, §8 and §9 only where the unit touches a foundation, an integration, an event, scale or a seam.
13. `standards/conventions.md` and `standards/patterns.md`, in full.
14. `decisions.md`, in parts: every decision naming this group, its screens, its records or its modules. A decision wins over an older document text it changed; if it seems to contradict a current document, say so in Step 7 rather than choose.
15. **Earlier plans** in `delivery/units/`, in parts: the header, A0, A2's surface table, A4, A6, A7, A11 and C3. They say what exists, which IDs are taken, and what was moved into this unit. Closed plans: C3 only, unless a row names this unit.
16. **In worklog mode:** every worklog's unapplied entries on any document above. With the documents, they are the current state; note which entries the plan relies on.
17. **The stack profile**, in full: `api-contract.md`, `validation-messages.md`, `ui/usage-rules.md` (a unit with screens), `commons.md` (a unit that adopts or changes a shared module).
18. **Sources** (`sources/`): only where the project's `CLAUDE.md` maps a part to this command, or a current document cites one for a fact the unit needs. Never the Blueprint in full.

Then look at the code in each repository Architecture §1 names, **on its main line**: its latest commit, the modules and tables that exist, endpoints, screens, shared parts already extracted, the seed. Record what already exists in the unit's scope. Unmerged branches are reference only; lift code from one only as a decision, listed in A13.

---

## Step 4: Walk the design

Before deciding anything, list what the artboards contain. Write no file yet.

1. **Surfaces:** every page, table, card, menu, sheet, drawer, form, dialog, tab and email. Give each an ID by the template's scheme, extending surfaces earlier plans built (keeping their ID). Notices, toasts and states belong to their surface and get no ID. A surface fitting no kind is a decision (a new kind and its block), never a quiet invention.
2. **Actions:** every button and menu item, and what it does.
3. **Values:** every column, field, fact and figure a person sees.
4. **Text:** every title, subtitle, message, notice, hint and toast, copied exactly. Every number in the text (days, limits, amounts) is a setting or a constant.
5. **Implied but not drawn:** every drawn item whose result has no artboard.
6. **Not buildable yet:** every surface or value that needs a module, provider or unit not yet built.

---

## Step 5: Decide

Take every decision, in the order a reviewer meets them. A fact goes into its table with a short source stamp (`RULE-017`, `DEC-131`, `Architecture §4`, `SCR-093`, `AUI-033`, `Rule 8`); a choice with a real alternative goes into A14 as question, answer and why, naming the alternative rejected. Never leave a decision to the reader. Never decide again what a current document or the decision log already settled: copy it with its stamp.

**Part A, once for the unit**

1. **Standard behaviour (A0):** from the stack profile's standard behaviour and `standards/conventions.md`, what holds for every page, form, endpoint, read and module block in this unit.
2. **Considered for every unit (A1):** answer the method's list and the stack profile's rows. Keep the ones that apply.
3. **Scope (A2):** what is built, deferred (only when it needs an outside setup that isn't ready, work the build index places later, or a unit not yet built) and implied. A designed surface is built whole; only the part later work feeds is deferred, showing its true current state, or disabled with its reason, or hidden (a whole page or navigation item only). Name the seam and its honest default.
4. **Rules (A3):** every rule, its code (`RECORD_CONDITION`), its statement, every layer that enforces it, and its source: the `RULE` or `VAL` in Requirements, or the plan decision when no business rule is behind it. A business rule the unit settles for the first time gets a C2 promotion row. Field validation messages follow the stack profile's message rules.
5. **Actions and roles (A4).**
6. **Lifecycle and allowed actions (A5):** the server computes allowed actions and reasons and sends them with each record; the screen only shows them.
7. **Modules (A6):** every module touched, by its Architecture name, kinds, layer and folder. A module Architecture lacks is a decision and a C2 row. Every call between modules is a direct call allowed by the layers, a critical event, an after-commit event or a registration, as Architecture §4 says.
8. **Data (A7):** tables and the source of every displayed value. A value held by a provider or another module is fetched once per request at most, never per row.
9. **Settings and constants (A8).**
10. **What each command causes (A9):** the only place each command's effects are written. Copy the transaction and event shape from Architecture §4.
11. **Seed (A10):** from the sample data; a record for every state the artboards show. A record the sample data lacks is added there first (a C2 row).
12. **Patterns and parts (A11):** reuse, extract now (the design shows it twice or more) or one-off, by `PAT` ID. An extraction gets a C2 row.
13. **Differs from the design (A12):** every difference and proposal, agreed in Step 7. The exact text goes in the block where it shows.
14. **Reconciled (A13):** where code or documents disagree with the approved design, and the fix.

**Part B, per surface, from the surface down**

15. **Each surface:** its block from the library, stating only what differs from A0. A list is small (loaded whole; state its limit) or large (served in pages; every saved query, filter and server line). Every response a form or dialog handles, except those A0 covers, with exact text, where it shows and what refreshes. A screen reads only what the error contract allows.
16. **Data:** every list a page shows has its own query, even one returning fixed values today. Per page, one combined query or parallel ones (parallel when the surfaces make sense apart). Query keys and client functions are named from the query and action IDs; files sit in the owning module's folder.
17. **Endpoints, module, migrations, and any other block** (email, money command, approval, import, job, document, report, public page, cross-tenant). Endpoint blocks point to A9 for effects.

**The cut**

18. **Checkpoints:** one visible behaviour each, from the surface down to the database, each building only on earlier ones: the list first, then what creates records, then what changes them. One with nothing to see stands alone only when it is security- or data-critical. Split past about 600 changed lines (not counting tests, generated files and migrations) by surface or action, never by layer.
19. **Chunks:** 2 to 5, each a capability a named role can use, with a one-sentence demo and the PO's acceptance steps.
20. **Verification:** per checkpoint, tests in plain language, named by rule code, at least one failing without the change; the developer's check; design parity. At the close: demo, regression, financial checks, manual steps, and C2's promotions.

---

## Step 6: Check the decisions

Before asking the PO anything, run the plan checks below against the decisions as if the plan were written. Fix every failure you can decide yourself. Every failure needing the PO joins Step 7's list. Nothing is written yet: everything needing the PO is found first and asked at once.

**Plan checks**

*Content*
- A1: the method's list and the stack profile's rows were all answered; the plan shows the ones that apply and names the rest in one line.
- A2: every surface on the artboards is built, deferred or implied, with an ID and a checkpoint; no notice, toast or state has an ID; no new kind or block without a decision.
- A2: nothing deferred shows a placeholder or sample data.
- A3: every rule has a code, a statement, where it is enforced, a source (`RULE`, `VAL` or a decision) and a test; every field rule has its exact message.
- A4: every action has its roles, surfaces and endpoint. A5: every status has its allowed actions; every refusal its exact text and where it shows.
- A6: every module is named as Architecture names it, with kinds, layer and folder; every call between modules is one of the four kinds.
- A7: every displayed value has a source; every value not available yet is deferred. A8: every number in the artboards' text has an owner.
- A9: every command that changes data has a row with every cell answered; no module calls audit or notification directly.
- A10: every artboard state has a seed record from the sample data. A12: every difference has the PO's agreement. A14: every decision names the alternative rejected.
- Part B: every checkpoint is one visible behaviour, within the size limit, with every block it needs; every list has its own query; every surface that loads has its states; every server list has every query, filter and server line; every chunk has acceptance steps.
- C2: every rule, pattern, convention, sample record, module or foundation first settled here has a promotion row.

*Writing (METHOD.md section 8.2)*
- Complete in itself: no fact given only as a link or a stamp; no "TBD", "to be decided" or "the builder decides".
- One fact, one place: effects only in A9; screen text only in its block (A12 says what differs and why); artboard titles only in A2 and the parity lines; a rule's statement only in A3.
- Defaults once: no block row repeats A0.
- Small blocks are sentences.
- Names and IDs: artboards by exact title; a repeat's answer says what happens, then the key or "no key"; client functions and query keys carry their action or query ID; every file path is inside its module's folder; roles, not people, except seed data.
- The header's Sources and Versions are one line each; no checklist or working notes are left in the plan.
- Length in check: Part A and each checkpoint section are as short as their decisions allow, with no fact the build doesn't need and nothing said twice. A part that runs long has its one-line reason in the header's Length line; there is no fixed limit (MD-002).

---

## Step 7: Ask the PO

Show, in one message, only what the command may not decide alone, numbered, each with a recommendation:

- **Design changes:** every A12 difference, including what shows for each deferred item.
- **Implied but not drawn:** the proposed content of each (title, text, buttons, outcome).
- **Product questions:** anything the build needs that no document answers and that changes what a person sees or what the product does. A technical choice isn't a product question: decide it in A14.
- **Conflicts:** a decision that seems to contradict a current document, and any reconcile item whose fix changes approved behaviour.

Then, for information, the plan at a glance:

> "Before I write the plan for B{UU}, these need your decision:
>
> **Design changes**
> 1. [Artboard title: what's drawn, what would be built, why.] My recommendation: [answer].
>
> **Implied but not drawn**
> 2. [Surface, implied by …: proposed title, text, buttons, outcome.] My recommendation: [answer].
>
> **Product questions**
> 3. [Question.] My recommendation: [answer].
>
> **At a glance:** [N] chunks, [M] checkpoints.
> - B{UU}.1 [Chunk]: [demo]. B{UU}.1.1 [goal]; B{UU}.1.2 [goal]
> - Deferred: [items and where they go]
>
> Answer by number, or 'yes to all'. Everything else is decided in the plan, with its reasons, for you and the developer to review."

Ask in plain chat, never through a selection widget. An answer that opens a new question is asked as its own numbered item. If nothing needs a decision, say so, show the glance, and still wait. Write nothing until the PO answers.

---

## Step 8: Write the plan

Write `projects/{CODE}/delivery/units/b{UU}-{slug}.md` from the template; `{slug}` is the unit's name in lower case with hyphens (`b03-staff-and-roles.md`, `b-spine-first-payment.md`).

- Fill every section from Steps 3 to 7 and the PO's answers. Each agreed design change goes in A12 with the PO's agreement and date.
- Copy into each checkpoint the blocks it needs, filled, stating only what differs from A0. Remove the block library and every guide.
- Status: `Draft`. Every checkpoint: `Planned`. No Built blocks. Header: Code base with today's date and each repository's latest main-line commit; Sources and Versions one line each.
- C3 starts with any finding an earlier plan sent to this unit.
- Run the plan checks again on the written plan and fix every failure. A check that still needs the PO means Step 6 missed it: ask it as in Step 7 before finishing. Record "Plan checks: passed, v1.0, {date}" in Part C.

---

## Step 9: Update the project

1. **`delivery/index.md`:** add or update the unit's row (plan, `Draft`, `0 of N built`).
2. **`decisions.md`:** a `DEC` entry recording the plan written and what the PO decided in Step 7, with the link.
3. **C2 promotions** are applied at the unit's close by `/build-checkpoint`, not now. A Step 7 answer that changes a current document now (the screen map, design rules, sample data, Requirements) is applied now, with its own `DEC` entry.

**In worklog mode, unless you hold the hold:** items 1 to 3 are entries in your own worklog, in the shape of `projects/TEMPLATE/worklogs/worklog.template.md`: a **New file** entry for the plan with the index row, a **Decision** entry with a provisional ID, and a **Change** entry for each current document a Step 7 answer changes, marked Urgent if someone is building the affected unit now. Commit and push the plan and your worklog together, so the other person sees both. Otherwise commit nothing unless the PO asks.

---

## Step 10: Report

```
UNIT PLANNED: {CODE} B{UU} {Name}
─────────────────────────────────────────
Plan:          delivery/units/b{UU}-{slug}.md (Draft)
Chunks:        {N}
Checkpoints:   {M}, plus the close
Decisions:     {N} in A14; {N} design changes agreed
Deferred:      {items and where they go, or none}
Reconciled:    {N}, fixed in {checkpoints}
Promotions:    {N} rows in C2, applied at the close
Also changed:  delivery/index.md, decisions.md{, other documents}
               {or, in worklog mode: entries {IDs} in worklogs/{name}.md}
─────────────────────────────────────────
Next: the PO and the developer review the plan and set it Approved,
then /build-checkpoint {CODE} B{UU}.1.1
```
