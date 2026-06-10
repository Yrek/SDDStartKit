# Agent Profiles

This file defines the allowed operating profiles for AI agents in this repository.

The purpose is to reduce context load, prevent scope drift, and keep one shared source of truth.

## Core rule

Before doing work, select exactly one profile:

- `REQUIREMENTS`
- `PLANNING`
- `IMPLEMENTATION`
- `REVIEW`
- `SECURITY`

Do not switch profile during the same task unless the task is stopped and the reason is recorded in `governance/agent/current-workflow-state.md` or in the response.

All profiles must follow `AGENTS.md`, `constitution.md`, and the source-of-truth precedence rules.

## Context budget policy

Load the smallest context that can safely answer the task.

Default startup for all profiles:

1. `AGENTS.md`
2. `governance/agent/current-workflow-state.md`
3. `governance/agent/agent-profiles.md`
4. the active short prompt from `governance/agent/short-prompts.md`, when applicable

Do not read `prompt-templates.md` during routine implementation unless explicitly asked to create or revise prompts/governance workflow.

Do not read the full `security-spec.md` unless the active work touches a security trigger listed in `AGENTS.md`, `constitution.md`, or the active feature spec.

Do not read the full `master-requirements.md` during routine implementation. Use the active feature spec and plan instead.

## Output budget policy

Default agent output should be compact:

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

Avoid restating accepted history unless it directly affects the current decision.

## Profile: REQUIREMENTS

Use for requirement intake, decomposition, clarification, and feature slicing.

Read:
- `governance/requirements/master-requirements.md`
- `governance/requirements/requirements-index.md`
- `governance/requirements/open-questions.md`
- `governance/requirements/feature-map.md`
- `governance/requirements/change-log.md`
- relevant ADRs only
- security spec or checklist only when classifying security relevance

Write:
- `governance/requirements/requirements-index.md`
- `governance/requirements/open-questions.md`
- `governance/requirements/feature-map.md`
- `governance/requirements/change-log.md`
- `governance/requirements/traceability.md` scaffold updates

Forbidden:
- production code changes
- implementation plans that assume unresolved requirements
- silent requirement expansion

Exit criteria:
- requirements have stable IDs
- open questions are explicit
- feature slices are bounded
- security relevance is marked

## Profile: PLANNING

Use for architecture review, ADR proposals, and feature `spec.md`/`plan.md` creation.

Read:
- active requirements from `requirements-index.md`
- `feature-map.md`
- active or target `specs/<feature>/spec.md`
- relevant ADRs
- relevant sections of `security-spec.md` when security triggers apply
- relevant code only when needed to plan safely

Write:
- `specs/<feature>/spec.md`
- `specs/<feature>/plan.md`
- draft ADRs in `governance/adr/`
- traceability scaffolds when needed

Forbidden:
- production code implementation
- test implementation
- changing accepted requirements without REQUIREMENTS profile work first

Exit criteria:
- included scope is explicit
- excluded scope is explicit
- dependencies and blockers are explicit
- tests and security controls are planned

## Profile: IMPLEMENTATION

Use for coding one approved feature slice.

Read:
- `specs/<active-feature>/spec.md`
- `specs/<active-feature>/plan.md`
- directly relevant ADRs
- directly relevant code and tests
- traceability files only when updating coverage
- relevant security sections only when security triggers apply

Write:
- production code for the active slice only
- tests for the active slice only
- `specs/<active-feature>/review.md`
- `governance/requirements/traceability.md`
- `governance/security/security-traceability.md`, if relevant
- `governance/agent/current-workflow-state.md`, if milestone/blocker changes

Forbidden:
- implementing outside the active slice
- unrelated refactors
- changing master requirements
- changing global governance rules unless explicitly requested
- reading broad historical context without need

Exit criteria:
- tests pass or failures are reported
- review is updated
- traceability is updated
- remaining gaps are explicit

## Profile: REVIEW

Use for formal review, acceptance checks, doc-only corrections, and detecting drift.

Read:
- active `spec.md`, `plan.md`, and `review.md`
- diff or changed files
- related tests
- related traceability files
- relevant ADRs
- relevant security sections when security triggers apply

Write:
- `specs/<feature>/review.md`
- review findings
- traceability corrections
- doc-only corrections when explicitly requested

Forbidden:
- broad refactors
- new feature implementation
- silently accepting partial coverage

Exit criteria:
- findings are classified by severity
- acceptance recommendation is clear
- deferred work is explicit and linked to later slice or open question

## Profile: SECURITY

Use when work touches security-sensitive areas or when a security review is requested.

Triggers:
- authentication, SSO, MFA
- authorization or visibility
- tenant or organization isolation
- file upload, document handling, export, or import
- offline/local storage
- APIs or external integrations
- audit logging
- encryption, secrets, tokens
- cross-tenant sharing
- signatures, document integrity, evidence value
- LLM/AI/OCR/MCP features

Read:
- `governance/security/security-spec.md`
- `governance/security/security-review-checklist.md`
- `governance/security/security-traceability.md`
- active feature spec/plan/review
- relevant ADRs
- relevant code and tests

Write:
- `governance/security/security-traceability.md`
- security findings in `review.md`
- security test recommendations
- security ADR proposals when needed

Forbidden:
- self-approving security exceptions
- choosing permissive behavior when requirements are ambiguous
- ignoring existing security-spec MUST rules

Exit criteria:
- applicable controls are identified
- gaps are explicit
- tests or mitigations are specified
- unresolved security uncertainty blocks implementation or acceptance
