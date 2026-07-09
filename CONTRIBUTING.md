# Contributing

Thanks for taking a look at Intent Transfer Prompting.

This skill is meant to stay compact, practical, and behavior-changing. Contributions should preserve that shape.

## Guidelines

- Keep the skill focused on intent transfer, calibrated freedom, decision state, constraints, and evidence gates.
- Prefer examples and evals over abstract prompt-writing theory.
- Avoid adding broad prompt-engineering techniques unless they fix a demonstrated failure.
- Keep `SKILL.md` short enough that an agent can load and apply it quickly.

## Testing Changes

Before proposing a behavior change, test it against at least these prompt classes:

- Codex implementation handoff
- Codex review prompt
- Reset handoff
- Ambiguous PM handoff

Document what failed before the change and how the change improves the result.
