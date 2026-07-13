# Plans

This directory holds the delivery plans, one directory per plan. A plan is a
self-contained body of work with a goal, a scope, and a decomposition into tasks
that span one or more code repositories.

## Layout

Each plan is a directory:

```
plans/
├── INDEX.md          # The catalog of merged plans, in implementation order.
├── TEMPLATE.md       # The starting point for a new plan.
└── <slug>/           # The root directory for one discrete delivery plan.
    ├── README.md     # The main plan document.
    └── …             # Sequence diagrams, data, mock-ups, or other artifacts.
```

A plan's directory holds its `README.md` – the plan itself, copied from
[`TEMPLATE.md`](./TEMPLATE.md) – plus any supporting artifacts. The slug is the
plan's permanent identity – the directory's are never renamed.

## How it works

1. A plan is opened as a draft pull request, on a `plan/<slug>` branch. The main
   document lives at `plans/<slug>/README.md`.

2. The plan moves through its lifecycle: `DRAFT` → `PLANNED` → `IN PROGRESS` →
   `DONE`, or `PLANNED`/`IN PROGRESS` → `ABANDONED`.

3. The plan decomposes the work into tasks. Each task names one target
   repository and links to its concrete issue or pull request there — that
   linked tracker, not this document, owns the task's live status. The
   `Dependency graph` orders the tasks across repositories.

4. On merge — once the plan is done or abandoned — it is appended to the [plan
   index](./INDEX.md), in implementation order. A plan is never deleted, even if
   abandoned.

See the [contributing guide](../CONTRIBUTING.md) for the full process, and the
[agent skills](../.agents/skills/) that help automate it.
