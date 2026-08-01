# Complete plan

Handles the `IN PROGRESS` → `DONE` transition.

Checks every task has shipped and merges the plan into the `main` trunk.

## How to invoke

> Complete plan

Run it on a `plan/<slug>` branch, or from `main` to pick from the `#in-progress`
PRs.

## Recommended models

A fast, cheap model is sufficient to run this skill, which involves only
mechanical tasks. There are no judgment calls that benefit from deep reasoning.
