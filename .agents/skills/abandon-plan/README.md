# Abandon plan

Drops a plan before completion, from `PLANNED` or `IN PROGRESS` to `ABANDONED`,
and merges it as a record.

## What it does

- Confirms the decision to drop the plan is settled and the reason is recorded.
- Sets `Status: ABANDONED` and updates `Last updated` (the settled date).
- Swaps the current lifecycle label for `#abandoned`.
- Closes the associated discussion thread.
- Squash-merges the pull request into `main` (with your confirmation).
- Appends the plan to `plans/INDEX.md`, in implementation order.

## How to invoke

> Abandon plan

Run it on a `plan/<slug>` branch, or from `main` to pick from the open plan PRs.

## Recommended models

A fast, inexpensive model is enough. This is a status transition and merge,
with the actual decision to abandon already made by you.

## Notes

An abandoned plan is preserved permanently — its value is the record of why it
was dropped. A plan still being written (never agreed) should have its PR closed
instead of being recorded as abandoned.
