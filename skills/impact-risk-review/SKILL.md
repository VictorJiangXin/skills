---
name: impact-risk-review
description: Use before or after code changes, reviews, refactors, optimizations, dependency changes, cache changes, batching changes, API/data-contract changes, or production-facing fixes to assess blast radius, correctness risk, performance/resource impact, concurrency, downstream traffic, compatibility, observability, and rollback safety.
---

# Impact Risk Review

## Purpose

Treat every meaningful change as an intervention in a running system. Passing tests do not prove the change is safe; they are one piece of evidence.

## Review Dimensions

1. Blast radius.
   - Which callers, modules, data types, environments, tasks, or background jobs can observe the change?
   - Is the change on a hot path, shared utility, initialization path, persistence path, or public API?

2. Correctness and contracts.
   - Does the change preserve input/output semantics, ordering, validity, null/empty behavior, error behavior, and compatibility?
   - Does it change an implicit contract that tests may not cover?

3. Performance and resources.
   - Does it add remote calls, repeated I/O, large allocations, unbounded caches, serialization, locks, retries, or N+1 behavior?
   - Are batching, cache scope, eviction, TTL, and memory ownership explicit?

4. Concurrency and lifecycle.
   - Does the change introduce shared mutable state, goroutines/tasks, cancellation needs, race potential, or cleanup obligations?
   - Does the data live at request, task, process, or persistent scope?

5. Operational behavior.
   - Are errors observable and actionable?
   - Are logs, metrics, and fallback behavior appropriate for the path?
   - Is rollback safe if data or external state has changed?

6. Verification adequacy.
   - Do the tests cover the real affected behavior, not only the edited helper?
   - Is performance or remote/integration verification needed?

## Risk Classification

Use:

```text
High: likely production breakage, data corruption, contract break, severe performance/resource risk, or unsafe external side effect.
Medium: real behavioral or operational risk with bounded scope, missing important verification, or plausible performance regression.
Low: maintainability, clarity, localized behavior, or minor verification gap.
```

## Output Shape

```text
Impact review:
- Blast radius:
- Correctness risks:
- Performance/resource risks:
- Compatibility risks:
- Observability/rollback:
- Required verification:
- Decision: proceed / revise / block
```

Block the change when a high risk is unmitigated or the blast radius cannot be determined.
