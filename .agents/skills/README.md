# Agent skills for managing delivery plans

The skills available to agents in this project are:

- **[scaffold-plan](./scaffold-plan/):** \
  Scaffolds a new delivery plan, ready for the user to work on.
  Sets the status to `DRAFT`.

- **[finalize-plan](./finalize-plan/):** \
  Handles the `DRAFT` → `PLANNED` transition.

- **[implement-plan](./implement-plan/):** \
  Handles the `PLANNED` → `IN PROGRESS` transition.

- **[complete-plan](./complete-plan/):** \
  Handles the `IN PROGRESS` → `DONE` transition.

- **[abandon-plan](./abandon-plan/):** \
  Handles the `PLANNED`/`IN PROGRESS` → `ABANDONED` transition.

The **scaffold-plan** skill .......

```mermaid
flowchart LR
  scaffold["🤖<br/>scaffold"]:::agentic
  write["🧑<br/>write"]:::anthropic
  finalize["🤖<br/>finalize"]:::agentic
  implement["🤖<br/>implement"]:::agentic
  complete["🤖<br/>complete"]:::agentic
  abandon["🤖<br/>abandon"]:::agentic

  scaffold ==> write
  write ==> finalize
  finalize ==> implement
  implement ==> complete
  finalize -.-> abandon
  implement -.-> abandon

  classDef agentic fill:#cce5ff,stroke:#004085,color:#004085,stroke-width:2px
  classDef scripted fill:#e2e3e5,stroke:#4b5157,color:#383d41,stroke-width:2px
  classDef anthropic fill:#fff3cd,stroke:#856404,color:#856404,stroke-width:2px,stroke-dasharray:2 3
```

## Compatibility

These skills are compatible with the [Agent Skills](https://agentskills.io/)
convention. Most agent harnesses support this convention natively, but
workarounds may be required for harnesses that do not.
