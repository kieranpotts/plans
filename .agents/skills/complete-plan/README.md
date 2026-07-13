# `/complete-plan`

Moves a plan from `IN PROGRESS` to `DONE`, and merges it.

## What it does

- Confirms every task has shipped, by following each task's linked tracker.
- Sets `Status: DONE` and updates `Last updated` (the settled date).
- Swaps the `#in-progress` label for `#done`.
- Closes the associated discussion thread.
- Squash-merges the pull request into `main` (with your confirmation).
- Appends the plan to `plans/INDEX.md`, in implementation order.

## How to invoke

```
/complete-plan
```

Run it on a `plan/<slug>` branch, or from `main` to pick from the `#in-progress`
PRs.

## Notes

Will not complete a plan whose tasks have not all shipped. Does not merge
without your explicit go-ahead. To drop a plan instead of completing it:
[`/abandon-plan`](../abandon-plan/README.md).
