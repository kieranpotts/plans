---
name: finalize-plan
description: >-
  Mark a draft delivery plan ready for review once its decomposition is
  complete. Use when the user says "this plan is ready", "mark the plan
  ready", "the breakdown is done", or "take the plan out of draft".
license: MIT
metadata:
  interactive: yes
  preferred_model: prose-writing
---

# Finalize plan

Use this skill to move a plan from `DRAFT` to `PLANNED`: confirm the breakdown
and dependency graph are complete, apply the `#planned` label, and remove the
pull request's draft status so the plan can be reviewed.

Do NOT use this skill to scaffold a new plan (use
[`/scaffold-plan`](../scaffold-plan/SKILL.md)) or to advance an agreed plan (use
[`/implement-plan`](../implement-plan/SKILL.md),
[`/complete-plan`](../complete-plan/SKILL.md), or
[`/abandon-plan`](../abandon-plan/SKILL.md)).

## Input

Determine the following information from the surrounding context and
environment, if possible.

- Target — REQUIRED. Infer the plan from the checked-out branch
  (`plan/<slug>`). If on `main`, list open draft pull requests and ask the user
  to choose.

## Output

The plan document updated to `Status: PLANNED`, the PR carrying `#planned` and
taken out of draft.

## Instructions

1.  Identify the plan and its PR.

    Infer the target from the current checked-out branch (`plan/<slug>`). If
    on `main`, list open draft pull requests and ask the user to choose:

    ```sh
    gh pr list --draft --json number,title,headRefName
    ```

    Then checkout the branch and read the plan document
    (`plans/<slug>/README.md`).

2.  Verify the rules.

    Read the document in full, check each rule, and report any failures. Stop
    if unmet.

3.  Update the document.

    Set `Status` to `PLANNED` and `Last updated` to today's date.

4.  Apply the `#planned` label.

    ```sh
    gh pr edit <number> --add-label "#planned"
    ```

5.  Remove the draft status.

    ```sh
    gh pr ready <number>
    ```

6.  Commit and push.

    ```sh
    git commit -am "chore: mark <short lowercase plan description> ready for review"
    git push
    ```

## Rules

- You MUST NOT mark a PR ready until the breakdown is complete.

  An incomplete plan cannot be sequenced or reviewed. The completeness gate is
  mandatory.

- The document MUST be reasonably complete.

  `Summary`, `Scope`, and `Approach` contain substantive, plan-specific
  content — not the generic placeholder prose carried over from `TEMPLATE.md`.

- The task breakdown MUST be present and well-formed.

  Every task has a stable ID, a target repository, and a link to its concrete
  tracker item there. No task names more than one repository. The `Depends on`
  column is filled.

- The dependency graph MUST match the breakdown.

  Every edge in the `Dependency graph` corresponds to a `Depends on` entry in
  the table, and vice versa.

- There MUST be no leftover template text.

  No italic placeholder prompts, no unfilled tokens (`#...`, `YYYY-MM-DD`,
  `T01`/`owner/repo` example rows), remain.

- The metadata header MUST be filled in.

  `Authors`, `Created`, `Last updated`, `Plan PR`, and `Target repositories`
  are set; the `Discussion thread` field links the thread (which stays open
  until the plan is done or abandoned); `Status` is `DRAFT` (this skill
  advances it to `PLANNED`).

- You MUST NOT use this skill to start or settle the plan.

  This skill only moves `DRAFT` → `PLANNED`.

## Success criteria

- The PR is no longer a draft (`isDraft: false`).

- The `#planned` label is applied.

- `Last updated` is today's date and `Status` is `PLANNED`.
