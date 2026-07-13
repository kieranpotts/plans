# Overview

A delivery plan is a version-controlled answer to a single question: given an
agreed requirement, a settled technical decision, and a target design, _what
concrete work, in what order, across which repositories_, gets us from the
present system to the intended one?

This repository is the fourth stage in a chain of version-controlled
documentation. The process flows: specify the requirement → RFC any
architecturally-significant decisions → design the solution → then plan the
implementation.

- The [software requirements specification
  (SRS)](https://github.com/kieranpotts/specs) records _what_ the system does —
  its requirements, in business terms.

- The [requests for comments (RFC)](https://github.com/kieranpotts/rfc) archive
  records _how_ the significant technical decisions were made, and _why_.

- The [design docs](https://github.com/kieranpotts/design) describe _what the
  system looks like_ — its as-is architecture.

- This repository plans the implementation — _when, and in what order_, the work
  gets done.

The linkage upstream is deliberately loose. A plan cites the artifacts it
implements in its `References` section, but a plan is its own unit. It need not
map one-to-one to a single requirement or decision, and completing a plan does
not mechanically flip any upstream artifact's state.

In this repository, the contents of the `main` trunk capture plans that have
been implemented or abandoned. Temporary branches and PR are used to manage
future and in-flight work. While a plan is open, it is a mutable working
document. Tasks are added, dropped, and re-sequenced as reality unfolds. Once
the plan is done or abandoned, it settles and is merged to `main` as a permanent
record.

This repository does not replace the issue tracker of each code repository. It
sits above them as a development planning tool. Each task in a plan lives in
exactly one code repository and links out to a concrete issue or pull request
there. That linked tracker owns the task's live status — whether it is open, in
review, or merged. To see where a task stands, you follow the link.

What this repository adds is the layer no individual tracker can provide: the
cross-repository decomposition and sequencing. A single plan may touch several
code repositories, and the order in which their tasks must be done — the
dependency graph and its critical path — is a fact that spans all of them at
once. That graph is the highest-value artifact here, precisely because it lives
nowhere else.
