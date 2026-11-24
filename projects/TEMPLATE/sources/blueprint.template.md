# Blueprint: [Milestone name]

> **Guide.** Frozen once approved (METHOD.md section 6). The milestone thought through in one document, written by the PO with the AI, iteratively, until the whole milestone is clear. There is no command for writing it (principle 9): the PO and the AI work through it part by part, starting from the Roadmap's section for this milestone and the research.
>
> When the PO approves it, `/split-blueprint` moves its content into the living artifacts. Each part below names where it goes (METHOD.md section 6.2), so the split is mechanical and reviewed by its disposition table. After the split, this document is never edited: a correction lives in the living artifact, with "Corrected by DEC-NNN" at the passage here.
>
> **While writing:**
> - Mark every fact's source: **(verified)** from a primary document, **(unverified)** from press, a provider or a vendor, **(validate)** from general knowledge, to confirm with a real customer. Unverified and validate facts become open questions.
> - Give every rule an example with real figures where it involves an amount or a sequence.
> - Write requirements as business rules, never mechanisms. The solution shape (Part VII) is the only part about how it is built.
> - Decisions not yet made go in Part VIII with a recommended starting position, not into the text as if settled.

| Field | Value |
|---|---|
| Version | [0.1] |
| Status | [Draft / Approved, date, DEC-NNN] |
| Milestone | [M1], in the [product family] Roadmap |
| Market | [Where it is sold first] |
| Sources | [Research notes and the Roadmap sections used] |

---

# Part I: Product case → Charter §1–3, §7, §8

## 1. Thesis

[The problem, who has it, what the product does about it, the outcomes, and what it is not trying to do.]

## 2. Customers

[Segments, why these first, their constraints, the pilot.]

## 3. Commercial model

[Pricing, who pays, costs outside our control, the go or stop test for the next milestone.]

## 4. Rollout

[Pilot sequence, going live, the second customer, handover.]

## 5. Risks

| Risk | Impact | Mitigation or gate |
|---|---|---|

# Part II: Scope and stages → Charter §4–6

## 6. Capability maturity

| Capability area | Core | Standard | Advanced | Target |
|---|---|---|---|---|

**Quality floor:** [What every capability meets.]

## 7. Stages

| Stage | Delivers | Exit gates | Depends on |
|---|---|---|---|

## 8. Not in this milestone

| Capability | Deferred to | Why |
|---|---|---|

# Part III: Language and actors → Requirements §1–2

## 9. Language

[Terms that must not be confused, with their meanings, and the money and time principles that hold everywhere.]

## 10. Actors

[Who acts, what each does and never does, what each sees; separation of duties, and the control where one person holds every role.]

# Part IV: Domain areas → Requirements §3–4

> **Guide.** One section per business area, in the order work flows. Each with the same parts, so the split gives each capability, rule and lifecycle its ID.

## 11. [Area]

**Purpose:** [One sentence.] **Owns:** [ ] **Doesn't own:** [ ]

**Capabilities:**
1. [The product lets … ]

**Lifecycle:** [Statuses and moves.]

**Rules and examples:**
- [Rule.] Example: [real figures].

**Controls:** [What must always hold for this area to be safe.]

**Later:** [What Standard and Advanced would add.]

# Part V: Experiences → Requirements §5, then the screen map

## 12. [Workspace or output]

[Who uses it, on what device, the journeys it must support, the principles it follows, what it must never show.]

# Part VI: Behaviour and acceptance → Requirements §6–8

## 13. Variations

[How customers legitimately differ, with a first sorting of who settles each: platform-fixed, customer-configurable, external, not in this release.]

## 14. Scenarios

[Named cases, the awkward ones included: each with what happens and the expected outcome.]

## 15. Acceptance

[What must be proved before each stage is released, by area.]

# Part VII: Solution shape → Architecture

## 16. Shape

[Deployables, applications, repositories, environments, providers, portability.]

## 17. Foundations and readiness

[What we build on that we didn't build, its status (verified, needs extension, missing, external) and the test that would prove it.]

## 18. Modules

[Each module, what it owns and never owns, its layer; what may share a package and what must never be combined.]

## 19. Interactions and consistency

[For every trigger that changes several modules: what is synchronous, what shares one transaction, what follows as an event, the guarantee. The races the product must survive.]

## 20. Integrations, data and security

[Outside systems and their contracts; tenancy, identity, personal data, audit, secrets, recovery.]

## 21. Events, scale and seams

[The event catalogue; expected load and when to extract; what to preserve for later milestones and the test that checks each seam.]

# Part VIII: Open decisions → Requirements §9, or the decision log once settled

| # | Decision | Recommended starting position | Owner | Needed by |
|---|---|---|---|---|
| D01 | [Decision] | [Position] | [Role] | [Stage or area] |

---

## Approval

> **Guide.** Before approving, the PO confirms each part. Then `/split-blueprint` runs.

- [ ] Product case and customers
- [ ] Scope, stages and what is left out
- [ ] Language, actors and every domain area's rules
- [ ] Experiences, variations and scenarios
- [ ] Solution shape
- [ ] Open decisions, each with an owner and a deadline

| Version | Date | Change |
|---|---|---|
| 0.1 | [YYYY-MM-DD] | [First draft] |
