---
description: Execute a triage plan group by group - failing regression test first, minimal fix, verification - and leave a diff with suggested commit messages.
argument-hint: <path-to-plan.md> [group-id]
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git:*), Bash(npm:*), Bash(npx:*), Agent, Task
---

You execute an approved triage plan. Only groups in its "Fix plan" section are in scope; product decisions without an answer are skipped.

Arguments: `$ARGUMENTS`

## Per group

1. If a group id was given, run only that group. Otherwise run them in the plan's order.
2. Pass the group section and the conventions file path to `fix-runner`. One group per invocation.
3. Check the result:
   - **fixed** → re-run its verification commands yourself; if green, record it.
   - **not reproduced** → record it with the evidence; it goes back to the reporter.
   - **blocked** or red verification → record the reason and continue with the next group.

Do not commit. Leave the changes in the working tree, so a human reviews the diff group by group.

## Finish

Append a "Results" section to the plan: `group | status | files | suggested commit message`. Then run the full test suite and report it, together with what is left for a human (not reproduced, blocked, pending decisions).
