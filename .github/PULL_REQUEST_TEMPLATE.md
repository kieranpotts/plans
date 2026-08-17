Copy the "Summary" section from the plan document here.

- Discussion thread: [Link] (REQUIRED)

> [!IMPORTANT]
> Please use the discussion thread linked above, not comments on this pull
> request, to provide feedback on this plan. This keeps the PR thread focused on
> the edit history of the plan document.

----

## Checklist

The author(s) of the plan MUST update this checklist as it moves through its
lifecycle. They MUST NOT merge this PR before the plan is decided — either
`DONE` (every task has shipped) or `ABANDONED`. A plan stays open through
implementation. See the [contributing guidelines](../CONTRIBUTING.md) for more
details about state transitions.

On opening this PR (open it as a draft):

- [ ] The branch is named `latest/plan/<slug>`.
- [ ] The plan is saved at `plans/<slug>/README.md`, with the metadata header
  filled in.
- [ ] Supporting artifacts live under `plans/<slug>/` and are referenced from
  the plan's `README.md`.
- [ ] An associated discussion thread is opened and linked (both above and in
  the plan document's `Discussion thread` field).
- [ ] The plan document's `Status` is `DRAFT`.

Move from **`DRAFT`** to **`#planned`** when:

- [ ] The `Task breakdown` is complete: every row has a stable ID, a short
  imperative title, exactly one target repository, a link to its concrete
  tracker item, and its dependencies.
- [ ] The `Dependency graph` matches the `Depends on` column.
- [ ] No generic template text or unfilled placeholders remain.
- [ ] The plan is written in American English.
- [ ] The plan document's `Status` is updated to `PLANNED`.
- [ ] The PR is marked "ready for review" (taken out of draft status).

Move from **`#planned`** to **`#in-progress`** when:

- [ ] Implementation has started.
- [ ] The plan document's `Status` is updated to `IN PROGRESS`.

Keep the breakdown current while the plan is in progress:

- [ ] Tasks added, dropped, or re-sequenced as reality unfolds, with the
  dependency graph updated to match.
- [ ] Task IDs left untouched — re-sequencing changes the `Depends on` column,
  never the IDs.
- [ ] No live task status recorded in the plan. That belongs in each task's
  linked tracker.

Merge this PR when the plan is decided:

- [ ] Every task has shipped (`DONE`), or the plan is dropped (`ABANDONED`).
- [ ] The plan document's `Status` is updated to `DONE` or `ABANDONED`.
- [ ] The PR is squash-merged, with the commit message
  `update: <description> - DONE|ABANDONED`.
- [ ] The associated discussion thread is closed.
- [ ] _After the merge_, the plan is appended to `plans/INDEX.md` on `latest/main`.

After merging, complete these tasks:

- Delete the `latest/plan/*` branch.

> [!IMPORTANT]
> A plan MUST NOT be merged into `latest/main` until it is decided. Abandoned
> plans are merged too — a plan document is never deleted, and the plan
> directory is never renamed, because the slug is its identity.
