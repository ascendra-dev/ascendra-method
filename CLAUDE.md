# Ascendra Method: rules for the AI

Read [`METHOD.md`](METHOD.md) before any work in this repository. It is binding, and its vocabulary (section 4) is the only vocabulary.

## Working

- One step at a time. Stop for the PO's review before the next step.
- Ask questions in plain chat, one at a time, each with a recommendation.
- Commit only when the PO asks.
- Check current files, never memory, before stating a fact.

## The method stays generic

Everything outside `projects/` and `stacks/` works for any domain and any business. A domain example may illustrate a point and is labelled as one. Technology choices live in `stacks/{name}/` and in each project's Architecture, never in a command or template.

## Templates

Each template lives in `projects/TEMPLATE/` at the path its document takes in a project, named `{name}.template.md` (`STATUS.template.md`, `units/unit.template.md`). `/init-project` copies single documents without the `.template` suffix; a document with many copies (a unit plan, a worklog, a runbook) is copied from its template when one is made. No file in `projects/TEMPLATE/` is ever named `CLAUDE.md`, so it can't load as live instructions.

- `> **Guide.**` blocks tell the AI how to fill a section. They are removed from the filled document.
- `[Square brackets]` are placeholders.
- A template states the document's kind (METHOD.md section 6) in its first guide block.

## Projects

`projects/{CODE}/` is the project's own git repository and is ignored here. Its `CLAUDE.md` names its stack profile, whether worklog mode is on, and what each command reads. In worklog mode, check `git config user.name` and `worklogs/hold.md` before changing any document.

## Changing the method

A change to `METHOD.md`, a template or a command is a method decision: written in `decisions/` as `MD-NNN`, agreed by the PO, then applied, with a row in `METHOD.md`'s change log when it changes the method itself.
