# Charter: [Project code], [Milestone]

> **Guide.** Living, changed only by PO decision (METHOD.md section 6). Why this milestone exists, for whom, what is in and out, the stages it is released in and the gates each must pass, and what could go wrong. Written by `/split-blueprint` from the approved Blueprint; short and stable. Detail lives elsewhere: business rules in Requirements, how it is built in Architecture, screens in Experience.

| Field | Value |
|---|---|
| Version | [1.0] |
| Milestone | [M1 Name], in the [product family] Roadmap |
| From | [The Blueprint version, and the decision] |

---

## 1. Why

**The problem:** [Who has it, and what it costs them today. Two or three sentences.]
**The product:** [What it does about it, in two or three sentences.]

| Outcome | How we'll know |
|---|---|
| [What changes for the customer] | [The measure] |

**Not trying to:** [Non-goals, one line each.]

## 2. For whom

| Segment | Why them first | Their constraints |
|---|---|---|
| [Customer segment] | [Reason] | [Size, devices, budget, regulation] |

**Pilot:** [Who, why, and what we collect from them first. A pilot's answers are their configuration, never requirements.]

## 3. How it earns

[Pricing model, who pays what, costs that aren't ours (provider fees), and the test that decides whether to continue to the next milestone.]

## 4. Scope

> **Guide.** One table: each capability area with what Core, Standard and Advanced mean for this product, the target for this milestone, and the stage it arrives in. Requirements holds each area's individual capabilities (`CAP`); this table holds their maturity, once.

| Capability area | Core | Standard | Advanced | Target | Stage |
|---|---|---|---|---|---|
| [Area] | [What Core means] | [What Standard means] | [What Advanced means] | [Core / Standard / —] | [M1.1] |

**Quality floor:** [What every capability meets at any maturity: access control, data isolation, accuracy, safe repeats, audit, safe failure, recovery, monitoring, data protection, operator handling. "Core" means narrow but complete and safe, not a demo.]

## 5. Stages

> **Guide.** Each release within the milestone: what it delivers, and the exit gates it must pass before it is released. A stage is a commercial commitment, moved only by PO decision. Requirements' acceptance section holds the detailed checks each gate runs.

| Stage | Delivers | Exit gates | Depends on |
|---|---|---|---|
| [M1.1 Name] | [Capability areas] | [What must be true to release: product, integrity, security, operations] | [Outside parties, or "nothing outside us"] |

## 6. Not in this milestone

> **Guide.** What is deliberately left out, where it goes, and why, so scope doesn't creep back in. What the architecture preserves for later is in Architecture §9; this list is what is not built.

| Capability | Deferred to | Why |
|---|---|---|
| [Capability] | [M2 / later / not planned] | [Reason] |

## 7. Rollout

[Pilot sequence, the moment to go live, the second customer that tests generality, and the handover the customer needs (procedures, support).]

## 8. Risks

| Risk | Impact | Mitigation or gate |
|---|---|---|
| [Risk] | [What goes wrong] | [What prevents it, or the gate that checks it] |

---

## Change log

| Version | Date | Change | Decision |
|---|---|---|---|
| 1.0 | [YYYY-MM-DD] | Split from the Blueprint | [DEC-NNN] |
