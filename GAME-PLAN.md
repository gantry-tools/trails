# Trails initial game plan

This is the first bounded campaign. It turns the current product idea into tested
foundations without prematurely building a universal cloud platform.

Trails is a declarative infrastructure planning and execution system for humans,
conventional automation, and agents. Its defining flow is:

The canonical flow is composable rather than AI-dependent:

> **Describe or author -> `map.json` -> Visualize -> Validate -> Approve -> Generate -> Provision -> Verify**

Every stage must also work when skipped or entered directly. Examples:

```text
Web UI -> map.json -> generate script -> run script manually
Web UI -> map.json -> generate script + prompt -> agent runs approved script
Describe -> agent -> map.json -> Web UI -> review -> generate script -> run manually
Hand-written map.json -> validate -> plan -> script -> CI/human/agent
```

`map.json` is the portable desired-state source-of-truth contract shared with Gantry Atlas. The Web UI, CLI, generated
scripts, and generated agent instructions are interfaces to that same contract, not
separate configuration systems. Gantry workflows are first-class dogfood and should
be the best-supported experience, but Trails must work equally with mixed stacks and
with no Gantry software at all.

Each checkpoint should be independently reviewable and committed only after its
declared tests pass. No checkpoint authorizes paid infrastructure by implication.

## Founding product rules

1. **One portable declarative contract.** Everything Trails can provision must have
   a complete representation independent of Web UI, CLI, provider, and agent.
2. **`map.json` is canonical.** JSON Schema defines and validates the shared Trails/Atlas desired-state graph contract.
3. **Desired, plan, and state are distinct.** Authored intent must never be confused
   with resolved provider operations or observed infrastructure.
4. **No UI-only state.** Web edits change the same `map.json` contract the CLI, scripts, Atlas, and agents consume.
5. **AI is optional at every layer.** Agents may author `map.json`, explain plans, or execute approved scripts, but every supported workflow must remain usable through the Web UI, CLI, generated scripts, and ordinary automation without AI.
6. **Approval separates interpretation from mutation.** Agent-generated intent is
   imported, validated, rendered, and reviewed before infrastructure authority.
7. **Deterministic execution remains first-class.** Trails must not require an LLM
   where a repeatable script/API operation can perform the work.
8. **Gantry is deeply integrated, never required.** Gantry presets can add richer
   defaults, relationships, health checks, and lifecycle knowledge while compiling
   to general infrastructure/application primitives.
9. **Existing infrastructure is valid input.** Trails can configure resources it did
   not create without claiming lifecycle ownership of them.
10. **Secrets are references.** Portable manifests/backups never contain raw
    long-lived credentials by default.

## Core artifact model

The public contract is **`map.json`**. It is short, portable, human-inspectable, and
shared by Trails and Atlas. `map.json` always means desired architecture/intent;
observed runtime state must not change that meaning.

Trails should explicitly version these artifact classes:

- **`map.json` - desired Trail Map contract.** The complete desired infrastructure
  graph: waypoints, connections, trails, lifecycle ownership, provisioning intent,
  services, networking, data, backups, verification, budget, and policy. It may be
  authored by a human, Web UI, CLI, another program, or an agent.
- **`map.plan.json` - resolved plan.** Deterministic provider-specific choices,
  dependencies, create/update/delete/configure operations, costs, warnings, and
  unsupported requirements, bound to the digest of `map.json`.
- **`map.state.json` - durable observed/execution state.** Actual resource IDs,
  addresses, generated names, versions, configuration digests, ownership, timestamps,
  observations, and operation results. Atlas may consume or enrich compatible
  observed-state data, but `map.json` remains desired state.
- **Execution material.** Generated provision/configure/verify/teardown scripts,
  manual command sequences, CI packages, agent briefs/prompts, and handover bundles.
  These are derived artifacts bound to the digest of an approved plan.

### Trail Map terminology

`map.json` models infrastructure as a graph:

- **Waypoint** - any graph vertex: machine, service, database, domain, network,
  repository, external service, Gantry app, or other modeled entity. Internally the
  implementation may use conventional graph `node` terminology.
- **Connection** - an edge/relationship between waypoints. Direction is explicit per
  connection where meaningful; the map is not forced to be a digraph.
- **Trail** - a meaningful path or subgraph through connected waypoints.
- **Trailhead** - an endpoint of a particular trail. In an undirected trail either
  endpoint is a trailhead; directed trails may additionally expose source/destination.
- **Junction** - a contextual waypoint at which routes meet, branch, converge, or
  diverge. Trailhead/junction roles are normally derived relative to a trail rather
  than persisted as universal waypoint types.
