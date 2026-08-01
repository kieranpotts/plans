# Approve plan

Moves a plan from `DRAFT` to `PLANNED` — the breakdown is complete and open for
review.

## What it does

- Verifies the document is complete: `Summary`, `Scope`, `Approach`, a
  well-formed task breakdown, and a dependency graph that matches it.
- Checks no leftover template text remains.
- Sets `Status: PLANNED` and updates `Last updated`.
- Applies the `#planned` label.
- Takes the pull request out of draft so it can be reviewed. The discussion
  thread stays open — through review and on through implementation, until the
  plan is done or abandoned.

## How to invoke

> Approve plan

Run it on a `plan/<slug>` branch, or from `main` to pick from the open draft
PRs.

## Recommended models

A fast or mid-tier model is enough. The check is completeness — required
sections present, no placeholder text — not a judgment on the quality of
the breakdown.

## Notes

Will not proceed unless the breakdown and dependency graph are complete and free
of placeholder text. Next step once work begins:
[`/implement-plan`](../implement-plan/README.md).
