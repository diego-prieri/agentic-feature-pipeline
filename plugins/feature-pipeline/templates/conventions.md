# Project conventions

<!--
Copy this file to `.claude/pipeline/conventions.md` in your repo and fill it in.
Every subagent reads it before doing anything, so it is the single place where
your stack, rules and verification commands live. Keep it short and concrete:
agents follow examples better than principles.
-->

## Stack

- Language / framework: <!-- e.g. TypeScript, Next.js 14 (Pages Router), React 18 -->
- UI library: <!-- e.g. MUI v5 -->
- State: <!-- e.g. Context + useReducer in store/ -->
- Data access: <!-- e.g. axios client in network/, endpoints in api/modules/ -->

## Where things go

| Kind | Path | Reference example |
|---|---|---|
| Page | <!-- pages/ --> | <!-- pages/orders/index.tsx --> |
| Component | <!-- components/ --> | |
| Data hook / fetch wrapper | <!-- hooks/ --> | |
| Types | <!-- types/ --> | |
| Tests | <!-- next to the file, *.test.ts --> | |

## Rules

<!-- Things an agent would get wrong without being told. One line each. -->
- 
- 

## Verification commands

Agents run only these. Each must exit 0 for a step to count as done.

```
typecheck: npx tsc --noEmit
test:      npx jest --findRelatedTests {files}
lint:      npx eslint {files}
```

## Pull requests

- Base branch: main
- Open as draft: yes
- Title format: <!-- e.g. the branch name -->
