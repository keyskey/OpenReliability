# OpenReliability

> **The open workflow standard for reliable software.**
>
> OpenSpec defines how software is specified.  
> OpenReliability defines how software is safely changed, operated, and improved.

OpenReliability is an open standard for making reliability engineering workflows machine-readable, evidence-driven, and agent-executable.

## Naming

- **OpenReliability** — the full project and standard name
- **ORel** — the short name for the OpenReliability standard and project
- **`rel`** — the reference CLI implementation of ORel

ORel is the standard. `rel` is one implementation of that standard.

The project focuses on the common language of reliability work across **Change**, **Run**, and **Learn**—not on owning the agents, observability stack, CI/CD engine, or execution runtime that perform the work.

## Design constitution

OpenReliability is developed under a small set of architectural constraints:

- keep the core small and the schemas strong;
- standardize reliability work, not its implementation;
- treat evidence as a first-class artifact;
- keep agents and executors replaceable;
- integrate with existing ecosystems instead of rebuilding them;
- remain useful without any particular AI model, vendor, or platform.

See [`docs/constitution.md`](docs/constitution.md) for the full design constitution.
