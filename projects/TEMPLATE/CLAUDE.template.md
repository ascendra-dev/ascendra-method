# [Project code]: project rules

> **Guide.** Living (METHOD.md section 6). The AI's instructions for this project, read before any command runs. Keep it to what is specific to this project; the method's rules are in METHOD.md and are not repeated here.

| Field | Value |
|---|---|
| Project | [Name], milestone [M1] of the [product family] Roadmap |
| Stack profile | [`stacks/{name}/`, or "none"] |
| Code repositories | [Name and sibling folder of each, e.g. `../{product}-api`] |
| Baseline from | [The earlier project whose Architecture, design rules and design system this one started from, or "none"] |

## Start here

Read `STATUS.md`, then the document the next step names.

## Sources

> **Guide.** The files in `sources/`, each with a short name the rules below use.

| Short name | File |
|---|---|
| [BLUEPRINT] | `sources/[file].md` |

## Project rules

> **Guide.** Rules for this project that override a command's general wording, numbered. Only what the method can't know: a source rule, a domain boundary, a naming exception. Each says which commands it applies to.

1. [Rule]

## What each command reads

> **Guide.** Only where a command needs more, or less, than its own default reading list. Name exact files and sections.

| Command | Reads |
|---|---|
| [command] | [files and sections] |

## Manual testing

> **Guide.** METHOD.md section 8.5 and `guides/manual-testing.md` hold the rules. Fill this once the first registry is built; until then leave the placeholders. No command reads `delivery/testing/`.

| Field | Value |
|---|---|
| Testers | `delivery/testing/people.json` |
| Test page | [Its local address, e.g. `http://localhost:3000/dev/manual-tests`] |
| Path setting | [The setting in the web app's local environment file that names `delivery/testing/`, as the stack profile says] |

## Worklog mode

> **Guide.** Delete this section while one person works on the project. When two or more work at once, list each person; METHOD.md section 10 and `worklogs/worklog.template.md` hold the rules.

| Person | Git user | Worklog | ID prefix | Runs the sync |
|---|---|---|---|---|
| [Name, role] | [`git config user.name`] | `worklogs/[name].md` | [Z] | [Yes for the PO] |
