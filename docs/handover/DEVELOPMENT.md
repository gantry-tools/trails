# Development practice

Trails should develop as narrow vertical infrastructure contracts rather than a
wide collection of half-working provider calls.

## Checkpoint loop

```text
define exact intent and safety boundary
-> add fake-provider and contract tests
-> implement the smallest complete path
-> inject interruption and provider failures
-> verify idempotency, resume, and teardown
-> run manual and agent-facing workflows
-> perform live disposable validation only when authorized
-> retain redacted evidence
-> reconcile handover and roadmap
-> commit a clean checkpoint
```

## Testing layers

### Pure tests

- manifest parsing, validation, migrations, and diagnostics;
- capability matching and deterministic planning;
- dependency graphs and cycle detection;
- plan digests and approval invalidation;
- cost, policy, and resource-limit enforcement;
- state transactions, locking, and crash recovery;
- redaction and evidence serialization.

### Fake-provider contracts

- asynchronous creation and deletion;
- pagination and rate limiting;
- transient, permanent, authorization, and quota errors;
- timeout with an unknown external result;
- duplicate requests and idempotency keys;
- partial dependency success;
- out-of-band drift;
- orphan detection and complete teardown accounting.

Every real provider adapter should run against the same applicable lifecycle
contract, plus provider-specific cases.

### Local integration

- disposable local VMs or containers where useful;
- SSH host-key and credential behavior;
- service installation and restart;
- staged configuration apply and rollback;
- health and behavioral verification;
- support-bundle redaction.

### Live disposable evidence

Live tests require explicit authorization and hard provider-side constraints.
Record exact versions, account/project class without secrets, regions, resources,
plan digest, commands, timing, costs, failures, verification, teardown, and final
inventory. Never call an environment failure a pass.

## Adapter design

- Keep provider SDK types behind adapter boundaries.
- Preserve raw provider identity and meaningful provider-specific facts in state.
- Prefer read-before-create and observe-before-retry.
- Classify errors structurally rather than matching presentation strings where an
  API offers stable codes.
- Respect rate limits and bounded concurrency.
- Record API/SDK versions in evidence.
- Do not log authorization headers, user data containing secrets, private keys,
  signed URLs, kubeconfigs, or unredacted provider responses.

## Application blueprints

Blueprints describe application roles, configuration, dependencies, verification,
upgrade, backup, and recovery. They must use public application contracts and must
not leak special cases into provider adapters.

The initial Gantry blueprint should remain small until its manual equivalent is
documented and tested. An unrelated application blueprint is required before
claiming the core is general.

## Performance

Correctness, safety, and auditability dominate initial optimization. Still record:

- planning time for small and large inventories;
- provider request count;
- concurrency and rate-limit behavior;
- state size and journal growth;
- resume time after interruption;
- verification duration.

Performance work must not weaken observation, approval, journaling, or teardown.

## Documentation and claims

Examples are proposals until executable evidence promotes them. Documentation
must distinguish planned, experimental, and supported behavior. A provider name
in the roadmap is not a support claim.

Every support statement should specify the schema and adapter version, tested
resource families, provider/region scope, application workload, and known limits.
