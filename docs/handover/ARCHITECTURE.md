# Architecture direction

This document describes the intended architecture to guide the first checkpoints.
It is not an implemented-system description.

## Control flow

```text
authored `map.json`
  -> schema validation and normalization
  -> read-only capability discovery
  -> placement and dependency resolution
  -> immutable resolved plan plus cost/security summary
  -> plan-bound approval
  -> journaled adapter execution
  -> observed-state refresh
  -> behavioral and recovery verification
  -> reconciliation and human handover
```

Planning and execution must remain separate. Provider adapters receive resolved,
bounded operations; they must not silently expand user intent.

## Proposed components

### Trail Map frontend

Owns schema versions, source locations, canonical representation, validation,
defaults, provider-specific extensions, and diagnostics. Unknown behavior should
fail closed rather than disappear during normalization.

### Capability registry

Represents what a particular adapter version can discover and perform. Capability
snapshots make plans reproducible and allow API changes to be classified.

### Planner

Maps portable intent and explicit provider extensions onto concrete resources.
It owns placement, dependency ordering, failure-domain constraints, budgets,
estimated cost, and explanation. It produces no external side effects.

### Policy and approval engine

Applies hard account, provider, region, service, resource-count, cost, expiry,
network, and mutation-class limits. Approval is bound to the plan digest and
target identities.

### State store

Persists `map.json`, plans, inventory, observed state, locks, migrations, and
evidence references. Local state is the first target. Remote collaboration and
backends are later decisions.

### Operation journal

Records intent and results around every external action. It supports idempotency,
interruption analysis, safe resume, audit, and redacted support bundles.

### Execution engine

Runs dependency-aware operations with bounded concurrency, rate limiting,
timeouts, retries, and cancellation. Unknown outcomes require observation before
retry. Rollback is explicit and capability-aware, not assumed universally safe.

### Adapters

Candidate adapter families are:

- fake/local provider for deterministic testing;
- SSH host operation;
- VM/cloud providers such as Akamai and Vultr;
- Kubernetes API;
- AWS and other broad cloud services;
- application and database operational contracts.

Provider adapters expose real differences in networking, pricing, images,
firewalls, backups, DNS, load balancing, and lifecycle behavior. A least-common-
denominator abstraction is not the goal.

### Verification engine

Runs health, behavioral, topology, security, restart, backup, restore, failover,
and external-reachability checks. Verification results are first-class evidence,
not console prose.

### Experiment controller

Builds a disposable test-network lifecycle on the same planner, inventory,
journal, adapters, and verification engine used for durable deployments. It owns
phase transitions, baseline establishment, scenario scheduling, evidence capture,
TTL/lease enforcement, cancellation, and cleanup-finalizer state.

An experiment is not an unrestricted script. Each disruptive action must be a
typed, capability-checked operation bounded to exact test resources. Scenario
failure changes the result but must not bypass evidence capture or cleanup.

### Handover generator

Produces topology, inventories, URLs, versions, access procedures, configuration,
normal operations, upgrades, backups, recovery, limitations, costs, and manual
commands. It must be useful after the originating agent session is gone.

### Operator interface

Exposes the same typed discovery, planning, approval, execution, observation, and
recovery capabilities to humans, conventional automation, and agents. Operator
identity and authority are explicit inputs to policy decisions. A conversation
may supply intent, but it is never the durable execution or state protocol.

Agent replacement should require loading `map.json`, current inventory,
observed state, journal, pending approvals, evidence, and handover—not replaying
or summarizing the previous agent's transcript.

## Idempotency and unknown outcomes

Every create operation needs a deployment identity and provider-side correlation
mechanism where supported. If an API call times out, Trails must observe whether
the resource exists before retrying. Names alone are insufficient identifiers.

Deletion must resolve exact inventoried IDs, confirm ownership metadata, check
dependencies, and record results. Never use broad globs, inferred account-wide
cleanup, or unresolved environment variables as destructive targets.

## Multi-provider design

Provider-neutral placement should express resilience intent such as distinct
provider, region, zone, account, or network failure domains. Cross-provider
networking, DNS, replication, and backup remain explicit resources with their own
security and failure assumptions.

Multi-provider does not automatically imply high availability. Trails must state
whether a deployment uses cold restore, rebuild, warm standby, active/passive, or
active/active behavior and retain evidence for the claimed recovery model.

## Kubernetes design

Kubernetes is already a reconciliation system. Trails should use its API and
declarative resources, observe rollout, and retain manifests or stable values.
SSH into worker nodes must not become the normal workload-management path.

Cluster creation, application deployment, database operation, and multi-cluster
placement are separate capabilities and should enter through bounded slices.

## Agent integration

Agents consume structured schemas and results. Useful agent operations include:

- ask clarifying questions before plan resolution;
- compare valid topology options and costs;
- explain the resolved plan and risk;
- request approval for a bounded digest;
- invoke stable commands or APIs;
- diagnose structured failures;
- propose recovery without silently expanding authorization;
- generate and explain the final handover.

The orchestration engine—not the language model—owns state, limits, resource
identity, idempotency, retries, and authorization enforcement.

## Agent-managed self-hosting architecture

Trails occupies the durable layer between user-owned infrastructure and
replaceable operators. Provider accounts and deployed services remain usable if
Trails is temporarily unavailable; Trails state remains usable if the preferred
agent or model changes. Exportable manifests, inventories, journals, evidence,
configuration references, and manual commands prevent either side from becoming
an opaque managed-service dependency.

Long-running operation should be event- and policy-driven rather than a permanent
chat session. Schedulers, provider events, monitoring signals, and explicit human
requests may create proposed operations. The policy engine decides whether each
proposal is read-only, already authorized within a narrow standing policy, or
requires a new plan-bound approval.

## Optional Gantry integration

Trails core remains independent. Gantry services attach at public integration
points: Watchpost/Watchpost Agent as observers, Webfleet as an HTTP/API verifier,
Warden as an optional workspace, Cortex as an optional operator, and other tools
as application or report-building helpers. Equivalent non-Gantry components must
remain possible.
