# Propose plan

Handles the `DRAFT` → `PLANNED` transition.

Checks the breakdown and dependency graph are complete and takes the pull
request out of draft, ready for review.

## How to invoke

> Propose plan

> This plan is ready

> Mark the plan ready

> The breakdown is done

> Take the plan out of draft

Run it on a `plan/<slug>` branch, or from `main` to pick from the open draft
PRs.

## Recommended models

A mid-tier model is sufficient for this skill. It applies a readiness gate to
a concrete case — bounded judgment, not deep reasoning.
