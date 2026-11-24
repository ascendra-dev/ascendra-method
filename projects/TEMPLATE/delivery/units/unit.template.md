# Build Plan: [B03] [Unit name]

> **Guide.** Living until the unit's close, then frozen (METHOD.md section 6). One unit's plan: every implementation decision and the build order, for three readers at once: the PO (requirements, screens, rules, the demo), the developer (patterns, contracts, data, order) and the AI that builds it. Nothing is written for the AI alone.
>
> Written by `/plan-unit`, approved by the PO and the developer, built by `/build-checkpoint` one checkpoint at a time, closed at `B{UU}.X`. How a plan is written is METHOD.md section 8.2: complete in itself; each fact once; defaults stated once in A0; small blocks as sentences; the planner's working material kept out.
>
> - **Part A, the unit:** decided once for the whole unit, read before any checkpoint.
> - **Part B, the checkpoints:** in build order, grouped into chunks. With Part A, each checkpoint is everything needed to build, test and review it.
> - **Part C, the close:** the demo, regression, deploy, promotion and findings.
> - **Block library:** the blocks checkpoints are built from. Copied into checkpoints as needed, then removed from the written plan.
>
> **Every plan:**
> - **Copies, never points away.** A fact the build needs is written here, with a short source stamp: `(RULE-017)`, `(DEC-131)`, `(Architecture §4)`, `(SCR-093)`. A stamp says where it came from; the fact itself is here.
> - **Decides.** A choice with a real alternative is a decision in A15 (question, answer, why), and a table cell points to it (`D4`). No "TBD", no "the builder decides".
> - **Doesn't change the design silently.** What differs from the approved artboards, or what they imply but don't draw, is a proposal in A13, agreed by the PO before the plan is written.
> - **Follows the project's Architecture and the stack profile.** Modules, folders, layers, how effects travel (audit, events, emails), the error contract and the naming of migrations come from there, copied into A0, A7 and the blocks. A plan names no module the Architecture doesn't list; a new one is a decision here and a promotion at the close.
> - **Shows nothing fake.** A part that later work feeds shows its true current state (real data, even if empty) or is disabled with its reason. Only a whole page or navigation item of unbuilt work is hidden.
> - **Names roles, not people,** except the sample people of the seed.
> - **Names artboards by their exact title**, `SCR-NNN Screen name · state`.
>
> **When `/build-checkpoint` reads it:** the header, Part A in full, the checkpoint being built (or Part C), and the Built blocks of earlier checkpoints in the same chunk. The checkpoint's blocks are binding. It writes that checkpoint's Built block and Status, and appends to C2 and C3. A wrong or missing fact stops the build for the PO and is never filled in from other sources; an agreed change is made in place, with its reason in A14 or A12 and one line in Versions. The close applies C2 to the current documents and freezes the plan.
>
> **File:** `projects/{CODE}/delivery/units/b{UU}-{slug}.md`, where `{UU}` is the design group's number (`b03-staff-and-roles.md`) or the unit's short name (`b-spine-first-payment.md`).

**Status:** Draft

| Field | Value |
|---|---|
| Unit | [B03 Unit name: one sentence on what it delivers] |
| Design | [Design group and screens (SCR IDs), where the artboards are, the decision that approved them] |
| Depends on | [Units that must be closed first, or "none"] |
| Reviewers | PO: requirements, screens, rules, demo. Developer: patterns, contracts, data, order |
| Code base | [Each repository, its main line and the commit the plan was reconciled against] |
| Design parity | [How an artboard and the built page are rendered and compared: clock, seed, size] |
| Size | [N chunks, N checkpoints, and the close] |
| Sources | [One line: the documents and code read, by name] |
| Versions | [1.0 YYYY-MM-DD: first plan (DEC-NNN). One line per later version: what changed, and the decision] |

---

# Part A: the unit

## A0. Standard behaviour

> **Guide.** What holds for every surface and endpoint in this unit, copied from the project's `standards/conventions.md` and the stack profile. A block states a row only when its answer differs from this list. Keep it to what really is the same everywhere.

