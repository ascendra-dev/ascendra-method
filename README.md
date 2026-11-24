# Ascendra Method

A method for one product owner and an AI to take a product from an idea to production, with full control over every decision.

The PO thinks a milestone through in a Blueprint, designs every screen in dependency order, then builds unit by unit in small checkpoints anyone can see work. The AI researches, drafts, plans, builds and tests; the PO decides and approves at every gate.

**Read [`METHOD.md`](METHOD.md) first.** It holds the philosophy, lifecycle, artifacts, document kinds, vocabulary and commands.

## Start a project

1. Clone this repository and open its root in Claude Code (commands load from the root).
2. Run `/init-project`. It copies `projects/TEMPLATE/` to `projects/{CODE}/`, which becomes its own git repository.
3. Put research and the Roadmap in `projects/{CODE}/sources/`, and write the Blueprint with the AI.

Code lives in its own repositories, as sibling folders of this one.

## Layout

| Path | Holds |
|---|---|
| `METHOD.md` | The method |
| `.claude/commands/` | The commands |
| `guides/` | How each phase is done |
| `stacks/` | Technology profiles a project can choose |
| `decisions/` | The method's own decisions |
| `projects/TEMPLATE/` | The project skeleton, with every template |
| `projects/{CODE}/` | A project: its own repository, ignored here |
