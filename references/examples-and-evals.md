# Examples and evals

Use these examples to test whether a handoff preserves intent and authority without over-directing a capable agent.

## Manual agent relay

### Weak

```markdown
Tell the other agent to update the API and run tests. Use the approach we discussed.
```

This depends on missing conversation, hides who decided the approach, and gives the human courier no clean payload.

### Strong

```markdown
### Paste-ready prompt

You are implementing the already-approved diagnostics slice in `[repo]`.

Objective: expose enough sanitized evidence for the operator to understand why the existing scorer reached its verdict. This slice must not change verdict semantics, score bands, retrieval ranking, or policy gates.

Inspect the repository instructions, current branch/worktree state, and the existing adapter path before editing. The product boundary and public-output safety requirements are locked; you own the implementation structure, naming, and focused test placement. If the current contract cannot support the slice safely, stop and recommend the smallest compatible change.

Return the files changed and why, checks with actual outcomes, the locked invariants you verified, and anything still uncertain.

### Courier notes

Destination: implementation agent working in `[repo]`.
Paste verbatim. Bring back the implementation summary and verification results for review.
```

## Independent review

```markdown
Review `[artifact or diff]` independently and decide whether it is safe to ship.

The intended outcome is `[intent]`. Treat that intent and `[named invariants]` as locked, but do not assume the proposed implementation is correct. You own the diagnosis, severity ranking, and recommendation. This is read-only; do not modify files or external state.

Return findings first, ranked by impact, with precise evidence locations. Then list unresolved risks or verification gaps. If there are no material findings, say so directly.
```

## Research delegation

```markdown
Investigate `[question]` to support `[decision]`. Do not implement changes or contact anyone.

Use current primary sources where possible. Separate observed facts from inference and recommendation. The user has already decided `[locked decision]`; do not reopen it. `[open question]` remains open.

Return the strongest evidence, source links, conflicts or uncertainty, and a concise recommendation only if the evidence supports one.
```

## Reset handoff

```markdown
## Current frontier
[The last completed point and the next unfinished action.]

## User intent and decisions
- [Authoritative choices made by the user.]

## Verified facts
- [Observed state and completed checks.]

## Working assumptions
- [Re-checkable beliefs.]

## Open questions
- [Questions that remain questions, not implied tasks.]

## Artifacts
- [Repos, files, branches, plans, links, commands, and outcomes.]

## Recommended next move
[One concrete next action and why.]

## Do not lose
- [Scope, authorization, product meaning, or known failure modes.]
```

## Failure patterns

- **Command cosplay:** prescribing every local choice when the receiving agent has better repository evidence.
- **Authority laundering:** converting an agent recommendation into a user decision during relay.
- **Context dependence:** referring to "the approach above" or "what we discussed" in a standalone prompt.
- **Courier contamination:** mixing notes for the human with instructions for the receiving agent.
- **Generic mutation:** adding pull, commit, push, deploy, or edit instructions without authorization.
- **Proof mismatch:** requesting tests for research, or accepting a running job as proof of live completion.
- **Template gravity:** filling every heading even when a short paragraph transfers the intent cleanly.

## Eval rubric

| Metric | Pass condition |
|---|---|
| Intent | The receiver can state what must become true and why. |
| Provenance | User decisions, facts, assumptions, recommendations, and questions remain distinct. |
| Autonomy | The receiver owns appropriate judgment without inheriting undelegated product or side-effect authority. |
| Scope | Investigation, modification, publication, deployment, and other external actions are not conflated. |
| Standalone actionability | The receiver can begin without hidden conversation or invented context. |
| Evidence | Requested proof fits the actual work and completion boundary. |
| Relay integrity | Paste-ready content is separate from courier notes and survives manual transfer. |
| Restraint | Each instruction changes behavior or preserves important state. |

Test revisions against at least: a small subagent delegation, a human-carried implementation prompt, an independent review, a research task, and a reset handoff. Patch only repeated or consequential failures.
