---
name: abandon-plan
description: >-
  Drop a plan before completion. Use this skill when the user says something
  like "abandon this plan", "drop the plan", "cancel the plan", or
  "we're not doing this".
license: CC0-1.0
metadata:
  interactive: yes
  preferred_model: ollama/prose-writing
---

# Abandon plan

Use this skill to abandon a plan from either `PLANNED` or `IN PROGRESS`, when it
is dropped before completion. The plan is merged into `main` as a permanent
record of the decision and appended to
[`plans/INDEX.md`](../../../plans/INDEX.md).

## Parameters

Determine the following information from the surrounding context and
environment, if possible.

- **Target — REQUIRED.** Infer the plan from the checked-out branch
  (`plan/<slug>`). If on `main`, list the open plan pull requests and ask the
  user to choose.

- **Reason for dropping the plan — REQUIRED.** Recorded in the document if not
  already present.

## Success criteria

You will achieve the following outcomes:

<!-- The plan document updated to `Status: ABANDONED`, the PR carrying `#abandoned`
and squash-merged into `main`, its discussion thread closed, and a new row
appended to `plans/INDEX.md`. -->

- `Status` is `ABANDONED` and `Last updated` is today's date.

- The PR carries `#abandoned`, and is squash-merged into `main`.

- The associated discussion thread is closed.

- After merge: a `plans/INDEX.md` row is appended on `main`, with `Abandoned`
  status, in implementation order.

## Instructions

1.  Identify the plan and its current state.

    Infer the target from the current checked-out branch (`plan/<slug>`). If
    on `main`, list the open plan pull requests and ask the user to choose:

    ```sh
    gh pr list --label "#planned" --label "#in-progress" --json number,title,headRefName,labels
    ```

    Read the document. Confirm `Status` is `PLANNED` or `IN PROGRESS`, and
    note which lifecycle label the PR carries.

2.  Verify the rules.

    Ensure the reason for abandonment is recorded in the document. Report any
    unmet rule and stop.

3.  Update the document.

    Set `Status` to `ABANDONED` and `Last updated` to today's date (this
    becomes the settled date in the index).

4.  Swap the label.

    Remove whichever lifecycle label the PR currently carries:

    ```sh
    gh pr edit <number> --add-label "#abandoned" --remove-label "#planned"      # or --remove-label "#in-progress"
    ```

5.  Commit and push.

    The push is mandatory: the merge in the next step lands the *remote*
    branch, so an unpushed commit would leave `Status: ABANDONED` and the
    recorded reason behind.

    ```sh
    git commit -am "chore: abandon <short lowercase plan description>"
    git push
    ```

6.  Merge the pull request.

    Confirm with the user that the PR is ready to merge — do not merge without
    explicit instruction. Once confirmed, squash-merge it and delete the source
    branch on the upstream repository:

    ```sh
    gh pr merge <number> --squash --subject "plan: <short lowercase plan description> - ABANDONED" --delete-branch
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
    plan's title, `Abandoned` status, its target repositories, and the settled
    date. Append at the end. The plan directory is never renamed; no number is
    assigned.

    ```sh
    git commit -am "chore: record <short lowercase plan description> in plan index"
    git push
    ```

## Rules

- You MUST NOT abandon a plan that is not currently `PLANNED` or `IN PROGRESS`.

  A done plan cannot be abandoned, and a draft plan that was never agreed
  should simply have its PR closed rather than recorded as abandoned.

- The decision to drop the plan MUST be settled.

  Abandonment is a deliberate decision, not a pause. If the plan is merely
  stalled, leave it where it is.

- The reason for abandonment MUST be recorded in the document.

  The plan document captures why it was dropped, so the record explains
  itself. Add a short note to the `Summary` or `Open questions` section if one
  is not already present.

- You MUST NOT merge without the reason for abandonment recorded in the
  document.

  An abandoned plan's value is the record of why.

- You MUST push before merging.

  `gh pr merge` merges what is on the remote. The status change and the
  recorded reason for abandonment MUST both be on the remote branch before
  the merge — an abandoned plan's whole value is the record of why.

- You MUST NOT merge without explicit instruction from the user.

- You MUST NOT delete the plan.

  An abandoned plan is preserved permanently, exactly like a completed one.
