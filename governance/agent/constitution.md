# Agent Constitution

## Purpose

This constitution defines mandatory rules for AI agents analyzing, planning, implementing, or reviewing this repository.

## Profile discipline

Before work begins, the agent must select exactly one profile from `governance/agent/agent-profiles.md`.

Allowed profiles:
- `REQUIREMENTS`
- `PLANNING`
- `IMPLEMENTATION`
- `REVIEW`
- `SECURITY`

The selected profile controls which files may be read, which files may be written, and what is forbidden.

## Source-of-truth precedence

Use sources in this order:

1. Approved ADRs
2. `governance/security/security-spec.md`, where relevant
3. The approved feature spec for the current slice
4. `governance/requirements/master-requirements.md`
5. Existing codebase conventions

If sources conflict, stop and report the conflict rather than guessing.

## Operational authority rule

- `governance/agent/current-workflow-state.md` is lightweight operational memory only.
- Formal accepted records remain authoritative:
  - accepted `specs/*/review.md`
  - `governance/requirements/traceability.md`
  - `governance/security/security-traceability.md`
- If `current-workflow-state.md` conflicts with accepted review/traceability records, accepted records win.

## Hard rules

- No silent assumptions.
- No extra scope.
- No unrelated refactors unless explicitly approved.
- Tests are mandatory for implemented code changes.
- Traceability must be updated for implemented changes.
- Existing ADRs must be discovered and respected.
- Security controls must be applied where relevant, not only documented.
- Original traceability history must be preserved.
- Replaced or removed requirements must never be silently deleted.
- Work one slice at a time.
- Read only files needed for the selected profile and current task.
- Do not restate accepted history unless directly relevant.
- Doc-only governance or instruction fixes do not require full slice re-review unless explicitly required.
- Design references are visual guidance only and never source of truth for auth, tenant logic, security controls, or API contracts.

## Context budget rules

- Do not load all governance files by default.
- Use `short-prompts.md` for routine work.
- Use `prompt-templates.md` only for governance-heavy or prompt-authoring work.
- Use full `security-spec.md` only when security triggers apply or a security review is requested.
- Use active feature spec/plan as the implementation contract.

## Workflow-state maintenance

Keep `governance/agent/current-workflow-state.md` updated when meaningful milestones occur:
- active slice changes
- a slice is accepted
- a blocker is discovered or cleared
- the recommended next slice changes

## Requirement discipline

No feature, changed requirement, or scope expansion may be implemented unless it has first been:

1. registered in the requirements index,
2. classified,
3. linked to security relevance,
4. impact assessed,
5. mapped to a feature spec or architecture review.

## Security discipline

If a change touches any of the following, use `SECURITY` profile or trigger explicit security review:

- authentication
- SSO / MFA
- authorization / visibility
- tenant or organization separation
- file upload, document handling, import, or export
- offline/local storage
- APIs / integrations
- audit logging
- encryption, secrets, tokens, or credentials
- cross-tenant sharing
- signatures / document integrity / evidence value
- AI, OCR, LLM, or MCP-backed features

## Architecture review triggers

If a change affects any of the following, trigger architecture review before implementation:

- tenant boundary model
- role or authorization model
- offline/sync behavior
- document/versioning model
- audit/event model
- export/report visibility
- API or integration boundaries
- data retention / archival
- core workflow states

## Traceability discipline

Each implemented slice must map:
- requirement IDs
- related security controls
- code files
- test files
- status
