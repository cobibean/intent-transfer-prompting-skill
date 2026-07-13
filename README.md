# Intent Transfer Prompting Skill

Intent Transfer Prompting is a Codex skill for writing, reviewing, and relaying prompts that another agent, future session, worker, reviewer, or handoff recipient will act on.

The core idea is simple: write prompts as baton passes, not context dumps or command scripts. A good handoff preserves intent and provenance, grants capable agents the right autonomy, respects authorization boundaries, and asks for proof that fits the work.

## What It Helps With

- Codex implementation handoffs
- Subagent or worker prompts
- Human-carried agent-to-agent relays
- Review prompts
- Research and operations delegations
- Reset or continuation handoffs
- Prompt templates
- Paste-ready agent instructions

## Skill Contents

This repository ships the skill bundle as-is:

- `SKILL.md` - the main skill instructions
- `agents/openai.yaml` - agent metadata
- `references/examples-and-evals.md` - examples, eval rubric, and calibration guidance
- `references/provenance.md` - origin and design rationale

## Install

Clone this repository and copy it into your Codex skills directory:

```sh
git clone https://github.com/cobibean/intent-transfer-prompting-skill.git
mkdir -p ~/.codex/skills
cp -R intent-transfer-prompting-skill ~/.codex/skills/intent-transfer-prompting
```

Then start a new Codex session and invoke the skill as:

```text
$intent-transfer-prompting
```

## Usage

Use this skill before drafting, reviewing, or manually relaying instructions another model will execute. It also applies when spawning subagents or asking one agent to prepare a prompt for another.

For example:

```text
Use $intent-transfer-prompting to draft a Codex implementation handoff for this slice.
```

Or:

```text
Use $intent-transfer-prompting to review this worker prompt before I send it.
```

For a human-carried relay:

```text
Use $intent-transfer-prompting to turn this context into a standalone prompt I can paste into another agent. Keep courier notes separate from the paste-ready payload.
```

## Development

The skill payload is intentionally small and reference-driven. If you change behavior, test it against realistic prompts, especially:

- Small subagent delegations
- Human-carried implementation prompts
- Independent reviews and research tasks
- Reset handoffs

See `references/examples-and-evals.md` for the eval rubric.

## License

MIT. See `LICENSE`.
