# 🗺️ Delivery Plans

**A template for planning the implementation of changes across a software
system, via version control.**

This repository is the home of the delivery plans for [Project Name]. It is a
development planning system, kept under version control.

This repository answers _when, and in what order_, work gets done to implement
a new set of requirements and designs. A plan answers "how do we get from the
current system to the intended one?"

A plan is a self-contained body of work with a goal, a scope, and a
decomposition into tasks. Each task lives in exactly one code repository and
links out to the concrete issue or pull request there. That linked tracker – not
this repository – owns the task's live status.

This repository's value is the cross-repository decomposition and sequencing
that no individual issue tracker can see across.

A plan is a mutable working document while it is open, and becomes immutable
once the work is done or abandoned. This history of an individual plan is
tracked in Git.

## Ecosystem

This repository is one of six that form a coherent, version-controlled
documentation ecosystem. Each answers a different question about a software
system.

- [**📋 Software Requirements Specification (SRS)**](https://github.com/kieranpotts/specs) \
  Captures what the system does, in business terms.

- [**💬 Requests for Comments (RFC)**](https://github.com/kieranpotts/rfc) \
  Records how significant technical decisions were made, and why.

- [**📐 Design Docs**](https://github.com/kieranpotts/design) \
  Documents what the system looks like in production.

- [**🔍 Architecture Audits**](https://github.com/kieranpotts/audits) \
  Logs historical evaluations of the as-built system's structural integrity.

- [**🗺️ Delivery Plans**](https://github.com/kieranpotts/plans) (this repository) \
  Tracks when, and in what order, the work gets done.

- [**⚠️ Risk Register**](https://github.com/kieranpotts/risks) \
  Records the inherent security and privacy risks the system carries.

In addition, the [**✨ Agent Skills**](https://github.com/kieranpotts/skills)
collection offers composable agentic workflows that operate across all six
repositories.

This separation into dedicated repositories is intended for application software
that spans multiple code repositories, and potentially multiple teams, where the
requirements, decisions, designs, plans, audits, and risks are shared concerns
that sit above any single codebase.

For a standalone code repository – a small utility library, say – it may be
better to fold all documentation into the same repository.

## Contents

- [**Plans**](./plans/) \
  The delivery plans, one directory per plan.

  - The [`INDEX`](./plans/INDEX.md) lists plans in the order they were
    implemented.

  - The [`TEMPLATE`](./plans/TEMPLATE.md) is the starting point for a new plan.

- [**Contributing**](./CONTRIBUTING.md) \
  Step-by-step instructions for shepherding a plan through its lifecycle.

- [**Agents**](./AGENTS.md) and [**Skills**](./.agents/skills/) \
  Instructions for agents to manage the planning workflow with a high degree
  of autonomy.

- [**Documentation**](./docs/) \
  General guidance on how to get the most out of the planning process.

-----

Copyright © 2020-present Kieran Potts, [CC0 license](./LICENSE.txt)
