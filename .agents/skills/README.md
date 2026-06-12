# Agent skills

This repository ships a small set of [agent skills](https://agentskills.io/) — invoked as slash commands through agentic tools such as Claude Code — that automate the plan lifecycle.

There is one skill per state transition: `DRAFT` → `PLANNED` → `IN PROGRESS` → `DONE`, plus `PLANNED`/`IN PROGRESS` → `ABANDONED`. Each skill knows the gate rules for its own transition and will not proceed until they are met, which keeps the process consistent whether a human or an agent is driving it.

The skills are, in lifecycle order:

- **[`/draft-plan`](./draft-plan/)**: Scaffolds a new draft plan, ready for the user to complete. Sets up the branch and plan document from the template, and opens a draft pull request.

- **[`/finalize-plan`](./finalize-plan/)**: `DRAFT` → `PLANNED` — Confirms the breakdown and dependency graph are complete and free of leftover template text. Applies the `#planned` label and takes the pull request out of draft.

- **[`/implement-plan`](./implement-plan/)**: `PLANNED` → `IN PROGRESS` — Marks the initiative as underway once execution has begun. The pull request stays open and the plan stays mutable.

- **[`/complete-plan`](./complete-plan/)**: `IN PROGRESS` → `DONE` — Confirms every task has shipped (by following each linked tracker), squash-merges the pull request to `main`, and appends the plan to `plans/INDEX.md`.

- **[`/abandon-plan`](./abandon-plan/)**: `PLANNED`/`IN PROGRESS` → `ABANDONED` — Drops an initiative before completion, recording the reason, and merges the plan to `main` as a permanent record.

A typical journey runs `/draft-plan` → the user writes the plan → `/finalize-plan` → review → `/implement-plan` → implementation across the code repos → `/complete-plan`. If the initiative is dropped, `/abandon-plan` retires it instead.
