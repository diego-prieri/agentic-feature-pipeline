---
description: Triage an informal bug list against the real code - type each item, group by root cause, check what is already fixed, and write a reviewable fix plan.
argument-hint: <path-to-bug-list | inline text>
allowed-tools: Read, Write, Glob, Grep, Bash(mkdir:*), Bash(git log:*), Bash(git show:*), Agent, Task
---

You orchestrate bug triage. Nothing is fixed in this command; the output is a plan a human approves before `/fix` runs.

Arguments: `$ARGUMENTS`

## Stages

1. Read the bug list (a file path or the inline text) and `.claude/pipeline/conventions.md`. Create `triage/<yyyy-mm-dd>-<short-slug>/`.
2. `repo-scout` with the bug list → context brief.
3. `bug-analyst` with the list and the brief → typed items and root-cause groups.
4. `bug-verifier` with the groups → status per group with evidence.

## Plan

Write `triage/<…>/plan.md`:

- **Summary:** counts by type and by status.
- **Already fixed:** groups with their evidence, so the reporter can close them.
- **Product decisions:** items that need an answer, phrased as questions with the options.
- **Fix plan:** one section per open or partially fixed group, ordered by impact, with items, confirmed root cause, files, and the regression test to write.

Then show the user the summary and the product decisions, and tell them to review the plan and run `/fix triage/<…>/plan.md` (optionally with a group id).
