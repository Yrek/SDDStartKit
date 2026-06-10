# Agent Design

## Purpose

This repository uses a governance-first agent workflow so AI sessions can continue safely without reconstructing project context from chat history.

The design goals are:
- stable source of truth
- low token usage
- bounded feature implementation
- explicit read/write permissions per task type
- traceability from requirement to code and tests
- safe handoff between coding agents

## Shared source of truth

All profiles share the same formal records.

Source-of-truth precedence:

1. approved ADRs
2. `governance/security/security-spec.md`, where relevant
3. approved active feature spec
4. `governance/requirements/master-requirements.md`
5. existing codebase conventions

Operational state:
- `current-workflow-state.md` is lightweight memory only
- accepted `review.md` plus requirements/security traceability are authoritative
- conflicts must stop work until reported or resolved

## Agent profiles

Profiles are defined in `governance/agent/agent-profiles.md`.

Supported profiles:
- `REQUIREMENTS`
- `PLANNING`
- `IMPLEMENTATION`
- `REVIEW`
- `SECURITY`

A task must select exactly one profile before work begins.

Profiles define:
- what to read
- what to write
- what is forbidden
- exit criteria

This avoids multiple agents creating different truths.

## Startup model

Default startup:

1. Read `AGENTS.md`.
2. Read `governance/agent/current-workflow-state.md`.
3. Read `governance/agent/agent-profiles.md`.
4. Select one profile.
5. Read the relevant short prompt if this is routine work.
6. Load only profile-allowed task context.

`prompt-templates.md` is not routine startup context. It is only for governance-heavy or prompt-authoring tasks.

## Operating modes

Routine command names are maintained in `short-prompts.md`:

- `readiness-check`
- `requirements-normalize`
- `plan-slice`
- `implement-slice`
- `finish-slice`
- `review-slice`
- `security-review`
- `doc-only-correction`
- `accept-slice`
- `register-change`

Each command maps to one profile.

## Context minimization

The agent should load only what is needed for the task.

Examples:
- implementation reads active spec/plan and relevant code, not full master requirements
- security review reads full security spec, but only when security triggers apply
- requirements work reads master requirements, but does not read production code unless impact assessment requires it
- review reads diff, active spec/plan/review, tests, traceability, and relevant ADRs

## Slice discipline

Implementation must happen one slice at a time.

A feature slice is implementation-ready only when:
- `spec.md` exists
- `plan.md` exists
- included and excluded scope are explicit
- requirement IDs are linked
- relevant security controls are identified
- tests are planned
- blockers are explicit or absent

## Review and acceptance

A slice is not accepted until:
- implementation matches the approved spec
- tests cover acceptance criteria
- traceability is accurate
- security traceability is accurate where relevant
- deferred work is explicit
- accepted state is recorded in `review.md`

## Workflow-state maintenance

Update `current-workflow-state.md` only when operational state changes:
- active slice changes
- slice is accepted
- blocker is discovered or cleared
- next recommended slice changes

Do not use workflow state as a substitute for formal traceability or review records.

## Tool handoff

When switching tools or coding agents, provide only:
- selected profile
- active slice
- exact next action
- blockers
- files likely relevant

The receiving agent must then follow `AGENTS.md` and load context according to profile rules.
