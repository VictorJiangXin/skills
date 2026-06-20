---
name: proof-driven-engineering
description: "Use for complex engineering work where conclusions must be proven, not merely completed: debugging ambiguous production issues, reviewing or implementing systemic fixes, comparing old vs new engines or migrations, validating semantic parity, handling noisy traces/logs, reducing repeated mistakes into reusable rules, and answering users who ask for evidence chains, proof boundaries, root cause, methodology, or durable skill/process improvements."
---

# Proof Driven Engineering

## Core Principle

Treat engineering work as proof construction. Do not optimize for sounding decisive; optimize for making uncertainty explicit, compressing it into a coordinate system, and ending with a conclusion whose evidence boundary is visible.

The default standard is:

1. Define what would count as proven.
2. Build the coordinate system before interpreting facts.
3. Tie every conclusion to code, data, logs, tests, or controlled comparison.
4. State what is proven, what is not proven, and what would prove the rest.
5. Convert the lesson into a reusable trigger or checklist.

## Workflow

### 1. Define The Claim

Before fixing, reviewing, or explaining, write the claim in one sentence.

Examples:

- "This production behavior is caused by flag X being false for the relevant task."
- "This migration preserves old-engine semantics for this class of formulas."
- "This patch fixes the reported bug without changing unrelated behavior."

Then define the proof threshold:

- For a bug: the minimal code path, runtime parameters, and observation needed to establish root cause.
- For a migration: the old behavior, new behavior, shared input space, and parity oracle.
- For a fix: the failing case, the changed decision point, and regression coverage.
- For a review: the behavioral contract and the ways the change could violate it.

If the proof threshold is unknown, make that the first deliverable.

### 2. Build The Coordinate System

Complex systems usually look contradictory because multiple scopes are mixed together. Identify the axes before reading signals.

Common axes:

- Entity identity: request ID, tenant, project, type, object ID, task ID, node ID.
- Execution scope: main path vs follower path, sync path vs user path, debug path vs business path.
- Semantic scope: raw vs display, nil vs missing, zero vs empty, local vs remote, old vs new.
- Time scope: execution time, user timezone, boundary date, cache key, replay timestamp.
- Evidence scope: code definition, caller, serialized params, logs, persisted state, tests.

Do not conclude from a mixed trace until the relevant axes align.

### 3. Collect Evidence As A Chain

Prefer an evidence chain over isolated proof fragments. The chain should show how the fact moves through the system.

Good chain shape:

```text
entry input -> request struct -> command/service call -> adapter/serializer ->
runtime params -> branch condition -> side effect -> observation/test/log
```

For each link, capture one of:

- File/function/line or symbol name.
- Runtime parameter value.
- Log marker or trace ID.
- Persisted state or output.
- Test case name and assertion.

If one link is inferred rather than observed, label it as inference.

### 4. Use Comparison To Separate Signal From Noise

When the system is noisy, find a comparison:

- Broken sample vs healthy sample.
- Old engine vs new engine.
- First run vs rerun.
- Debug API vs business API.
- Main task vs derived task.
- Local unit path vs full replay path.

Compare marker sequences, not whole dumps. Define the decisive markers before scanning.

Example marker sequence:

```text
input accepted -> semantic descriptor selected -> business path executed ->
runtime branch selected -> side effect emitted -> downstream observer updated
```

If both samples share an early marker but diverge later, the divergence point becomes the next investigation target.

### 5. Prove Boundaries, Not Just Fixes

After implementation or analysis, classify the result:

- Proven: directly supported by evidence in this task.
- Guarded: covered by focused tests or assertions but not full-system proof.
- Plausible: consistent with evidence but missing at least one link.
- Unproven: not checked, or checked only through a non-equivalent path.
- Blocked: cannot be proven without unavailable data, environment, or user input.

Use careful language:

- Say "this focused test guards X" when the test does not compare full behavior.
- Say "this reproduces old-vs-new parity for these inputs" only when both implementations run on the same input space or a trustworthy recorded oracle is used.
- Say "this does not prove system-wide equivalence" when the matrix is incomplete.

### 6. Turn The Lesson Into A Default Rule

End by extracting reusable rules:

- Trigger: what future situation should activate the rule?
- Checklist: what must be checked before concluding?
- Evidence pattern: what proof shape works best?
- Failure mode: what previous mistake should be avoided?
- Automation candidate: can a script, test matrix, or query template remove repetition?

Do this even for one-off bugs; repeated judgment is where the leverage lives.

## Operating Patterns

### Debugging Pattern

Use this output shape:

```text
Conclusion:
Scope alignment:
Evidence chain:
Contradictions resolved:
What this proves:
What this does not prove:
Next reusable rule:
```

Default questions:

- Are there multiple tasks, projects, tenants, or requests in the same trace?
- Does the log print the full request or only an inner struct?
- Is the observed flag the upstream name, downstream name, or serialized alias?
- Could a derived path legitimately have different params from the main path?
- Are cleanup, dedup, or cache keys scoped tightly enough to prevent collisions?

### Migration / Parity Pattern

Do not accept "case passed" as "semantics migrated."

Build a matrix:

```text
domain type x input shape x execution path x output mode x context
```

Typical dimensions:

- Domain type: field type, node type, resource type, API variant.
- Input shape: missing, nil, typed nil, empty string, string "null", empty array, empty object, malformed, valid.
- Execution path: debug path, business path, replay path, remote call path.
- Output mode: raw, display, label, diagnostic, validity, side effect.
- Context: owner/source, timezone, permission, cache key, request time.

Proof levels:

- Focused guard: one behavior is protected.
- Matrix coverage: dimension combinations are represented.
- Old-vs-new parity: old and new systems agree on the same input or trusted oracle.
- Full regression: broad replay passes with known blockers resolved or explicitly accepted.

### Review Pattern

Lead with risks, not summaries.

For each risk, state:

```text
Claim:
Why it matters:
Evidence:
Missing proof:
Suggested verification:
```

If no issue is found, still name the residual risk and the tests or evidence reviewed.

### Implementation Pattern

Keep edits scoped to the proven decision point.

Before changing code:

- Identify the smallest branch, adapter, parser, projector, or cache key that owns the behavior.
- Check whether the old behavior is intentional, accidental, or compatibility-bound.
- Add tests at the narrowest level that catches the bug, plus broader tests when the changed behavior crosses modules.

After changing code:

- Run the targeted verification first.
- Run broader verification only when the blast radius warrants it.
- Explain the proof boundary of the tests that passed.

## Anti-Patterns

Avoid these:

- Treating a noisy aggregated trace as a single execution.
- Treating a focused unit test as system-wide proof.
- Treating a recipe, traversal, or route matrix as a semantic matrix.
- Comparing logs without aligning request/task/entity identity.
- Relying on debug paths to prove business-path behavior.
- Ignoring timezone, cache key, or replay-time context in time-sensitive logic.
- Hiding uncertainty to make the answer sound cleaner.
- Letting tool failures become reasoning failures.

## Final Response Standard

For substantial tasks, prefer this concise ending:

```text
What I can prove:
What remains unproven:
Verification run:
Reusable rule:
```

The final answer should be decisive where evidence is complete and explicit where it is not.
