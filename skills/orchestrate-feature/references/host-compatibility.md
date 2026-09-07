# Host compatibility

The shared workflow separates orchestration, implementation, and independent verification. Host APIs and model names differ.

## Codex

Use the host's native subagent tools when exposed. Astra/Luna/Terra are preferred roles, subject to actual availability; resolve supported identifiers before dispatch. The `collaboration` lifecycle examples in `agent-briefs.md` apply only when those tools exist. A persistent goal is optional and requires an explicit user request.

Codex loads user skills from `~/.agents/skills` and project skills from `.agents/skills`. Some installations also expose legacy/custom skill directories, including the `.codex/skills` location used by the original installation of this skill. Prefer the current documented discovery locations for new installs, and avoid duplicate copies with the same skill name. See [official skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills).

## Claude Code

Install the same skill folder in `~/.claude/skills/orchestrate-feature` or a project's `.claude/skills/orchestrate-feature`, and invoke `/orchestrate-feature`. Keep this skill in the main conversation: do not configure it with `context: fork` for orchestration. Use available Claude Code subagent facilities for bounded child work; do not try to call Codex's `collaboration.*` or goal APIs.

Choose models available to your Claude account/configuration. You can use the selected main model for orchestration and separate implementation and review agents with supported model aliases or inherited models. Independence means separate authorship/review context, even when both agents use the same model. Do not assume a cross-provider model alias or automatically change the user's model settings. Custom reusable agent definitions are optional; configure them with `/agents` if desired, following the current [subagent documentation](https://code.claude.com/docs/en/sub-agents).

The skill follows Claude Code's documented `SKILL.md` format and [installation locations](https://code.claude.com/docs/en/skills#where-skills-live). `agents/openai.yaml` is Codex UI metadata, not a Claude subagent definition. This package has not been end-to-end tested in Claude Code; the original workflow and forward testing ran in Codex. If a host lacks delegation, work locally and report the independent-review gate as unfulfilled when explicitly required.