- **Landmark** - a notable waypoint intentionally highlighted for humans/agents.
  Landmark status is persistent metadata and can coexist with any structural role.

The same stable waypoint and connection identities should be consumable by Atlas so
that desired-vs-observed reconciliation does not require a translation layer.

## Phase A - Freeze the contract

### Checkpoint 1 - Repository and decision foundation

- Select implementation language based on static distribution, API/SSH support,
  testability, and maintainability.
- Establish build, test, formatting, linting, license, contribution, and release
  skeletons.
- Record supported host platforms and explicit non-goals.
- Add architecture-decision records for consequential choices.

### Checkpoint 2 - Versioned `map.json` contract

- Define `gantry.map/v1`, map identity, schema version, metadata, ownership, purpose, and expiry.
- Publish one JSON Schema as the canonical Trails/Atlas public contract.
- Prove lossless JSON import/export and canonical hashing.
- Reject unknown or unsupported fields deliberately.
- Add deterministic parsing, validation, canonicalization, and diagnostics.
- Keep credentials and provider-generated identifiers out of authored intent.
- Define explicit secret references for environment/local/future external stores.

### Checkpoint 3 - Trail Map graph model

- Model waypoints/compute, workloads/services, containers, databases, storage,
  DNS, firewalls, reverse proxies, networks, ingress, data, backups, placement,
  resilience, verification, and budget constraints.
- Model existing/external resources separately from Trails-owned resources.
- Model connections such as uses, proxied-by, monitored-by, stores-in, and network reachability, including directed and undirected relationships.
- Model named trails over the graph and derive trailheads/junctions contextually.
- Support persistent landmark metadata independently of graph structure.
- Separate required intent from hints and provider-specific extensions.
- Represent unsupported combinations explicitly.

### Checkpoint 4 - Capability and plan model

- Define provider capabilities and versioned discovery snapshots.
- Resolve intent into exact resources, dependencies, operations, and costs.
- Make the plan deterministic for fixed intent and capabilities.
- Explain every provider-specific choice and rejected alternative.

### Checkpoint 5 - Approval contract

- Bind approvals to a digest of the resolved plan and mutation class.
- Separate apply, sensitive modification, failure drill, and destroy approvals.
- Expire approvals when the plan, target account, or inventory changes.
- Prove that unapproved and stale plans fail before external calls.

## Phase B - Web, CLI, and agent authoring

### Checkpoint 6 - Component and blueprint model

- Define reusable components that compile to the general intent primitives.
- Support ordinary integrations such as Git repositories, systemd services,
  Docker/Compose, PostgreSQL, Caddy/nginx, and custom services.
- Define Gantry components as first-class presets: Cortex, Warden, Trestle,
  Watchpost, Watchpost Agent, and Webfleet.
- Add a Gantry Stack blueprint with single-machine, split, one-per-service, and
  custom topology choices.
- Prove mixed Gantry/non-Gantry and zero-Gantry Trails use the same schema.

### Checkpoint 7 - CLI authoring and inspection

- Establish `trails init`, `validate`, `plan`, `inspect`, `export`, and `status`.
- Support common intent without requiring hand-written JSON while retaining direct
  JSON editing/import/export.
- Produce stable structured output for agents and automation.

### Checkpoint 8 - Web application shell

- Serve a self-contained Web UI from Trails.
- Make Trails/projects the top-level object rather than providers.
- Initial navigation: Overview, Architecture, Configuration, Plan, State, Activity,
  and Files/Exports.
- Load/save exactly the same canonical `map.json` as the CLI.
- Add import/export and raw JSON inspection/editing with schema diagnostics.

### Checkpoint 9 - Architecture and configuration experience

- Render resources, applications, domains, networks, and relationships as a clear
  topology for review rather than a decorative graph.
- Clicking a waypoint opens configuration, ownership, and relationship details.
- Add flows for new infrastructure, existing machines, managed services, Gantry
  apps, containers, repositories, and custom services.
- Give Gantry richer forms while storing portable `map.json` semantics underneath.
- Make mixed-provider and mixed Gantry/non-Gantry architectures natural.

### Checkpoint 10 - Plan and approval experience

- Render infrastructure, software, networking, DNS, destructive actions, warnings,
  and estimated cost before approval.
- Show friendly presentation and exact JSON/machine plan side by side.
- Editing invalidates the old plan/approval.
- Only after approval expose execution choices.

### Checkpoint 11 - Agent `map.json` authoring contract

- Publish a concise schema-oriented contract for asking any capable agent to produce
  valid `map.json`.
- Validate agent output identically to human-authored output.
- Let the Web UI import agent output and present it visually for human review/edit.
- Return actionable validation errors suitable for sending back to the agent.

Target workflow:

