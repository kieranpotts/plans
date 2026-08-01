# Abandon plan

Handles the `PLANNED`/`IN PROGRESS` → `ABANDONED` transition.

Drops the plan before completion and merges it as a permanent record of
the decision.

## How to invoke

> Abandon plan

Run it on a `plan/<slug>` branch, or from `main` to pick from the open plan PRs.

## Recommended models

A fast, cheap model is sufficient to run this skill, which involves only
mechanical tasks. There are no judgment calls that benefit from deep reasoning.
