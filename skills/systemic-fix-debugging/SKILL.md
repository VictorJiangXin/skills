---
name: systemic-fix-debugging
description: Use for bugs, test failures, regressions, mismatches, flaky behavior, performance regressions, or repeated user corrections where a local patch may hide a broader issue. Requires reproducing, grouping symptoms, identifying the shared root cause, comparing against authoritative behavior, and implementing a systemic fix with regression coverage instead of one-case fixes.
---

# Systemic Fix Debugging

## Purpose

Fix the class of problem, not the latest symptom. A failing case is evidence; it is not automatically the scope of the fix.

## Workflow

1. Reproduce and preserve evidence.
   - Capture the exact command, input, output, error, timing, and environment.
   - Confirm whether the issue is current, stale, data-related, environment-related, or code-related.

2. Inventory symptoms.
   - List all known failing examples, slow paths, mismatches, or affected callers.
   - Include examples that now pass if they explain the pattern.

3. Bucket by likely cause.
   - Separate ordering differences, invalid/missing data, mock drift, dependency behavior, contract breaks, performance regressions, and true semantic differences.
   - Do not merge buckets just because symptoms look similar.

4. Trace the shared root cause.
   - Walk backward through data flow and control flow until the first wrong decision, missing selector, bad state, incorrect assumption, or boundary violation is found.
   - Compare with authoritative implementation or live behavior only after confirming that source is the right authority.

5. Design a systemic fix.
   - Fix the owner of the rule, not a downstream consumer unless the consumer owns the rule.
   - Prefer a general model change, shared helper, contract update, or loader/planner correction over per-case branches.
   - If only a local patch is correct, state why the issue cannot generalize.

6. Add regression coverage.
   - Cover the minimal reproduction and at least one adjacent case proving the class of issue.
   - Add negative or boundary tests when the bug came from over-broad logic.

## Stop Conditions

Pause and reassess when:

- Three attempted fixes fail or reveal different symptoms.
- The fix requires changing architecture, public contracts, data semantics, or hot-path behavior.
- The authoritative behavior is unclear or suspected to be wrong.
- The only apparent fix is a hardcoded exception.

## Output Shape

```text
Triage:
- Reproduction:
- Symptom buckets:
- Root cause:
- Why this is systemic or local:
- Fix owner:
- Regression coverage:
- Remaining risks:
```