```text
human request
-> agent produces map.json
-> Trails imports and validates
-> Web UI renders architecture/configuration/cost
-> human edits and approves
-> execution material is generated from the approved plan
```

### Checkpoint 12 - Deterministic script and optional agent-brief generation

- Generate repeatable provision/configure/verify/teardown scripts/packages for deterministic operations; these must be fully usable without AI.
- Generate constrained agent briefs from the approved plan rather than merely
  stringifying JSON.
- Include plan digest, allowed accounts/projects/regions/resources, budgets, required
  verification, and prohibited undeclared mutations.
- Require agents to stop and report if the provider cannot satisfy the approved plan.
- Require structured return of resource identities/results for state reconciliation.
- Never embed raw secrets into exported material by default.

### Checkpoint 13 - Execution choice boundary

After approval expose composable paths rather than an AI/non-AI fork: Run with Trails,
Generate script/package, Generate script + agent instructions, Generate agent
instructions for unsupported/manual gaps, or Export approved plan only. All paths bind
to the same approved plan; choosing an agent never grants retroactive planning
authority. A human or CI system must be able to run generated scripts directly.

## Phase C - Durable execution without a cloud bill

### Checkpoint 14 - State and inventory

- Persist desired state, resolved plan, exact resource identities, observed state,
  schema versions, and ownership metadata.
- Use atomic local updates and crash-safe recovery.
- Define locking and concurrent-operator behavior.
- Model operator identity separately from agent/model/session identity.
- Prove that replacing an agent or losing its transcript grants no authority and
  loses no durable deployment knowledge.
- Provide inspect, export, and redacted support bundles.

### Checkpoint 15 - Operation journal

- Record each intended call, idempotency key, attempt, response classification,
  result, and rollback/continuation status.
- Redact secrets structurally rather than by log-string convention.
- Support interrupted-operation inspection and safe resume.

### Checkpoint 16 - Fake provider

- Implement create/read/update/delete, asynchronous convergence, pagination,
  rate limits, transient errors, permanent errors, and partial failure.
- Track every simulated billable resource class.
- Add deterministic clocks and failure injection.
- Generate a continuation bundle that a fresh human or agent can use without the
  originating conversation.
- Simulate runner loss and automatic expiry without relying on wall-clock sleeps.

### Checkpoint 17 - Execution engine

- Execute a dependency graph with bounded concurrency and retry policy.
- Distinguish retryable, terminal, authorization, budget, and unknown outcomes.
- Resume safely after interruption without duplicating resources.
- Never infer deletion targets from names or incomplete state.

### Checkpoint 18 - Reconciliation and teardown proof

- Compare desired, inventory, and observed fake-provider state.
- Report drift without mutating by default.
- Destroy in dependency-safe order under a separate approval.
- Verify that no simulated compute, disks, snapshots, addresses, load balancers,
  backups, DNS records, or network resources remain.
- Prove cleanup still runs after deployment, verification, and scenario failures.

## Phase D - Manual host operation

### Checkpoint 19 - SSH trust and inventory

- Import explicit hosts and roles.
- Verify known host keys and reject changed or unknown identity by policy.
- Use a narrow deployment account and short-lived keys.
- Perform read-only capability and operating-system discovery.

### Checkpoint 20 - Host bootstrap

- Plan and apply users, directories, packages, service definitions, firewall
  requirements, and health endpoints.
- Make repeated application idempotent.
- Provide exact manual commands corresponding to every operation.

### Checkpoint 21 - Configuration bundles

- Define versioned validate/diff/apply/export/rollback behavior.
- Separate shared configuration, per-waypoint configuration, secret references, node
  identity, runtime state, and database state.
- Add checksums, provenance, compatibility checks, and staged activation.

### Checkpoint 22 - Application adapter contract

- Define install, configure, start, stop, status, verify, upgrade, rollback, and
  uninstall operations.
- Require structured output and stable exit behavior for agents and automation.
- Implement a deliberately small unrelated demonstration service first.

## Phase E - Gantry-first and provider proof

### Checkpoint 23 - Gantry workflow proof

- Model one Watchpost server and one Watchpost Agent host, with Warden/Cortex or
  Trestle added only if useful to the proof.
- Exercise Web, CLI, and direct JSON authoring of the same Gantry topology.
- Keep Gantry-specific logic outside the orchestration core.
- Document complete manual setup, configuration, pairing, verification, and
  removal alongside the Trails path.
- Define the topology as an experiment with a hard TTL, cost ceiling, baseline,
  evidence bundle, and post-run inventory.

### Checkpoint 24 - Akamai read-only adapter

- Discover permitted regions, plans, images, networks, firewalls, and relevant
  prices without creating resources.
