---
name: abandon-plan
description: Drop a plan before completion — record the decision, label the PR, squash-merge it, and append it to the index. Use when the user says "abandon this plan", "drop the plan", "cancel the plan", or "we're not doing this".
license: MIT
---

# Abandon plan

Use this skill to abandon a plan from either `PLANNED` or `IN PROGRESS`, when it is dropped before completion. The plan is merged into `main` as a permanent record of the decision and appended to [`plans/INDEX.md`](../../../plans/INDEX.md).

Do NOT use this skill to complete a plan whose tasks all shipped (use [`complete-plan`](../complete-plan/SKILL.md)), or for any other transition — see [`draft-plan`](../draft-plan/SKILL.md), [`finalize-plan`](../finalize-plan/SKILL.md), [`implement-plan`](../implement-plan/SKILL.md).

## Transition gates: `PLANNED` | `IN PROGRESS` → `ABANDONED`

The plan MUST currently be `PLANNED` or `IN PROGRESS`. Confirm the following before abandoning. If unmet, report it and pause.

-   **The decision to drop the plan is settled.**

    Abandonment is a deliberate decision, not a pause. If the plan is merely stalled, leave it where it is.

-   **The reason is recorded.**

    The plan document captures why it was dropped, so the record explains itself. Add a short note to the `Summary` or `Open questions` section if one is not already present.

## Instructions

1.  **Identify the plan and its current state.**

    Infer the target from the current checked-out branch (`plan/<slug>`). If on `main`, list the open plan pull requests and ask the user to choose:

    ```sh
    gh pr list --label "#planned" --label "#in-progress" --json number,title,headRefName,labels
    ```

    Read the document. Confirm `Status` is `PLANNED` or `IN PROGRESS`, and note which lifecycle label the PR carries.

2.  **Verify the transition gates above.**

    Ensure the reason for abandonment is recorded in the document. Report any unmet gate and stop.

3.  **Update the document.**

    Set `Status` to `ABANDONED` and `Last updated` to today's date (this becomes the settled date in the index).

4.  **Swap the label.**

    Remove whichever lifecycle label the PR currently carries:

    ```sh
    gh pr edit <number> --add-label "#abandoned" --remove-label "#planned"      # or --remove-label "#in-progress"
    ```

5.  **Commit.**

    ```sh
    git commit -am "chore: abandon <short lowercase plan description>"
    ```

6.  **Merge the pull request.**

    Confirm with the user that the PR is ready to merge — do not merge without explicit instruction. Once confirmed, squash-merge it:

    ```sh
    gh pr merge <number> --squash --subject "plan: <short lowercase plan description> - ABANDONED"
    ```

7.  **Close the associated discussion thread.**

    The plan has merged, so its discussion is now closed. Find the discussion linked in the `Discussion thread` field, look up its node ID, and close it as resolved (`gh` has no native discussion command, so use the GraphQL API):

    ```sh
    gh api graphql -f query='
      query($owner:String!, $name:String!, $number:Int!) {
        repository(owner:$owner, name:$name) { discussion(number:$number) { id } }
      }' -F owner=<owner> -F name=<repo> -F number=<discussionNumber>

    gh api graphql -f query='
      mutation($id:ID!) {
        closeDiscussion(input:{discussionId:$id, reason:RESOLVED}) { discussion { closed } }
      }' -F id=<discussionId>
    ```

8.  **After merge, append to the index.**

    On `main`, append a row to [`plans/INDEX.md`](../../../plans/INDEX.md): the plan's title, `Abandoned` status, its target repositories, and the settled date. Append at the end. The plan directory is never renamed; no number is assigned.

    ```sh
    git commit -am "chore: record <short lowercase plan description> in plan index"
    git push
    ```

## Rules

-   **Only from `PLANNED` or `IN PROGRESS`.**

    A done plan cannot be abandoned, and a draft plan that was never agreed should simply have its PR closed rather than recorded as abandoned.

-   **Record the reason.**

    An abandoned plan's value is the record of why. Do not merge without it.

-   **Do not merge without instruction.**

-   **Never delete the plan.**

    An abandoned plan is preserved permanently, exactly like a completed one.

## Success criteria

- `Status` is `ABANDONED` and `Last updated` is today's date.

- The PR carries `#abandoned`, and is squash-merged into `main`.

- The associated discussion thread is closed.

- After merge: a `plans/INDEX.md` row is appended on `main`, with `Abandoned` status, in implementation order.

## References

- [General reference information for agents](../../../AGENTS.md)
