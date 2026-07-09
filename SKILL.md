---
name: intent-transfer-prompting
description: Intent-transfer workflow for writing or reviewing prompts that another agent or future session will act on. Use when drafting Codex or Codex-PM implementation handoffs, subagent or worker prompts, review prompts, prep-for-reset handoffs, prompt templates, or any paste-ready agent instruction; calibrates model freedom, decision state, constraints, and evidence gates instead of context dumps.
---

# Intent Transfer Prompting

Use this skill before sending or reviewing instructions that another model, Codex thread, worker, reviewer, or future self will act on.

Write prompts as baton passes, not context dumps. A good handoff transfers intent, assigns the right amount of judgment, and names the evidence that proves completion.

## Workflow

1. Classify the prompt before drafting.

| Class | Use for | Default freedom |
|---|---|---|
| Diagnosis / review | Find truth, risks, root cause, or recommendation | HIGH |
| Implementation handoff | Build a chosen slice while preserving invariants | MEDIUM |
| Locked execution | Apply an exact decision or fragile sequence | LOW |
| Reset handoff | Preserve state, uncertainty, and next move | MEDIUM |

Do not default Codex implementation prompts to LOW freedom. LOW is for fragile commands, exact migrations, tiny patches, or decisions the user has explicitly locked.

2. State the freedom level.

Use one short block:

```markdown
Freedom level: HIGH.
You own the diagnosis and recommendation. Do not assume the proposed plan is correct. If repo evidence contradicts it, explain the better path before coding.
```

```markdown
Freedom level: MEDIUM.
Locked: objective, user/product intent, invariants, forbidden drift, and verification requirements.
You own: implementation route, local structure, test placement, naming, and safer sequencing if repo reality suggests it.
If the plan conflicts with the codebase, stop and recommend the smallest correction instead of forcing it.
```

```markdown
Freedom level: LOW.
Follow the specified sequence exactly. Do not broaden scope or redesign. If a step cannot be completed safely, stop and report the blocker.
```

3. Separate decision state when ambiguity or drift risk exists.

```markdown
## Decision state

Locked decisions:
- ...

Agent-owned decisions:
- ...

Open questions:
- ...

Forbidden drift:
- ...
```

Never bury an open question as a task. Do not give Codex choices the user already decided, and do not remove all judgment when repo reality needs interpretation.

4. Keep constraints scarce and ranked.

Use 3 to 5 primary constraints. If more are necessary, group them under:

1. Must preserve
2. Prefer
3. Avoid unless necessary
4. Open judgment

5. Add an evidence gate.

```markdown
## Evidence to return

- Files changed and why.
- Commands/tests run with real outcomes.
- Invariants checked.
- What remains unverified and why.
- Any recommended follow-up.
```

For repo work, include known verification commands. For reset handoffs, include what the next agent should inspect first.

6. Cut no-op context.

Keep a sentence only if it preserves intent, prevents likely drift, transfers state, assigns freedom, or defines proof. Cut transcript residue, stale implementation wishes, and prompt-engineering jargon that does not change behavior.

## Templates

Use templates as pressure tests, not forms to fill mechanically. Shorter is better when intent still survives.

### Codex Handoff

```markdown
You are Codex working in [repo/path]. Pull latest main before starting.

## Objective
[What we are trying to make true.]

## Intent
[Why this matters and what user/product meaning must not get lost.]

## Freedom level
MEDIUM by default.
Locked: [objective, invariants, forbidden drift, evidence].
You own: [implementation route, local structure, tests, naming, safer sequencing].
If repo evidence contradicts this plan, stop and recommend the smallest correction.

## Decision state
Locked decisions:
- ...
Agent-owned decisions:
- ...
Open questions:
- ...
Forbidden drift:
- ...

## Constraints
- [3 to 5 ranked constraints.]

## Verification
Run:
- ...

## Return
- Summary of changes.
- Files changed.
- Tests/checks with outcomes.
- Invariants verified.
- Anything unverified or risky.
```

### Reset Handoff

```markdown
## Current state
[Where the work stands now.]

## User intent
[What the user actually wants, including style/product direction.]

## Known facts
- Verified / directly observed.

## User decisions
- Explicitly chosen by the user.

## Working assumptions
- Likely, but re-check before relying.

## Open questions
- Do not collapse these into tasks.

## Artifacts
- Repos, files, plans, prompts, links, commands, test outcomes.

## Recommended next move
[One concrete next action and why.]

## Watch-outs
[Failure modes, over-locking risk, verification gaps.]
```

## Review Gate

A prompt is ready when:

- Freedom is calibrated: HIGH for diagnosis/review, MEDIUM for most implementation, LOW only for fragile or locked execution.
- Decision state is honest: locked decisions, agent-owned choices, open questions, and forbidden drift are not mixed together.
- Intent survives reset: the prompt says why the task matters, not only what to do.
- Constraints are prioritized: 3 to 5 primary constraints, or extras ranked by importance.
- Evidence is defined: the next model knows what proof/checks/output to return.
- Every sentence earns its place.

## References

- Load `references/examples-and-evals.md` when a prompt feels verbose, over-locked, vague, or high-stakes.
- Load `references/provenance.md` only when you need the origin of the skill's rules.

## Future Codex-PM Integration

When a global Codex-PM skill exists, patch it to call this skill before worker prompts, review prompts, reset handoffs, and any user-facing paste-ready instruction. Keep this skill standalone until that dependency exists.