- Enforce account, region, resource-count, and cost ceilings.
- Compare provider responses against recorded contract fixtures.

### Checkpoint 25 - Akamai disposable deployment

- With separate explicit authorization, create the smallest suitable two-node
  environment.
- Bootstrap and deploy the approved Gantry or mixed blueprint.
- Exercise direct Trails execution, generated deterministic material, and generated
  agent instructions where practical.
- Use an agent such as DeepSeek as a replaceable executor, never the holder of state
  or approval authority.
- Verify pairing, monitoring, restart behavior, persistence, and external health.
- Exercise one bounded waypoint/host or service interruption and retain its timeline.
- Destroy and independently confirm that no billable resources remain.

### Checkpoint 26 - Agent handoff and replacement proof

- Have an agent generate valid `map.json` from a natural-language requirement.
- Import it, review/edit it through the Web UI, approve it, and generate execution
  material from that approved plan.
- Permit an agent to execute only the approved material.
- Replace the operating agent/session mid-campaign and complete status, diagnosis,
  and teardown solely from Trails state and generated handover artifacts.
- Record every place the agent required undocumented provider or Gantry knowledge as
  an AI-DX defect.

### Checkpoint 27 - Vultr adapter parity

- Implement the same bounded capability and lifecycle contract against Vultr.
- Preserve provider-specific limitations in plans and evidence.
- Run the same disposable Gantry deployment and teardown campaign.

### Checkpoint 28 - Multi-provider backup exercise

- Place primary service infrastructure on one provider and encrypted backups on
  the other.
- Verify backup integrity and restore onto a clean secondary-provider host.
- Record achieved recovery point and recovery time rather than promising them.
- Confirm teardown across both provider inventories.

The exercise should be expressible as a reusable test-network scenario rather
than a one-off sequence embedded in an agent prompt.

### Checkpoint 29 - First campaign assessment

- Compare plan stability, manual parity, provider abstraction pressure, security,
  cost accuracy, failure recovery, and orphan-resource behavior.
- Deploy one unrelated application without core Gantry conditionals.
- Run a full unrelated test-network lifecycle: provision, deploy, seed, establish
  baseline, disrupt, verify recovery, collect evidence, destroy, and audit.
- Assess whether Watchpost/Webfleet integration materially improves evidence while
  confirming that non-Gantry observers and checks remain supported.
- Replace the operating agent mid-campaign and complete status, diagnosis, and
  teardown solely from Trails state and the generated handover.
- Decide whether to continue toward Kubernetes/AWS, revise the manifest, or keep
  Trails as a focused Gantry deployment harness.
- Reconcile the handover and roadmap with evidence.

## Product boundary with Atlas and Watchpost

`map.json` is a shared contract, not a Trails-private format. Trails and Atlas must use
the same schema, stable waypoint IDs, connection semantics, trail definitions,
lifecycle ownership, and compatibility rules. The shared parser/schema/types should
live in Gantry Core (for example `gantry-core/map`) once implementation begins.

The invariant is:

```text
map.json = desired infrastructure graph / intent
```

Trails asks **how do we make reality match this map?** Atlas asks **how closely does
reality match this map now?** Neither tool may silently reinterpret the contract.

Trails owns **desired architecture, planning, provisioning, configuration, and
teardown**. It may show enough observed state and health to plan, verify, reconcile,
and safely destroy what it manages, but it should not grow into the permanent
multi-provider estate console.

The proposed Gantry Atlas project owns the **running estate view**: cross-provider
inventory, topology, relationships, provider state, health rollups, and desired-vs-
actual estate drift. Watchpost remains the deep monitoring/alerting/incident layer.

```text
Trails    desired architecture / provision / configure / teardown
   |
Atlas     actual estate / topology / relationships / health
   |
Watchpost deep telemetry / alerts / incidents
```

Trails must work without Atlas or Watchpost, and Atlas must be able to use `map.json` without Trails. Future integrations should exchange this versioned contract and compatible observed-state overlays rather than creating hidden dependencies.

## Required evidence for live checkpoints

Retain, with secrets and personal infrastructure details removed:

- exact repository heads and schema/adapter versions;
- provider, region, resource types, quantities, and declared limits;
- approved plan digest and approval stage;
- estimated and observed cost where available;
- operation journal and final inventory;
- application versions and configuration-bundle digests;
- functional, health, restart, recovery, and external verification results;
- teardown calls and post-teardown provider inventory;
- all deviations, manual interventions, and unsupported behavior.

The first campaign ends at Checkpoint 29. Later Kubernetes, AWS, clustered
Gantry, multi-region, continuous reconciliation, and general product work must be
planned from its evidence rather than appended automatically.
