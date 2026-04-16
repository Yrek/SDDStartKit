# AGENTS.md

## Purpose
This file is the mandatory entrypoint for coding agents working in this repository.

It is intentionally short.
The detailed governance rules live under `governance/`, but those files are not optional.
Read them before doing analysis, design, implementation, or review.

## Read first
Before doing any work, read these files in order:

1. `governance/agent/constitution.md`
2. `governance/agent/workflow.md`
3. `governance/agent/prompt-templates.md`
4. `governance/security/security-spec.md`
5. `governance/requirements/master-requirements.md`
6. `governance/requirements/requirements-index.md`
7. `governance/requirements/open-questions.md`
8. `governance/requirements/feature-map.md`
9. `governance/requirements/traceability.md`
10. `governance/security/security-traceability.md`
11. all ADRs in `governance/adr/`

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
