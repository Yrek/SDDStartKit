# Governance-First AI Agent Workflow Starter Pack

This starter pack is a reusable placeholder pack for future software projects.

It provides:
- a governance-oriented folder structure
- agent rules and staged workflow
- requirements, security, ADR, and traceability templates
- spec/plan/review templates for bounded feature slices

## Project purpose

Use this repository to run AI-assisted delivery with explicit governance controls:
- plan and execute one bounded slice at a time
- keep requirement and security traceability current
- reduce session overhead with lightweight operational context
- preserve formal acceptance records as source of truth

## How it is used across stages

The lifecycle is defined in `governance/agent/workflow.md`:
1. Stage 0: governance/context loading
2. Stage 1: requirement normalization/decomposition
3. Stage 2: architecture/design
4. Stage 3: feature slicing and planning (`spec.md` + `plan.md`)
5. Stage 4: implementation + tests
6. Stage 5: validation, traceability, and review (`review.md`)
7. Stage 6/6A: controlled change handling and new-requirement intake

## Session startup (lightweight)

At session start, read:
1. `governance/agent/current-workflow-state.md` (operational memory)
2. `governance/agent/constitution.md`
3. `governance/agent/workflow.md`
4. `governance/agent/short-prompts.md` (routine modes)
5. `governance/agent/agent-design.md` (operating model)

Then read only files required for the current task/slice.

Authority rule:
- Formal source of truth is accepted `specs/*/review.md` plus requirements/security traceability.
- `current-workflow-state.md` is non-authoritative operational memory.
- If conflict exists, formal review/traceability records win.

## Recommended repo setup

1. Place your real requirements in:
   - `governance/requirements/master-requirements.md`

2. Place your real security specification in:
   - `governance/security/security-spec.md`

3. Add any ADRs to:
   - `governance/adr/`

4. Ask the coding agent to follow:
   - `AGENTS.md`
   - `governance/agent/constitution.md`
   - `governance/agent/workflow.md`
   - `governance/agent/short-prompts.md`
   - `governance/agent/agent-design.md`

## Quickstart command examples

Use these prompt snippets with your coding agent.
Replace `<FEATURE ID + NAME>` with your active slice.

1. Readiness check
```md
Run `readiness-check` for:
- <FEATURE ID + NAME>

Use `governance/agent/short-prompts.md` and return:
- readiness status
- blockers
- exact next action
```

2. Stage 4 implementation
```md
Run `stage4-implement` for:
- <FEATURE ID + NAME>

Rules:
- one slice only
- no extra scope
- tests mandatory
- fail closed on security ambiguity
```

3. Formal review
```md
Run `formal-review` for:
- <FEATURE ID + NAME>

Return:
- findings by severity
- required corrections
- accept / not-accept recommendation
```

4. Doc-only correction
```md
Run `doc-only-correction` for:
- <FEATURE ID + NAME>

Do not change production code or tests.
Only update review/traceability/governance docs as needed.
```

5. Acceptance
```md
Run `accept` for:
- <FEATURE ID + NAME>

Confirm:
- no unresolved HIGH/MEDIUM findings
- review + traceability accuracy
- deferred items and owner
- recommended next slice
```
