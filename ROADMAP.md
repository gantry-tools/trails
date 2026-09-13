# Trails roadmap

## Destination

Trails aims to make independently operated infrastructure and realistic test
networks feel as approachable as managed hosting without sacrificing ownership,
inspectability, portability, or manual control.

The product category is **contract-driven self-hosting and infrastructure
composition**: infrastructure remains user-owned, `map.json` defines desired state,
and Trails supplies the planning, generation, control, and evidence layer. Agents may
assist or execute approved work, but AI is optional and must never be required for a
supported path.

A user should be able to author or import `map.json`, inspect the Trail Map and a
resolved topology/security/cost plan, approve bounded changes, generate deterministic
scripts, and receive a verified system plus a complete operational handover. The same
contract must work from the Web UI, CLI, scripts/CI, ordinary automation, or an
optional agent.

This destination includes virtual machines, cloud services, Kubernetes, bare SSH
hosts, multi-provider systems, and short-lived networks for integration, upgrade,
failure, and recovery testing. It does not justify claiming those surfaces before
their adapters and failure modes have been exercised on real disposable
infrastructure.

Trails is intended to be a standalone Gantry ecosystem project under the
principle **works well by itself, works even better with other Gantry tools**.
Gantry is its first demanding dogfood workload and integration family, not its
only application model.


## Shared Trail Map contract

`map.json` is the canonical desired-state contract for both Trails and Gantry Atlas.
It represents an infrastructure graph using stable **waypoints**, **connections**, and
**trails**. Trail endpoints are **trailheads**; route branching/convergence points are
contextual **junctions**; notable waypoints may be marked as **landmarks**. Connections
may be directed or undirected.

Trails realizes the desired map. Atlas observes the running estate and reconciles it
against the same map. Observed state may be exported separately (for example
`map.state.json`), but must never blur the invariant `map.json = intent`. The shared
schema/parser/types should ultimately live in Gantry Core so neither product forks the
contract.

## Product milestones

### Milestone 0 - Contract and local planning foundation

- Decide implementation language and supported platforms.
- Freeze experimental `gantry.map/v1` JSON Schema and explicit compatibility policy.
- Implement validation, normalization, stable plan output, and useful errors.
- Model waypoints, connections, trails, lifecycle ownership, landmarks, dependencies, placement, costs, expiry, approvals, and capabilities.
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
- Keep configuration, secrets, waypoint/host identity, runtime state, and data separate.
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

### Milestone 4 - Disposable test networks

- Define experiment manifests over ordinary deployment topology.
- Add workload deployment, fixture/data seeding, baseline checks, scenario phases,
  success criteria, evidence collection, and unconditional cleanup phases.
- Support bounded restarts, process/host loss, network interruption, latency,
  constrained resources, version skew, rolling upgrades, backup/restore, and
  provider-loss simulations where the target permits them safely.
- Add TTL enforcement, heartbeat/lease behavior, abandoned-run discovery, and
  post-destruction billable-resource reconciliation.
- Run the same scenario manually, through conventional automation, and through an
  agent using the durable experiment contract.
- Prove an unrelated distributed application without Gantry-specific core logic.

Exit condition: Trails can create, exercise, evidence, and completely remove a
useful multi-waypoint/multi-host environment, including after scenario or runner failure.

### Milestone 5 - Multi-provider resilience

- Add placement rules for provider, region, zone, and correlated failure domains.
- Connect provider networks through explicit secure tunnels such as WireGuard.
- Support encrypted cross-provider backups and scheduled restore verification.
- Introduce warm-standby and rebuild-from-state strategies.
- Run controlled provider-loss drills and record RTO/RPO evidence.
- Reconcile resources and costs across all participating providers.

Exit condition: a tested deployment survives the declared provider-loss model
without relying on unrecorded agent knowledge.

### Milestone 6 - Kubernetes backend

- Treat Kubernetes as a declarative backend rather than a fleet of SSH targets.
- Support existing kubeconfig/context discovery with strict scope selection.
- Resolve workloads into reviewable manifests or stable package values.
- Observe rollout, health, events, resource use, and rollback.
- Support managed clusters and lightweight k3s through separate bootstrap paths.
- Exercise multi-cluster placement and recovery without bypassing Kubernetes.

Exit condition: one Gantry workload and one unrelated workload pass repeatable
deployment, update, rollback, and teardown in disposable clusters.

### Milestone 7 - AWS and broader cloud services

- Add tightly scoped, short-lived IAM session operation.
- Begin with a narrow EC2/VPC/security-group/storage subset.
- Add Route 53, load balancing, object storage, managed databases, or EKS only as
  complete capability slices selected by evidence.
- Enforce account, region, service, tag, permission-boundary, and budget policy.
- Retain CloudTrail-compatible operation correlation.

Exit condition: each claimed AWS slice passes live tests without requiring broad
account permissions or undocumented console intervention.

### Milestone 8 - Reconciliation and operations

- Detect drift between desired, resolved, and observed state.
- Distinguish harmless, repairable, sensitive, and destructive drift.
- Add controlled upgrades, scaling, certificate renewal, backup monitoring, and
  scheduled verification.
- Provide plan-only recommendations by default; retain approval for mutations.
- Improve diagnostics and recovery after partial provider outages.

Exit condition: Trails can maintain supported systems over time without becoming
an unbounded autonomous production operator.

### Milestone 9 - General product qualification

- Deploy at least one unrelated real application without Gantry-specific core
  changes.
- Test Linux architectures, supported providers, Kubernetes variants, and CLI
  platforms within an explicit matrix.
- Fuzz `map.json` documents and provider responses; run sanitizers where applicable.
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

Agents should be able to discover capabilities, generate valid `map.json`, explain plans, request bounded approvals, invoke stable commands or approved scripts, interpret structured results, diagnose failures, and create handovers. Prompt text is not an API, and every supported workflow must also work without an agent.

### Agent-managed self-hosting

- Make ownership, operator identity, authority, and approval state explicit.
- Support replaceable agents and models through stable schemas, commands, and
  evidence rather than conversation-specific knowledge.
- Provide selectable autonomy policy: observe, recommend, execute approved plans,
  or perform narrowly pre-authorized routine operations.
- Keep recurring maintenance, backup checks, certificate renewal, updates, drift
  inspection, incident diagnosis, and recovery rehearsals visible and auditable.
- Produce continuously useful human handovers, not only an export at deployment.
- Test loss of the originating agent, workstation, and orchestration process as
  operational continuity cases.

### Test-network experience

Experiments should be reproducible from `map.json` plus retained artifacts rather
than a chat transcript. The same topology should support selectable scenarios,
observers, traffic generators, evidence collectors, and cleanup policy. Scenario
failure must not skip evidence capture or teardown.

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
manual SSH path and one inexpensive two-node Gantry deployment. Use it to prove
the first complete disposable test-network lifecycle. Reassess the abstraction
before adding the second provider, Kubernetes, or AWS.
