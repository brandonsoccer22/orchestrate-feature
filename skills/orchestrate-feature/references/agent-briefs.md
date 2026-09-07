# Agent briefs and lifecycle

Use these fields selectively; a small task needs a small brief. Give model-overridden agents enough context to act without inherited history.

## Implementer brief

```text
Task: <one bounded unit and intended behavior>
Repository / current candidate: <path and revision or working-tree baseline>
Requirements: <authoritative sources and concrete acceptance criteria>
Prerequisites: <accepted dependency revisions and agreed interfaces>
You own: <exclusive write paths>
Other writers / shared files: <who owns what; how to request a change>
Constraints: <scope, data isolation, applicable permissions, relevant conventions>
Verify: <appropriate checks; distinguish required checks from suggested ones>
Return: changed paths, commands/results, unresolved findings, and candidate revision.
Do not change source task acceptance or declare the overall goal complete.
```

A self-contained brief should state important shared decisions in prose rather than requiring the child to reconstruct the entire conversation. Link source documents for details. Do not copy secrets, synthetic account credentials, raw audio, or irrelevant output into handoffs when a local fixture/reference suffices.

## Independent verifier brief

```text
Verify <unit> at <revision or clearly frozen file set> against <requirements>.
Inspect actual implementation and wiring, derive missing acceptance checks, and
run the checks appropriate to the requested behavior. Own <test/report paths>;
report production fixes to the orchestrator rather than racing another writer.
Return pass or actionable findings with file references, exact evidence,
coverage limits, and requirements that remain unproven. Do not check task boxes.
```

An initial review should receive requirements and raw candidate artifacts, not a script for finding a predetermined answer. A fix verification can receive the prior finding and expected invariant. If the candidate changes during a review, identify the change and rerun only the affected review/checks; do not attach stale evidence to a new revision.

## Native tool lifecycle

Use the tools exposed by the current host; names and schemas can change. When the host provides the `collaboration` tools:

- `list_agents`: inspect current status and capacity before decisions that depend on them.
- `spawn_agent`: create a bounded child; model overrides require the host-supported fork mode. Supply concrete work and its owned paths.
- `send_message`: steer an active child or exchange information. It may not start a new turn for an idle child.
- `followup_task`: resume an idle/completed child with a concrete task; reuse its context when relevant.
- `interrupt_agent`: stop current work when necessary for an ownership transfer or correction. Verify the resulting state before another writer edits those paths.
- `wait_agent`: wait for a live agent's progress without rapid polling. Inspect returned messages and resulting files; neither a timeout nor an agent's final prose establishes acceptance.

Call orchestration tools directly when the host requires it; do not assume they exist inside a JavaScript tool-orchestration namespace. Track returned handles, not guessed IDs.

User-visible task creation and messaging APIs are different from subordinate agents. Do not create sidebar tasks, send work into other user-owned conversations, or hand off the current task merely to simulate subagents. Use them only when the user actually requests that workflow.

When native subagents are unavailable, say so and retain the plan/acceptance structure while working locally. Do not claim independent verification by a separate model if none ran. If independent agent review is an explicit acceptance requirement, leave that gate open until it can be fulfilled.
