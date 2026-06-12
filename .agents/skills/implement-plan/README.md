# Start implementation of plan

Moves a plan from `PLANNED` to `IN PROGRESS` — execution has begun.

## What it does

- Confirms work on at least one task has actually started.
- Sets `Status: IN PROGRESS` and updates `Last updated`.
- Swaps the `#planned` label for `#in-progress`.

The pull request stays open and the plan stays mutable — the breakdown and dependency graph may continue to evolve as the work unfolds.

## How to invoke

```
/implement-plan
```

Run it on a `plan/<slug>` branch, or from `main` to pick from the `#planned` PRs.

## Notes

Live task status is not tracked here — it lives in each task's linked tracker. Next step when every task ships: [`complete-plan`](../complete-plan/README.md). To drop the initiative instead: [`abandon-plan`](../abandon-plan/README.md).
