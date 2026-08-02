# Abandon plan

Handles the `PLANNED`/`IN PROGRESS` → `ABANDONED` transition.

Drops the plan before completion and merges it as a permanent record of
the decision.

## How to invoke

> Abandon plan

Run it on a `plan/<slug>` branch, or from `main` to pick from the open plan PRs.

## Recommended models

A mid-tier model is sufficient for this skill. The steps are procedural, but
holding the gate in front of them requires a bit more effort.
