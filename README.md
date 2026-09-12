# Trails

Trails is an experimental, provider-neutral infrastructure composition project.
It is intended to make self-hosted systems easier to plan, provision, verify,
operate, recover, and hand back to a human across virtual machines, Kubernetes,
cloud services, and multiple providers.

The long-term interaction is simple:

```text
describe the desired system
-> inspect topology, security, and cost
-> approve a concrete plan
-> provision and configure
-> verify behavior and recovery
-> retain an auditable manual handover
```

Agents should be excellent Trails operators, but agents are not the source of
truth. Every deployment must remain understandable and operable through public
manifests, commands, inventories, evidence, and recovery procedures.

## Project status

Trails is currently a design-stage project. There is no released CLI, manifest
schema, provider adapter, or compatibility promise yet. The initial work is to
freeze a narrow contract and prove it with one inexpensive Gantry deployment.

Read these documents before implementation:

- [HANDOVER.md](HANDOVER.md) - authority, boundaries, and development rules.
- [ROADMAP.md](ROADMAP.md) - staged product and engineering roadmap.
- [GAME-PLAN.md](GAME-PLAN.md) - the first bounded execution campaign.
- [Architecture](docs/handover/ARCHITECTURE.md) - proposed control-plane model.
- [Safety](docs/handover/SAFETY.md) - credentials, cost, approval, and teardown.
- [Development](docs/handover/DEVELOPMENT.md) - checkpoint and evidence practice.

## Intended scope

Trails may eventually coordinate:

- ordinary SSH-accessible Linux hosts;
- Akamai, Vultr, AWS, and other compute-provider APIs;
- Kubernetes clusters, including managed Kubernetes and lightweight k3s;
- DNS, firewalls, private networks, load balancers, storage, and databases;
- deployments spanning providers and failure domains;
- configuration propagation, upgrades, backups, restore drills, and failover;
- Gantry applications and unrelated user-defined workloads.

This is a destination, not a current support claim.

## Core principles

1. **Manual-first, agent-friendly.** An agent automates documented operations; it
   never becomes the only entity that understands a deployment.
2. **Plan before mutation.** Resolve intent into an inspectable topology, action
   plan, security model, and price estimate before creating resources.
3. **State outside chat.** Desired state, resolved resources, operation journals,
   and observed state are durable artifacts rather than conversational memory.
4. **Explicit approval boundaries.** Planning, provisioning, sensitive changes,
   failover exercises, and destruction are distinct authorization stages.
5. **Provider-neutral intent, provider-specific truth.** Trails can normalize
   intent without hiding meaningful provider capabilities or limitations.
6. **Verification is part of deployment.** A resource existing is not evidence
   that the system works. Health, behavior, recovery, and external reachability
   need executable checks.
7. **Safe teardown is a feature.** Billable compute, disks, snapshots, addresses,
   load balancers, backups, and DNS must be inventoried and accounted for.
8. **Portability over lock-in.** Users should be able to inspect, reproduce,
   migrate, or manually operate what Trails creates.

## Non-goals for the first version

Trails will not initially be a complete Terraform, Pulumi, Ansible, Crossplane,
or Kubernetes replacement. It will not support every cloud service, continuously
change production without approval, store raw long-lived secrets in manifests,
or promise automatic recovery before restore and failure drills prove it.

## Candidate command shape

The public interface is not frozen, but the first contract should remain small:

```sh
trails validate deployment.yaml
trails plan deployment.yaml
trails apply deployment.yaml
trails status
trails verify
trails destroy
```

Every mutating command should also have a dry-run or plan representation suitable
for direct human use, conventional automation, and agent orchestration.

## License

No license has been selected yet. Do not copy external implementations or schema
material into the repository until licensing and attribution are settled.
