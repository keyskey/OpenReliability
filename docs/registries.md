# ORel Semantic Registries

ORel uses versioned semantic registries for machine-interpreted identifiers that must carry stable meaning across Workflow Profiles, policies, agents, and executors.

The goal is simple:

> **Semantic fields should be interoperable without forcing every organization into one closed vocabulary.**

## Registry model

A semantic value used by a normative ORel field must resolve to one of:

1. an **ORel core identifier**; or
2. an identifier from an explicitly declared, globally unique **extension namespace**.

This allows the standard vocabulary to remain portable while preserving room for organization-specific and vendor-specific concepts.

## Candidate registries

The initial registry categories are expected to include:

- **Trigger Registry** — why a workflow starts;
- **Context Registry** — what context a workflow requires;
- **Capability Registry** — what kind of work an action requires;
- **Evidence Registry** — what kind of evidence is produced or consumed;
- **Decision Registry** — what kind of decision a workflow produces.

Additional registries should be introduced only when repeated workflow implementations demonstrate a need for globally shared semantics.

## Core and extension namespaces

Conceptually:

```text
ORel Core Namespace
  ├─ trigger/...
  ├─ context/...
  ├─ capability/...
  ├─ evidence/...
  └─ decision/...

Extension Namespace
  └─ organization- or vendor-specific identifiers
```

A future concrete syntax may use declared prefixes, URIs, URNs, or another globally unique mechanism.

For example, an implementation might eventually support something similar to:

```yaml
namespaces:
  orel: https://openreliability.dev/registry/v1/
  acme: https://engineering.acme.example/openreliability/

triggers:
  - orel:trigger/performance/latency-regression
  - acme:trigger/market-session-open

requires:
  - orel:context/service
  - orel:context/production-baseline
```

This example is illustrative. The exact identifier syntax is not standardized yet.

## What should be registry-backed

Registry-backed identifiers are appropriate when a value is expected to be:

- matched by machines;
- shared across Workflow Profiles;
- mapped to executors or providers;
- validated by schema or policy;
- exchanged between implementations.

Examples include:

```text
trigger
context requirement
capability
evidence type
decision type
```

## What should remain free-form

Human-facing prose should remain flexible.

Examples include:

- purpose;
- rationale;
- description;
- operator guidance;
- explanatory notes.

ORel should not turn ordinary documentation into an ontology.

## Resolution contract

A conforming implementation should eventually be able to answer:

- What does this identifier mean?
- Which registry or namespace owns it?
- What schema or semantic definition applies?
- Is it deprecated or superseded?
- Can a provider or executor satisfy it?

This enables contracts such as:

```text
Workflow Profile
  requires: orel:context/production-baseline
        ↓
Context Provider
  advertises: can resolve production-baseline
```

and:

```text
Workflow Profile
  capability: orel:capability/load-test
        ↓
Executor
  advertises: can execute load-test
```

## Versioning

Registry values must be versionable and deprecatable without requiring a new ORel constitution.

The registry should therefore evolve independently from:

- the Constitution;
- individual Workflow Profiles;
- the `rel` reference implementation.

## Initial status

The registry categories and namespace rules are part of the current design direction.

The concrete identifier set is intentionally provisional until ORel implements real Workflow Profiles and learns which distinctions are durable enough to standardize.
