---
name: complete-plan
description: >-
  Mark an in-progress plan as done once every task has shipped, and land it in
  the main trunk. Use this skill when the user says something like "complete
  this plan", "the plan is done", "all tasks shipped", or "finish the plan".
  Do not use this skill for a plan that is being dropped before completion.
compatibility: >-
  requires Read, Edit, Bash (git, gh)
license: CC0-1.0
---

# Complete plan

Progress a delivery plan from `IN PROGRESS` to `DONE`, landing it in the
`latest/main` trunk and recording it in the plan index. Do not complete a plan
whose tasks have not all shipped — check each task's tracker rather than the
plan document, which never records live status.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Target plan — REQUIRED.** Infer it from the checked-out branch
  (`latest/plan/<slug>`). If on `latest/main`, list the `#in-progress` pull
  requests and ask the user to choose.

- **Merge confirmation — REQUIRED.** Explicit instruction from the user that
  the pull request may be merged. Never assume it.

## Success criteria

- `Status` MUST be `DONE` and `Last updated` MUST be today's date, in the plan
  document at `plans/<slug>/README.md`.

- The pull request MUST carry `#done` in place of `#in-progress`, and MUST be
  squash-merged into `latest/main` with the subject `update: <description> - DONE`.

- The plan's branch MUST be deleted from the upstream repository.

- The associated discussion thread MUST be closed.

- `plans/INDEX.md` on `latest/main` MUST carry a new row for the plan, with
  `Done` status, appended at the end of the table.

- The plan document MUST NOT have been deleted or its directory renamed, and
  no code repository MUST have been touched.

## Instructions

1.  Identify the plan and its pull request.

    Infer the target from the checked-out branch (`latest/plan/<slug>`). If on
    `latest/main`, list the `#in-progress` pull requests and ask the user to
    choose:

    ```sh
    gh pr list --label "#in-progress" --json number,title,headRefName
    ```

    Check out the branch and read `plans/<slug>/README.md`.

2.  Confirm the plan is currently `IN PROGRESS` and the pull request carries
    the `#in-progress` label.

3.  Verify every rule below. Follow each task's tracker link to confirm it has
    shipped. Report any unmet rule and stop without changing anything.

4.  Set `Status` to `DONE` and `Last updated` to today's date. This becomes
    the settled date recorded in the plan index.

5.  Swap the lifecycle label.

    ```sh
    gh pr edit <number> --add-label "#done" --remove-label "#in-progress"
    ```

6.  Commit and push.

    ```sh
    git commit -am "chore: complete <description>"
    git push
    ```

    `<description>` is the short lowercase description in the pull request
    title, after the `create: ` prefix.

7.  Merge the pull request, once the user has confirmed it may be merged.
    Squash-merge it and delete the source branch upstream:

    ```sh
    gh pr merge <number> --squash --delete-branch \
      --subject "update: <description> - DONE"
    ```

8.  Delete the branch directly, if it survived the merge.

    ```sh
    git push origin --delete latest/plan/<slug>
    ```

9.  Close the discussion thread linked from the document's
    `Discussion thread` field. `gh` has no native discussion command, so look
    up the thread's node ID and close it as resolved via the GraphQL API:

    ```sh
    gh api graphql -f query='
      query($owner:String!, $name:String!, $number:Int!) {
        repository(owner:$owner, name:$name) {
          discussion(number:$number) { id }
        }
      }' -F owner=<owner> -F name=<repo> -F number=<discussionNumber>

    gh api graphql -f query='
      mutation($id:ID!) {
        closeDiscussion(input:{discussionId:$id, reason:RESOLVED}) {
          discussion { closed }
        }
      }' -F id=<discussionId>
    ```

10. On `latest/main`, append a row to `plans/INDEX.md`: the plan's title, `Done`
    status, its target repositories, and the settled date. Append at the end —
    the index is ordered by implementation, not alphabetically or by date.

    ```sh
    git commit -am "chore: record <description> in plan index"
    git push
    ```

11. Output a summary of your actions.

## Rules

- You MUST NOT complete a plan that is not currently `IN PROGRESS`.

  This skill only moves `IN PROGRESS` → `DONE`. A plan in `DRAFT` or
  `PLANNED` has not been worked on, so there is nothing to complete.

- Every task MUST have shipped, confirmed through its linked tracker item.

  Follow each link and check the issue or pull request is closed or merged.
  The plan document deliberately does not record live task status, so it
  cannot be used as evidence.

- The task breakdown MUST reflect what was actually done.

  Any tasks added, dropped, or re-sequenced during implementation are
  reflected in the final breakdown and dependency graph before the merge.

- You MUST push before merging.

  `gh pr merge` merges what is on the remote. A status change committed
  locally but not pushed is silently dropped, leaving the merged plan still
  reading `IN PROGRESS` on `latest/main`.

- You MUST NOT merge without explicit instruction from the user.

  The merge is irreversible in practice: the branch is deleted and the
  discussion thread closed in the same sequence.

- You MUST NOT delete the plan document or rename its directory.

  A completed plan is a permanent record, and the slug is its identity.
