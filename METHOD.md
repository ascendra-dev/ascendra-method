# Ascendra Method

| Field | Value |
|---|---|
| Version | 1.4 |
| Date | 2026-11-25 |
| Status | Agreed by the PO, 2026-11-25 |

A method for one product owner and an AI to take a product from an idea to production, with full control over every decision. It works for any domain and any business. Examples in this document come from a fee collection product and are illustrations only.

---

## 1. Philosophy

1. **The PO owns every decision; the AI does the work.** The AI researches, drafts, plans, builds and tests. The PO directs, reviews and approves at defined gates. Nothing important is a guess.
2. **Think the whole milestone through, then design all of it, then build.** A milestone is written as one Blueprint and designed screen by screen before any of it is built. Seeing the whole product early is what makes the build order and the boundaries right.
3. **Build in dependency order.** Things nothing depends on come first; everything after builds on finished, proven work. There is no throwaway skeleton: each foundation is built and proven by the first unit that needs it.
4. **Decisions, not descriptions.** A plan is a list of decisions, each written as question, answer and why, so reviewers check decisions instead of filling gaps.
5. **Small steps you can see work.** Every step of a build is one behaviour a reviewer can watch working end to end, from the screen down to the database.
6. **Current truth is separate from history.** A document either says what is true now or records what happened. Never both. This is what keeps the method maintainable as a project grows.
7. **Settle once.** A current document states each fact in one place. A plan copies what it needs from the current documents, never the other way round.
8. **Documents are written for the AI and the reviewer at once.** Precise, concise, no repetition, no history inside a current document, in plain language a PO or developer can review without decoding.
9. **Prove before encoding.** A command, template or rule is written only after real work has proved it. Until then the work is done by hand with the AI.
10. **Sources first, general knowledge labelled.** The AI works from the project's own sources. General knowledge fills gaps only, marked "[General knowledge: not in source]". Facts from real customers override judgement.

---

## 2. Roles

| Role | Does | Decides |
|---|---|---|
| **PO** (product owner) | Directs, reviews every artifact, accepts each chunk, closes each unit | Scope, requirements, design, every decision the AI may not take alone |
| **Developer** | Reviews plans and each checkpoint's code, runs the manual checks | Implementation detail within an approved plan, raised as findings when it matters |
| **AI** | Researches, drafts, plans, builds, tests, records | Everything it can justify from approved artifacts, written down with its reason; stops for the PO on anything else |

One person may hold the PO and developer roles. When two or more people work at once, worklog mode applies (section 10).

---

## 3. Lifecycle

Five phases. Each ends at a PO gate.

| # | Phase | Produces | Gate |
|---|---|---|---|
| 1 | **Direction** | Research, Roadmap | PO accepts the direction |
| 2 | **Definition** | Blueprint, then Charter, Requirements, Architecture baseline | PO approves the Blueprint; the split is reviewed |
| 3 | **Design** | Experience: screen map with design groups in dependency order, then each group designed | PO approves the screen map, then each design group |
| 4 | **Delivery** | Build index, then for each unit: build plan, chunks, checkpoints, close | PO and developer approve each plan; PO accepts each chunk and closes each unit |
| 5 | **Release** | Each stage released when its exit gates pass | PO confirms the Charter's exit gates |

**Direction** spans milestones: the Roadmap says where the product family is going, which milestones, in what order, and what each must prepare for the next. It changes only when direction changes.

**Definition** is where a milestone is thought through. The Blueprint is written by the PO with the AI, iteratively, until the whole milestone is clear: why, scope, the domain, the experiences, the architecture, the delivery stages. When the PO approves it, its content is split into the Charter, Requirements and the Architecture baseline, and the Blueprint is frozen (section 6.2).

**Design** starts with the screen map: every screen of the milestone, grouped into design groups in dependency order. That order is the first draft of the build order. Each group is then designed in full, at final detail, and approved (section 7). Design is where requirements meet reality: a gap or a contradiction found while drawing is decided by the PO and corrected in Requirements.

**Delivery** follows the build index. Each unit is planned, approved, built checkpoint by checkpoint, accepted chunk by chunk, and closed (section 8).

**Release** is by stage. A milestone may be released in stages, each a commercial commitment with exit gates in the Charter.

Phases overlap where it is safe: plans are written two or three units ahead of building; a design change during delivery goes back through a design decision.

---

## 4. Vocabulary

Agreed with the PO, 2026-11-25. One meaning per word.

