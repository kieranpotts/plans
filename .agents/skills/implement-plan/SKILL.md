---
name: implement-plan
description: >-
  Mark a plan as in-progress once implementation has started. Use this skill
  when the user says something like "implement plan", "start this plan",
  "work has begun", "the plan is underway", or "move the plan to in progress".
  Do not use this skill to carry out the implementation work itself, which is
  delivered task by task in the target code repositories.
compatibility: >-
  requires Read, Edit, Bash (git, gh)
license: CC0-1.0
---

# Implement plan

Progress a delivery plan from `PLANNED` to `IN PROGRESS`, recording that work
on it has begun. Despite its name, this skill changes only the plan document
and its pull request. It MUST NOT write, review, or ship any of the plan's
tasks.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Target plan — REQUIRED.** Infer it from the checked-out branch
  (`latest/plan/<slug>`). If on `latest/main`, list the `#planned` pull requests
  and ask the user to choose.

## Success criteria

- `Status` MUST be `IN PROGRESS` and `Last updated` MUST be today's date, in
  the plan document at `plans/<slug>/README.md`.

- The pull request MUST carry `#in-progress` in place of `#planned`.

- The discussion thread MUST remain open, since feedback continues to gather
  there while the plan evolves.

- No code repository MUST have been touched, and no task tracker item MUST
  have been opened, edited, or closed. The only change is to this repository's
  plan document and its pull request.

- The pull request MUST NOT be merged, since `IN PROGRESS` is not a terminal
  state.

## Instructions

1.  Identify the plan and its pull request.

    Infer the target from the checked-out branch (`latest/plan/<slug>`). If on
    `latest/main`, list the `#planned` pull requests and ask the user to choose:

    ```sh
    gh pr list --label "#planned" --json number,title,headRefName
    ```

    Check out the branch and read `plans/<slug>/README.md`.

2.  Confirm the plan is currently `PLANNED` and that the pull request carries
    the `#planned` label.

    ```sh
    gh pr view <number> --json labels
    ```

3.  Verify every rule below. Report any that is unmet and stop without
    changing anything.

4.  Set `Status` to `IN PROGRESS` and `Last updated` to today's date.

5.  Swap the lifecycle label.

    ```sh
    gh pr edit <number> --add-label "#in-progress" --remove-label "#planned"
    ```

6.  Commit and push.

    ```sh
    git commit -am "chore: start <description>"
    git push
    ```

    `<description>` is the short lowercase description in the pull request
    title, after the `create: ` prefix.

7.  Output a summary of your actions.

## Rules

- You MUST NOT start a plan that is not currently `PLANNED`.

  This skill only moves `PLANNED` → `IN PROGRESS`. A plan MUST NOT move
  backwards or skip states.

- Work MUST have actually begun.

  At least one task is underway in its linked tracker. Moving a plan to
  `IN PROGRESS` signals that the work is live, not merely agreed.

- The task breakdown MUST be settled enough to build from.

  Initial review has concluded. The discussion thread nevertheless stays
  open, because feedback may continue as the plan evolves.

- You MUST NOT implement any of the plan's tasks.

  Each task is delivered through its linked tracker in its own code
  repository, by whoever owns that work. This skill's entire remit is the
  state transition; it stops at the boundary of this repository.

- You MUST NOT merge the pull request at this transition.

  The pull request is merged only once the plan is done or abandoned.

- The task breakdown and dependency graph MAY continue to evolve while the
  plan is `IN PROGRESS`.

  Tasks may be added, dropped, or re-sequenced as reality unfolds. Task IDs
  are stable, so never repurpose one. Live task status stays in each task's
  linked tracker, never in the plan document.
