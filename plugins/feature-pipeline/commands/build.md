---
description: Execute the steps of a 03-implementation-steps.md one at a time with a fresh subagent each, verify after every step, and open a draft PR.
argument-hint: <path-to-03-implementation-steps.md> [--dry-run]
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git:*), Bash(gh:*), Bash(npm:*), Bash(npx:*), Agent, Task
---

You execute an existing plan. You do not reinterpret the architecture; if the plan is wrong, stop and say so instead of improvising.

Arguments: `$ARGUMENTS`

## Setup

1. Read the plan and `.claude/pipeline/conventions.md`.
2. Require a clean working tree (`git status --porcelain` empty). Create a branch `feature/<slug>` from the base branch named in the conventions.
3. Order the steps by their dependency list.
4. With `--dry-run`, print the ordered steps and their files, then stop.

## Per step

1. Pass the step's prompt, unchanged, plus the conventions file path, to the `step-runner` subagent. One step per invocation.
2. When it returns, re-run the step's "Done when" commands yourself. Trust the result, not the report.
3. Green → commit with `step N: <title>` and continue.
   Red → send the failure output back to `step-runner` once. Still red → stop, leave the branch as it is, and report which step failed and why.

Keep a running log in `specs/<slug>/build-log.md`: step, status, files, commit.

## Finish

1. Run the full test suite and the typecheck from the conventions.
2. Push the branch and open a PR as the conventions say (draft by default). The body links the spec folder, lists the steps with their commits, and lists anything marked ASSUMED in 02 under "Needs confirmation".
3. Report the PR link and the final verification results.
