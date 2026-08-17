---
name: abandon-plan
description: >-
  Drop a delivery plan before completion, and land it in the main trunk as a
  permanent record of the decision. Use this skill when the user says
  something like "abandon this plan", "drop the plan", "cancel the plan", or
  "we're not doing this". Do not use this skill for a plan whose tasks have
  all shipped.
compatibility: >-
  requires Read, Edit, Bash (git, gh)
license: CC0-1.0
---

# Abandon plan

Progress a delivery plan from `PLANNED` or `IN PROGRESS` to `ABANDONED`,
landing it in the `latest/main` trunk and recording it in the plan index. The
whole value of an abandoned plan is the record of why it was dropped, so do not
merge one without that reason written into the document.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Target plan — REQUIRED.** Infer it from the checked-out branch
  (`latest/plan/<slug>`). If on `latest/main`, list the open plan pull requests
  and ask the user to choose.

- **Reason for dropping the plan — REQUIRED.** Why the work is not going
  ahead. Record it in the document if it is not already there.

- **Merge confirmation — REQUIRED.** Explicit instruction from the user that
  the pull request may be merged. Never assume it.

## Success criteria

- `Status` MUST be `ABANDONED` and `Last updated` MUST be today's date, in the
  plan document at `plans/<slug>/README.md`.

- The document MUST state why the plan was dropped.

- The pull request MUST carry `#abandoned` in place of whichever lifecycle
  label it held, and MUST be squash-merged into `latest/main` with the subject
  `update: <description> - ABANDONED`.

- The plan's branch MUST be deleted from the upstream repository.

- The associated discussion thread MUST be closed.

- `plans/INDEX.md` on `latest/main` MUST carry a new row for the plan, with
  `Abandoned` status, appended at the end of the table.

- The plan document MUST NOT have been deleted or its directory renamed, and
  no code repository MUST have been touched.

## Instructions

1.  Identify the plan and its pull request.

    Infer the target from the checked-out branch (`latest/plan/<slug>`). If on
    `latest/main`, list the open plan pull requests and ask the user to choose:

    ```sh
    gh pr list --search 'label:"#planned","#in-progress"' \
      --json number,title,headRefName,labels
    ```

    Repeated `--label` flags AND together, matching only a PR that carries
    both labels at once — which none ever does, since the two are mutually
    exclusive lifecycle states. The comma-separated `--search` query above
    matches either.

    Check out the branch and read `plans/<slug>/README.md`.

2.  Confirm the plan is currently `PLANNED` or `IN PROGRESS`, and note which
    lifecycle label the pull request carries — you need it in step 5.

3.  Verify every rule below. Ensure the reason for abandonment is recorded in
    the document, adding a short note to `Summary` or `Open questions` if
    there is not one already. Report any unmet rule and stop.

4.  Set `Status` to `ABANDONED` and `Last updated` to today's date. This
    becomes the settled date recorded in the plan index.

5.  Swap the lifecycle label, removing whichever one the pull request holds.

    ```sh
    gh pr edit <number> --add-label "#abandoned" \
      --remove-label "#planned"   # or "#in-progress"
    ```

6.  Commit and push.

    ```sh
    git commit -am "chore: abandon <description>"
    git push
    ```

    `<description>` is the short lowercase description in the pull request
    title, after the `create: ` prefix.

7.  Merge the pull request, once the user has confirmed it may be merged.
    Squash-merge it and delete the source branch upstream:

    ```sh
    gh pr merge <number> --squash --delete-branch \
      --subject "update: <description> - ABANDONED"
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

10. On `latest/main`, append a row to `plans/INDEX.md`: the plan's title,
    `Abandoned` status, its target repositories, and the settled date. Append
    at the end — the index is ordered by implementation, not alphabetically or
    by date.

    ```sh
    git commit -am "chore: record <description> in plan index"
    git push
    ```

11. Output a summary of your actions.

## Rules

- You MUST NOT abandon a plan that is not currently `PLANNED` or
  `IN PROGRESS`.

  A `DONE` plan cannot be abandoned. A plan still in `DRAFT` was never agreed,
  so its pull request should simply be closed rather than recorded as an
  abandoned decision.

- The decision to drop the plan MUST be settled.

  Abandonment is deliberate and permanent, not a pause. If the plan is merely
  stalled, leave it where it is.

- The reason for abandonment MUST be recorded in the document before merging.

  An abandoned plan's only value is the record of why, so a merge without it
  produces a permanent record that explains nothing.

- You MUST push before merging.

  `gh pr merge` merges what is on the remote. The status change and the
  recorded reason committed locally but not pushed are silently dropped,
  leaving the merged plan still reading `PLANNED` or `IN PROGRESS` on
  `latest/main`.

- You MUST NOT merge without explicit instruction from the user.

  The merge is irreversible in practice: the branch is deleted and the
  discussion thread closed in the same sequence.

- You MUST NOT delete the plan document or rename its directory.

  An abandoned plan is preserved permanently, exactly like a completed one,
  and the slug is its identity.
