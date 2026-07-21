---
name: complete-plan
description: >-
  Mark an in-progress plan as done once every task has shipped — set its
  status, label the PR, squash-merge it, and append it to the index. Use when
  the user says "complete this plan", "the plan is done", "all tasks
  shipped", or "finish the plan".
license: MIT
metadata:
  interactive: yes
---

# Complete plan

Use this skill to move a plan from `IN PROGRESS` to `DONE`, once every task in
the plan has shipped. This is the point at which the plan's pull request is
squash-merged into `main`, and the plan is appended to
[`plans/INDEX.md`](../../../plans/INDEX.md) in implementation order.

Do NOT use this skill for any other transition — see
[`/abandon-plan`](../abandon-plan/SKILL.md) to drop a plan, or
[`/draft-plan`](../draft-plan/SKILL.md),
[`/finalize-plan`](../finalize-plan/SKILL.md),
[`/implement-plan`](../implement-plan/SKILL.md).

**Input:** Target — REQUIRED. Infer the plan from the checked-out branch
(`plan/<slug>`). If on `main`, list the `#in-progress` pull requests and ask
the user to choose.

**Output:** The plan document updated to `Status: DONE`, the PR carrying
`#done` and squash-merged into `main`, its discussion thread closed, and a new
row appended to `plans/INDEX.md`.

## Transition gates: `IN PROGRESS` → `DONE`

The plan MUST currently be `IN PROGRESS` (a PR carrying `#in-progress`). Confirm
**all** of the following before completing. If any is unmet, report it and
pause.

-   **Every task has shipped.**

    Each task's linked tracker item is closed/merged. Follow the links to
    confirm — do not rely on the plan document, which does not record live
    status.

-   **The breakdown reflects what was actually done.**

    Any tasks added, dropped, or re-sequenced during implementation are
    reflected in the final breakdown and dependency graph.

## Instructions

1.  **Identify the plan and confirm it is `IN PROGRESS`.**

    Infer the target from the current checked-out branch (`plan/<slug>`). If on
    `main`, list the `#in-progress` pull requests and ask the user to choose:

    ```sh
    gh pr list --label "#in-progress" --json number,title,headRefName
    ```

    Read the document. Check `Status` is `IN PROGRESS` and the PR carries
    `#in-progress`.

2.  **Verify the transition gates above.**

    Follow each task's tracker link to confirm it has shipped. Report any unmet
    gate and stop.

3.  **Update the document.**

    Set `Status` to `DONE` and `Last updated` to today's date (this becomes the
    settled date in the index).

4.  **Swap the label.**

    ```sh
    gh pr edit <number> --add-label "#done" --remove-label "#in-progress"
    ```

5.  **Commit.**

    ```sh
    git commit -am "chore: complete <short lowercase plan description>"
    ```

6.  **Merge the pull request.**

    Confirm with the user that the PR is ready to merge — do not merge without
    explicit instruction. Once confirmed, squash-merge it:

    ```sh
    gh pr merge <number> --squash --subject "plan: <short lowercase plan description> - DONE"
    ```

7.  **Close the associated discussion thread.**

    The plan has merged, so its discussion is now closed. Find the discussion
    linked in the `Discussion thread` field, look up its node ID, and close it
    as resolved (`gh` has no native discussion command, so use the GraphQL API):

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

    On `main`, append a row to [`plans/INDEX.md`](../../../plans/INDEX.md): the
    plan's title, `Done` status, its target repositories, and the settled date
    (the document's `Last updated`). Append at the end — the index is ordered by
    implementation. The plan directory is never renamed; no number is assigned.

    ```sh
    git commit -am "chore: record <short lowercase plan description> in plan index"
    git push
    ```

## Rules

-   **Only from `IN PROGRESS`.**

    Never complete a plan that is still in `DRAFT` or `PLANNED`.

-   **Done means every task shipped.**

    Confirm via the linked trackers, not the plan document.

-   **Do not merge without instruction.**

-   **Never delete the plan.**

    A completed plan is a permanent record.

## Success criteria

- `Status` is `DONE` and `Last updated` is today's date.

- The PR carries `#done`, not `#in-progress`, and is squash-merged into `main`.

- The associated discussion thread is closed.

- After merge: a `plans/INDEX.md` row is appended on `main`, with `Done` status,
  in implementation order.

## References

- [General reference information for agents](../../../AGENTS.md)
