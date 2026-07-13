---
name: draft-plan
description: >-
  Scaffold a new delivery plan and open it as a
  draft pull request. Use when the user wants to plan the implementation of a body
  of work, or says "draft a plan", "new plan", "start a plan", or "plan this".
license: MIT
metadata:
  interactive: yes
---

# `/draft-plan`

Use this skill to start a new delivery plan: scaffold the branch and document
from the template, then open a draft pull request with the artifacts in place,
ready for the user to complete.

This is the entry point to the plan lifecycle. The PR stays a draft while the
user writes it.

Do NOT use this skill to advance an existing plan. See
[`/finalize-plan`](../finalize-plan/SKILL.md),
[`/implement-plan`](../implement-plan/SKILL.md),
[`/complete-plan`](../complete-plan/SKILL.md), or
[`/abandon-plan`](../abandon-plan/SKILL.md).

## Instructions

1.  **Capture the plan.**

    Establish the goal of the plan, its scope, and the code repositories it is
    expected to touch. If the user did not provide this, prompt them for it.

2.  **Create a short, descriptive slug.**

    For example, a plan to harden the checkout flow against duplicate
    submissions might have the description "checkout hardening" and the slug
    "checkout-hardening". Confirm with the user if unsure.

3.  **Create the branch.**

    ```sh
    git checkout main
    git pull
    git checkout -b plan/<slug>
    ```

4.  **Create the plan directory from the template.**

    Copy `plans/TEMPLATE.md` to `plans/<slug>/README.md`.

    The plan lives in its own directory, so the user can add supporting
    artifacts (sequence diagrams, data, mock-ups) alongside the `README.md`.

5.  **Fill in the metadata header.**

    - `Authors`: the Git user's name and GitHub handle (run `git config
      user.name` if needed).
    - `Created` and `Last updated`: today's date in `YYYY-MM-DD` format.
    - `Status`: `DRAFT`.
    - `Target repositories`: the repositories the plan is expected to touch, if
      known.
    - Leave `Plan PR` and `Discussion thread` to be filled once the PR and
      thread exist.

    Leave the prose sections (`Summary`, `Scope`, `Approach`) and the `Task
    breakdown` / `Dependency graph` as template placeholders for the user to
    complete. You MAY seed the `References` section with any upstream artifacts
    the user named (spec proposals, RFCs, design docs).

6.  **Commit and open a draft pull request.**

    ```sh
    git add plans/<slug>/
    git commit -m "plan: <short lowercase plan description>"
    git push -u origin plan/<slug>
    gh pr create --draft --title "plan: <short lowercase plan description>" --fill
    ```

    Record the PR number in the document's `Plan PR` field, then commit and
    push:

    ```sh
    git commit -am "chore: link plan PR"
    git push
    ```

7.  **Open a discussion thread.**

    Every plan PR MUST have an associated discussion thread, where all review
    feedback is gathered. `gh` has no native discussion command, so use the
    GraphQL API. Look up the repository ID and the `Plans` discussion category:

    ```sh
    gh api graphql -f query='
      query($owner:String!, $name:String!) {
        repository(owner:$owner, name:$name) {
          id
          discussionCategories(first:20) { nodes { id name } }
        }
      }' -F owner=<owner> -F name=<repo>
    ```

    Create the discussion, referencing the PR, and capture its URL:

    ```sh
    gh api graphql -f query='
      mutation($repoId:ID!, $categoryId:ID!, $title:String!, $body:String!) {
        createDiscussion(input:{repositoryId:$repoId, categoryId:$categoryId, title:$title, body:$body}) {
          discussion { url }
        }
      }' -F repoId=<repoId> -F categoryId=<categoryId> \
        -f title="plan: <short lowercase plan description>" \
        -f body="Discussion thread for the **<short lowercase plan description>** plan (PR #<number>). Please leave all feedback here, not on the pull request."
    ```

    Record the returned URL in the document's `Discussion thread` field, and add
    it to the pull request description, so the two cross-reference each other:

    ```sh
    gh pr edit <number> --body "$(gh pr view <number> --json body -q .body)

    Discussion thread: <discussionUrl> — Please leave all review feedback there, not on this pull request."
    ```

    Then commit and push the document change:

    ```sh
    git commit -am "chore: link discussion thread"
    git push
    ```

## Rules

-   **A plan is for a coherent body of work.**

    If the request decomposes into a single task, it does not need a plan; the
    user should open a ticket directly in the relevant code repository. Say so
    before scaffolding.

-   **One plan per branch and pull request.**

    Never bundle multiple plans into one branch. If the user describes several
    independent bodies of work, scaffold separate plan branches.

-   **Branch from `main`, not from any other branch.**

    If the local `main` is behind the remote, pull first.

-   **Open the PR as a draft.**

    A new plan is not yet ready for review. It MUST be opened as a draft pull
    request, carrying no lifecycle label.

-   **Every plan PR has an associated discussion thread.**

    Opened with the PR (even as a draft) using the `Plans` discussion category,
    and linked from both the document's `Discussion thread` field and the PR.
    All review feedback belongs in the discussion, not the PR's comments.

-   **Do not assign a numeric ID.**

    Plans have no numeric ID. The slug is the identity. Plans are recorded in
    `plans/INDEX.md` only after merge.

## Success criteria

- Branch `plan/<slug>` exists and is checked out.

- `plans/<slug>/README.md` exists, a copy of `TEMPLATE.md` with the metadata
  header filled in and `Status: DRAFT`.

- A draft pull request is open, titled `plan: <short lowercase plan
  description>`, carrying no lifecycle label.

- An associated discussion thread is open, linked from the document's
  `Discussion thread` field and from the PR.

## References

- [General reference information for agents](../../../AGENTS.md)
