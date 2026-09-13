# Safety contract

Trails will interact with systems that cost money, hold data, expose networks,
and can be difficult to recover. Safety controls are core behavior.

## Authorization stages

Treat these as distinct stages:

1. **Discover:** read-only provider and target inspection.
2. **Plan:** resolve intent, cost, security, and exact mutations without applying.
3. **Apply:** create or modify only the resources in an approved plan digest.
4. **Sensitive operation:** rotate credentials, alter access, restore data,
   promote replicas, or run disruptive failure tests under separate approval.
5. **Destroy:** delete exact inventoried resources under explicit approval.

Approval for one stage does not imply approval for another. Changed plans,
accounts, targets, costs, or inventories invalidate earlier approval.

## Agent authority

Agent convenience does not expand authority. Every agent invocation has an
authenticated operator identity, declared role, permitted targets, capability
set, expiry, and approval context. Switching models, agents, interfaces, or
sessions never carries implicit authorization forward.

Standing authorization, if supported later, must be narrow and mechanically
enforced—for example, restart one named service after a failed health check or
renew a certificate inside a declared window. It needs explicit bounds, expiry,
rate limits, revocation, notification, and journal entries. Ambiguous diagnosis,
cost increases, topology changes, credential changes, destructive recovery, and
unknown ownership stop for human review.

No safety decision may depend solely on an agent's memory, confidence, prompt, or
natural-language summary.

## Credential rules

- Prefer short-lived session credentials and dedicated roles.
- Use least privilege and resource-tag conditions where providers support them.
- Restrict accounts, projects, regions, services, networks, and mutation classes.
- Keep secret references in state; never store raw credentials in authored
  `map.json`, generated scripts, logs, evidence, support bundles, or agent transcripts.
- Generate unique short-lived SSH credentials and verify host keys.
- Rotate or revoke bootstrap credentials after commissioning.
- Never repurpose discovered credentials for a different provider or deployment.

AWS work additionally requires explicit account and region allowlists, permission
boundaries, session expiry, resource-tag policy, CloudTrail correlation, and deny
rules for IAM/organization mutation unless a later contract specifically needs
and authorizes them.

## Cost and lifetime controls

Before mutation, enforce:

- maximum estimated hourly and monthly cost;
- maximum cost for the approved operation or exercise;
- maximum resource count by type;
- allowed plans/sizes, regions, providers, and paid services;
- deployment expiry and maximum resource lifetime;
- optional maximum storage, snapshot, address, egress, and backup exposure.

Re-evaluate limits when actual resources or prices differ from the plan. Stop for
approval rather than substituting a more expensive resource silently.

## Network safety

- Default to private connectivity and deny-by-default ingress.
- Open only documented ports to explicit sources.
- Keep Warden, Cortex, databases, control planes, and administrative endpoints
  private or identity-protected by default.
- Make public origins, proxy trust, TLS termination, and forwarded-header policy
  explicit.
- Do not infer that provider-private networks extend across providers.
- Treat tunnels, routing, DNS, and load balancers as separately verified assets.

## Data safety

- Never use customer or irreplaceable production data in early campaigns.
- Distinguish configuration propagation from database replication or file copying.
- Encrypt backups before they leave the originating trust boundary.
- Retain independent checksums and restoration instructions.
- A backup claim requires a restore test; a failover claim requires a failover
  exercise and measured recovery results.
- Destruction must identify protected data and retention policy before deletion.

## Partial failure

Do not blindly roll back after every failure. Some reversals are destructive or
may remove the only healthy copy. The journal must classify operations as safely
retryable, observable, compensatable, irreversible, or requiring human review.

For unknown API outcomes, observe provider state before retrying. For lost local
state, use ownership metadata and read-only discovery to reconstruct candidates,
then require confirmation before adopting or deleting anything.

## Teardown

Teardown is complete only after a fresh provider inventory confirms the expected
absence or retained status of every billable class, including:

- compute instances and Kubernetes clusters;
- attached and detached disks;
- snapshots, images, backups, and object storage;
- public addresses and reserved IPs;
- load balancers and gateways;
- managed databases;
- DNS zones and records created by the deployment;
- private networks, firewalls, tunnels, and related services.

Automatic expiry is a secondary safeguard. It must not silently delete resources
whose identity, ownership, or data-protection status is uncertain.

## Early-campaign rule

Use the fake provider first. The first authorized live campaign should use the
smallest inexpensive disposable topology, a hard cost ceiling, non-production
domains and data, and immediate post-run teardown verification.

## Test-network disruption rules

- Apply disruptive scenarios only to resources owned by the exact experiment.
- Require a separate approved scenario plan for network partitions, service or
  node termination, resource pressure, failover, restore, and destructive tests.
- Respect provider acceptable-use rules; do not generate abusive public traffic,
  uncontrolled denial of service, scanning, or attacks on third-party systems.
- Bound traffic, duration, concurrency, latency, packet loss, CPU, memory, disk,
  and restart counts in enforceable configuration.
- Keep control access and cleanup paths independent from the failure being tested
  where practical.
- Treat loss of the experiment runner as a required cleanup/resume case.
- Never reuse a production hostname, network, credential, data set, backup, or
  account-wide resource as a disposable test target without an explicit later
  production-testing contract.
