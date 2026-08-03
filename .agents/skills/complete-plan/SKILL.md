---
name: complete-plan
description: >-
  Mark an in-progress plan as done once every task has shipped. Use this skill
  when the user says something like "complete this plan", "the plan is done",
  "all tasks shipped", or "finish the plan".
license: CC0-1.0
metadata:
  interactive: yes
  preferred_model: ollama/WORKFLOW_STANDARD
---

# Complete plan

Progress a delivery plan from `IN PROGRESS` to `DONE`, landing the plan in
the `main` trunk.

## Parameters

Determine the following information from the surrounding context and
environment, if possible.

- **Target — REQUIRED.** Infer the plan from the checked-out branch
  (`plan/<slug>`). If on `main`, list the `#in-progress` pull requests and ask
  the user to choose.

## Success criteria

<!-- The plan document updated to `Status: DONE`, the PR carrying `#done` and
squash-merged into `main`, its discussion thread closed, and a new row
appended to `plans/INDEX.md`. -->

- `Status` MUST be `DONE` and `Last updated` MUST be today's date.

- The PR MUST carry `#done`, not `#in-progress`, and MUST be squash-merged
  into `main`.

- The associated discussion thread MUST be closed.

- After merge, a `plans/INDEX.md` row MUST be appended on `main`, with `Done`
  status, in implementation order.

## Instructions

1.  Identify the plan and confirm it is `IN PROGRESS`.

    Infer the target from the current checked-out branch (`plan/<slug>`). If
    on `main`, list the `#in-progress` pull requests and ask the user to
    choose:

    ```sh
    gh pr list --label "#in-progress" --json number,title,headRefName
    ```

    Read the document. Check `Status` is `IN PROGRESS` and the PR carries
    `#in-progress`.

2.  Verify the rules.

    Follow each task's tracker link to confirm it has shipped. Report any
    unmet rule and stop.

3.  Update the document.

    Set `Status` to `DONE` and `Last updated` to today's date (this becomes the
    settled date in the index).

4.  Swap the label.

    ```sh
    gh pr edit <number> --add-label "#done" --remove-label "#in-progress"
    ```

5.  Commit and push.

    The push is mandatory: the merge in the next step lands the *remote*
    branch, so an unpushed commit would leave `Status: DONE` behind.

    ```sh
    git commit -am "chore: complete <short lowercase plan description>"
    git push
    ```

6.  Merge the pull request.

    Confirm with the user that the PR is ready to merge — do not merge without
    explicit instruction. Once confirmed, squash-merge it and delete the source
    branch on the upstream repository:

    ```sh
    gh pr merge <number> --squash --subject "plan: <short lowercase plan description> - DONE" --delete-branch
    ```

7.  Delete the branch, if it remains.

    In case the branch was not automatically deleted from the upstream
    repository, delete it directly:

    ```sh
    git push origin --delete plan/<slug>
    ```

8.  Close the associated discussion thread.

    The plan has merged, so its discussion is now closed. Find the discussion
    linked in the `Discussion thread` field, look up its node ID, and close it
    as resolved (`gh` has no native discussion command, so use the GraphQL
    API):

    ```gh
    gh api graphql -f query='
      query($owner:String!, $name:String!, $number:Int!) {
        repository(owner:$owner, name:$name) { discussion(number:$number) { id } }
      }' -F owner=<owner> -F name=<repo> -F number=<discussionNumber>

    gh api graphql -f query='
      mutation($id:ID!) {
        closeDiscussion(input:{discussionId:$id, reason:RESOLVED}) { discussion { closed } }
      }' -F id=<discussionId>
    ```

9.  After merge, append to the index.

    On `main`, append a row to [`plans/INDEX.md`](../../../plans/INDEX.md): the
    plan's title, `Done` status, its target repositories, and the settled date
    (the document's `Last updated`). Append at the end — the index is ordered
    by implementation. The plan directory is never renamed; no number is
    assigned.

    ```sh
    git commit -am "chore: record <short lowercase plan description> in plan index"
    git push
    ```

## Rules

- You MUST NOT complete a plan that is not currently `IN PROGRESS`.

  Never complete a plan that is still in `DRAFT` or `PLANNED`.

- Every task MUST have shipped.

  Each task's linked tracker item is closed/merged. Follow the links to
  confirm — do not rely on the plan document, which does not record live
  status.

- The breakdown MUST reflect what was actually done.

  Any tasks added, dropped, or re-sequenced during implementation are
  reflected in the final breakdown and dependency graph.

- You MUST confirm every task has shipped via its linked tracker, not the
  plan document.

  The plan document does not record live status.

- You MUST push before merging.

  `gh pr merge` merges what is on the remote. A status change committed
  locally but not pushed is silently dropped from `main`, leaving the merged
  plan still reading `IN PROGRESS`.

- You MUST NOT merge without explicit instruction from the user.

- You MUST NOT delete the plan.

  A completed plan is a permanent record.
