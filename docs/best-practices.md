# Best practices

Guidance on scoping a plan and writing a task breakdown that earns its keep. For
the lifecycle mechanics, see the [contributing guide](../CONTRIBUTING.md).

## Scoping a plan

- **One plan, one goal.** A plan should have a single, statable outcome — what
  "done" looks like in one sentence. If you cannot write that sentence, it is
  probably two plans.

- **Size it to a coherent body of work, not a single ticket.** A plan that
  decomposes into one task is not needed — open the ticket directly in the code
  repository. A plan that sprawls across a dozen loosely-related goals is too
  big to sequence meaningfully; split it.

- **Name what is out of scope.** The `Scope` section should be explicit about
  boundaries. A clear "not this" is what lets a reader judge whether the
  breakdown is complete.

## Writing the task breakdown

- **One task, one repository.** Each task names exactly one target repository. A
  unit of work that spans two repositories is two tasks with a dependency
  between them.

- **Link to the real tracker.** Every task links to its concrete issue or pull
  request in the code repository. That link owns the live status — do not
  restate status in the plan. If a task has no tracker item yet, that is a
  signal the task is not ready to start.

- **Keep task IDs stable.** IDs (`T01`, `T02`, …) are assigned in creation order
  and never reused or renumbered. When you re-sequence, you change the `Depends
  on` column and the dependency graph — not the IDs. Stable IDs keep
  cross-references and the graph valid as the plan churns.

- **Make dependencies real.** A dependency means task B genuinely cannot start
  until task A is done — a schema must exist before code uses it, an endpoint
  before a client calls it. Do not encode mere preference as dependency;
  over-constraining the graph hides parallelism that would speed the work up.

- **Keep the graph in sync.** The dependency graph is built from the `Depends
  on` column. They must agree. The graph is the highest-value artifact in the
  plan — it is the one view that spans every repository the plan touches — so it
  is worth keeping honest.

## Keeping a plan healthy while it runs

- **Re-plan in the open.** When reality diverges from the plan — a task turns
  out unnecessary, a new dependency appears — update the breakdown and the
  graph. The git history captures the change; that is the record.

- **Don't let the plan become a status board.** Live status belongs in the
  linked trackers. The plan's job is the shape of the work, not its
  moment-to-moment state. If you find yourself editing the plan to mark things
  done, stop — follow the links instead.

- **Settle promptly.** When the last task ships, mark the plan `DONE` and merge
  it. A plan that lingers `IN PROGRESS` after the work is finished erodes trust
  in the index.
