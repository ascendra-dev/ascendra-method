# Screen Map: [Project code], [Milestone]

> **Guide.** Living (METHOD.md section 6); after approval it changes only by decision. Written by `/map-screens` from the Charter, Requirements (workspaces and outputs, capabilities, lifecycles, variations), Architecture §4 (interactions) and the stack profile's UI library. Every screen of the milestone appears once, in its home design group, and the groups are in dependency order (METHOD.md section 7.1). `/design-group` reads its group's rows; `/plan-unit` reads them to find the unit's screens.
>
> **Not here:** how screens look and behave (design rules), business rules (Requirements), why a decision was made (decision log; a row names its `DEC`), drawing tools (the design runbook).

| Field | Value |
|---|---|
| Version | [1.0] |
| Designs | [Where the drawn designs are: canvas links, source folders] |
| Status | [Draft / Approved; every group's status is in section 4] |

---

## 1. Workspaces and outputs

| Workspace or output | Who | Access | Device |
|---|---|---|---|
| [Staff workspace] | [Roles] | [Sign-in, own customer only] | [Desktop and tablet; responsive] |
| [Documents] | [Who receives them] | — | [A4, email] |

## 2. Navigation

> **Guide.** For each workspace: its navigation items in order, which role sees each, and where each role lands after signing in. An item appears only for a role that can use it.

| Workspace | Item | Group | Roles | Opens |
|---|---|---|---|---|
| [Staff workspace] | [Item] | [Navigation group] | [Roles] | [SCR-NNN] |

**Landing:** [Each role's first screen, and what it is until that screen's unit is built.]

## 3. Design order

### 3.1 Rules for the order

> **Guide.** METHOD.md section 7.1's rules, as this project applies them: write each with its project example, and add any project-specific rule.

1. **Identity first:** [what this project settles before any screen]
2. **Dependency first:** [example]
3. **Flows, not modules:** [example]
4. **Views across areas after those areas:** [example]
5. **Settings after their flows:** [example]
6. **Outputs once their content is settled:** [example]
7. **Shared patterns at first use:** [example]
8. **Not designed:** [stages waiting on an outside party]

### 3.2 How a group is drawn

[Where groups are drawn (pages, canvases); each group's block: row 1 the default flow, then variation rows the triage keeps, then states; artboard titles `SCR-NNN Screen name · state`; parts of a large group. Tool steps are in the design runbook.]

### 3.3 The groups

| Group | Name | Page | Depends on | Size | Parts if split | Status |
|---|---|---|---|---|---|---|
| G01 | [Name] | [Canvas page] | [Groups] | [S / M / L] | [Where it splits] | [Not started / Flow note / Drawing / Approved, date, DEC-NNN] |

## 4. Screens by group

> **Guide.** One subsection per group, in order. A one-line note says what the group needs first. Each screen once, with its type (the design rules' page recipes, or Print, Email), the surfaces it opens (sheet, dialog, page, drawer), the stage it first works in, and its rows on the canvas.

### G01 [Group name]

[Needs: … One line.]

| Screen | Name | Type | Opens | Stage | Rows |
|---|---|---|---|---|---|
| SCR-001 | [Name] | [Standard / Tabbed / Dashboard / Report / Wizard / Print / Email] | [Sheet: …; Dialog: …; Page: …] | [M1.1] | [1: the default flow. 2: variation … 3: states] |

## 5. Coverage

> **Guide.** Every Charter capability and where it is seen. A capability with no screen is marked, and becomes a named unit in the build index.

| Capability | Screens |
|---|---|
| [CAP-NNN or Charter capability] | [SCR IDs, or "no screen: unit B-SPINE"] |

## 6. Parked

> **Guide.** Options the variation triage keeps out of this milestone, each saying what it would add where, so the screens above keep room for it. Nothing here is drawn.

- **[Option] ([VAR-NNN]).** [What it would add, on which screens. Until then, …]

---

## Change log

| Version | Date | Change | Decision |
|---|---|---|---|
| 1.0 | [YYYY-MM-DD] | [First map] | [DEC-NNN] |
