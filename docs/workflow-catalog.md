# ORel Workflow Catalog

This document is the evolving catalog of recurring Reliability Engineering workflows standardized by OpenReliability (ORel).

The catalog is **not** part of the project constitution. It is expected to change as the community validates workflow boundaries, names, and semantics in real practice.

## Model

The catalog is a collection of versioned **Workflow Profiles**.

A Workflow Profile is both:

1. an entry in the catalog of known reliability work; and
2. the portable semantic model of how that work is performed.

There is intentionally no separate "Work Catalog entry" type.

A profile may begin as `draft` with only a stable identity, purpose, and rough scope. As the workflow is validated, the profile can gain triggers, required context, action dependencies, evidence requirements, and completion semantics.

Conceptually:

```text
Workflow Catalog
  ├─ change/prr                 → Workflow Profile
  ├─ change/deployment-risk     → Workflow Profile
  ├─ run/incident               → Workflow Profile
  ├─ run/performance            → Workflow Profile
  └─ learn/postmortem           → Workflow Profile
```

## Lifecycle groups

ORel currently organizes workflows around three lifecycle groups:

- **CHANGE** — work that helps software change safely;
- **RUN** — work that helps production systems operate reliably;
- **LEARN** — work that converts operational evidence into future improvements.

These groups are organizational aids, not rigid execution phases.

## Initial catalog

The following list is a starting taxonomy, not a finalized boundary model.

### CHANGE

- Production Readiness Review
- Deployment Risk Assessment
- Migration Readiness
- Rollback / Recovery Verification
- Release Verification
- Post-deployment Verification

### RUN

- Incident Response
- Root Cause Investigation
- Vulnerability Response
- Performance Engineering
- Capacity Engineering
- Resilience / Chaos Engineering
- Observability Improvement
- SLO Review
- Backup / Restore Verification
- Disaster Recovery Exercise
- Dependency Risk Review
- CI / Delivery Performance
- Toil Reduction

### LEARN

- Postmortem
- Reliability Review
- Trend Analysis
- Spec / Architecture Feedback

## Open questions

The initial taxonomy intentionally leaves several questions open:

- Which items are independent workflows versus subflows of another workflow?
- Which workflows need variants by severity, system criticality, or change type?
- Which workflows are better represented as reusable capabilities rather than top-level profiles?
- Where should security and governance workflows sit relative to Reliability Engineering?
- Which profiles should be standardized first based on repeatability and evidence requirements?

These questions should be resolved through real workflow implementations rather than ontology design alone.

## Organization-specific workflows

ORel profiles are portable defaults.

Organizations can specialize them using their own context, policy, and experience:

```text
ORel Workflow Profile
        +
Organization Context / Policy / Experience
        ↓
Organization-specific Workflow
        ↓
Agent Skill / Automation / Human Playbook
```

For example, an organization may derive a deployment-risk workflow that knows its service criticality model, rollback rules, production architecture, historical incidents, and required verification policy.

A context platform such as Tacit can supply that organizational knowledge, but ORel remains independent of any specific context provider.
