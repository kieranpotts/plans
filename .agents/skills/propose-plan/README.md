# Propose plan

Handles the `DRAFT` → `PLANNED` transition.

Checks the breakdown and dependency graph are complete and takes the pull
request out of draft, ready for review.

## How to invoke

> Propose plan

Run it on a `plan/<slug>` branch, or from `main` to pick from the open draft
PRs.

## Recommended models

A fast, cheap model is sufficient to run this skill, which involves only
mechanical tasks. There are no judgment calls that benefit from deep reasoning.
