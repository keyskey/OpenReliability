# OpenReliability Constitution

This document defines the architectural constraints of OpenReliability.

These are not implementation details. They are the rules that should remain true as the project grows.

## Mission

> **The open workflow standard for reliable software.**

OpenSpec defines how software is specified.  
OpenReliability defines how software is safely changed, operated, and improved.

OpenReliability exists to make reliability engineering work portable, machine-readable, evidence-driven, and executable by humans or agents.

The project should standardize **the language and lifecycle of reliability work**, while allowing the systems that execute that work to remain replaceable.

---

## 1. Standard first, implementation second

OpenReliability defines *what reliability work means* before prescribing *how it must be executed*.

A hypothesis should not require k6.
An incident investigation should not require OpenSRE.
A deployment verification should not require Kubernetes.
A workflow should not require a particular agent.

The standard may define concepts such as:

- Case
- Finding
- Hypothesis
- Action
- Evidence
- Decision
- Learning

but implementations remain free to use different tools and runtimes.

**Reliability Standard ≠ Reliability Implementation.**

This follows the same principle that makes specifications such as OpenSLO portable across vendors.

---

## 2. Small Core, Strong Schema

The core must remain intentionally small.

OpenReliability should prefer a small number of stable domain primitives with explicit schemas over a large framework containing integrations, agents, rules, and execution logic.

The core should primarily own:

- reliability domain schemas;
- workflow state and dependency semantics;
- validation;
- policy contracts;
- evidence and provenance semantics;
- portable interfaces for context and execution.

The core should not become a collection of every reliability best practice.

If a capability can live in a workflow pack, Agent Skill, adapter, MCP server, or external executor, it should usually live there.

---

## 3. Evidence is a first-class artifact

An agent assertion is not evidence.

A recommendation such as "this release is risky" or "the database is the bottleneck" is a hypothesis until it is supported by traceable evidence.

Evidence should carry enough provenance to answer questions such as:

- What produced this result?
- Against which code or configuration version?
- When was it observed?
- Which hypothesis or decision does it support or reject?
- Can another tool or human reproduce or inspect it?

OpenReliability should optimize for **evidence-backed decisions**, not for generating more prose.

---

## 4. Workflows are portable; executors are replaceable

Reliability workflows should describe required work without binding that work to a specific implementation.

For example:

```text
Action: verify peak-load behavior
Capability: load-test
```

may be executed by k6, Gatling, Locust, a managed service, or a future agent.

Likewise:

```text
Action: investigate incident
Capability: incident-investigation
```

may be executed by a human SRE, OpenSRE, Codex, Claude Code, or another system.

OpenReliability should standardize the contract, not monopolize the worker.

---

## 5. Policy and agent reasoning are separate concerns

Deterministic organizational requirements must not be hidden inside prompts.

Examples:

- Tier-1 changes require post-deployment verification.
- Database migrations require rollback or recovery evidence.
- Critical user journeys require explicit performance verification.

These belong in policy.

Agents may decide:

- which hypotheses are most relevant;
- which experiment is most informative;
- what evidence to gather next;
- which candidate remediation to test.

In short:

```text
Policy = what must be true or must be done
Agent  = how to reason within those constraints
Tool   = how work is executed
Evidence = what actually happened
```

---

## 6. Actions, not ceremonial phases

Reliability work is rarely a single linear checklist.

A change may need parallel performance, security, and resilience verification. An incident may branch into several competing hypotheses. A performance investigation may iterate until evidence converges.

OpenReliability should therefore model work as dependency-aware actions and artifacts rather than forcing every case through a rigid sequence of named phases.

The model should naturally support:

- dependency graphs;
- fan-out / fan-in;
- optional actions;
- policy-driven requirements;
- iteration;
- human checkpoints.

However, OpenReliability should **not** become a general-purpose workflow engine.

Execution may be delegated to Spec Kit, GitHub Actions, Argo, Tekton, Temporal, local agents, or other runtimes.

---

## 7. AI is optional; determinism belongs in the core

OpenReliability is designed for an agentic future, but the standard must remain useful without an LLM.

Parsing, schema validation, dependency resolution, policy evaluation, state transitions, and evidence provenance should be deterministic wherever possible.

AI is valuable for open-ended work such as:

- hypothesis generation;
- context interpretation;
- experiment design;
- investigation;
- candidate remediation generation.

The project should avoid turning deterministic infrastructure concerns into prompt behavior.

---

## 8. Integrate; do not recreate ecosystems

OpenReliability should consume existing standards and tools whenever possible.

Examples include:

- OpenSpec / Spec Kit for Build-phase specification workflows;
- OpenSLO for service-level objectives;
- MCP for agent/tool capability discovery;
- OpenTelemetry for telemetry;
- GitHub / GitLab for changes;
- OpenSRE and other agents for investigation;
- k6 / Gatling / Locust for performance experiments;
- Chaos Mesh / Litmus for fault injection;
- Argo / Tekton / GitHub Actions for execution.

The project should only introduce a new abstraction when existing ones cannot express the required reliability semantics.

---

## 9. Git-native and inspectable by default

A reliability case should be reviewable without a proprietary UI.

Schemas and artifacts should have human-readable representations such as YAML and Markdown, and work naturally with Git where that makes sense.

A web service or database may improve scale and collaboration later, but the standard must not depend on one.

The preferred developer experience is:

```bash
rel new change
rel next
rel status
rel validate
```

with all meaningful state inspectable by humans and agents.

---

## 10. BUILD, CHANGE, RUN, and LEARN form one loop

OpenReliability does not treat production operations as a lifecycle disconnected from software development.

The target loop is:

```text
BUILD
  ↓
CHANGE
  ↓
RUN
  ↓
LEARN
  └────────→ BUILD / CHANGE
```

Production evidence should be able to invalidate assumptions, create new constraints, and feed changes back into specifications and designs.

OpenReliability should integrate with systems such as OpenSpec rather than attempting to own the Build phase itself.

---

## 11. Vendor neutrality is a product requirement

No core schema should require:

- a specific cloud;
- Kubernetes;
- a specific CI/CD platform;
- a specific observability vendor;
- a specific agent;
- a specific model provider;
- Tacit or any commercial product.

Commercial products may implement superior adapters or context providers, but the OpenReliability standard and reference implementation must remain independently useful.

---

## 12. Reference implementation is not the standard

The `rel` CLI is the reference implementation of OpenReliability, not OpenReliability itself.

The current implementation direction is expected to favor:

- Go for the CLI and deterministic core;
- YAML / JSON Schema for portable artifacts;
- CEL for lightweight policy and conditions where appropriate;
- Git-native local state first;
- MCP / HTTP / subprocess adapters for external capabilities.

These choices may evolve without redefining the conceptual standard.

Implementations in other languages should be possible if they conform to the same schemas and semantics.

---

## Non-goals

OpenReliability is not intended to become:

- an observability platform;
- an incident management SaaS;
- a general-purpose AI SRE agent;
- a universal workflow engine;
- a CI/CD system;
- a service catalog;
- a knowledge management system;
- a replacement for OpenSpec or OpenSLO;
- a giant library of vendor integrations in the core.

It should be the thin, durable layer that allows these systems to participate in the same reliability workflow.

---

## Design test

When evaluating a new feature, ask:

1. Does this define a reusable reliability concept or contract?
2. Does it improve evidence-backed decisions?
3. Can it remain independent of a particular executor or vendor?
4. Could it live outside the core as a pack, skill, adapter, or executor?
5. Are we standardizing reliability engineering, or accidentally rebuilding another platform?

If the answer to the last question is "rebuilding another platform," OpenReliability should probably integrate instead.
