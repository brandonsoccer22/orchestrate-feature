---
name: orchestrate-feature
description: Deliver a scoped software feature using an orchestrator, bounded implementation subagents, and independent verification subagents. Use for coordinated multi-agent development, dependency-driven execution of a Spec Kit plan, or a Conductor-like workflow without running Conductor.
---

# Orchestrate Feature

Coordinate implementation through the host's native subagent tools. Preserve the user's feature scope and acceptance gates across agent turns and context resets. This skill supports planning-only requests too; a plan is not authorization to implement it.

## Roles and model selection

For Codex, default role preferences are **Astra for orchestration/design**, **Luna for implementation**, and **Terra for independent testing/review**. Treat these as configurable preferences, not interchangeable names or claims about which model is actually running.

- Follow explicit user model choices. Resolve model identifiers and supported reasoning settings from current host/tool metadata before dispatch; do not invent identifiers.
- The current task is the orchestrator. Loading this skill cannot change its model. On Codex, if it is not Astra, be transparent and coordinate with the current model unless the user chooses otherwise. On Claude Code, use the selected Claude model and available Claude subagent models; Astra/Luna/Terra are not Claude model identifiers.
- When a preferred child model is available and overrides are permitted, select it explicitly. If unavailable, disclose that and use an allowed equivalent unless the user's exact model requirement prevents substitution.
- Observe fork/model compatibility. In environments where full-history forks cannot use model overrides, provide a self-contained bounded brief with no fork or a limited fork.

Roles are not a fixed three-agent limit: several implementers or verifiers may work on independent units when paths, dependencies, and host capacity permit. Do not make an unavailable implementer model a reason to use the same agent as both author and independent verifier.

The orchestrator owns architecture, design decisions, shared contracts, task scheduling, integration, user updates, and final acceptance. Implementers own assigned production changes. Verifiers read actual code, devise meaningful tests, inspect results, and report defects independently. The orchestrator may repair integration defects directly; those repairs still need verification.

## Establish the execution contract

Read repository instructions, user scope, authoritative specs, existing decisions, current Git state, and any execution ledger. Preserve existing edits and already accepted work.

Use the existing task/dependency system. For Spec Kit, keep project specs authoritative and regenerate any repository-specific execution projection with its documented command. Do not create a competing requirements source or invoke Conductor simply because the plan references it.

Record only the state needed to resume reliably: task ID, acceptance criteria, dependencies, path ownership, live agent/process handles, implementation revision, verification evidence, and status. An existing issue tracker or execution file is sufficient; avoid building an orchestration platform for this workflow.

Create a persistent goal only when the user explicitly requests one. Its objective should name the real deliverable and required gates; add a token budget only when explicitly requested. If a goal already exists, inspect and continue it rather than creating a duplicate. No goal tool is required to use the workflow.

## Schedule bounded work

Choose dependency-ready tasks and reserve their write paths. Delegate only concrete work that can proceed alongside useful orchestrator work, within the current concurrency limit.

- A task is ready when its required prerequisites are integrated and accepted, not merely reported as implemented. Independent scaffolding may proceed before a contract is accepted only when it does not consume that contract.
- Give implementers exclusive paths. Serialize shared schemas, generated contracts, lockfiles, migrations, and integration configuration when edits could collide.
- Before transferring path ownership, stop or finish the former owner's work and confirm it has relinquished those paths. A message alone is not proof that a running writer stopped.
- Use isolated worktrees only when they materially reduce conflicts; follow repository placement and integration conventions. Otherwise, explicit ownership in the shared checkout is sufficient.
- Keep dependent consumers idle while shared decisions are unresolved. The orchestrator can design interfaces, inspect running behavior, review finished work, or prepare independent tests in the meantime.

On Claude Code, keep orchestration in the main conversation, use its available subagent facilities, and resolve their actual lifecycle and model controls. Codex-specific goal and collaboration tools are optional host capabilities, not prerequisites. For host details, read [host-compatibility.md](references/host-compatibility.md).

Before dispatch or reassignment, read [agent-briefs.md](references/agent-briefs.md) for the compact implementer/verifier handoff format and tool lifecycle distinctions.

## Run the implementation and review loop

1. Send the implementer acceptance criteria, exact owned paths, dependencies, constraints, and a bounded stopping point. Require actual changes and evidence, not just a suggested solution.
2. Inspect its diff and authoritative test output. A completed agent turn means that turn ended; it does not establish task acceptance.
3. Assign an independent verifier the requirements and candidate revision/files. Ask it to derive coverage from requirements and inspect runtime wiring. Do not steer an initial review toward an intended pass.
4. Give actionable findings back to the implementer, or take explicit ownership of an integration repair. Keep fixes scoped to the finding and preserve unrelated behavior.
5. Reverify affected paths after changes. Once required checks pass, do not repeat broad suites without new changes or an unresolved concern.
6. Integrate an intentional atomic commit when repository instructions or the user's workflow call for commits. Record the exact revision and evidence before unblocking dependent tasks.

Use a fresh verifier when independence has eroded because the original verifier became the primary implementer. Reusing an agent for related follow-ups is normally preferable to spawning a new one for every message. Spawn a new agent for an independent ready task, a distinct specialty, or a clean review context—not to evade a failure, permission boundary, or exhausted capacity.

## Evidence that supports acceptance

A green test proves only what it exercises. Explicitly check distinctions that matter to this feature:

- An interface or container binding versus a runtime path that actually uses it.
- A fake client method versus real SDK serialization/decoding through an injected transport.
- Unit/backend evidence versus a requested real-stack browser journey.
- Browser/web behavior versus native device or simulator behavior.
- Parsed or queued input versus an accepted authoritative outcome.
- A provider segment versus an application-level final result; cancellation during a pending finish, if streaming is involved.

These are examples of coverage boundaries, not mandatory technologies for every feature. Read the applicable requirements and test the boundaries they actually specify. Never narrow a gate merely to obtain a passing result. If a gap appears during final review, reopen that work and keep the milestone incomplete.

For UI features, the orchestrator should directly inspect requested interaction/design flows with available browser or native tools. Use independent verifiers for code/tests as well. Preserve text or other fallback requirements, test-data isolation, and the user's authorization boundaries. Mocked cloud tests do not imply live cloud verification or permission to provision services.

## Resume, report, and finish

Resume from current files, diffs, accepted revisions, agent status, and confirmed live process handles. Poll an existing live operation; an observation timeout is not permission to restart it. If an operation appears hung, inspect its elapsed time, latest output, and resource ownership before deciding whether interruption is warranted; preserve useful diagnostics and confirm termination before restarting. Use an idle/completed agent's follow-up task operation when work must resume—ordinary messages may not wake it.

Keep the user informed about findings, repaired issues, remaining gates, and actual results. When asked for percentage progress, distinguish accepted tasks from partially implemented work; label a weighted percentage as an estimate. Do not equate lines written, agents finished, or green unit tests with feature completion.

Before completion, reconcile every explicit requirement against evidence, the final revision, and source task state. Check requested browser/native gates, generated artifacts, cleanup, commits, and remaining limitations. Mark only accepted tasks complete; leave out-of-scope future tasks unchanged. Complete a persistent goal only after this audit passes. Report the delivered behavior, key checks, relevant artifacts, and material limitations concisely.
