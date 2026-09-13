# Agent-managed self-hosting

Trails is intended to make self-hosting feel closer to managed hosting while keeping AI assistance optional and
preserving the reason to self-host in the first place:

> **Your infrastructure, agent operated, human controlled.**

The user keeps ownership of infrastructure, data, credentials, configuration,
domains, provider accounts, and operational decisions. Trails makes agents useful
operators of that infrastructure without making an agent, model, conversation,
or Trails-hosted service the only place the system can be understood.

This is an optional operating mode, not the base dependency model. A user must be able
to author/review `map.json`, generate deterministic scripts, execute them manually or
through CI, verify the result, and tear it down without an AI system. Agent operation
composes around those same public contracts.

## What changes from traditional self-hosting

Traditional self-hosting often makes the owner the permanent operations team.
With Trails, an agent may handle much of the routine work:

- compare topology, provider, security, and cost options;
- provision and configure approved resources;
- distribute versioned configuration and deploy applications;
- verify health, behavior, backups, restore, upgrades, and recovery;
- observe drift and propose or perform policy-authorized maintenance;
- diagnose incidents from structured evidence;
- create test networks and run bounded failure exercises;
- prepare a current operational handover after every material change.

The result should offer much of the convenience of managed hosting without
requiring the user to surrender infrastructure or data ownership to a platform.

## What does not change

The user remains the authority. Agent intent is not infrastructure authorization.
Trails—not a prompt—enforces target identity, permissions, cost ceilings, expiry,
approval scopes, operation ordering, and destructive boundaries.

An agent may explain and recommend freely from authorized observations. Mutation
depends on explicit approval or a narrow, revocable standing policy. Trails must
not broaden authorization because an agent believes a change is helpful.

## Operator replaceability

Agents are interchangeable operators, not custodians of hidden state. A new
human, agent, model, or automation system should be able to continue using:

- authored `map.json` contracts and immutable resolved plans;
- current resource inventory and observed state;
- operation journal and pending approval state;
- secret references and access procedures, without exposed secret values;
- configuration and artifact versions;
- verification, backup, restore, and recovery evidence;
- exact manual commands and limitations.

The replacement path must not depend on access to the original chat transcript.
Loss of the originating agent, workstation, or orchestration process is a normal
continuity test, not an exceptional reconstruction exercise.

## Graduated autonomy

Deployments should choose an explicit autonomy policy rather than a single global
"autonomous" switch:

1. **Observe:** gather state and evidence without mutation.
2. **Recommend:** prepare a concrete plan and wait for approval.
3. **Execute approved work:** apply only a specific approved plan digest.
4. **Operate within policy:** perform narrowly pre-authorized recurring actions
   with bounds, expiry, notifications, and audit.

Sensitive, destructive, ambiguous, or scope-expanding work always returns to a
human approval boundary. Production autonomy must be earned capability by
capability through tests and evidence.

## Durable operations

Agent-managed operation is not a permanently open chat session. Monitoring,
schedules, provider events, and human requests produce structured observations or
proposals. Trails records them, evaluates policy, executes authorized operations,
and records results. Agents may help interpret and coordinate this process, but
durable state and recovery do not live inside model context.

The handover is a living product surface. After each material operation it should
reflect topology, versions, cost, access, routine commands, maintenance, backups,
restore, known risks, pending work, and recovery procedures.

## Portability and exit

Users must be able to change infrastructure providers, agents, models, and
interfaces independently. Provider-specific capabilities remain explicit, but
portable intent and exportable operational artifacts should prevent artificial
lock-in.

If Trails is removed, the deployed system should continue running and the user
should retain enough instructions to operate or dismantle it manually. If an
agent is removed, Trails should retain everything a replacement operator needs.
That two-sided exit path is the practical test of human control.

## Product test

Trails succeeds when a user can own serious self-hosted infrastructure without
personally performing every routine operation—and can still inspect, override,
replace, recover, migrate, or shut it down without asking permission from Trails
or any particular agent provider.
