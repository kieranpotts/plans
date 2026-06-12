# 🗺️ Implementation Plans

**A template for planning the implementation of changes across a software system, via version control.**

This repository is the home of the implementation plans for [Project Name]. It is a development planning system, kept under version control.

This repository answers _when, and in what order_, work gets done. Given the upstream artifacts, it plans the implementation: _what concrete work, in what order, across which repositories_, gets us from the present system to the intended one?

A plan is a self-contained body of work with a goal, a scope, and a decomposition into tasks. Each task lives in exactly one code repository and links out to the concrete issue or pull request there. That linked tracker – not this repository – owns the task's live status. This repository's value is the cross-repository decomposition and sequencing that no individual issue tracker can see across.

This is NOT a living description of production. A plan describes _future and in-flight work_, which is inherently provisional. A plan is a mutable working document while it is open, and settles once the work is done or abandoned. A plan's own Git history is its record; there is no separate decision log.

## Ecosystem

This repository is one of four that form a coherent, version-controlled documentation ecosystem modeling the software development lifecycle. Each is the reference implementation of an opinionated workflow, and answers a different question about the system:

- [**📋 Software Requirements Specification (SRS)**](https://github.com/kieranpotts/specs): Records _what_ the system does, in business terms.

- [**💬 Requests for Comments (RFC)**](https://github.com/kieranpotts/rfc): Records _how_ significant technical decisions were made, and _why_.

- [**📐 Design Docs**](https://github.com/kieranpotts/design): Describe _what the system looks like_, its as-is architecture.

- **🗺️ Implementation Plans**: Capture _when, and in what order_, the work gets done (this repository).

The [**skills**](https://github.com/kieranpotts/skills) collection provides an agentic workflow that operates across all four.

This separation into dedicated repositories is intended for application software that spans multiple code repositories, and potentially multiple teams, where the requirements, decisions, designs, and plans are shared concerns that sit above any single codebase. For a standalone code repository – a small utility library, say – it is better to fold these artifacts and skills directly into that repository, rather than maintain them separately.

## Contents

- [**Plans**](./plans/): The implementation plans, one directory per plan. Each holds its `README.md` (the plan) plus any supporting artifacts – sequence diagrams, data, mock-ups.

  - The [`INDEX`](./plans/INDEX.md) lists plans in the order they were implemented.

  - The [`TEMPLATE`](./plans/TEMPLATE.md) is the starting point for a new plan.

- [**Contributing**](./CONTRIBUTING.md): Step-by-step instructions for shepherding a plan through its lifecycle.

- [**Agents**](./AGENTS.md) and [**Skills**](./.agents/skills/): Instructions for agentic tools to manage the planning workflow with a high degree of autonomy.

- [**Documentation**](./docs/): General guidance on how to get the most out of the planning process.

-----

Copyright © 2020-present Kieran Potts, [CC0 license](./LICENSE.txt)
