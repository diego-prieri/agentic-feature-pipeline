---
description: Turn a functional spec into a user flow, a technical definition and self-contained implementation steps, ready for /build.
argument-hint: <path-to-functional-spec> [path-to-prototype.html]
allowed-tools: Read, Write, Glob, Grep, Bash(mkdir:*), Agent, Task
---

You orchestrate the specification pipeline. You coordinate subagents and check their output; you do not write the deliverables yourself. Stages hand off through files on disk, so each subagent starts with a clean context and only what it needs.

Arguments: `$ARGUMENTS`

## Setup

1. Resolve the conventions file: `.claude/pipeline/conventions.md` in the repo. If it does not exist, stop and tell the user to copy it from `${CLAUDE_PLUGIN_ROOT}/templates/conventions.md` and fill it in.
2. Read the functional spec. Derive a short kebab-case feature slug from its title.
3. Create the output directory `specs/<slug>/`.

## Stages

Run them in order. After each one, read the file it wrote and check it against the gate before moving on. If a gate fails, send the subagent back once with the specific problem; if it fails again, stop and report.

| # | Subagent | Input | Output | Gate |
|---|---|---|---|---|
| 1 | `repo-scout` | spec, conventions | brief (returned) | every path exists |
| 2 | `flow-designer` | spec, prototype, brief | `01-user-flow.md` | has acceptance criteria in Given/When/Then |
| 3 | `tech-architect` | 01, brief, conventions | `02-technical-definition.md` | every acceptance criterion maps to a test |
| 4 | `impl-planner` | 02, conventions | `03-implementation-steps.md` | every step has a self-contained prompt and a "Done when" |

Save the brief from stage 1 as `specs/<slug>/00-context-brief.md` so later stages and humans can read it.

## Human checkpoint

After stage 2, if `01-user-flow.md` lists open questions, show them to the user and wait. Product decisions are not the pipeline's to make. Continue only with answers, recording them in 01 under "Decisions".

## Finish

Write `specs/<slug>/README.md` linking the four files, then tell the user:
- the output directory;
- the number of steps and anything marked ASSUMED in 02;
- the next command: `/build specs/<slug>/03-implementation-steps.md`.
