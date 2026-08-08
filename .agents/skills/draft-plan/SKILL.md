---
name: draft-plan
description: >-
  Draft a new delivery plan and open it as a draft pull request. Use this
  skill when the user wants to plan the implementation of a body of work, or
  says something like "draft a plan", "new plan", "start a plan", or
  "plan this". Do not use this skill to write the task breakdown itself, nor
  to advance a plan that already exists.
compatibility: >-
  requires Read, Write, Edit, Bash (git, gh)
license: CC0-1.0
---

# Draft plan

Scaffold a new delivery plan from the template and open it as a draft pull
request, with the branch, metadata header, and discussion thread in place.
Do not decompose the work into tasks — the breakdown is the user's to write.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Goal and scope — REQUIRED.** What the body of work is meant to achieve,
  and where its boundaries lie.

- **Target repositories — REQUIRED.** The code repositories the plan is
  expected to touch. Every task must eventually name exactly one of them, so
  an empty list means the work is not yet understood well enough to plan.

- **Upstream artifacts — OPTIONAL.** Spec proposals, RFCs, or design documents
  that motivate the plan. Seed these into the document's `References` section
  when the user names them.

## Success criteria

- Branch `plan/<slug>` MUST exist and MUST be checked out.

- `plans/<slug>/README.md` MUST exist as a copy of `plans/TEMPLATE.md`, with
  the metadata header filled in and `Status` set to `DRAFT`.

- A draft pull request MUST be open, titled `create: <description>`, carrying
  no lifecycle label.

- A discussion thread MUST be open, linked from the document's
  `Discussion thread` field and from the pull request description.

- The prose sections and the task breakdown MUST remain as template
  placeholders, and `plans/INDEX.md` MUST be unchanged — a plan is indexed
  only once it is merged.

## Instructions

1.  Establish the goal, scope, and target repositories of the work. Prompt the
    user for anything missing.

2.  Write a short description of the plan, in the present tense, full
    lowercase, and not terminated by a period. Keep it in memory; it is used
    verbatim in the branch slug, the commit subjects, and the pull request
    title. This is `<description>` below.

3.  Transform the description into a hyphen-delimited slug. For example,
    "checkout hardening" becomes "checkout-hardening". Confirm the slug with
    the user if unsure.

4.  Create the branch.

    ```sh
    git checkout main
    git pull --rebase
    git checkout -b plan/<slug>
    ```

5.  Copy `plans/TEMPLATE.md` to `plans/<slug>/README.md`.

    The plan lives in its own directory so the user can add supporting
    artifacts — sequence diagrams, data, mock-ups — beside the `README.md`.

6.  Fill in the metadata header:

    - `Authors`: the Git user's name and GitHub handle (run `git config
      user.name` if needed).
    - `Created` and `Last updated`: today's date, in `YYYY-MM-DD` format.
    - `Status`: `DRAFT`.
    - `Target repositories`: the repositories the plan is expected to touch.
    - Leave `Plan PR` and `Discussion thread` blank for now.

    Leave `Summary`, `Scope`, `Approach`, `Task breakdown`, and
    `Dependency graph` as template placeholders. You MAY seed `References`
    with any upstream artifacts the user named.

7.  Commit, push, and open a draft pull request.

    ```sh
    git add plans/<slug>/
    git commit -m "create: <description>"
    git push -u origin plan/<slug>
    gh pr create --draft --title "create: <description>" --fill
    ```

    If the `gh` client is unavailable or not authenticated, stop and report
    the error.

8.  Record the pull request number in the document's `Plan PR` field, then
    commit and push.

    ```sh
    git commit -am "chore: link plan PR"
    git push
    ```

9.  Open a discussion thread. `gh` has no native discussion command, so use
    the GraphQL API. First look up the repository ID and the `Plans`
    discussion category:

    ```sh
    gh api graphql -f query='
      query($owner:String!, $name:String!) {
        repository(owner:$owner, name:$name) {
          id
          discussionCategories(first:20) { nodes { id name } }
        }
      }' -F owner=<owner> -F name=<repo>
    ```

    Then create the discussion and capture its URL:

    ```sh
    gh api graphql -f query='
      mutation($repoId:ID!, $categoryId:ID!, $title:String!, $body:String!) {
        createDiscussion(input:{
          repositoryId:$repoId, categoryId:$categoryId,
          title:$title, body:$body
        }) { discussion { url } }
      }' -F repoId=<repoId> -F categoryId=<categoryId> \
        -f title="create: <description>" \
        -f body="Discussion thread for the <description> plan (PR
    #<number>). Please leave all feedback here, not on the pull request."
    ```

10. Cross-reference the thread. Record its URL in the document's
    `Discussion thread` field, and append it to the pull request description:

    ```sh
    gh pr edit <number> --body "$(gh pr view <number> --json body -q .body)

    Discussion thread: <discussionUrl> — Please leave all review feedback
    there, not on this pull request."
    ```

    Then commit and push the document change.

    ```sh
    git commit -am "chore: link discussion thread"
    git push
    ```

11. Output a summary of your actions.

## Rules

- You MUST NOT draft a plan for a single task.

  If the request decomposes into one task, it does not need a plan; the user
  should open a ticket directly in the relevant code repository. Say so before
  drafting.

- There MUST be exactly one new plan per branch and per pull request.

  If the user describes several independent bodies of work, draft a separate
  plan branch for each.

- You MUST branch from `main`, not from any other branch.

  If the local `main` is behind the remote, pull first. Rebase rather than
  merge, to keep the history linear.

- You MUST open the pull request as a draft, carrying no lifecycle label.

  A new plan is not yet ready for review. The `#planned` label belongs to a
  later transition.

- Every plan pull request MUST have an associated discussion thread.

  All review feedback belongs in the thread, not in the pull request's
  comments, so that it survives the squash-merge.

- You MUST NOT assign a numeric ID.

  The slug is the plan's identity, and the directory is never renamed. Plans
  are recorded in `plans/INDEX.md` only after merge.

- You MUST NOT write the task breakdown or the dependency graph.

  Decomposing the work is the user's job. Leave those sections as template
  placeholders.
