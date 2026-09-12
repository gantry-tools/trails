# Trails initial game plan

This is the first bounded campaign. It turns the current product idea into tested
foundations without prematurely building a universal cloud platform.

Each checkpoint should be independently reviewable and committed only after its
declared tests pass. No checkpoint authorizes paid infrastructure by implication.

## Phase A - Freeze the contract

### Checkpoint 1 - Repository and decision foundation

- Select implementation language based on static distribution, API/SSH support,
  testability, and maintainability.
- Establish build, test, formatting, linting, license, contribution, and release
  skeletons.
- Record supported host platforms and explicit non-goals.
- Add architecture-decision records for consequential choices.

### Checkpoint 2 - Versioned manifest envelope

- Define deployment identity, schema version, metadata, ownership, purpose, and
  expiry.
- Reject unknown or unsupported fields deliberately.
- Add deterministic parsing, validation, canonicalization, and diagnostics.
- Keep credentials and provider-generated identifiers out of authored intent.

### Checkpoint 3 - Intent model

- Model nodes, roles, workloads, networks, ingress, data, backups, placement,
  resilience, verification, and budget constraints.
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

## Phase B - Durable execution without a cloud bill

### Checkpoint 6 - State and inventory

- Persist desired state, resolved plan, exact resource identities, observed state,
  schema versions, and ownership metadata.
- Use atomic local updates and crash-safe recovery.
- Define locking and concurrent-operator behavior.
- Provide inspect, export, and redacted support bundles.

### Checkpoint 7 - Operation journal

- Record each intended call, idempotency key, attempt, response classification,
  result, and rollback/continuation status.
- Redact secrets structurally rather than by log-string convention.
- Support interrupted-operation inspection and safe resume.

### Checkpoint 8 - Fake provider

- Implement create/read/update/delete, asynchronous convergence, pagination,
  rate limits, transient errors, permanent errors, and partial failure.
- Track every simulated billable resource class.
- Add deterministic clocks and failure injection.
- Simulate runner loss and automatic expiry without relying on wall-clock sleeps.

### Checkpoint 9 - Execution engine

- Execute a dependency graph with bounded concurrency and retry policy.
- Distinguish retryable, terminal, authorization, budget, and unknown outcomes.
- Resume safely after interruption without duplicating resources.
- Never infer deletion targets from names or incomplete state.

### Checkpoint 10 - Reconciliation and teardown proof

- Compare desired, inventory, and observed fake-provider state.
- Report drift without mutating by default.
- Destroy in dependency-safe order under a separate approval.
- Verify that no simulated compute, disks, snapshots, addresses, load balancers,
  backups, DNS records, or network resources remain.
- Prove cleanup still runs after deployment, verification, and scenario failures.

## Phase C - Manual host operation

### Checkpoint 11 - SSH trust and inventory

- Import explicit hosts and roles.
- Verify known host keys and reject changed or unknown identity by policy.
- Use a narrow deployment account and short-lived keys.
- Perform read-only capability and operating-system discovery.

### Checkpoint 12 - Host bootstrap

- Plan and apply users, directories, packages, service definitions, firewall
  requirements, and health endpoints.
- Make repeated application idempotent.
- Provide exact manual commands corresponding to every operation.

### Checkpoint 13 - Configuration bundles

- Define versioned validate/diff/apply/export/rollback behavior.
- Separate shared configuration, per-node configuration, secret references, node
  identity, runtime state, and database state.
- Add checksums, provenance, compatibility checks, and staged activation.

### Checkpoint 14 - Application adapter contract

- Define install, configure, start, stop, status, verify, upgrade, rollback, and
  uninstall operations.
- Require structured output and stable exit behavior for agents and automation.
- Implement a deliberately small unrelated demonstration service first.

## Phase D - First Gantry and provider proof

### Checkpoint 15 - Two-node Gantry blueprint

- Model one Watchpost server and one Watchpost Agent host.
- Keep Gantry-specific logic outside the orchestration core.
- Document complete manual setup, configuration, pairing, verification, and
  removal alongside the Trails path.
- Define the topology as an experiment with a hard TTL, cost ceiling, baseline,
  evidence bundle, and post-run inventory.

### Checkpoint 16 - Akamai read-only adapter

- Discover permitted regions, plans, images, networks, firewalls, and relevant
  prices without creating resources.
- Enforce account, region, resource-count, and cost ceilings.
- Compare provider responses against recorded contract fixtures.

### Checkpoint 17 - Akamai disposable deployment

- With separate explicit authorization, create the smallest suitable two-node
  environment.
- Bootstrap and deploy the Gantry blueprint.
- Verify pairing, monitoring, restart behavior, persistence, and external health.
- Exercise one bounded node or service interruption and retain its timeline.
- Destroy and independently confirm that no billable resources remain.

### Checkpoint 18 - Vultr adapter parity

- Implement the same bounded capability and lifecycle contract against Vultr.
- Preserve provider-specific limitations in plans and evidence.
- Run the same disposable Gantry deployment and teardown campaign.

### Checkpoint 19 - Multi-provider backup exercise

- Place primary service infrastructure on one provider and encrypted backups on
  the other.
- Verify backup integrity and restore onto a clean secondary-provider host.
- Record achieved recovery point and recovery time rather than promising them.
- Confirm teardown across both provider inventories.

The exercise should be expressible as a reusable test-network scenario rather
than a one-off sequence embedded in an agent prompt.

### Checkpoint 20 - First campaign assessment

- Compare plan stability, manual parity, provider abstraction pressure, security,
  cost accuracy, failure recovery, and orphan-resource behavior.
- Deploy one unrelated application without core Gantry conditionals.
- Run a full unrelated test-network lifecycle: provision, deploy, seed, establish
  baseline, disrupt, verify recovery, collect evidence, destroy, and audit.
- Assess whether Watchpost/Webfleet integration materially improves evidence while
  confirming that non-Gantry observers and checks remain supported.
- Decide whether to continue toward Kubernetes/AWS, revise the manifest, or keep
  Trails as a focused Gantry deployment harness.
- Reconcile the handover and roadmap with evidence.

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

The first campaign ends at Checkpoint 20. Later Kubernetes, AWS, clustered
Gantry, multi-region, continuous reconciliation, and general product work must be
planned from its evidence rather than appended automatically.
