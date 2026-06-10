# AGENTS.md

## Purpose

This file is the mandatory entrypoint for coding agents working in this repository.

The repository uses governance-first, slice-based development. Agents must work from approved requirements, feature specs, plans, reviews, ADRs, and traceability records.

## Mandatory startup

Before any task:

1. Read `governance/agent/current-workflow-state.md`.
2. Read `governance/agent/agent-profiles.md`.
3. Select exactly one profile for the task:
   - `REQUIREMENTS`
   - `PLANNING`
   - `IMPLEMENTATION`
   - `REVIEW`
   - `SECURITY`
4. Read the active short prompt from `governance/agent/short-prompts.md` when applicable.
5. Read only the additional files allowed by the selected profile.

Do not load the full governance pack by default.

## Context budget rules

- Use the smallest safe context.
- Do not read `governance/agent/prompt-templates.md` during routine implementation unless the task is to create or revise governance prompts/workflow.
- Do not read the full `governance/security/security-spec.md` unless the task touches a security trigger.
- Do not read the full `governance/requirements/master-requirements.md` during routine implementation.
- Implement from approved `specs/<feature>/spec.md` and `specs/<feature>/plan.md`, not directly from master requirements.
- Do not restate accepted history unless directly relevant.

## Source-of-truth precedence

Use sources in this order:

1. Approved ADRs
2. `governance/security/security-spec.md`, where relevant
3. Approved active feature spec
4. `governance/requirements/master-requirements.md`
5. Existing codebase conventions

Operational memory is not formal truth:

- `governance/agent/current-workflow-state.md` is lightweight operational memory only.
- Accepted `specs/*/review.md`, `governance/requirements/traceability.md`, and `governance/security/security-traceability.md` are authoritative for accepted status and coverage.
- If sources conflict, stop and report the conflict.

## Non-negotiable rules

- No silent assumptions.
- No extra scope.
- No unrelated refactors unless explicitly approved.
- Work one slice at a time.
- Tests are mandatory for implemented code changes.
- Traceability must be updated for implemented changes.
- Existing ADRs must be respected.
- Security controls must be applied where relevant.
- If security-critical uncertainty exists, fail closed.
- Design references are visual guidance only; they are never source of truth for authentication, tenant logic, security controls, or API contracts.

## Requirement discipline

No new feature, changed requirement, or scope expansion may be implemented unless it has first been:

1. registered in the requirements index,
2. classified,
3. linked to security relevance,
4. impact assessed,
5. mapped to a feature spec or architecture review.

## Security review triggers

Trigger explicit security review when a change touches:

- authentication
- SSO or MFA
- authorization or visibility
- tenant or organization separation
- file upload, document handling, import, or export
- offline or local storage
- APIs or integrations
- audit logging
- encryption, secrets, tokens, or credentials
- cross-tenant sharing
- signatures, document integrity, or evidence value
- AI, OCR, LLM, or MCP-backed features

## Architecture review triggers

Trigger architecture review before implementation when a change affects:

- tenant boundary model
- role or authorization model
- offline/sync behavior
- document/versioning model
- audit/event model
- export/report visibility
- API/integration boundaries
- data retention or archival
- core workflow states

## Feature slice workflow

A coding agent should usually implement from:

- `specs/<feature>/spec.md`
- `specs/<feature>/plan.md`

After implementation, update:

- `specs/<feature>/review.md`
- `governance/requirements/traceability.md`
- `governance/security/security-traceability.md`, if security controls apply
- `governance/agent/current-workflow-state.md`, if active/accepted/blocked/next slice changes

## Output expectations

Use compact output by default:

```md
Profile:
Slice:
Scope:
Changed:
Tests:
Traceability:
Blockers:
Next:
```

Long explanations should be reserved for requirement analysis, architecture review, formal review, or explicit user request.