| Term | Meaning | ID |
|---|---|---|
| **Product family** | The set of products a Roadmap covers | — |
| **Milestone** | One commercial product release in the Roadmap, planned by one Blueprint | `M1` |
| **Stage** | A release within a milestone, with its own exit gates | `M1.1` |
| **Phase** | A step of this method's lifecycle (section 3). Never "stage" | — |
| **Project** | One milestone's working folder: its artifacts and decisions, and the code repositories it builds in. When a milestone closes, its Architecture, design rules and design system become the starting baseline of the next milestone's project, carried forward once as current truth; Charter and Requirements start fresh from the new Blueprint | `{CODE}` |
| **Capability** | Something the product lets someone do, at a maturity level (Core, Standard, Advanced) | `CAP-001` |
| **Rule** | A business rule that always holds, in business words, with no code or technology. A rule first settled in design or in a plan gets its ID in Requirements at that group's record step or the unit's close | `RULE-001` |
| **Validation** | A check on input or state, with its message | `VAL-001` |
| **Rule code** | A refusal the code returns: the same code in the API's response, the screen's response table and the test name. A plan's rules table cites the `RULE-001` or `VAL-001` behind it, or the plan decision when no business rule is behind it | `RECORD_CONDITION`, for example `STAFF_LAST_ADMIN` |
| **Variation** | A way real customers legitimately differ, and who settles it | `VAR-001` |
| **Scenario** | A named business case, used to test rules and design | `SCN-001` |
| **Open question** | A requirement or fact not yet decided, with its owner. One taken from the Blueprint notes its origin there (for example "Blueprint D06") | `OPEN-001` |
| **Workspace** | Where a kind of user opens the product: a staff workspace, an operator workspace, a public page with no sign-in | — |
| **Output** | What the product prints or sends: documents and emails | — |
| **Screen** | One screen of a workspace, or one output | `SCR-NNN` |
| **Design group** | Screens designed together, in dependency order. A design term only | `G01` |
| **Part** | A slice of a large design group, reviewed on its own. Never "checkpoint" | `G10 part 1` |
| **Flow note** | The short note agreed in chat before a design group or part is drawn: screens, rules shown, states, numbered decisions | — |
| **Artboard** | One drawn state of one screen, titled `SCR-NNN Screen name · state` | — |
| **Design rule** | A settled rule for how screens look and behave, kept once in the design rules | `DR-001` |
| **Unit** | What is planned and built as one: usually one design group; sometimes part of one, or a named unit with no screens (a money spine, a security unit). Parts of another group a unit carries stay inside it, and the build index says so. Build order lives in the build index, never in the ID | `B03` builds `G03`; `B08a`, `B08b` split a group; `B-SPINE` has no group |
| **Build plan** | One unit's plan: its decisions, then its chunks and checkpoints | file `delivery/units/b03-{slug}.md` |
| **Chunk** | A capability a named role can use, demonstrated in one sentence. One pull request into the unit's branch; the PO accepts it before it merges. The unit's branch merges into the main line once, at the close | `B03.1`, `B-SPINE.1` |
| **Checkpoint** | One behaviour a reviewer can see work, from the screen down to the database, with its tests. One commit | `B03.1.1`, `B-SPINE.1.2` |
| **Close** | The unit's last step: final checks, promotion (section 8.4), sign-off | `B03.X`, `B-SPINE.X` |
| **Surface** | A page, form, sheet, dialog, menu, table, card or email that a build plan names and builds | `page.staff.list`, `form.staff.invite` |
| **Pattern** | A settled reusable recipe, screen or full stack | `PAT-001` |
| **Decision** | Something the PO settled that changes a current document | `DEC-001` in the decision log; a project carried over from an earlier log keeps its numbers |
| **Plan decision** | A decision inside one build plan, part of its content | `D1`, and `P1` for a design proposal, local to the plan; cited from outside it with the unit: "B03 D33" |
| **Entry** | A worklog entry, provisional until synced | `{initial}-{n}`, for example `Z-3`; a sync turns a decision entry into a `DEC-NNN` |

