# Governance-First AI Agent Workflow Starter Pack

This starter pack is a reusable governance structure for AI-assisted software projects.

It is designed to keep coding agents aligned with:
- accepted requirements
- bounded feature slices
- ADRs
- security controls
- traceability
- current work state

## Core idea

Do not ask an agent to read everything and build everything.

Use:

```text
one selected profile
one active slice
minimal relevant context
strict traceability
compact output
```

## Main files

- `AGENTS.md` — mandatory entrypoint for agents
- `governance/agent/agent-profiles.md` — allowed agent profiles and read/write boundaries
- `governance/agent/current-workflow-state.md` — lightweight operational state
- `governance/agent/short-prompts.md` — compact routine prompts
- `governance/agent/prompt-templates.md` — longer governance prompts, not default runtime context
- `governance/agent/workflow.md` — staged lifecycle
- `governance/agent/constitution.md` — mandatory rules
- `governance/requirements/*` — requirements and traceability
- `governance/security/*` — baseline security controls and security traceability
- `governance/adr/*` — architectural decision records
- `specs/<feature>/*` — bounded feature specs, plans, and reviews

## Agent profiles

Each task must use exactly one profile:

- `REQUIREMENTS` — capture, normalize, classify, and slice requirements
- `PLANNING` — create specs, plans, architecture notes, and ADR drafts
- `IMPLEMENTATION` — implement one approved slice
- `REVIEW` — review, acceptance, and doc-only corrections
- `SECURITY` — security review and security traceability

Profiles prevent the agent from loading too much context or writing to the wrong files.

## Lifecycle

1. Stage 0: load governance and select profile
2. Stage 1: normalize requirements
3. Stage 2: architecture and ADR review
4. Stage 3: create bounded feature `spec.md` and `plan.md`
5. Stage 4: implement one approved slice
6. Stage 5: review, test, and update traceability
7. Stage 6: controlled change handling

## Lightweight session startup

For routine work, tell the agent:

```md
Follow AGENTS.md.
Select the correct profile from governance/agent/agent-profiles.md.
Use the matching short prompt from governance/agent/short-prompts.md.
Load only profile-allowed context.
```

For implementation, use:

```md
Run implement-slice for:
- <FEATURE ID + NAME>

Rules:
- one slice only
- no extra scope
- tests mandatory
- update review and traceability
- fail closed on security ambiguity
```

For review, use:

```md
Run review-slice for:
- <FEATURE ID + NAME>

Return findings by severity and accept / not accept recommendation.
```

## Context budget policy

Routine implementation should not read:
- the full master requirements
- all ADRs if only one is relevant
- the full security spec unless a security trigger applies
- prompt templates unless prompt/workflow docs are being changed

The active feature `spec.md` and `plan.md` are the implementation contract.

## Source of truth

Formal truth is:

1. accepted `specs/*/review.md`
2. `governance/requirements/traceability.md`
3. `governance/security/security-traceability.md`
4. approved ADRs

`current-workflow-state.md` is only operational memory. If it conflicts with formal records, formal records win.

## Recommended setup for a new project

1. Put the real business requirements in `governance/requirements/master-requirements.md`.
2. Review `governance/security/security-spec.md` and keep it as the baseline security constraint.
3. Use `requirements-normalize` to create requirement IDs and feature slices.
4. Use `plan-slice` to create the first `spec.md` and `plan.md`.
5. Use `implement-slice` only after the slice is approved.
6. Use `review-slice` and `accept-slice` before moving on.

## Practical rule

The agent should usually not implement directly from the master requirements file.

It should implement from an approved bounded feature slice while respecting ADRs, security controls, and traceability.
