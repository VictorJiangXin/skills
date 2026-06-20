---
name: architecture-before-implementation
description: Use before implementing non-trivial features, refactors, bug fixes, data model changes, API changes, performance optimizations, or cross-module work to validate the architecture, main abstractions, boundaries, data flow, lifecycle, extensibility, and failure modes before writing code. Trigger when a change could become a one-off patch, bypass the system model, or affect future maintainability.
---

# Architecture Before Implementation

## Purpose

Prevent local patches from becoming architecture debt. Before changing code, identify the intended system model and prove the implementation path fits it.

## Architecture Gate

1. Define the problem class.
   - Is this a missing capability, incorrect model, integration mismatch, performance issue, compatibility gap, or test/data problem?
   - Avoid treating a systemic issue as a single failing example.

2. Locate the main model.
   - Identify the core domain objects, ownership boundaries, contracts, lifecycle, and data flow.
   - Prefer extending the main model over adding side paths.

3. Check fit and alternatives.
   - State the preferred design and at least one rejected alternative.
   - Explain why the chosen path is simpler, safer, or more extensible.
   - Do not choose a design only because it is the smallest diff.

4. Validate boundaries.
   - Confirm allowed files/modules, dependency direction, public contracts, persistence effects, and compatibility expectations.
   - Do not modify adjacent systems unless the task explicitly includes them or the architecture requires it.

5. Define the implementation slice.
   - Make the first slice small, coherent, and reviewable.
   - Include tests or verification that prove the architectural behavior, not just the edited line.

## Design Standards

- Do not create special-case code unless evidence proves the problem is truly local.
- Do not bypass the central abstraction to make one scenario pass.
- Do not duplicate constants, protocols, loaders, validators, or data-shaping rules when a shared owner exists.
- Do not introduce hidden global state, unbounded caches, implicit ordering, or implicit retries without a lifecycle and limit.
- Do not change external behavior, performance profile, storage format, or API shape without naming the compatibility impact.
- Prefer demand-driven loading, explicit selectors, bounded batching, and clear ownership when dealing with expensive data access.

## Required Output Before Coding

For substantial changes, state:

```text
Architecture check:
- Problem class:
- Main abstraction / owner:
- Data flow:
- Boundaries:
- Rejected alternatives:
- Chosen approach:
- Verification:
```

If any line cannot be answered, continue investigation before implementation.
