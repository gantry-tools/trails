# Disposable test networks

Disposable test networks are a central Trails product use case. They let users
exercise realistic multi-host and multi-waypoint applications without maintaining a permanent lab or
manually repeating provider, SSH, Kubernetes, deployment, observation, and
cleanup work.

This document describes the target contract. No scenario is implemented merely
because it is listed here.

## Product position

Trails should work as a standalone test-network tool for arbitrary applications.
Within the Gantry ecosystem it works even better: Watchpost and Watchpost Agent
can observe the environment, Webfleet can exercise sites and APIs, Warden can
provide an operational workspace, and Cortex can assist investigation. These are
optional integrations over public contracts.

## Experiment lifecycle

```text
validate experiment
-> discover target capabilities
-> resolve topology, cost, lifetime, and scenarios
-> approve provision plan
-> create network
-> configure hosts and deploy workload
-> seed disposable data
-> verify healthy baseline
-> approve and run bounded disruptions
-> verify expected failure and recovery
-> collect evidence
-> destroy exact experiment resources
-> refresh every provider inventory
-> issue result and cleanup report
```

Provisioning success is not the experiment result. The result combines workload
assertions, scenario observations, recovery behavior, evidence integrity, and
cleanup reconciliation.

## `map.json` experiment concepts

Test-network topology must use the same `map.json` graph contract as ordinary Trails
deployments. Experiments add scenario/evidence/lifetime policy around that map rather
than inventing a second topology language. This keeps the same waypoints, connections,
trails, landmarks, and stable identities usable by Atlas when an experiment is live.


An experiment should extend a normal deployment with:

- purpose, owner, unique identity, expiry, and maximum lifetime;
- hard cost and resource ceilings;
- workload artifacts and immutable versions;
- fixture or data-seeding steps using non-production data;
- baseline health and behavioral assertions;
- traffic sources and observation positions;
- ordered or dependency-linked scenarios;
- typed disruptions and enforceable bounds;
- recovery expectations and optional RTO/RPO objectives;
- evidence collectors and redaction policy;
- cleanup policy and explicitly protected resources.

The stable schema must distinguish a desired outcome from an observed result.

## Candidate scenarios

Useful bounded scenarios include:

- process crash and service restart;
- host/waypoint reboot, replacement, or loss;
- connection refusal, DNS failure, and dependency unavailability;
- network latency, jitter, bounded packet loss, and partitions;
- CPU, memory, disk-space, or file-descriptor pressure;
- rolling deployment and rollback;
- old/new version skew;
- database primary loss and replica promotion;
- backup corruption detection and clean restore;
- region or provider loss simulated through exact test-resource isolation;
- certificate renewal and expiration rehearsals;
- client retries, duplicate delivery, and reconnect behavior.

Each scenario needs target ownership checks, capability checks, maximum impact,
abort conditions, recovery actions, and expected observations. Trails must not
offer arbitrary destructive shell text as a safe scenario abstraction.

## Practical application families

The same machinery should help test:

- clustered HTTP and RPC services;
- replicated databases and caches;
- queues and worker fleets;
- monitoring and remote-agent systems;
- backup, synchronization, and disaster-recovery software;
- service discovery, proxies, and meshes;
- multiplayer or realtime servers;
- self-hosted CI runners and build farms;
- multi-region and multi-provider applications;
- Kubernetes operators and controllers.

## Observability and verification

An experiment may combine:

- provider and Kubernetes events;
- host/service logs and metrics;
- structured application health checks;
- internal and external HTTP/API checks;
- traffic-generator results;
- data-integrity and replication assertions;
- resource and cost observations;
- an ordered scenario/recovery timeline.

Watchpost, Watchpost Agent, and Webfleet should be first-class optional adapters.
Ordinary commands, OpenTelemetry-compatible systems, Prometheus, curl, and custom
checks must also be viable.

## Agent operation

An agent is particularly useful for choosing valid topology options, explaining
cost and failure assumptions, coordinating multiple APIs, observing convergence,
diagnosing a failed scenario, and producing the final report. The agent invokes
the experiment contract; it does not define resource ownership or remember the
only copy of the state.

The agent must stop at approval boundaries and must not broaden a failing
experiment into unapproved repair, extra resources, or destructive diagnosis.

## Cleanup and abandonment

Every experiment needs a durable cleanup-finalizer state before its first resource
is created. TTL and provider tags enable discovery, but expiry does not justify
unsafe deletion. Cleanup uses exact inventoried IDs and verifies ownership.

Runner loss, local state damage, provider timeouts, failed assertions, and failed
recovery are expected test cases. A separate read-only reconciliation command
should locate abandoned experiment resources and prepare an explicit cleanup plan.

Completion requires a fresh inventory across every involved provider, region,
Kubernetes cluster, and billable resource class. Retained resources must be
intentional, protected, documented, and costed.

## Evidence bundle

A redacted experiment bundle should contain:

- authored `map.json` and resolved plan;
- plan and approval digests;
- exact Trails, adapter, blueprint, workload, and scenario versions;
- provider/region/resource summary and costs;
- configuration and artifact digests without secrets;
- operation and observation timeline;
- baseline, disruption, recovery, and final assertions;
- relevant logs, metrics, and external checks;
- cleanup journal and post-teardown inventories;
- result classification, limitations, and manual interventions;
- enough commands and context for a human to reproduce or investigate.

## First proof

The first live proof should be deliberately small: one Watchpost server and one
remote Watchpost Agent, a bounded service interruption, observable recovery, a
complete evidence bundle, and independently verified teardown. The next proof
should use an unrelated application to confirm the core is not a Gantry-only
deployer.
