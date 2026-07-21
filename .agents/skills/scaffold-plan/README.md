# Scaffold plan

Scaffolds a new delivery plan and opens it as a draft pull request.

## What it does

- Creates a `plan/<slug>` branch.
- Copies `plans/TEMPLATE.md` to `plans/<slug>/README.md`.
- Fills in the metadata header (authors, dates, `Status: DRAFT`, target
  repositories).
- Seeds the `References` section with any upstream artifacts you named.
- Commits and pushes the change.
- Opens a draft pull request and links it from the document.
- Opens an associated discussion thread for review feedback, cross-linked with
  the PR.

## How to invoke

> Scaffold plan

> Draft checkout hardening.

## Examples

- `/scaffold-plan`: Agent prompts you for the goal, scope, and target
  repositories, then scaffolds the branch, document, and draft PR.

- `/scaffold-plan <Description>`: Scaffolds immediately based on your
  description.

You complete the `Summary`, `Scope`, `Approach`, task breakdown, and dependency
graph yourself. Once done, use [`/finalize-plan`](../finalize-plan/README.md) to
mark the PR as ready for review.
