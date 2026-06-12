# Implementation Plans

**A template for planning the implementation of changes across a software system, via version control.**

This repository is the home of the implementation plans for [Project Name]. It is issue tracking and project management in one, kept under version control.

This repository answers _when, and in what order_, work gets done. It is the fourth stage in a chain of version-controlled documentation:

- The [software requirements specification (SRS)](https://github.com/kieranpotts/specs) records _what_ the system does — its requirements, in business terms.

- The [requests for comments (RFC)](https://github.com/kieranpotts/rfc) archive records _how_ the significant technical decisions were made, and _why_.

- The [design docs](https://github.com/kieranpotts/design) describe _what the system looks like_ — its as-is architecture.

- This repository plans the implementation: given those upstream artifacts, _what concrete work, in what order, across which repositories_, gets us from the present system to the intended one?

The unit of planning is an **initiative**: a self-contained body of work with a goal, a scope, and a decomposition into tasks. Each task lives in exactly one code repository and links out to the concrete issue or pull request there. That linked tracker — not this repository — owns the task's live status. This repository's value is the cross-repository decomposition and sequencing that no individual issue tracker can see across.

This is NOT a living description of production. A plan describes _future and in-flight work_, which is inherently provisional. A plan is a mutable working document while its initiative is open, and settles once the initiative is done or abandoned. A plan's own Git history is its record; there is no separate decision log.

A template for new code repositories.

## 📓 Documentation

- [**Requirements**](./docs/requirements.md)
- [**Installation**](./docs/installation.md)
- [**Usage**](./docs/usage.md)
- [**Development**](./docs/development.md)

-----

Copyright © 2020-present Kieran Potts, [MIT license](./LICENSE.txt)
