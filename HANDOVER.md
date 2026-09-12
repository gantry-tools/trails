# Trails development handover

This is the authoritative entry point for people and coding agents working on
Trails. Read this file, `README.md`, `ROADMAP.md`, `GAME-PLAN.md`, and the files in
`docs/handover/` before changing product behavior or making support claims.

## Current identity

- Product: Trails.
- Stage: design and contract formation.
- Repository state: documentation foundation only.
- Intended role: provider-neutral infrastructure planning and operation with
  excellent manual, automated, and agent-operated workflows.
- First dogfood workload: a small Gantry topology on disposable infrastructure.
- Generality test: the same core must deploy at least one unrelated workload
  without Gantry-specific logic.

No implementation language, manifest version, state format, provider, CLI syntax,
or release schedule is settled merely because it appears as an example.

## Product boundary

Trails owns:

- desired-topology validation;
- capability-aware planning and placement;
- cost and blast-radius presentation;
- approval boundaries and operation plans;
- provider, Kubernetes, and SSH adapter contracts;
- durable resource inventory and operation journal;
- configuration distribution orchestration;
- deployment, verification, drift inspection, recovery exercises, and teardown;
- complete human-readable operational handovers.

Trails does not own:

- application-specific data semantics;
- a hidden agent-only protocol;
- cloud-provider control planes;
- Kubernetes reconciliation internals;
- secret-manager implementations;
- database replication algorithms;
- general remote shell behavior;
- unsupported provider capabilities disguised as portable abstractions.

Applications and adapters must expose their actual contracts. Trails composes
them and retains evidence; it does not pretend that unlike systems are identical.

## Three equal operating paths

Every supported operation should be possible through the same durable contract:

1. **Manual:** documented commands, manifests, configuration bundles, join and
   revoke procedures, backup/restore steps, and diagnostic output.
2. **Conventional automation:** scripts, CI, configuration-management tools, or
   external infrastructure-as-code systems consuming stable machine interfaces.
3. **Agent operation:** an agent proposes, invokes, observes, verifies, and
   explains those same operations under explicit authorization.

An agent may improve usability and diagnosis, but no supported deployment may
depend on unrecoverable facts held only in chat history.

## State and artifact model

The design must distinguish:

- **desired state:** user-authored intent and constraints;
- **resolved plan:** exact provider choices, resources, operations, dependencies,
  estimated costs, and approval scope;
- **resource inventory:** provider IDs, regions, roles, ownership, expiry, and
  relationships;
- **secret references:** identifiers pointing to protected secret storage, never
  raw credentials embedded in normal state;
- **operation journal:** append-oriented record of attempted actions and results;
- **observed state:** provider and host facts gathered after operations;
- **evidence:** health, behavioral, recovery, and teardown results;
- **handover:** human-readable topology, access, maintenance, and recovery guide.

Configuration, secrets, runtime state, database state, and node identity are
different classes of data. Do not implement configuration propagation by copying
an entire application data directory between machines.

## Central safety rule

```text
agent intent != authorization to mutate infrastructure
```

Read `docs/handover/SAFETY.md`. At minimum:

- discovery and planning are read-only;
- a concrete resolved plan is approved before apply;
- destructive operations require separate, resource-specific approval;
- credentials are short-lived and least-privilege;
- provider/account/region/resource ceilings fail closed;
- cost and resource-lifetime limits are enforceable controls, not prompt advice;
- teardown reconciles every billable resource class after deletion;
- partial failure preserves enough state for safe continuation or recovery;
- production credentials and customer data are excluded from early testing.

Never work around provider permission failures, budget limits, approval gates,
unknown ownership, or uncertain resource identity.

## Architecture direction

The expected high-level pipeline is:

```text
intent -> validate -> discover capabilities -> resolve plan -> approve
       -> execute adapters -> observe -> verify -> reconcile -> hand over
```

Provider adapters translate resolved operations; they do not independently infer
desired topology. The orchestration core owns dependency ordering, idempotency,
journaling, recovery boundaries, and evidence. See
`docs/handover/ARCHITECTURE.md`.

## Gantry relationship

Gantry is Trails' first demanding blueprint, not its hard-coded product model.
A future Gantry example may include clustered Watchpost and Webfleet services,
Watchpost Agent across hosts, Trestle, Warden, Cortex, private networking,
databases, backups, DNS, TLS, monitoring, and provider-loss recovery.

Keep Gantry-specific defaults and verification in a blueprint or integration
package. Provider adapters and core planning must remain useful to unrelated
applications.

## Checkpoint standard

Every implementation checkpoint must define:

1. exact supported and unsupported behavior;
2. deterministic unit and contract tests;
3. idempotency and interrupted-operation behavior;
4. credential and permission boundaries;
5. cost and resource-lifetime controls;
6. a local or fake-provider test before paid infrastructure;
7. exact external resources created and deleted when live testing is authorized;
8. verification and retained evidence;
9. documentation and roadmap reconciliation;
10. a clean, reviewable commit.

Do not turn green mock tests into a cloud-support claim. A provider or Kubernetes
backend is supported only after disposable live infrastructure has passed create,
observe, update, failure, recovery, and teardown gates appropriate to its scope.

## Repository and public-action rules

- Preserve unfamiliar state or evidence until its ownership is classified.
- Do not commit credentials, private keys, raw tokens, kubeconfigs, state with
  secrets, or customer infrastructure details.
- Do not provision paid resources, alter DNS, access external hosts, push, tag,
  release, publish, or destroy remote resources without explicit authorization.
- A checkpoint commit does not imply a release or compatibility promise.
- Public claims must identify the manifest/schema version, adapter versions,
  tested provider/region, workload, and evidence boundary.

## Deeper handovers

- `docs/handover/ARCHITECTURE.md` - core, adapters, state, and reconciliation.
- `docs/handover/SAFETY.md` - credentials, budgets, approvals, and destruction.
- `docs/handover/DEVELOPMENT.md` - implementation and evidence workflow.
- `ROADMAP.md` - staged product direction.
- `GAME-PLAN.md` - initial bounded checkpoints.

Update this file whenever an architectural or operational invariant becomes
settled. Historical results belong in dated evidence, not as timeless claims.
