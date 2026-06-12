---
name: implement-plan
description: Mark a plan as in progress once implementation has started. Use when the user says "start this plan", "work has begun", "the plan is underway", or "move the plan to in progress".
license: MIT
---

# Start implementation of plan

Use this skill to move a plan from `PLANNED` to `IN PROGRESS`, once implementation of the plan has begun. The pull request stays open through implementation — it is not merged here.

Do NOT use this skill for any other transition — see [`draft-plan`](../draft-plan/SKILL.md), [`finalize-plan`](../finalize-plan/SKILL.md), [`complete-plan`](../complete-plan/SKILL.md), or [`abandon-plan`](../abandon-plan/SKILL.md).

## Transition gates: `PLANNED` → `IN PROGRESS`

The plan MUST currently be `PLANNED` (a PR carrying `#planned`). Confirm the following before starting. If unmet, report it and pause.

-   **Review has concluded.**

    Feedback gathered on the plan PR's discussion thread has been resolved and the breakdown has stabilized. Starting implementation closes the review.

-   **Work has actually begun.**

    At least one task is underway in its linked tracker. Starting a plan signals that the work is live, not merely agreed.

## Instructions

1.  **Identify the plan and confirm it is `PLANNED`.**

    Infer the target from the current checked-out branch (`plan/<slug>`). If on `main`, list the `#planned` pull requests and ask the user to choose:

    ```sh
    gh pr list --label "#planned" --json number,title,headRefName
    ```

    Read the document. Check `Status` is `PLANNED` and the PR carries `#planned` (`gh pr view <number> --json labels`).

2.  **Verify the transition gate above.**

3.  **Update the document.**

    Set `Status` to `IN PROGRESS` and `Last updated` to today's date.

4.  **Swap the label.**

    ```sh
    gh pr edit <number> --add-label "#in-progress" --remove-label "#planned"
    ```

5.  **Close the associated discussion thread.**

    Review has concluded once implementation begins, so the plan's discussion is closed. Find the discussion linked in the `Discussion thread` field, look up its node ID, and close it as resolved (`gh` has no native discussion command, so use the GraphQL API):

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

6.  **Commit and push.**

    ```sh
    git commit -am "chore: start <short lowercase plan description>"
    git push
    ```

## Rules

-   **Only from `PLANNED`.**

    Never start a draft or already-running plan.

-   **The plan stays open and mutable.**

    While `IN PROGRESS`, the breakdown and dependency graph MAY continue to evolve as reality unfolds — tasks added, dropped, or re-sequenced. Do NOT repurpose a task ID. Do NOT track live task status here; it lives in each task's linked tracker.

-   **Do not merge.**

    The PR is merged only when the plan is done or abandoned.

## Success criteria

- `Status` is `IN PROGRESS` and `Last updated` is today's date.

- The PR carries `#in-progress`, not `#planned`.

- The associated discussion thread is closed.

## References

- [General reference information for agents](../../../AGENTS.md)