| Where | Standard |
|---|---|
| Every page | [Who can't open it sees …; no header actions, notices or tabs unless the block lists them] |
| Every form | [Submit disabled while sending; how a validation response shows; hints bare unless stated] |
| Every endpoint | [Path prefix; where controllers live; tenancy; the standard error responses; idempotency rule] |
| Every read | [No audit, events or emails; a repeat returns the same] |
| Every module block | [The module's kind, layer and folder, when the unit has one main module] |

## A1. Considered for every unit

> **Guide.** `/plan-unit` answers the method's list below and the stack profile's own list for every unit. The plan shows only the rows that apply, each pointing to where it is handled, then one line naming every row that doesn't.
>
> The method's list: small list loaded whole; large list from the server; form or wizard; actions allowed by status and role; status lifecycle; approval by a second person; money or another critical record written in one transaction; import from a file; preview then commit; background job; printed document; email or notification; report or figures; public page without sign-in; operator or cross-tenant access; a setting that changes behaviour; fixed values; outside provider; depends on later work; implied but not drawn; differs from the design; new module or contract function; a call between modules; a shared or copied module changed; new or changed table, and existing rows; reference numbers; repeating a command; two people changing one record; audit entries; events after the commit; a pattern extracted; seed additions; phone width.

| Concern | Applies: where |
|---|---|
| [Concern] | [How, and the section or checkpoint] |

Not applicable: [concerns, comma-separated].

## A2. Scope

> **Guide.** Walk every artboard of the unit. Every page, table, card, menu, sheet, drawer, form, dialog, tab and email is a surface with an ID and the checkpoint that builds it. Notices, toasts and states are not surfaces: they are described inside their surface. Nothing may be missing from all three tables.
>
> **IDs:** surfaces `<kind>.<record>.<purpose>` in kebab-case (kinds: `page`, `table`, `query`, `form`, `dialog`, `sheet`, `drawer`, `tab`, `menu`, `card`), carried in code as the root element's test ID. A client function is named after its action (`staff.change-roles` → `staffChangeRoles`); a query key after its query ID. Endpoints by action plus method and path; contract functions `module.function`; rule codes `RECORD_CONDITION`; events past tense. A new surface kind or block is a decision in A15 and a promotion at the close.

**Surfaces:**

| Surface | Kind | Screen | Artboards | What it is | Built in |
|---|---|---|---|---|---|
| [page.record.purpose] | [Kind] | [SCR-NNN] | [Exact artboard titles] | [One line] | [B03.1.1] |

**Deferred: depends on later work.**

| What | Why it waits | Shown meanwhile | Wired by |
|---|---|---|---|
| [Item] | [Needs …] | [True current state / disabled with "exact reason" / hidden (whole page or navigation item only)] | [Unit, and the seam it replaces] |

**Implied but not drawn.**

| What | Implied by | Proposal | Built in |
|---|---|---|---|
| [Surface] | [The drawn item that needs it] | [P1] | [B03.1.1] |

## A3. Rules

> **Guide.** Every rule the unit enforces, decided once, because one rule drives several surfaces. The API always enforces it; the screen may only hint; the database enforces it where it can. Source: the `RULE` or `VAL` in Requirements, or the plan decision when no business rule is behind the code. A business rule first settled here gets its `RULE` at the close.
>
> Correct: "`STAFF_LAST_ADMIN` | The school always keeps at least one active School admin | API: change roles, deactivate. Screen: menu note, Sheet notice. Database: none, it spans rows | `menu.staff.row`, `sheet.staff.member` | RULE-031 | `STAFF_LAST_ADMIN: two admins removing each other leave one`"
> Incorrect: "Admin rule | Can't remove admin | Backend"

| Code | Rule | Enforced in | Surfaces | Source | Test |
|---|---|---|---|---|---|
| [RECORD_CONDITION] | [Statement] | [API; screen hint; database] | [Surfaces] | [RULE-NNN / VAL-NNN / D4] | [Test name] |

**Field validation** (messages from the stack profile's message rules, required first):

| Form › field | Rule | Message |
|---|---|---|
| [form.record.purpose › field] | [Required / format / length / range] | [Exact message] |

## A4. Actions and roles

> **Guide.** Every action, who may take it, where it appears. The action name is the permission the server checks and the endpoint's name. The screen shows what the server allows (A5); it never decides permissions itself.

| Action | What it does | Roles | Surfaces | Endpoint | Second person? |
|---|---|---|---|---|---|
| [record.verb] | [One line] | [Roles] | [Surfaces] | [METHOD /path] | [No / yes: the approval block] |

**Opening the unit's pages:** [Who may open each page, where it sits in navigation, and what everyone else sees]

## A5. Lifecycle and allowed actions

> **Guide.** For each record with a status: every status, its badge, the actions allowed in it and where each leads; then every reason an action isn't allowed, with its exact text. The server computes allowed actions and reasons and sends them with each record. Write "No record in this unit has a status" if so.

**[Record] lifecycle:**

| Status | Badge | Allowed actions | Each action leads to |
|---|---|---|---|
| [Status] | [Label, colour] | [Actions] | [Action → status] |

**When an action isn't allowed:**

| Action | Not allowed when | Code | Text shown | Where |
|---|---|---|---|---|
| [Action] | [Condition] | [CODE] | [Exact text] | [Menu note / Sheet notice / disabled with hint / absent] |

**How allowed actions reach the screen:** [The field on each record, with its type]

## A6. Modules

> **Guide.** Every module the unit touches, including one that only gains configuration, by its name in the Architecture, with its kinds, layer and folder. Every call between modules is a direct contract call (allowed by the layers), a critical event inside the transaction, an after-commit event, or a registration (a higher module registering with a lower one).

| Module | Kind | Layer | Folder | New or existing | Gains in this unit | Contract functions and events |
|---|---|---|---|---|---|---|
| [module] | [From the Architecture] | [Layer] | [Path] | [New / existing] | [Records, rules, endpoints, configuration] | [module.function; events, critical ones marked] |

**Calls between modules:**

| From | To | What for | How |
|---|---|---|---|
| [module] | [module] | [Purpose] | [Direct call / critical event / after-commit event / registration] |

## A7. Data

> **Guide.** Tables in summary (each migration is specified in full in its checkpoint), and where every value a person sees comes from: walk every artboard, because a value from a provider or another module is easy to miss until the build.

**Tables:**

| Table | Module | New or changed | Key columns | Access rules | Migration |
|---|---|---|---|---|---|
| [table] | [module] | [New / adds …] | [Columns] | [Tenancy, grants] | [File, checkpoint] |

**Where each value comes from:**

| Surface › field | Shown as | Source | Fetched by | Available now? |
|---|---|---|---|---|
| [table.record.purpose › column] | [Format] | [Table / provider / other module / calculated] | [Query or contract function] | [Yes / no: deferred] |

## A8. Settings and constants

> **Guide.** Every value that changes behaviour, its owner and how it is loaded. Check the artboards' text for numbers (days, limits, amounts): each has an owner here.

| Value | Kind | Owner | Default | Loaded with | Used by |
|---|---|---|---|---|---|
| [Name] | [Customer setting / platform-fixed / environment] | [Module] | [Value] | [Query, constant or environment] | [Surfaces, rules] |

## A9. What each command causes

> **Guide.** The only place a command's effects are written. Every endpoint that changes data has a row, every cell answered ("none" where nothing happens). Endpoint blocks point here.

| Endpoint | Audit entry | Events after the commit | Emails or notifications | A repeat | Concurrency |
|---|---|---|---|---|---|
| [action] | [What is recorded, inside the transaction] | [EventName, payload] | [SCR-NNN to whom, by which reaction] | [What happens when it arrives twice; key or no key] | [Version check / not needed, why] |

## A10. Seed

> **Guide.** The sample records this unit adds, so every built state compares with its artboard using the same people and figures (the project's sample data). Seeded through the modules' contracts, never by inserting rows. Every artboard state has a record that produces it.

| Records | Produces | Artboards | Through |
|---|---|---|---|
| [What] | [Which state] | [Exact titles] | [Contract function] |

## A11. Patterns and parts

> **Guide.** Every pattern and part used, by its `PAT` ID where it has one: **reuse**, **extract now** (the design shows it twice or more) or **one-off**. Say where an extracted part lives. An extracted pattern is promoted at the close.

| Pattern or part | ID | Decision | Lives in | Used by |
|---|---|---|---|---|
| [Name] | [PAT-NNN or —] | [Reuse / extract now / one-off] | [Path] | [Surfaces] |

## A12. Differs from the design

> **Guide.** Every difference from the approved artboards, and every proposal for something implied but not drawn: what differs and why, with the PO's agreement. The exact text lives in the block where it shows, not here. Deferred items from A2 appear here too.

| # | Where | Differs how | Why | Agreed |
|---|---|---|---|---|
| P1 | [Surface or artboard] | [One line] | [Reason] | [PO, date; DEC-NNN if logged] |

## A13. Reconciled with existing code

> **Guide.** Where existing code or documents disagree with the approved design, and the fix, from reading the main line of each repository. Code lifted from an unmerged branch is listed here and reviewed like new code. Remove a row once a later version of the plan no longer needs it.

| # | What disagrees | Where | Fix | Fixed in |
|---|---|---|---|---|
| 1 | [Description] | [File, table or section] | [Fix] | [B03.1.1] |

## A14. Decisions

> **Guide.** Every choice with a real alternative. The why names the alternative rejected. Nothing the Architecture, design rules or decision log already settled is decided again: it is copied with its stamp.
>
> Correct: "D4 | How does the list get Last sign-in? | Recorded by our module when it first sees a new session | Asking the provider costs a network call per list load and ties the code to it (Architecture §1)"
> Incorrect: "D4 | Last sign-in | From the provider | Simple"

| # | Question | Answer | Why |
|---|---|---|---|
| D1 | [Question] | [Answer] | [Reason, and the alternative rejected] |

---

# Part B: the checkpoints

## B0. Build order

> **Guide.** **A checkpoint is one behaviour a reviewer can see work:** usually one user action, from the surface down to the database, with its tests. A checkpoint with nothing to see stands alone only when it is security- or data-critical, proved through the API. Order: the list that shows records, then the action that creates them, then the actions that change them.
>
> **Chunks:** 2 to 5, each one capability a named role can use, with a one-sentence demo. One pull request per chunk, one commit per checkpoint. **Size:** about 600 changed lines per checkpoint, not counting tests, generated files and migrations; split by surface or action, never by layer.

| Checkpoint | Goal | Surfaces | Endpoints | Depends on | Status |
|---|---|---|---|---|---|
| **B03.1 [Chunk]** | **Demo:** [Role does what] | | | | |
| B03.1.1 | [One sentence] | [Surfaces] | [Actions] | [Checkpoint or unit] | Planned |

## B03.1 [Chunk name]

**Demo:** [Role does what, end to end, in one sentence]

### B03.1.1 [Short title]

> **Guide.** Four lines, then the blocks this checkpoint needs, in this order: screen blocks in the order a person meets them, data, endpoints, module, migration, then any other block (email, money command, approval, import, job, document, report, public link, cross-tenant). Each block states only what differs from A0. Finish with tests, the developer's check and design parity.

**Goal:** [What a reviewer sees working]
**Depends on:** [Checkpoints and units]
**Builds:** [Surfaces, endpoints, migrations, parts]
**Rules:** [Codes from A3]

[Blocks]

**Tests**

| Test | Level | Proves |
|---|---|---|
| [CODE: behaviour, or surface: behaviour] | [Unit / database / API / browser] | [Decision or rule, when not obvious] |

**Developer's check:** (1) [Step]: [expected]. (2) …

**Design parity:** [Exact artboard titles compared, and the A12 differences each shows]

**Status:** Planned

---

**B03.1 acceptance (PO)**

> **Guide.** The steps the PO follows on the running product before the chunk merges, in the screen's own words, with the seed's people.

| # | Step | Expected | Compare with |
|---|---|---|---|
| 1 | [Sign in as … and do …] | [What shows] | [Exact artboard title, or —] |

---

# Part C: the close

## C1. Close, B03.X

> **Guide.** Built last by `/build-checkpoint`: the demo end to end, the regression of every closed unit, the financial checks where money moved, the merge and the deploy. Then promotion (C2), then the plan is frozen. The PO and the developer sign off.

**Demo:**

| # | Step | Expected |
|---|---|---|
| 1 | [Step] | [Result] |

**Regression:** [Every closed unit's browser tests]
**Financial checks:** [Which, or "no money moves in this unit"]
**Manual steps:** [Settings, seeds, provider changes the PO or developer makes, in order]
**Sign-off:** PO: [date]. Developer: [date].
**Built B03.X:** [Added at the close]

## C2. Promotion

> **Guide.** Everything this unit settled that outlives it, moved into the current documents at the close (METHOD.md section 8.4). Filled as the unit goes; applied at the close.

| Settled here | Goes to | Applied |
|---|---|---|
| [A business rule clarified or first settled: D7] | [Requirements, new RULE-NNN] | [Date, or "not yet"] |
| [An architecture decision, a foundation proved, a convention, a pattern extracted, a design change, a capability delivered] | [Architecture / standards (conventions, patterns) / Experience / build index] | |

## C3. Findings for later units

> **Guide.** What a later unit must act on. `/build-checkpoint` appends here; `/plan-unit` reads this section of every closed plan.

| Found in | What | Goes to |
|---|---|---|
| [Checkpoint] | [Finding] | [Unit or document] |

**Plan checks:** [Passed, version, date. Run by `/plan-unit`; the list is in the command]

---

# Block library

> **Guide.** Removed from the written plan. Copy each block a checkpoint needs and fill it; leave out any row whose answer is A0's. A block with only two or three facts is written as a sentence. Every block names the `PAT` it follows, where one exists. Each surface kind has its block: `page` (with `tab` as its Tabs row), `table` (small list or large list), `card`, `menu`, `sheet`, `drawer`, `form`, `dialog`, and `query` (the data line).

**Page: `[page.record.purpose]`** ([PAT-NNN])

| | |
|---|---|
| Route | [Path and parameters; opened from where] |
| Who | [Action, roles] |
| Recipe | [The design rules' recipe, and anything above or around the header] |
| Title / subtitle | [Exact text] |
| Header actions | [Button text → action → where] |
| Notices | [Exact text, and the rule or setting that shows it] |
| Tabs | [IDs, labels, the default, in the URL or not] |
| Contents | [What sits on the page, in order, with exact card titles and text] |
| States | [Loading, empty, error for each part that loads, when not shown in its own block] |

**Small list: `[table.record.purpose]`** (loaded whole; [PAT-NNN])

| | |
|---|---|
| Why whole | [Reason, and the limit before it must become a large list] |
| Search, filters, sort | [Fields; options and defaults; default order and sortable columns] |
| Selection and bar actions | [If any] |
| Row click, row actions | [Opens what; menu ID] |
| Empty, error, loading | [Exact text; skeleton shape] |

| # | Column | Header | Cell | Format | Frozen | Shown | Source |
|---|---|---|---|---|---|---|---|
| 1 | [key] | [Header] | [Part] | [Format] | [Yes / no] | [Yes / no] | [A7 row] |

**Large list: `[table.record.purpose]`** (served in pages; [PAT-NNN])

> **Guide.** Every saved query is its own row with its own server line. Follow the stack profile's list rules (shape, parameters, export).

| | |
|---|---|
| Why server | [Row count, or filters over data the browser can't hold] |
| Page size, selection, bar actions | [Values] |
| Row click, row actions | [Opens what; menu ID] |
| Export | [Formats, columns, limit; or none] |
| Empty, error, loading | [Exact text] |

Columns as the small list, plus Sortable. Then:

| Query | Label | Shows | Fixed parameters | Default sort | Default? |
|---|---|---|---|---|---|
| [query.record.purpose] | [Label] | [Records] | [Parameters] | [Column, direction] | [Yes / no] |

| Filter field | Type | Operators | Options from |
|---|---|---|---|
| [Field] | [Type] | [Operators] | [List / lookup] |

| Query | Endpoint | Parameters | Filters map to | Index |
|---|---|---|---|---|
| [query.record.purpose] | [METHOD /path] | [Parameters] | [Column or join] | [Index, or none and why] |

**Card: `[card.record.purpose]`:** [Contents copied from the artboard; the query that loads it; its states.]

**Menu: `[menu.record.purpose]`** (opens from [row ⋯ / Sheet header]; [PAT-NNN])

| Item | Action | Shown when | Not allowed when | Reason shown | Opens |
|---|---|---|---|---|---|
| [Text, icon] | [Action] | [Status and role] | [Code from A5] | [Exact text] | [Surface] |

**Sheet: `[sheet.record.purpose]`** (opens from [row click]; [PAT-NNN]): title [exact text]; loaded by [the row, or its own query and why]; [read only, changes from the row's menu].

| Section | Contents | Source |
|---|---|---|
| [Label] | [Fields and exact labels] | [A7 rows] |

Notices: [Exact text, and the rule that shows each.]

**Drawer: `[drawer.record.purpose]`:** opens from [where]; side, width and phone layout [values]; title [exact]; loaded by [query]; contents [sections or the form it holds]; closing [and unsaved input].

**Form: `[form.record.purpose]`** ([kind: page, dialog, sheet or wizard, by the form rule]; [PAT-NNN])

| Field | Label | Control | Required | Hint | Default |
|---|---|---|---|---|---|
| [field] | [Label] | [Control] | [Yes / no] | [Exact hint] | [Value] |

Buttons: [text → endpoint; secondary]. Leaving with changes: [what is asked]. Form-level rules: [rules across fields and their messages, if any].

| Response | Shows | Where | Then |
|---|---|---|---|
| [Success code] | [Exact toast or inline text] | [Toast / inline] | [Goes to; refreshes] |
| [422 CODE] | [Exact text; say when it is the API's message] | [Field / form alert] | [Stays] |
| [Network or server error] | [Exact text, with the reference] | [Form alert] | [Stays] |

> **Guide.** List every response except those A0 already covers. A screen reads only what the stack profile's error contract allows (usually a code and a message); data a refusal needs comes from a query the screen already makes.

**Dialog: `[dialog.record.purpose]`** (opens from [item]; [PAT-NNN]): title [exact]; description [exact]; inputs [none, or the form]; buttons [Cancel; confirm text and style]; calls [endpoint]. Then the response table, as the form's.

**Data:** [Query ID, key, client function, endpoint → response type, file, used by, refetched when; one line per query. Parallel or one combined query, with the decision.]

**Endpoint: `[METHOD /path]`** ([action], [file]; effects: A9)

| | |
|---|---|
| Request | [Type, fields, how input is normalised; what returns a validation error] |
| Response | [Status and type] |
| Rules | [Codes from A3] |
| Other errors | [Only those A0 doesn't cover] |
| Reads | [Settings from A8] |
| Order | [The steps in order, and which share one transaction] |
| Cost | [Rows read and written; the index used] |

```ts
// Only where a shape must be exact
```

**Module:** [Contract functions and what each returns; service methods and the rules they check; data read and written; calls to other modules, by kind.]

**Migration: `[file]`**

| | |
|---|---|
| Creates or changes | [Tables, functions] |
| Access rules | [Tenancy policy, grants] |
| Existing rows | [How they change, when there are any] |
| Reversible | [How, or why not] |

| Column | Type | Null | Default | Constraint |
|---|---|---|---|---|
| [name] | [type] | [Yes / no] | [Value] | [Check, unique, reference] |

Indexes: [each, and the query it serves].

**Email: [SCR-NNN Name]** ([PAT-NNN])

| | |
|---|---|
| Sent | [On which event, by which reaction; to whom] |
| Subject | [Exact text] |
| Body | [Each part, with its exact text and data] |
| Link | [What it opens; how long it works] |
| If it fails | [What happens and where it shows] |

**Money command:** [the command; every record written in one transaction, in order; how it is counted once; what a repeat does and its key; the financial checks; each money rule broken on purpose and the test that must fail.]

**Approval:** [request type; raised by; approved by (never who raised it); on approve; on reject and the reason asked; expiry; shown in; the module that applies it.]

**Import:** [file type, columns and downloadable template; each row check and its message; the preview; all rows or valid rows only, and why; the result shown; in the request or a background job.]

**Background job:** [name; started by; one step's work; progress shown; retry; partial failure; what a repeated step does.]

**Document: [SCR-NNN Name]:** [made when; reference number scheme; each field and its source; kept for how long; reprint rules.]

**Report: [SCR-NNN Name]:** [each figure and exactly how it is calculated; the as-at moment; filters and defaults; export; rows read and how it stays fast.]

**Public page: [SCR-NNN]:** [how the link identifies the record and how long it works; what it shows and never shows; abuse limits; phone layout.]

**Cross-tenant access:** [operator roles; which tenants and how one is chosen; how every access is recorded; what is never allowed.]

**Built block** (added by `/build-checkpoint`, at most ten lines)

**Built B03.1.1** ([date], [time taken]): [result in one sentence]
- Files: [by area]
- Differences from the plan: [each, or "none"]
- Tests: [passed, where; parity result]
- Commit: `[message]`
