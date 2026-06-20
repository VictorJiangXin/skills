---
name: verification-closure
description: Use before declaring software work complete, committing, handing off, opening a PR, reporting a fix, or marking a task done to define and run verification matched to the change impact. Requires evidence from tests, builds, lint, diff inspection, integration checks, performance checks, or manual validation as appropriate, and requires clearly stating unverified risk.
---

# Verification Closure

## Purpose

Close work with evidence. A task is not complete because code was edited; it is complete when the verification is appropriate for the impact and the remaining risk is explicit.

## Workflow

1. Classify the change.
   - Documentation-only, local code, shared helper, cross-module behavior, public API, data migration, concurrency, performance, or production-facing operation.

2. Build the verification matrix.
   - Map each affected behavior to at least one verification action.
   - Prefer narrow tests first, then broader tests when the blast radius requires them.

3. Run verification.
   - Use the repository's established commands when available.
   - Inspect diffs as well as test results.
   - For long-running checks, record command, start time, status, and final result.

4. Interpret results.
   - Passing tests support the conclusion; they do not replace reasoning about uncovered risk.
   - Failed or skipped checks must be reported with cause and impact.

5. Close or escalate.
   - Mark complete only when required checks pass or the user explicitly accepts the unverified risk.
   - If verification exposes a new issue, return to diagnosis instead of hiding it in the final response.

## Minimum Verification Guidance

- Formatting-only: diff check or formatter confirmation.
- Local logic: focused unit tests plus diff inspection.
- Shared logic: focused tests plus affected package/module tests.
- Cross-module or public contract: integration or contract tests.
- Performance/resource change: benchmark, profiling, measurement, or explicit bounded reasoning.
- Remote or production-facing behavior: environment-specific validation or a documented reason it cannot be run.

## Completion Packet

Use this shape:

```text
Verification:
- Commands run:
- Results:
- Diff reviewed:
- Coverage of acceptance criteria:
- Not run / why:
- Residual risk:
```

Do not say "done", "fixed", "safe", or "passing" unless the evidence above supports that exact claim.
