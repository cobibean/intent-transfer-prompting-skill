# Intent Transfer Prompting Skill

Intent Transfer Prompting is a Codex skill for writing or reviewing prompts that another agent, future session, worker, reviewer, or handoff recipient will act on.

The core idea is simple: write prompts as baton passes, not context dumps. A good handoff transfers intent, assigns the right amount of judgment, and names the evidence that proves completion.

## What It Helps With

- Codex implementation handoffs
- Subagent or worker prompts
- Review prompts
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

Use this skill before drafting or reviewing instructions another model will execute.

For example:

```text
Use $intent-transfer-prompting to draft a Codex implementation handoff for this slice.
```

Or:

```text
Use $intent-transfer-prompting to review this worker prompt before I send it.
```

## Development

The skill payload is intentionally small and reference-driven. If you change behavior, test it against realistic prompts, especially:

- Codex implementation handoffs
- Codex review prompts
- Reset handoffs
- Ambiguous PM handoffs

See `references/examples-and-evals.md` for the eval rubric.

## License

MIT. See `LICENSE`.
