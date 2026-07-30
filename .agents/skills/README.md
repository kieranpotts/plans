# Agent skills

Skills available to agents in this repository are:

- **[Scaffold plan](./scaffold-plan/):**
  Scaffolds a new draft plan, ready for the user to complete.
  Sets the status to `DRAFT`.

- **[Finalize plan](./finalize-plan/):**
  Handles the `DRAFT` → `PLANNED` transition.

- **[Implement plan](./implement-plan/):**
  Handles the `PLANNED` → `IN PROGRESS` transition.

- **[Complete plan](./complete-plan/):**
  Handles the `IN PROGRESS` → `DONE` transition.

- **[Abandon plan](./abandon-plan/):**
  Handles the `PLANNED`/`IN PROGRESS` → `ABANDONED` transition.

## Conventions

One structural convention recurs across the `SKILL.md` files in this
directory:

- **Transition gates.** Skills that handle a state transition (finalize,
  implement, complete, abandon) open their gating logic with a
  `## Transition gates: <FROM> → <TO>` heading, e.g. "Transition gates:
  `IN PROGRESS` → `DONE`". This section lists the conditions that MUST be
  satisfied before the transition is allowed to proceed.

None of the skills in this directory currently close with a `## References`
section, unlike some other repositories in this ecosystem.

## Compatibility

Agent harnesses are converging on the `./.agents/skills/` path for dynamic
retrieval of project-specific skills. This is compatible with the Agent Skills
convention — see https://agentskills.io/.

As of May 2026, OpenAI Codex, GitHub Copilot, Gemini CLI, Google Antigravity,
OpenCode, and Pi will auto-discover these skills, but Claude Code and Cursor
will not.

You will require workarounds for incompatible harnesses. For Claude Code, you
can simply symlink this directory from `.claude/skills/`. Cursor requires more
effort to transpile these skills into its native "rules" format.
