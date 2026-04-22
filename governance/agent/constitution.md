# Agent Constitution

## Purpose
This constitution defines the mandatory rules the coding agent must follow when analyzing, designing, implementing, or validating the system.

## Source-of-truth precedence
The agent must use sources in this order:

1. Approved ADRs
2. `governance/security/security-spec.md`
3. The approved feature spec for the current slice
4. `governance/requirements/master-requirements.md`
5. Existing codebase conventions

If sources conflict, the agent must stop and report the conflict rather than guessing.

## Operational authority rule
- `governance/agent/current-workflow-state.md` is lightweight operational memory only.
- Formal accepted records remain authoritative:
  - accepted `specs/*/review.md`
  - `governance/requirements/traceability.md`
  - `governance/security/security-traceability.md`
- If `current-workflow-state.md` conflicts with accepted review/traceability records, the accepted review/traceability records win.

## Hard rules
- No silent assumptions.
- No extra scope.
- No unrelated refactors unless explicitly approved.
- Tests are mandatory for implemented changes.
- Traceability must be updated.
- Existing ADRs must always be discovered and respected.
- Security controls must be applied where relevant, not only documented.
- Original traceability history must be preserved.
- Replaced or removed requirements must never be silently deleted.
- Work one slice at a time.
- Read only files needed for the current task/stage after startup context is loaded.
- Do not restate accepted history unless directly relevant.
- Doc-only governance or instruction fixes do not require a full slice re-review unless explicitly required.
- Design references are visual guidance only and never source-of-truth for auth, tenant logic, security controls, or API contracts.

## Workflow-state maintenance
Keep `governance/agent/current-workflow-state.md` updated when meaningful milestones occur:
- a slice is accepted
- the active slice changes
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
If a change touches any of the following, it must trigger explicit security review:
- authentication
- SSO / MFA
- authorization / visibility
- tenant separation
- file upload
- offline storage
- APIs / integrations
- audit logging
- encryption
- cross-tenant sharing
- signatures / document integrity

## Architecture review triggers
If a change affects any of the following, it must trigger architecture review before implementation:
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
