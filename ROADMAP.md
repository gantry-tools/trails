# Trails roadmap

## Destination

Trails aims to make independently operated infrastructure feel as approachable
as managed hosting without sacrificing ownership, inspectability, portability,
or manual control.

A user should be able to state a desired system and its constraints, inspect a
resolved topology/security/cost plan, approve it, and receive a verified system
plus a complete operational handover. The same contract should work when invoked
manually, through ordinary automation, or by an agent.

This destination includes virtual machines, cloud services, Kubernetes, bare SSH
hosts, and multi-provider systems. It does not justify claiming those surfaces
before their adapters and failure modes have been exercised on real disposable
infrastructure.

## Product milestones

### Milestone 0 - Contract and local planning foundation

- Decide implementation language and supported platforms.
- Freeze an experimental manifest schema and explicit versioning policy.
- Implement validation, normalization, stable plan output, and useful errors.
- Model dependencies, placement, costs, expiry, approvals, and capabilities.
- Establish durable local state, inventory, journals, and evidence formats.
- Build a deterministic fake provider and failure-injection harness.
- Prove that planning and dry-run perform no external mutations.

Exit condition: identical inputs and discovered capabilities yield a stable,
reviewable plan, and interrupted fake-provider operations can resume safely.

### Milestone 1 - Manual SSH deployment

- Inventory existing SSH-accessible Linux hosts.
- Verify host keys and use short-lived deployment credentials.
- Bootstrap users, directories, services, firewalls, and health checks.
- Introduce versioned configuration bundles with validate/diff/apply/rollback.
- Keep configuration, secrets, node identity, runtime state, and data separate.
- Produce a complete manual handover and uninstall procedure.

Exit condition: a user can reproduce and operate the deployment without an agent.

### Milestone 2 - First provider adapters

- Add Akamai and Vultr capability discovery and resolved-plan adapters.
- Provision VMs, private networks, firewalls, addresses, DNS where authorized,
  storage, snapshots, and load balancers within explicit limits.
- Tag every resource with owner, purpose, deployment ID, and expiry.
- Verify apply idempotency, partial failure, resume, and exhaustive teardown.
- Compare estimated and observed costs where provider APIs permit it.

Exit condition: both adapters pass live disposable create/update/verify/destroy
campaigns with no orphaned billable resources.

### Milestone 3 - Gantry dogfood network

- Define Gantry as a blueprint rather than core logic.
- Begin with one Watchpost server and one remote Watchpost Agent.
- Expand to multiple agents, Webfleet, Trestle, Warden, and Cortex roles.
- Add private networking, reverse proxying, TLS, DNS, backups, and monitoring.
- Define manual configuration propagation, join, revoke, upgrade, and recovery.
- Exercise rolling restarts, node replacement, network interruption, and restore.

Exit condition: Trails provisions and hands over a useful Gantry network while
every operation remains manually documented.

### Milestone 4 - Multi-provider resilience

- Add placement rules for provider, region, zone, and correlated failure domains.
- Connect provider networks through explicit secure tunnels such as WireGuard.
- Support encrypted cross-provider backups and scheduled restore verification.
- Introduce warm-standby and rebuild-from-state strategies.
- Run controlled provider-loss drills and record RTO/RPO evidence.
- Reconcile resources and costs across all participating providers.

Exit condition: a tested deployment survives the declared provider-loss model
without relying on unrecorded agent knowledge.

### Milestone 5 - Kubernetes backend

- Treat Kubernetes as a declarative backend rather than a fleet of SSH targets.
- Support existing kubeconfig/context discovery with strict scope selection.
- Resolve workloads into reviewable manifests or stable package values.
- Observe rollout, health, events, resource use, and rollback.
- Support managed clusters and lightweight k3s through separate bootstrap paths.
- Exercise multi-cluster placement and recovery without bypassing Kubernetes.

Exit condition: one Gantry workload and one unrelated workload pass repeatable
deployment, update, rollback, and teardown in disposable clusters.

### Milestone 6 - AWS and broader cloud services

- Add tightly scoped, short-lived IAM session operation.
- Begin with a narrow EC2/VPC/security-group/storage subset.
- Add Route 53, load balancing, object storage, managed databases, or EKS only as
  complete capability slices selected by evidence.
- Enforce account, region, service, tag, permission-boundary, and budget policy.
- Retain CloudTrail-compatible operation correlation.

Exit condition: each claimed AWS slice passes live tests without requiring broad
account permissions or undocumented console intervention.

### Milestone 7 - Reconciliation and operations

- Detect drift between desired, resolved, and observed state.
- Distinguish harmless, repairable, sensitive, and destructive drift.
- Add controlled upgrades, scaling, certificate renewal, backup monitoring, and
  scheduled verification.
- Provide plan-only recommendations by default; retain approval for mutations.
- Improve diagnostics and recovery after partial provider outages.

Exit condition: Trails can maintain supported systems over time without becoming
an unbounded autonomous production operator.

### Milestone 8 - General product qualification

- Deploy at least one unrelated real application without Gantry-specific core
  changes.
- Test Linux architectures, supported providers, Kubernetes variants, and CLI
  platforms within an explicit matrix.
- Fuzz manifests and provider responses; run sanitizers where applicable.
- Validate large inventories, concurrency, rate limits, and API version changes.
- Produce reproducible packages, migrations, compatibility policy, and releases.

Exit condition: evidence determines whether Trails is ready to graduate from an
experimental Gantry infrastructure harness into a general standalone product.

## Cross-cutting tracks

### Manual operability

Manual deployment, configuration propagation, cluster membership, secrets,
backup, restore, update, rollback, and teardown procedures evolve alongside
agent workflows. Agent convenience never substitutes for these contracts.

### Agent experience

Agents should be able to discover capabilities, generate manifests, explain
plans, request bounded approvals, invoke stable commands, interpret structured
results, diagnose failures, and create handovers. Prompt text is not an API.

### Security and governance

Least privilege, short-lived credentials, allowlists, deny rules, secret
references, provenance, audit journals, host-key verification, and approval
boundaries are product behavior and require executable tests.

### Cost safety

Hourly, monthly, per-operation, resource-count, region, service, and maximum-age
limits should be enforced before provider calls and rechecked during operation.
Automatic expiry is defense in depth, not permission for silent destruction.

### Provider relationships

After a credible live demonstration, prepare provider-specific engineering briefs
for Akamai, Vultr, AWS, and others. Lead with reproducible infrastructure usage,
agent-operated composition, manual parity, safety evidence, and a concrete request
for technical evaluation or infrastructure credits rather than vague partnership
language.

## Current priority

Do not start by supporting many providers. Complete Milestone 0, then prove one
manual SSH path and one inexpensive two-node Gantry deployment. Reassess the
abstraction before adding the second provider, Kubernetes, or AWS.
