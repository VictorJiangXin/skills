---
name: fact-first-development
description: Use before non-trivial software development, debugging, refactoring, review, task resumption, or multi-agent work to build a verified fact ledger from the current repository state, authoritative docs, code, tests, and tool outputs before planning or changing code. Trigger when facts may be stale, the prompt contains historical context, the task references prior work, or the user asks to avoid assumptions.
---

# Fact First Development

## Purpose

Establish the facts before reasoning from them. Do not treat conversation history, task prompts, memory, or prior conclusions as current truth until verified against the workspace and authoritative sources.

## Workflow

1. Confirm the execution context.
   - Run or inspect enough to know current directory, repository root, branch, dirty state, relevant config, and active task entry points.
   - Do not hardcode paths when the current workspace can reveal them.

2. Identify authority sources.
   - Prefer repository instructions, module entry points, task specs, design decisions, tests, generated contracts, and current code over memory.
   - Record which source is authoritative for scope, behavior, constraints, and completion criteria.

3. Build a fact ledger.
   - Current state: what branch, files, task, tests, failures, and changes exist now.
   - Requirements: what the user explicitly asked for and what is out of scope.
   - Constraints: boundaries, forbidden changes, compatibility rules, performance limits, and verification requirements.
   - Unknowns: what is not yet proven and what evidence would resolve it.

4. Resolve conflicts before implementation.
   - If docs, code, tests, and user input disagree, do not silently choose the convenient source.
   - Prefer verified current behavior for diagnosis, but do not rewrite stable requirements just because code differs.
   - Ask for confirmation only when the conflict changes scope, architecture, data semantics, or risk.

5. Proceed only when the ledger is sufficient.
   - For simple local commands, keep the ledger implicit.
   - For non-trivial implementation, summarize the key facts before planning or editing.

## Hard Rules

- Do not use historical context as a task fact without re-checking.
- Do not start implementation from an assumed branch, path, task phase, or failing case.
- Do not promote planned, draft, or speculative content to current fact.
- Do not ignore dirty worktrees or unrelated user changes.
- Do not mark a blocker resolved until the source of truth says it is resolved.

## Output Shape

For non-trivial tasks, provide:

```text
Fact ledger:
- Workspace:
- Authoritative sources:
- Current task / scope:
- Confirmed constraints:
- Unknowns / conflicts:
- Next action:
```

Keep this concise; the goal is to prevent wrong work, not to create a report.
