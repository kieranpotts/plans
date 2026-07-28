# [Project Name] – Delivery Plans

The capitalized words REQUIRED, MUST, MUST NOT, RECOMMENDED, SHOULD,
SHOULD NOT, OPTIONAL, and MAY are to be interpreted as described in
[IETF RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## Project overview

See the [README](./README.md) for an overview of this project, and how it fits
alongside the sibling SRS, RFC, design, audits, and risks repositories.

This repository is documentation, not code. There is nothing to build, lint,
or run.

## Project structure

- **`plans/`:** The delivery plans, one directory per plan (`plans/<slug>/`),
  each holding its `README.md` and any supporting artifacts.

  - **`plans/INDEX.md`** lists plans in the order they were implemented.

  - **`plans/TEMPLATE.md`** is the starting point for a new plan.

- **`docs/`:** General guidelines for humans to get the most out of the
  planning process.

## Lifecycle

See [CONTRIBUTING.md > Lifecycle](./CONTRIBUTING.md#lifecycle) for the state
machine diagram and the table of permitted transitions.

## Workflow

See [CONTRIBUTING.md > Workflow](./CONTRIBUTING.md#workflow) for the
step-by-step process for shepherding a plan from `DRAFT` to `DONE`/`ABANDONED`.

## Rules

Agents MUST follow the rules in [CONTRIBUTING.md > Rules](./CONTRIBUTING.md#rules).
Re-read the rules before creating, transitioning, or merging a plan, rather
than relying on your memory of a prior state of the rules.

## Skills

The **`.agents/skills/`** directory provides on-demand skills for managing the
lifecycle of a plan. See the [README](./.agents/skills/README.md) for
descriptions of the available skills and their triggers.

It is RECOMMENDED to use these skills to drive state transitions.
