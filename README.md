# Orchestrate Feature

A reusable agent skill for delivering software features with an orchestrator, implementation agents, and independent verification agents. It uses native subagents; it does not require Codex Conductor, a server, or an API key of its own.

The workflow was developed while delivering a Laravel and React Native feature with Spec Kit. It is repository-agnostic and also works with other task plans.

## How it works

- The orchestrator owns design, dependencies, shared contracts, integration, and acceptance.
- Implementers receive bounded tasks with exclusive write paths.
- Independent verifiers inspect actual behavior and derive tests from requirements.
- Fix/review cycles continue until the requested gates pass; completed agent turns alone do not count as acceptance.
- Agents can be reused, resumed, or replaced, with ownership and live-process state preserved.

Codex preferences are **Astra → orchestration**, **Luna → implementation**, and **Terra → verification**. These are configurable preferences, not required model identifiers. On Claude Code, select available Claude models for the same roles. A skill cannot switch the main conversation's model or enable unavailable agent tools.

## Get the repository

Clone the repository (GitHub authentication and repository access are required while it is private):

```sh
git clone https://github.com/brandonsoccer22/orchestrate-feature.git
cd orchestrate-feature
```

If you already have the local checkout, `cd` into it instead. Run the install commands below from the repository root. They copy only the skill, not Git history or repository documentation. If the destination already exists, stop and review/back up that installation before replacing it.

## Install in Codex

For all projects:

```sh
mkdir -p "$HOME/.agents/skills"
if [ -e "$HOME/.agents/skills/orchestrate-feature" ] || [ -L "$HOME/.agents/skills/orchestrate-feature" ]; then
  echo 'An installation already exists; review it before replacing it.'
else
  cp -R skills/orchestrate-feature "$HOME/.agents/skills/orchestrate-feature"
fi
```

For one project, copy the same folder into that project's `.agents/skills/` directory. Avoid duplicate installations under different skill-discovery roots, including an existing `~/.codex/skills/orchestrate-feature` installation.

Invoke it in Codex:

```text
Use $orchestrate-feature to implement this Spec Kit plan.
Prefer Astra for orchestration, Luna for implementation, and Terra for verification.
```

Codex can discover matching skills automatically; if the new skill does not appear, restart Codex. See [official Codex skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills).

## Install in Claude Code

For all projects:

```sh
mkdir -p "$HOME/.claude/skills"
if [ -e "$HOME/.claude/skills/orchestrate-feature" ] || [ -L "$HOME/.claude/skills/orchestrate-feature" ]; then
  echo 'An installation already exists; review it before replacing it.'
else
  cp -R skills/orchestrate-feature "$HOME/.claude/skills/orchestrate-feature"
fi
```

For one project, use that project's `.claude/skills/` directory instead. Then invoke:

```text
/orchestrate-feature Implement this feature using separate implementation and verification agents. Use the Claude models available in this session.
```

The shared instructions use the documented Claude skill format. Claude uses its own agent tools and models; Codex model names and `collaboration.*` calls are not portable commands. The skill stays in the main conversation, which coordinates child agents. Custom `/agents` definitions are optional. See [Claude skills](https://code.claude.com/docs/en/skills) and [Claude subagents](https://code.claude.com/docs/en/sub-agents).

Claude compatibility is based on its documented format and facilities; an end-to-end Claude run has not been performed. Codex's `agents/openai.yaml` is UI metadata and should not be installed as a Claude agent definition.

## Example requests

```text
Use this skill to plan the feature only. Do not implement yet.
```

```text
Use this skill to implement the approved plan. Create a goal for the full milestone,
including browser integration tests and independent acceptance review.
```

```text
Resume the current milestone. Inspect live agents and test handles before restarting work.
Report accepted tasks separately from work still awaiting verification.
```

Spec Kit is optional. Existing repository instructions and acceptance criteria remain authoritative. Installing this skill does not authorize deployments, cloud provisioning, unrelated changes, or publishing.

## Update or remove

Pull reviewed updates with `git pull --ff-only` in this checkout, then compare the skill folder with your installed copy. Back up any local customizations before replacing it. To uninstall, remove only the `orchestrate-feature` folder from the discovery location you chose. This repository does not change global agent configuration or register background services.

## Files

- [`skills/orchestrate-feature/SKILL.md`](skills/orchestrate-feature/SKILL.md): core workflow.
- [`references/agent-briefs.md`](skills/orchestrate-feature/references/agent-briefs.md): dispatch briefs and agent lifecycle.
- [`references/host-compatibility.md`](skills/orchestrate-feature/references/host-compatibility.md): Codex/Claude differences.
- [`agents/openai.yaml`](skills/orchestrate-feature/agents/openai.yaml): Codex discovery metadata.
