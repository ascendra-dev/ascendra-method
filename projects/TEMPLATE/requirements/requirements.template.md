# Requirements: [Project code], [Milestone]

> **Guide.** Living (METHOD.md section 6): always says what is true now. Written first by `/split-blueprint` from the approved Blueprint, then corrected in place by design and build (a design group's record step, a unit's close), each change with a decision in the log. Old text is deleted, not marked superseded; the change log at the end names the decision.
>
> **What belongs here:** the business: its language, actors, what the product lets them do, the rules that always hold, how customers legitimately differ, the cases that test it all, and what is still open. **What doesn't:** modules, tables, endpoints, events, technology (Architecture), screens and layout (Experience), why and for whom (Charter). State the business rule ("a payment is counted once"), never its mechanism.
>
> **IDs** (METHOD.md section 4): `CAP`, `RULE`, `VAL`, `VAR`, `SCN`, `OPEN`, three digits, never reused. Every item has a **status**: Proposed (not yet agreed), Approved, or Deferred (with where it went). A plan cites these IDs; it never restates them differently.
>
> **Domain areas** are business areas (fees, payments, reconciliation), not modules. One area often maps to one module, and Architecture says which.

| Field | Value |
|---|---|
| Version | [1.0] |
| From | [The Blueprint version it was split from, and the decision] |
| Earlier IDs | [For a project that had IDs before: each old prefix and its new one, e.g. `UBR` → `RULE`, `V` → `VAR`. Otherwise delete this row] |

---

## 1. Language

> **Guide.** Every term that means something specific here. Binding in every document, in code and in conversation: a synonym is corrected, never allowed to drift in. An exception for one context (an external provider's word) says so and is used only there.

| Term | Meaning | Not to be confused with | Also called (where) |
|---|---|---|---|
| [Term] | [One or two sentences] | [The term it is often mixed up with, and the difference] | [Context-only synonym, or —] |

## 2. Actors

> **Guide.** Every person, organisation or system that acts. What each does, what each may never do, and the boundary of what each can see. Then the separation-of-duties rules, and what replaces them where one person holds every role.

| Actor | Does | Never does | Sees |
|---|---|---|---|
| [Actor] | [Responsibilities] | [Limits] | [Access boundary] |

**Separation of duties:** [Who may not approve what they recorded; the compensating control for a one-person customer.]

## 3. Domain areas

> **Guide.** One subsection per area, in the order work flows. Each has the same five parts. Rules that cross areas go in section 4.

### 3.1 [Area name]

**Purpose:** [One sentence: what this area decides or records.]
**Owns:** [Records and decisions it owns.] **Doesn't own:** [What it relies on another area for.]

**Capabilities**

> **Guide.** The area's maturity target (Core, Standard, Advanced) is in the Charter's capability table, once. Here: each thing the product lets someone do, and the stage it arrives in.

| ID | The product lets … | Stage | Status |
|---|---|---|---|
| CAP-001 | [Actor does what, in one sentence] | [M1.1] | [Approved] |

**Records and lifecycles**

| Record | Statuses | Moves | Notes |
|---|---|---|---|
| [Record] | [Status → status → …] | [Which action moves it, and who] | [What never happens: "never deleted once used"] |

**Rules**

> **Guide.** A rule is a statement a test can check. A worked example under the table when the rule involves an amount or a sequence.

| ID | Rule | Status |
|---|---|---|
| RULE-001 | [Statement] | [Approved] |

**Validations**

| ID | Checked | Message | Status |
|---|---|---|---|
| VAL-001 | [Field or state, and the check] | [Exact message] | [Approved] |

## 4. Rules across areas

> **Guide.** Rules that hold everywhere: how money and time are treated, what is never deleted, what is counted once, the quality floor every capability meets. Same columns as an area's rules.

| ID | Rule | Status |
|---|---|---|
| RULE-0NN | [Statement] | [Approved] |

## 5. Workspaces and outputs

> **Guide.** What each workspace and output must let its users do, as requirements, not design: who uses it, on what device, the journeys it must support, what it must never show. The screen map turns this into screens.

| Workspace or output | Who | Device | Must support | Never |
|---|---|---|---|---|
| [Staff workspace] | [Actors] | [Desktop, phone…] | [Journeys, by capability] | [What it must never show or allow] |

## 6. Variations

> **Guide.** Every way real customers legitimately differ, and who settles it. Make a variation customer-configurable only if real customers differ on it and one fixed answer would stop some from using the product; otherwise fix it for everyone. A setting never overrides a rule. A pilot customer's answer is their configuration, never a requirement.
>
> Settled by: **Platform-fixed** (one answer for all), **Customer-configurable** (options built now, a default, chosen at onboarding), **External** (law, contract, a provider), **Not in this release** (recognised, deliberately left out, with what it would add).

| ID | Variation | Settled by | Options in this release | Default | Set by, when | Limit | Capabilities | Status |
|---|---|---|---|---|---|---|---|---|
| VAR-001 | [How customers differ] | [One of four] | [Options] | [Default] | [Role, moment] | [What a setting can't do] | [CAP IDs] | [Approved] |

## 7. Scenarios

> **Guide.** Named business cases that test the rules and the design: the awkward ones (duplicates, reversals, timing, two people at once) as well as the main path. Each names the rules it exercises and the outcome.

| ID | Situation | Expected outcome | Rules | Status |
|---|---|---|---|---|
| SCN-001 | [What happens] | [What the product does, and what stays true] | [RULE IDs] | [Approved] |

## 8. Acceptance

> **Guide.** What must be proved before each stage is released, by area: the checks a stage's exit gate runs. Each cites the rules or scenarios it proves. Units' tests prove them one by one; the Charter's exit gates name the stage.

| Area | Must be proved | Proves |
|---|---|---|
| [Area] | [Check, in plain words] | [RULE, SCN IDs] |

## 9. Open questions

> **Guide.** What isn't decided yet, who decides, and by when (the unit or stage that needs the answer). A question answered moves its answer into the right section and leaves here, with its decision in the log. One taken from the Blueprint notes its origin.

| ID | Question | Owner | Needed by | Recommendation | Origin |
|---|---|---|---|---|---|
| OPEN-001 | [Question] | [Role] | [Unit or stage] | [Recommended answer] | [Blueprint D06 / design / build] |

---

## Change log

| Version | Date | Change | Decision |
|---|---|---|---|
| 1.0 | [YYYY-MM-DD] | Split from the Blueprint | [DEC-NNN] |
