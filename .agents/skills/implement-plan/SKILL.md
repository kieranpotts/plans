---
name: implement-plan
description: >-
  Mark a plan as in-progress once implementation has started. Use this skill
  when the user says something like "implement plan", "start this plan",
  "work has begun", "the plan is underway", or "move the plan to in progress".
license: CC0-1.0
metadata:
  interactive: yes
  preferred_model: ollama/WORKFLOW_BASIC
---

# Implement plan

Use this skill to progress a delivery plan from `PLANNED` to `IN PROGRESS`.

## Parameters

Determine the following information from the surrounding context and
environment, if possible.

- **Target — REQUIRED.** Infer the plan from the checked-out branch
  (`plan/<slug>`). If on `main`, list the `#planned` pull requests and ask the
  user to choose.

## Success criteria

You will achieve the following outcomes:

<!-- The plan document updated to `Status: IN PROGRESS`, the PR carrying
`#in-progress` in place of `#planned`. -->

- `Status` MUST be `IN PROGRESS` and `Last updated` MUST be today's date.

- The PR MUST carry `#in-progress`, not `#planned`.

- The discussion thread MUST remain open.

## Instructions

1.  Identify the plan and confirm it is `PLANNED`.

    Infer the target from the current checked-out branch (`plan/<slug>`). If
    on `main`, list the `#planned` pull requests and ask the user to choose:

    ```sh
    gh pr list --label "#planned" --json number,title,headRefName
    ```

    Read the document. Check `Status` is `PLANNED` and the PR carries `#planned`
    (`gh pr view <number> --json labels`).

2.  Verify the rules.

3.  Update the document.

    Set `Status` to `IN PROGRESS` and `Last updated` to today's date.

4.  Swap the label.

    ```sh
    gh pr edit <number> --add-label "#in-progress" --remove-label "#planned"
    ```

    Leave the discussion thread open — it stays open through implementation,
    as feedback may continue while the plan evolves, and is closed when the PR
    is merged.

5.  Commit and push.

    ```sh
    git commit -am "chore: start <short lowercase plan description>"
    git push
    ```

## Rules

- You MUST NOT start a plan that is not currently `PLANNED`.

  Never start a draft or already-running plan.

- The breakdown MUST be agreed.

  Initial review has settled and the breakdown is stable enough to start
  building. The discussion thread stays open — feedback may continue as the
  plan evolves during implementation.

- Work MUST have actually begun.

  At least one task is underway in its linked tracker. Starting a plan
  signals that the work is live, not merely agreed.

- The breakdown and dependency graph MAY continue to evolve while `IN
  PROGRESS`.

  Tasks may be added, dropped, or re-sequenced as reality unfolds. Do NOT
  repurpose a task ID. Do NOT track live task status here; it lives in each
  task's linked tracker.

- You MUST NOT merge the PR at this transition.

  The PR is merged only when the plan is done or abandoned.
