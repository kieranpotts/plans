# Complete plan

Handles the `IN PROGRESS` → `DONE` transition.

Checks every task has shipped and merges the plan into the `main` trunk.

## How to invoke

> Complete plan

Run it on a `plan/<slug>` branch, or from `main` to pick from the `#in-progress`
PRs.

## Recommended models

A mid-tier model is sufficient for this skill. The steps are procedural, but
verifying that every task actually shipped requires a bit more effort.
