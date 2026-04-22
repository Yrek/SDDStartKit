# AGENTS.md

## Purpose
This file is the mandatory entrypoint for coding agents working in this repository.

It is intentionally short.
The detailed governance rules live under `governance/`, but those files are not optional.
Read them before doing analysis, design, implementation, or review.

## Read first
Before doing any work, read these files in order for startup context:

1. `governance/agent/current-workflow-state.md` (operational context only)
2. `governance/agent/constitution.md`
3. `governance/agent/workflow.md`
4. `governance/agent/short-prompts.md` (for routine slice work where applicable)
5. `governance/agent/agent-design.md`
6. `governance/agent/prompt-templates.md`

Then read only the additional files needed for the current task/stage (security spec, requirements files, traceability files, ADRs, and feature slice files) before analysis, design, implementation, or review.

## Authority and scope rules
- `governance/agent/current-workflow-state.md` is lightweight operational memory only.
- Formal accepted records remain the source of truth: accepted `specs/*/review.md`, `governance/requirements/traceability.md`, and `governance/security/security-traceability.md`.
- If any conflict exists, accepted review/traceability files win.
- Work one slice at a time.
- Read only files needed for the current task.
- Do not restate accepted history unless directly relevant to the current task.
- Doc-only governance/instruction fixes do not require a full slice re-review unless explicitly required.
- Design references are visual guidance only; they are never the source of truth for auth, tenant logic, security controls, or API contracts.

## How to work in this repo
Work in stages, not in one large pass.

Follow this sequence:
1. governance/context loading
2. requirement normalization or refinement
3. architecture/design
4. feature slicing and planning
5. implementation
6. validation and traceability
7. change handling

Do not skip directly from large requirements to code unless there is already an approved feature spec and plan.

## Non-negotiable rules
- No silent assumptions.
- No extra scope.
- No unrelated refactors unless explicitly approved.
- Tests are mandatory for implemented changes.
- Traceability must be updated.
- Existing ADRs must be respected.
- Security controls must be applied where relevant.
- If sources conflict, stop and report the conflict.
- If security-critical uncertainty exists, fail closed.

## Requirement discipline
No new feature, changed requirement, or scope expansion may be implemented unless it has first been:
1. registered in the requirements index,
2. classified,
3. linked to security relevance,
4. impact assessed,
5. mapped to a feature spec or architecture review.

## Security discipline
Trigger explicit security review when a change touches:
- authentication
- SSO or MFA
- authorization or visibility
- tenant separation
- file upload or attachment handling
- offline/local storage
- APIs or integrations
- audit logging
- encryption, secrets, or token handling
- cross-tenant sharing
- signatures, document integrity, or evidence value

## Architecture review triggers
Trigger architecture review before implementation when a change affects:
- tenant boundary model
- role or authorization model
- offline/sync behavior
- document/versioning model
- audit/event model
- export/report visibility
- API/integration boundaries
- data retention/archival
- core workflow states

## How to use the specs folder
Do not usually implement directly from the master requirements file.

Implement from an approved bounded feature slice in:
- `specs/<feature>/spec.md`
- `specs/<feature>/plan.md`

After implementation, update:
- `specs/<feature>/review.md`
- `governance/requirements/traceability.md`
- `governance/security/security-traceability.md`

## Default execution pattern
For ambiguous or large tasks:
1. plan first
2. restate scope
3. list files to change
4. list tests to add
5. list security controls to respect
6. then implement only the approved slice

## Output expectations
When completing work, report:
- implemented requirement IDs
- implemented security controls
- files changed
- tests added
- remaining gaps, assumptions, or risks

## Workflow-state maintenance
Keep `governance/agent/current-workflow-state.md` updated when meaningful milestones occur:
- a slice is accepted
- the active slice changes
- a blocker is discovered or cleared
- the recommended next slice changes
