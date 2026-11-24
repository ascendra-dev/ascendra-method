# Architecture: [Project code], [Milestone]

> **Guide.** Living (METHOD.md section 6). How the product is built: the decisions that are expensive to change, and what every unit must follow. Written first by `/split-blueprint` from the Blueprint's architecture sections (or carried forward from the previous milestone's project as its baseline), then corrected in place: a unit's close promotes what it settled or proved (METHOD.md section 8.4). Old text is deleted; the change log names the decision.
>
> **Belongs here:** shape, foundations and their readiness, modules, how they interact and stay consistent, integrations, data and security, events, scale, seams for later milestones, and what is deliberately not decided yet. **Elsewhere:** how code is written day to day and reusable recipes (`standards/conventions.md`, `standards/patterns.md`), business rules (Requirements), the stack's shared standards (the stack profile, which this document names and doesn't repeat).
>
> A plan copies what it needs from here with a stamp (`Architecture §4`), so section numbers stay stable: add a section at the end rather than renumber.

| Field | Value |
|---|---|
| Version | [1.0] |
| Stack profile | [`stacks/{name}/`, or "none"] |
| Baseline from | [The earlier project this started from, or "the Blueprint"] |

---

## 1. Shape

| Decision | Choice | Why |
|---|---|---|
| Stack | [Languages, frameworks, database] | [One line, or the decision] |
| Deployables | [What runs where, and why that many] | |
| Applications | [Which workspaces are served by which application] | |
| Repositories | [Each repository and what it holds] | |
| Environments | [Local, staging, production, and what differs] | |
| Providers | [Hosting, database, sign-in, email, files: each and where] | |
| Portability | [What a provider change would cost, and the seams that keep it to configuration] | |

## 2. Foundations and readiness

> **Guide.** Everything the product builds on that it didn't build itself (shared modules, libraries, providers), and whether each is proven. This is the foundation's risk register: a unit's close updates a row when it proves something or finds a gap. Status: **Verified** (proven here, with evidence), **Needs extension** (works, but not for everything this product needs), **Missing** (must be built), **External** (outside our control).

| Foundation | Gives us | Status | Evidence or gap | Proved by |
|---|---|---|---|---|
| [Name and version] | [What we rely on it for] | [One of four] | [Test or finding] | [Unit, or "not yet"] |

**Changes to copied code:** [Where changes to copied shared code are logged, if the stack copies rather than installs it.]

## 3. Modules

> **Guide.** Every module, with its two kinds (where its code comes from; what it is for) and its layer. Each appears when the first unit needs it. A module's responsibility is stated once, here.

| Module | Source | Purpose | Layer | Owns | Never owns | First built in |
|---|---|---|---|---|---|---|
| [module] | [Shared / to become shared / product] | [Business / platform / integration / front end] | [Layer] | [Records, decisions] | [What it must leave to others] | [Unit] |

**Layers:** [The layers in order, and who may call whom. An upward need is a registration or an event, never a call.]

**Packaging:** [Which modules may share a package, and which must never be combined, and why.]

**Folder layout:** [Each repository's top folders, and where a module's code lives.]

## 4. Interactions and consistency

> **Guide.** For every trigger that changes records in more than one module: what is called synchronously, what shares one transaction, what follows as an event, and the guarantee kept. Plans copy the row for their command instead of deciding this again.

| Trigger | Synchronous, in one transaction | Afterwards | Guarantee |
|---|---|---|---|
| [What happens] | [Module calls, in order] | [Events and their reactions] | [What must always hold] |

**Effects:** [How audit entries, events and emails travel: inside the transaction or after it, and through what.]

**Races:** [Each race the product must survive, and how: locks, versions, idempotency.]

## 5. Integrations

> **Guide.** Every outside system: what we send, what we receive, in what shape, and how we trust it.

| System | Direction | Contract | Trust and security | Failure handling |
|---|---|---|---|---|
| [Provider] | [Out / in / both] | [Operations and shapes] | [Signatures, secrets, allowlists] | [Retries, inquiry, what the user sees] |

## 6. Data, tenancy and security

| Concern | Decision |
|---|---|
| Tenancy | [How each customer's data is kept apart, and how it is proved] |
| Identity and access | [Sign-in, roles, permissions, how they reach each request] |
| Personal data | [What is collected, what never is, retention] |
| Audit | [What is recorded, and that entries are never changed] |
| Secrets | [Where they live; what is never logged] |
| Recovery | [Backups, restore, targets] |

## 7. Events

> **Guide.** Every event: who raises it, who reacts, and the rule it carries. Past tense names.

| Event | Raised by | Reacted to by | Rule |
|---|---|---|---|
| [EventName] | [Module] | [Modules or reactions] | [What it guarantees] |

## 8. Scale and extraction

[What load is expected and when (peaks); how the product scales before splitting; what would justify extracting a module, and the checklist before doing so.]

## 9. Seams for later milestones

> **Guide.** What this milestone preserves so later ones don't rebuild it, and the fitness test a unit's close can run to check the seam still holds.

| Seam | Preserve now | Don't build yet | Fitness test |
|---|---|---|---|
| [Concept] | [What to keep general] | [What to resist] | [A check that fails if the seam leaks] |

## 10. Not decided yet

| Decision | Decided when |
|---|---|
| [Decision] | [The unit or event that needs it] |

---

## Change log

| Version | Date | Change | Decision |
|---|---|---|---|
| 1.0 | [YYYY-MM-DD] | [First version] | [DEC-NNN] |
