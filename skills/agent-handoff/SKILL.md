---
name: agent-handoff
description: Use when pausing, resuming, switching machines, delegating to another agent, handing off long-running development, asking another agent to continue, or summarizing current state after partial work. Produces a complete handoff packet with verified facts, workspace state, branch/commit, changed files, decisions, blockers, verification, risks, and exact next steps.
---

# Agent Handoff

## Purpose

Make continuation cheap and safe. A good handoff lets another agent resume from verified state without rereading the whole history or repeating failed work.

## Workflow

1. Capture current state.
   - Repository root, branch, commit, dirty status, active task, and relevant commands.
   - Include whether changes are committed, staged, unstaged, pushed, or only local.

2. Separate facts from interpretation.
   - Facts: tool outputs, file paths, test results, user-confirmed decisions.
   - Interpretation: hypotheses, suspected root causes, proposed next steps.
   - Mark anything uncertain.

3. Summarize work performed.
   - Changed files and why they changed.
   - Behavior changed and behavior intentionally not changed.
   - Important rejected approaches or failed attempts.

4. Record verification.
   - Commands run, results, artifacts, reports, and skipped checks.
   - Include failures that are still relevant.

5. Define next steps.
   - Give the next agent a small ordered list.
   - Include scope, constraints, commands, expected outputs, and escalation points.

## Handoff Packet Template

```text
Handoff:
- Objective:
- Workspace / branch / commit:
- Current status:
- Authoritative facts:
- Decisions confirmed by user:
- Changed files:
- Verification run:
- Known failures / blockers:
- Risks and constraints:
- Next steps:
- Do not do:
```

## Rules

- Do not hide uncertainty to make the handoff look cleaner.
- Do not omit failed attempts if they prevent repeated work.
- Do not ask the next agent to trust conversation history; point to files, commands, and evidence.
- Do not include unrelated project history.
- Keep the handoff concise enough to paste into a new agent prompt.
