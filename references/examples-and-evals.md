# Examples And Evals

Use this reference when calibrating prompt style, especially after a prompt feels verbose, vague, over-locked, or missing intent.

## Bad To Good: Codex Implementation

### Bad: over-locked and noisy

```markdown
Codex, implement the calibration diagnostics exactly as described. Create `diagnostics.ts`, add route fields, update the UI labels, use these function names, keep the order below, and run all checks. Do not deviate.

Steps:
1. Add every field from the plan.
2. Put parser data first.
3. Add tests.
4. Push when done.
```

Why it fails:

- Treats Codex as a command executor when repo reality may require judgment.
- Does not say which decisions are truly locked.
- Does not identify agent-owned choices.
- Defines no proof beyond "run checks."
- "Do not deviate" can force stale or brittle implementation.

### Good: medium freedom with intent transfer

```markdown
You are Codex working in `cobibean/chop-checker-app`. Pull latest `main` before starting.

## Mission
Add a small diagnostics surface that helps Cobi see why the scorer reached its verdict. The intent is not to tune verdicts yet; it is to expose evidence so policy/scoring changes can be made from data instead of vibes.

## Decision state
Freedom level: MEDIUM.

Locked decisions:
- This slice is diagnostics-only. Do not change verdict semantics, score bands, retrieval ranking, or policy gates.
- Preserve existing public API compatibility.
- Keep browser output sanitized; no local paths, auth details, provider errors, or raw internal dumps.

Agent-owned decisions:
- Exact component/helper boundaries.
- Field names inside internal helpers, if public adapter output stays compatible.
- Test placement and any small local refactor needed to keep the slice clean.

Open questions:
- If the existing adapter cannot expose this safely without an API contract change, stop and recommend the smallest compatible contract change before coding.

Forbidden drift:
- Do not make the AI narrator or planner own scoring evidence.
- Do not "fix" calibration by adding verdict-changing rules in this slice.

## Task
Read the current API/app adapter path, add the minimal diagnostics fields and UI surface needed to explain parser/retrieval/policy evidence, and cover it with focused tests.

## Verification / return evidence
Run the relevant lint/type/test commands available in the repo. Return:
- files changed and why;
- tests/checks run with real outcomes;
- invariants verified;
- anything blocked or unverified.
```

Why it works:

- Locks product intent and architectural invariants.
- Lets Codex use repo judgment for implementation details.
- Names open questions instead of pretending the plan is perfect.
- Defines proof of completion.

## Bad To Good: Codex Review

### Bad: asks for agreement

```markdown
Review this plan and tell me if it looks good. Focus on bugs and missing tests.
```

Why it fails:

- Does not grant enough freedom to reject the premise.
- Does not name important invariants.
- Gives no return shape for actionable synthesis.

### Good: high freedom review

```markdown
You are an independent Codex reviewer. Review the diff at `/tmp/review.patch`.

## Mission
Decide whether this slice is safe to hand back to Cobi as reviewed. You are not here to rubber-stamp the plan; challenge it against the code and invariants.

## Decision state
Freedom level: HIGH.

Locked:
- The feature intent is diagnostics visibility, not verdict-changing calibration.
- Public browser output must not expose local paths, auth details, secrets, or raw provider errors.

You own:
- Finding architecture, data-flow, compatibility, and test gaps.
- Recommending whether issues are blockers or follow-ups.

## Return format
- Verdict: PASS / PASS WITH FIXES / FAIL.
- Invariant checklist with PASS/FAIL and evidence.
- Concrete concerns with file/line references where possible.
- Suggested smallest fixes.
```

## Bad To Good: Reset Handoff

### Bad: transcript dump with fake certainty

```markdown
We talked about prompt writing. The prompt skill is better now. Next time continue improving it and use it for Codex prompts and reset prompts.
```

Why it fails:

- Collapses the actual correction into "improve prompts."
- Does not capture the over-locking failure.
- Does not distinguish verified facts, decisions, assumptions, and recommendations.
- Lets a future session treat a suggestion as a command.

### Good: truth capsule

```markdown
## Current state
Verified facts:
- Cobi said prior prompt writing was often verbose while missing truth/intent.
- The sharpest correction: Codex prompts often get too little freedom, making Codex behave like a command executor instead of using repo judgment.
- The new skill direction is intent transfer, not generic prompt-engineering theory.

User intent:
- Build a skill that triggers whenever Codex drafts another-agent prompt, worker prompt, review prompt, or reset handoff.
- Preserve model judgment while locking true intent, invariants, and proof.

Decisions made:
- Skill name/direction: `intent-transfer-prompting`.
- Core mechanism: calibrate freedom, split locked vs agent-owned vs open decisions, define evidence gates, and cut non-behavior-changing context.

Working assumptions:
- Most Codex implementation prompts should default to MEDIUM freedom unless the task is fragile or the user locked exact semantics.
- Reset handoffs need truth labels so future sessions do not treat assumptions as facts.

Open questions:
- Which concrete old prompts should become the long-term eval set?

Recommended next move:
- Test the skill against 2 real past Codex prompts and 2 reset handoffs, then patch only repeated or severe misses.

Do not lose:
- This is about preserving intent and calibrated judgment, not prettier formatting.
```

## Eval Rubric

Use this rubric when reviewing a prompt draft or improving the skill.

| Metric | Pass condition |
|---|---|
| Intent preservation | A new agent can state what matters and why in one sentence. |
| Freedom correctness | Freedom level matches task fragility; Codex is not over-locked by default. |
| Decision state | Locked, agent-owned, open, and forbidden drift are separated when relevant. |
| Constraint priority | There are 3 to 5 primary constraints, or extras are ranked by priority. |
| Actionability | The next agent can start without guessing the task. |
| Evidence gate | The prompt names what proof/checks/output must come back. |
| Truth labels for reset | Reset handoffs distinguish facts, decisions, assumptions, recommendations, and open questions. |
| Token discipline | Every sentence preserves intent, prevents drift, transfers state, assigns freedom, or defines proof. |

## Minimal Eval Set

When improving this skill, test against at least these four realistic tasks:

1. Codex implementation: write a prompt for a diagnostics-only slice where product intent is locked but implementation details are not.
2. Codex review: write a prompt asking Codex to challenge a plan/diff, not rubber-stamp it.
3. Reset handoff: hand off a messy conversation with decisions, assumptions, and product nuance.
4. Ambiguous PM handoff: write a prompt where the user has not chosen an implementation path; the next agent should recommend before coding.

Patch the skill only for repeated or high-severity misses. Do not add generic prompt-engineering techniques unless they fix a demonstrated failure.
