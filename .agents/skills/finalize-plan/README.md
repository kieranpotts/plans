# Finalize plan

Moves a plan from `DRAFT` to `PLANNED` — ready for review.

## What it does

- Verifies the document is complete: `Summary`, `Scope`, `Approach`, a well-formed task breakdown, and a dependency graph that matches it.
- Checks no leftover template text remains.
- Sets `Status: PLANNED` and updates `Last updated`.
- Applies the `#planned` label.
- Takes the pull request out of draft.

## How to invoke

```
/finalize-plan
```

Run it on a `plan/<slug>` branch, or from `main` to pick from the open draft PRs.

## Notes

Will not proceed unless the breakdown and dependency graph are complete and free of placeholder text. Next step once work begins: [`implement-plan`](../implement-plan/README.md).
