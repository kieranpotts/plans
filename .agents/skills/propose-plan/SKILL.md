---
name: propose-plan
description: >-
  Mark a draft delivery plan ready for review once the decomposition work is
  complete. Use this skill when the user says something like "propose plan",
  "this plan is ready", "mark the plan ready", "the breakdown is done", or
  "take the plan out of draft". Do not use this skill to write the task
  breakdown, nor to start or settle a plan.
compatibility: >-
  requires Read, Edit, Bash (git, gh)
license: CC0-1.0
---

# Propose plan

Progress a delivery plan from `DRAFT` to `PLANNED`, by checking the task
breakdown is complete and taking its pull request out of draft. Do not write
or repair the breakdown yourself — report what is missing and stop.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Target plan — REQUIRED.** Infer it from the checked-out branch
  (`latest/plan/<slug>`). If on `latest/main`, list the open draft pull requests
  and ask the user to choose.

## Success criteria

- `Status` MUST be `PLANNED` and `Last updated` MUST be today's date, in the
  plan document at `plans/<slug>/README.md`.

- The pull request MUST carry the `#planned` label.

- The pull request MUST no longer be a draft (`isDraft: false`).

- The discussion thread MUST remain open, since review feedback continues to
  gather there.

- The plan's prose, task breakdown, and dependency graph MUST be unchanged by
  this skill, and the pull request MUST NOT be merged.

## Instructions

1.  Identify the plan and its pull request.

    Infer the target from the checked-out branch (`latest/plan/<slug>`). If on
    `latest/main`, list the open draft pull requests and ask the user to choose:

    ```sh
    gh pr list --draft --json number,title,headRefName
    ```

    Check out the branch and read `plans/<slug>/README.md` in full.

2.  Confirm the plan is currently `DRAFT`, and verify every rule below.
    Report any that is unmet and stop without changing anything.

3.  Set `Status` to `PLANNED` and `Last updated` to today's date.

4.  Apply the `#planned` label.

    ```sh
    gh pr edit <number> --add-label "#planned"
    ```

5.  Take the pull request out of draft.

    ```sh
    gh pr ready <number>
    ```

6.  Commit and push.

    ```sh
    git commit -am "chore: mark <description> ready for review"
    git push
    ```

    `<description>` is the short lowercase description in the pull request
    title, after the `create: ` prefix.

7.  Output a summary of your actions.

## Rules

- You MUST NOT propose a plan that is not currently `DRAFT`.

  This skill only moves `DRAFT` → `PLANNED`. A plan MUST NOT move backwards
  or skip states.

- The document MUST be substantively complete.

  `Summary`, `Scope`, and `Approach` carry plan-specific content, not the
  generic placeholder prose carried over from `plans/TEMPLATE.md`.

- The task breakdown MUST be present and well-formed.

  Every task carries a stable ID, names exactly one target repository, links
  to its concrete tracker item there, and has its `Depends on` column filled.

- The dependency graph MUST match the task breakdown.

  Every edge in the `Dependency graph` corresponds to a `Depends on` entry in
  the table, and vice versa. An incomplete or inconsistent plan cannot be
  sequenced or reviewed, which is why this gate is mandatory.

- There MUST be no leftover template text.

  No italic placeholder prompts and no unfilled tokens (`#...`,
  `YYYY-MM-DD`, the `T01` / `owner/repo` example rows) remain.

- The metadata header MUST be filled in.

  `Authors`, `Created`, `Last updated`, `Plan PR`, `Target repositories`, and
  `Discussion thread` are all set.

- You MUST NOT edit the plan's content to make it pass the gate.

  Fixing the breakdown yourself would defeat the check. Report the gaps and
  leave them to the user.
