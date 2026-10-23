# agentic-feature-pipeline

Spec-to-PR and bug-to-fix pipelines for [Claude Code](https://claude.com/claude-code), built from single-purpose subagents that hand off through files on disk.

```
/spec   functional spec ─▶ repo-scout ─▶ flow-designer ─▶ tech-architect ─▶ impl-planner
                              brief        01-user-flow    02-technical      03-steps (self-contained prompts)
                                                ▲ human answers open questions
/build  03-steps ─▶ step-runner × N (fresh context per step, verified + committed each) ─▶ draft PR

/triage bug list ─▶ repo-scout ─▶ bug-analyst ─▶ bug-verifier ─▶ plan.md (human review)
/fix    plan.md ─▶ fix-runner × N (failing test first, minimal fix) ─▶ diff + commit messages
```

## Why it is built this way

One long agent session that specs, designs and codes a feature drifts: early decisions get lost, context fills with file dumps, and mistakes compound. This pipeline is designed against that.

- **One job per subagent.** Each stage has a narrow role and a narrow tool set. The scout cannot write files; the architect cannot run commands; only the runners touch code.
- **Files are the interface.** Stages read and write `00`–`03` documents instead of sharing a conversation. Each subagent starts with a clean context and only the inputs it needs, and every intermediate decision is reviewable in a PR.
- **Self-contained step prompts.** The planner writes each implementation step so a fresh agent can execute it with no other context: files, types, a file to imitate, tests, and the commands that define "done".
- **Gates, not trust.** The orchestrator re-runs each step's verification commands itself before committing, and stops at the first step that stays red instead of piling changes on top of a failure.
- **Humans decide product questions.** Ambiguities surface as open questions and product decisions; the pipeline waits instead of guessing.
- **Test-first fixes.** A fix starts with a regression test that must fail for the reported reason. A bug that does not reproduce is reported, not "fixed".
- **Triage against reality.** Before fixing, the verifier checks code and git history, because a bug list is usually partly stale.
- **One conventions file.** Stack, file layout, rules and verification commands live in `.claude/pipeline/conventions.md`. Every agent reads it, so the pipeline adapts to a repo without editing the agents.

## Install

```bash
claude plugin marketplace add diego-prieri/agentic-feature-pipeline
claude plugin install feature-pipeline@agentic-feature-pipeline
```

Then, in your repo:

```bash
mkdir -p .claude/pipeline
cp ~/.claude/plugins/marketplaces/agentic-feature-pipeline/plugins/feature-pipeline/templates/conventions.md .claude/pipeline/
```

Fill in the conventions file. The verification commands matter most: they are what "done" means for every step.

## Use

```text
/spec docs/ticket-assignment.md
/build specs/ticket-assignment/03-implementation-steps.md --dry-run
/build specs/ticket-assignment/03-implementation-steps.md

/triage qa/bugs-thursday.md
/fix triage/2026-10-23-assignee/plan.md G2
```

`examples/ticket-tracker/` has a functional spec and an informal bug list to try it on.

## Layout

```
plugins/feature-pipeline/
  commands/   spec · build · triage · fix          (orchestrators)
  agents/     repo-scout · flow-designer · tech-architect · impl-planner · step-runner
              bug-analyst · bug-verifier · fix-runner
  templates/  conventions.md
```

## Adapting it

- **Another stack:** only the conventions file changes. The agents are stack-agnostic.
- **A stricter gate:** add commands (lint, e2e, coverage) under "Verification commands"; the runners and the orchestrator run whatever is listed.
- **More stages:** add an agent with one job and its output file, then a row in the orchestrator's stage table with its gate.

## License

MIT
