# [Project Name] – Delivery Plans

The capitalized words REQUIRED, MUST, MUST NOT, RECOMMENDED, SHOULD, SHOULD NOT,
OPTIONAL, and MAY are to be interpreted as described in [IETF RFC
2119](https://www.ietf.org/rfc/rfc2119.txt).

## Project overview

This repository holds the delivery plans for [Project Name] – _when, and in what
order_, changes to the system are implemented. It is a development planning
tool, implemented in a version control system. It is documentation, not code.
There's nothing to build, lint, or run.

A **plan** is a self-contained body of work with a goal, a scope, and a
decomposition into tasks. Each task lives in exactly one code repository and
links out to the concrete issue or pull request there, which owns the task's
live status. This repository tracks the cross-repository decomposition and
sequencing, not the moment-to-moment status of individual tasks.

Unlike the sibling SRS and design repositories, the artifacts here describe
_future and in-flight work_, not the current state of production. A plan is a
mutable working document while it is open, and settles once the work is done or
abandoned. The plan's git history is its record.

## Project structure

- **`plans/`:**
  The delivery plans, one directory per plan (`plans/<slug>/`), each holding its
  `README.md` and any supporting artifacts.

  - **`plans/INDEX.md`** lists plans in the order they were implemented.

  - **`plans/TEMPLATE.md`** is the starting point for a new plan.

- **`docs/`:**
  General guidelines for humans to get the most out of the planning process.

## Plan lifecycle

Each plan moves through a defined state machine. The current state is shown in
the plan document's `Status` field, and tracked via a lifecycle label on its
pull request.

- **`DRAFT`:**
  The plan is being written. The pull request is open as a draft. Not yet ready
  for review.

- **`PLANNED`:**
  The decomposition is complete and open for review. The pull request is marked
  ready for review and labeled `#planned`, with feedback gathered on its
  discussion thread. Work has not yet started.

- **`IN PROGRESS`:**
  Implementation is underway. Tasks are being delivered through their linked
  trackers in the code repositories. The pull request carries `#in-progress`.
  The discussion thread stays open – feedback may continue as the plan evolves.

- **`DONE`:**
  Every task has shipped. The plan is merged into `main` and recorded in
  `plans/INDEX.md` in implementation order.

- **`ABANDONED`:**
  The plan is dropped before completion. It is merged into `main` as a permanent
  record of the decision, recorded in `plans/INDEX.md`.

The only allowed state transitions are:

- (New plan) → DRAFT
- DRAFT → PLANNED
- PLANNED → IN PROGRESS
- IN PROGRESS → DONE
- PLANNED → ABANDONED
- IN PROGRESS → ABANDONED

Transitions not listed above are NOT permitted. A plan MUST NOT skip states (eg.
DRAFT directly to IN PROGRESS).

## Rules

- MUST write in American English.

- The `main` trunk is the default branch. Plans are developed on `plan/<slug>`
  branches cut from `main`, and integrated back into `main` via pull requests
  once the plan is done or abandoned.

- Each plan is a single, coherent body of work. Author it on a `plan/<slug>`
  branch and open a pull request titled `plan: <description>`, where
  `<description>` is a short prose title, written full lowercase (not the
  hyphenated branch slug).

- The plan document MUST NOT track the live status of individual tasks.
  Status lives in the linked code-repo tracker for each task. The `Tracker` link
  is followed to see where a task stands.

- Each task in the breakdown MUST name exactly one target repository and link
  to its concrete issue or pull request there.

- Task IDs (`T01`, `T02`, …) are stable and assigned in creation order,
  not execution order. Re-sequencing changes the `Depends on` column and the
  dependency graph, never the IDs.

- The `Dependency graph` MUST be kept in sync with the `Depends on` column
  of the task table.

- Upstream linkage is loose and reference-only. A plan cites the spec
  proposal(s), RFC(s), and design doc(s) it implements via its `References`
  section. A plan is its own unit and need not map 1:1 to any upstream artifact.

- A plan PR carries exactly one lifecycle label – `#planned`, `#in-progress`,
  `#done`, or `#abandoned`. A pull request is opened initially as a draft while
  the document is refined.

- Every plan PR MUST have an associated discussion thread, opened with the
  PR (even as a draft) and used for all feedback. The thread stays open for the
  life of the plan – through `DRAFT`, `PLANNED`, and `IN PROGRESS` – and is
  closed when the PR is merged.

- A plan MUST NOT be merged into `main` until it is decided – either done
  or abandoned.

- Plan branches MUST be squash-merged to `main`, with the squash commit
  message `plan: <description> - DONE|ABANDONED`.

- After merge, the plan is recorded in [`plans/INDEX.md`](./plans/INDEX.md),
  appended in the order it was implemented (or abandoned). The plan directory is
  never renamed; the slug is its identity. No numeric ID is assigned.

- Never delete a plan document, including abandoned ones. The git history of
  the plan is its permanent record.

## Skills

The [`.agents/skills/`](./.agents/skills/) directory provides on-demand skills
for managing the plan lifecycle. See the [README](./.agents/skills/README.md)
for descriptions of the available skills.