A design group and a unit are deliberately different words: design order and build order start the same and then diverge (a unit may be built before an earlier group, or carry another group's parts).

**ID rules:**

- Numbered list IDs have three digits (`RULE-017`, `SCR-093`, `DEC-200`); design groups and units have two (`G03`, `B03`).
- A project adopting this method with IDs of its own swaps the prefix and keeps the number, so nothing is renumbered (`UBR-017` becomes `RULE-017`, `V22` becomes `VAR-022`). Requirements' header lists each old prefix with its new one.
- Frozen documents keep the IDs they were written with.
- Where two old lists would land on the same numbers, the smaller or mostly settled one is renumbered on the move, each item noting its origin.

---

## 5. Artifacts

Every project holds the same artifacts. Each has one job.

### 5.1 Direction

| Artifact | Job | Kind |
|---|---|---|
| **Research** | Evidence: market, regulation, technology, competitors | Frozen per note |
| **Roadmap** | The product family's milestones, principles, module map across milestones, what each milestone prepares for the next | Living, rarely changed |

### 5.2 Milestone

| Artifact | Job | Kind |
|---|---|---|
| **Blueprint** | The milestone thought through, in one document. The source of the three below | Frozen at approval |
| **Charter** | Why the milestone exists, for whom, how it earns; scope as one capability table (capability, Core / Standard / Advanced definition, target, stage, deferred to); the quality floor every capability meets; stages and their exit gates; what is deliberately not built yet; risks | Living, changed only by PO decision |
| **Requirements** | Language and glossary; actors and their limits; for each domain area: purpose, what it owns and doesn't, capabilities, lifecycles, rules, validations, controls; experiences per workspace and output; variations; scenarios; acceptance tests; open questions. Every item has an ID and a status (Proposed, Approved, Deferred) | Living |
| **Architecture** | Shape and deployment; foundations reused and their readiness (each with status and evidence); modules and their packaging; module interactions and consistency (which calls share a transaction, which effects follow as events); integration contracts; data, privacy, tenancy, security; performance and extraction; seams for later milestones with their fitness tests; what is deliberately not decided yet | Living |
| **Standards** | The project's own standards, beside its stack profile's: **conventions** (how code is written in this project, and its deviations from the profile) and **patterns** (settled reusable recipes, screen or full stack) | Living |
| **Experience** | The **screen map** (workspaces and outputs, navigation per role, every screen, design groups in dependency order, parked options); the **design rules** (every settled rule for how screens look and behave, once); the **sample data** every design uses; the designs; the design system | Living; after approval, changed only by decision |
| **Delivery** | The build index: units in build order, each with its status, and the coverage of every capability by a unit; one build plan per unit | Index living; a plan is living until its close, then frozen |

### 5.3 Operations

| Artifact | Job | Kind |
|---|---|---|
| **Status** | One page: where the project is, what is next, open loose ends | Snapshot |
| **Decision log** | One entry per decision: ID, date, who, what, one line of why, link to the document it changed | Log |
| **Runbooks** | How to do a recurring procedure: environments, deploy, publishing designs, rebuilding the design system | Runbook |
| **Worklogs** | Each person's entries in worklog mode | Log, emptied at each sync |
| **Archive** | Frozen documents moved out of the way | Frozen |

---

## 6. Document kinds

The kind decides how a document is edited. This is the rule that keeps a project maintainable.

| Kind | Edited how | Keeps clean by |
|---|---|---|
| **Living** | In place. Always says what is true now | Old content is deleted, not marked superseded. The why is in the decision log. A short change log at the end: version, date, what changed, decision ID |
| **Snapshot** | Replaced completely | A size budget |
| **Log** | Added to only | Each entry is short and points to the document it changed |
| **Frozen** | Never, after its moment | Corrections live elsewhere; a one-line "Corrected by DEC-NNN" pointer at the corrected passage |
| **Runbook** | When the procedure changes | Steps only: no history, no reasoning |

### 6.1 Example

A design session finds that the product needs no Site under a School.

- **Requirements:** the school structure section now says "one school, no Site". The Site entity is deleted.
- **Decision log:** `DEC-127 · 2026-11-14 · PO · Site removed: no school in the target segment runs separate sites · requirements/requirements.md#school-structure`
- **Blueprint:** unchanged, with one line at its Site paragraph: "Corrected by DEC-127".

To learn what is true, read Requirements. To learn why, read one line of the log.

### 6.2 The Blueprint split

When the PO approves the Blueprint:

| Blueprint content | Goes to |
|---|---|
| Thesis, segments, commercial model, rollout, risks | Charter |
| Scope tiers and capability maturity | Charter, merged into one capability table |
| Stages, their exit gates, launch criteria | Charter |
| Deferred and out of scope | Charter lists it; Requirements holds any detail |
| Actors, language, domain areas, experiences, scenarios, lifecycles, open decisions, acceptance tests | Requirements, each item given an ID and a status |
| Architecture, reused assets and readiness, modules and packaging, interactions and consistency, integration contracts, data and security, performance, event catalogue | Architecture |
| Seams for later milestones | "Not built yet" to the Charter; "preserve now" and the fitness tests to Architecture |
| A first delivery plan | Not ported: the screen map and build index replace it. Its stages stay in the Charter |

The split is reviewed by its disposition table: every Blueprint section, and where it went or why it was dropped.

### 6.3 Size budgets

| Document | Budget |
|---|---|
| Status | One page, about 400 words |
| Decision log entry | Two lines |
| Worklog entry | Its fixed fields, no narrative |
| Build plan | No fixed limit: as short as its decisions allow, complete in itself, with each fact once (section 8.2). A part that runs long states why in the plan's header |
| Checkpoint | About 600 changed lines of code, not counting tests, generated files and migrations. A larger one is split by surface or action, never by layer |

---

## 7. Design

### 7.1 The screen map

Made once per milestone from the Charter, Requirements, the Architecture's module interactions and the stack profile's UI library; revised by decision.

1. **Workspaces and outputs** (who works where, on what device; what is printed or sent) and **navigation** per role.
2. **Every screen**, derived from capabilities, experiences and lifecycles: every state-changing action has somewhere to happen.
3. **One home group per screen.** A screen is drawn once, complete at full milestone detail, in its home group. A later group revisits it only if it changes it.
4. **Groups in order**, by these rules:
   - Identity first: brand, shared email and print templates, the sign-in layout, so no later group redraws them.
   - Dependency first: a screen comes after every screen whose records it needs.
   - Flows, not modules: where one action creates records in several modules, group by the work people do.
   - Views across areas (accounts, reports, dashboards) after those areas.
   - Settings after the flows they change.
   - Documents and messages once their content is settled; later stages as variation rows, with reserved space marked.
   - Shared patterns designed at their first use.
   - Stages waiting on an outside party aren't designed.
5. **Size and split point** for each group, so a large one is reviewed in parts.
6. **Coverage:** every Charter capability has a screen or is marked as having none (it becomes a named unit later). Variations not drawn go to the parked list, each saying what it would add.

### 7.2 Designing a group

One group at a time, in the screen map's order.

1. **Read:** the group's screen-map rows; Requirements for its records (rules, lifecycles, variations, scenarios); the design rules in full; the UI library's components and showcase through the stack profile; the sample data; the approved designs of the groups it depends on; the design runbook.
2. **Flow note** in chat: the screens, the rules they must show, the states, and numbered decisions each with a recommendation. Gaps in the sources are raised as their own decisions. The PO answers before drawing starts.
3. **Concepts first** when a look or style is being chosen: two or three, the PO picks.
4. **Draw:** row 1 the default flow, then variation rows the triage keeps, then states. Each artboard titled `SCR-NNN Screen name · state`. Real library parts at their defaults, never a look-alike or an override. Dark mode and phone width only for a special case when the library already supports both.
5. **Check and publish** as the design runbook says.
6. **Review** by the PO, part by part for a large group.
7. **Record:** the screen-map row marked approved; decisions logged; Requirements corrected in place; every review finding written once as a design rule and applied to every affected artboard.

### 7.3 Keeping design consistent

- The design rules are the only place a design rule lives. A new rule that contradicts one is a decision, and the old text is replaced.
- An approved artboard changes only by decision; the groups that show it are named in the decision.
- Sample data is one realistic, consistent set, used by every artboard and later by the seed.
- Tool-specific steps (how screens are drawn, rendered and published, and the tool's limits) live in the design runbook, never in a command.

---

## 8. Delivery

### 8.1 The build index

Units in build order, from the screen map's dependency order, adjusted where building needs a different order. Each unit: its plan, status, checkpoint count, close date. Below it, every Charter capability and the unit that builds it, so nothing is lost and nothing is built twice.

### 8.2 Planning a unit

The AI writes the build plan from the approved design and the current Requirements and Architecture. It takes every decision it can and writes each as question, answer and why. It stops for the PO only on:

- a change to the approved design;
- content for something the design implies but doesn't draw;
- a product question the artifacts don't answer.

The plan has three parts:

- **The unit:** standard behaviour, scope, rules, actions and roles, lifecycles, modules, data, settings, effects of each command, seed data, patterns used, differences from the design, decisions.
- **The checkpoints:** grouped into chunks, in build order. Each checkpoint names what it builds, the rules it enforces, the blocks for each surface, its tests and the developer's check. Each chunk has the PO's acceptance steps.
- **The close:** final checks, proposed changes for other documents' owners, findings for later units.

**How a plan is written** (agreed on the B03.2.2 sample, 2026-11-25):

- **Complete in itself.** A reviewer never needs another document to understand a decision; the plan copies what it needs from its sources.
- **One fact, one place in the plan.** Each command's effects (audit entry, events, emails, what a repeat does, concurrency) live only in the unit's effects table; an endpoint block points to its row. Screen text lives in the block where it shows; the differences table says only what differs and why. Artboard titles appear in the scope table and the checkpoint's parity line.
- **Defaults stated once.** The unit's standard behaviour lists what holds everywhere (standard error responses, tenancy, the module's folder, buttons disabled while sending, no idempotency key where that's the rule). A block row appears only when its answer differs.
- **Small blocks are sentences.** A block with two or three facts is a line, not a table.
- **The planner's working material stays out.** Sources read is one line; the every-unit checklist shows only what applies, with the rest named in one line; the plan checks are run by the command and recorded as one line; version history is one line per version, with reasons in the decision log.

The PO and the developer approve the plan before building starts. Plans are written two or three units ahead, each approved before the next is planned.

### 8.3 Building

One checkpoint at a time, in the plan's order, each verified and committed on its own; the build goes on through the chunk and stops only for the PO, a failure it can't fix within the plan, or the chunk's end. The build reads the plan and the code, not the wider sources: the plan carries everything it needs. A gap the plan leaves is sorted:

- **Asked:** it changes the design, the scope, Requirements, Architecture, money or security behaviour, or needs a dependency, a secret or a product answer. The PO settles it before it is built.
- **Decided by the AI:** any other gap with one clear answer in the conventions, patterns, the plan's own decisions or the existing code. The AI decides, builds, and lists it.

Either way the plan is corrected in place. After building: tests pass, and the checkpoint's Built block records what was built, how long it took, what the AI decided itself, differences from the plan and the commit. At the chunk's end one report gives the acceptance steps and every decided item.

At a chunk's end the PO reads what the AI decided and runs the acceptance steps on the running product, then the chunk's pull request merges into the unit's branch. At the close, after the regression, the unit's branch merges into the main line.

### 8.4 Closing a unit and promotion

The close runs the final checks (the demo, the regression of every closed unit, the integrity checks the Charter's quality floor requires). Then **promotion**: everything the unit settled that outlives it moves into the current documents.

| Settled in the plan | Promoted to |
|---|---|
| A business rule clarified, corrected or first settled here | Requirements, with a `RULE-001` the plan then cites |
| An architecture decision (a module boundary, a transaction rule, an event, a library choice) | Architecture |
| A foundation proved, or found wanting | Architecture, foundations and readiness |
| A convention (naming, logging, error handling, testing) | Standards, conventions |
| A reusable recipe used for the first time | Standards, patterns |
| A design change | Experience |
| A capability delivered | Build index coverage |
| A finding for a later unit | That unit's plan, or the build index |

Then the plan is frozen and the PO signs off. The next unit reads current truth, never an earlier plan.

---

## 9. Changes

Every change after approval is a decision: the PO decides, the log gets one entry, the living document is edited in place. A change that affects an approved plan updates the plan and names the checkpoints affected. Nothing is changed quietly.

---

## 10. Worklog mode

For a project where two or more people work at once.

- Each person writes only their own worklog, new files they create while those are drafts, and code on their own branches.
- Every change to a current document is a worklog entry of a fixed kind: Decision, Built, Status, Change, Finding, New file, Question. Each names its target and the version it was written against.
- The PO applies all entries at each chunk's acceptance (the sync), resolves conflicts, numbers the decisions and empties the worklogs.
- **The hold:** whoever holds it edits the current documents directly, for a day working alone or a planning session. The other keeps writing their worklog.

A one-person project edits documents directly.

---

## 11. Standards

| Level | Holds | Lives in |
|---|---|---|
| **Method** | No technology. The shape of each artifact | This repository: this document and the templates |
| **Stack profile** | Standards shared by every project on the same technology: UI library usage, API contract, layering, validation messages, shared-module adoption | `stacks/{name}/` in this repository. A project names its profile in its `CLAUDE.md`. A library's own reference documents are linked from the profile, never copied; usage rules learned while building with it live in the profile |
| **Project** | This project's own choices and deviations | `standards/`: conventions and patterns |

A project convention that proves itself in a second project is promoted to the stack profile, by a method decision.

---

## 12. Repository layout

```
ascendra-method/
├── METHOD.md            this document
├── README.md            what it is and how to start
├── CLAUDE.md            instructions for the AI in this repository
├── .claude/commands/    the commands
├── guides/              how each phase is done
├── stacks/              stack profiles
├── decisions/           the method's own decisions
└── projects/
    ├── TEMPLATE/        the project skeleton, holding every template
    └── {CODE}/          a project, its own git repository, ignored here
```

```
projects/{CODE}/
├── CLAUDE.md            project rules: stack profile, worklog mode, what each command reads
├── STATUS.md            snapshot
├── decisions.md         log
├── sources/             research, Roadmap, the approved Blueprint, anything provided (frozen)
├── charter/             charter.md
├── requirements/        requirements.md (split by domain area if it grows)
├── architecture/        architecture.md
├── standards/           conventions.md, patterns.md
├── experience/          screen-map.md, design-rules.md, sample-data.md, design/, design-system/
├── delivery/            index.md, units/
├── runbooks/
├── worklogs/
└── archive/
```

Code lives in its own repositories, as sibling folders of this one, named in the project's Architecture.

---

## 13. Commands

Only commands proven in real work, or clearly needed (principle 9).

| Command | Phase | Does |
|---|---|---|
| `/init-project` | Any | Copies the template project into `projects/{CODE}/` |
| `/split-blueprint` | Definition | Splits an approved Blueprint into Charter, Requirements and Architecture, with the disposition table |
| `/map-screens` | Design | Writes or revises the screen map: workspaces and outputs, navigation, every screen, design groups in dependency order, coverage, parked options (section 7.1) |
| `/design-group` | Design | Designs one group or part: reads, flow note, concepts, draw, check, review, record (section 7.2) |
| `/plan-unit` | Delivery | Writes one unit's build plan (today `/plan-build-group`) |
| `/build-checkpoint` | Delivery | Builds, verifies and records one checkpoint, or the close with its promotion |
| `/sync-worklogs` | Delivery | Applies every worklog entry, in worklog mode |

Writing the Blueprint is done with the AI, guided by its template and `guides/`, not by a command, until a command earns its place.

---

## Change log

| Version | Date | Change |
|---|---|---|
| 0.1 | 2026-11-25 | First draft, from the ASCENDRA-PAY-004 experience and the review of `ascendra-framework` |
| 0.2 | 2026-11-25 | Section 7 Design added (screen map, designing a group, consistency); `/map-screens` and `/design-group`; Experience gains design rules and sample data; terms Part, Flow note, Artboard, Design rule |
| 0.3 | 2026-11-25 | Section 8.2: how a plan is written (one fact one place, defaults once, effects in the unit table), agreed on the B03.2.2 sample |
| 0.4 | 2026-11-25 | Section 4 reviewed and agreed: project is one milestone with a carried-forward baseline; Workspace, Output, Surface; DEC-NNN and plan-local D and P; Rule code beside RULE and VAL; unit IDs (B03, B08a, B-SPINE); chunk kept; three-digit IDs and prefix swap for existing projects |
| 1.0 | 2026-11-25 | Philosophy: principle 7 reworded (plans copy from current documents), 9 Prove before encoding and 10 Sources first added. Whole document reviewed and agreed by the PO |
| 1.1 | 2026-11-25 | Layout: Charter and Requirements each get a folder (`charter/`, `requirements/`), so Requirements can split by domain area; templates are named `{name}.template.md` |
| 1.2 | 2026-11-25 | Standards becomes its own artifact and folder (`standards/conventions.md`, `standards/patterns.md`), the project-level twin of the stack profile; Architecture holds only the expensive-to-change decisions |
| 1.3 | 2026-11-25 | Section 11: a library's own reference documents are linked, never copied; usage rules learned while building with it live in the stack profile |
| 1.4 | 2026-11-25 | MD-002: plans keep their length in check with a stated reason; gaps asked or decided by the AI; building through the chunk; chunk pull requests into the unit's branch, merged into the main line at the close |
