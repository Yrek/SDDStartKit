# Prompt Templates

This file contains longer prompts for governance-heavy work.

For routine coding, prefer `short-prompts.md`.

Do not load this file by default during implementation. Load it only when creating, repairing, or reviewing governance artifacts, requirements, architecture, specs, or prompt/workflow documentation.

## Universal startup

```md
Follow AGENTS.md.

Select exactly one profile from governance/agent/agent-profiles.md:
- REQUIREMENTS
- PLANNING
- IMPLEMENTATION
- REVIEW
- SECURITY

Load only the context allowed by that profile.

Apply source-of-truth precedence:
1. approved ADRs
2. security specification where relevant
3. approved active feature spec
4. master requirements
5. existing code conventions

Rules:
- one slice at a time
- no silent assumptions
- no extra scope
- no unrelated refactors
- tests mandatory for code changes
- traceability mandatory for implemented changes
- fail closed on security ambiguity
- current-workflow-state is operational memory only
- accepted review.md plus requirements/security traceability are authoritative

Use compact output unless a detailed artifact is requested.
```

## Stage 0/1: governance and requirement normalization

```md
Profile: REQUIREMENTS

Execute governance and requirement normalization only.
Do not write code.
Do not create implementation plans unless explicitly requested.

Read:
- AGENTS.md
- governance/agent/current-workflow-state.md
- governance/agent/agent-profiles.md
- governance/requirements/master-requirements.md
- governance/requirements/requirements-index.md
- governance/requirements/open-questions.md
- governance/requirements/feature-map.md
- governance/requirements/change-log.md
- governance/requirements/traceability.md
- relevant ADRs only
- security spec only enough to classify security relevance

Tasks:
1. Verify that requirements are captured with stable IDs.
2. Split broad requirements into atomic requirements.
3. Mark type, priority, source, dependencies, and security relevance.
4. Identify conflicts, gaps, duplicates, and open questions.
5. Update requirements-index, open-questions, feature-map, change-log, and traceability scaffold as needed.
6. Recommend bounded feature slices.

Return:
- files changed
- requirement IDs added/changed
- open questions
- recommended next slices
- blockers
```

## Stage 2: architecture review

```md
Profile: PLANNING

Perform architecture review only.
Do not write production code.

Read:
- normalized requirements
- feature map
- relevant ADRs
- relevant security specification sections
- relevant existing code only if needed to avoid unsafe design assumptions

Assess:
- domain boundaries
- tenant/organization isolation
- roles and authorization
- audit/event model
- document/file model
- import/export/integration boundaries
- background jobs and async behavior
- data retention and archival
- failure modes
- security review triggers

Write:
- architecture notes in the relevant plan/spec if requested
- draft ADRs only for decisions that need durable approval

Return:
- architecture summary
- ADRs needed
- risks/blockers
- recommended implementation order
```

## Stage 3: create or refine feature slice

```md
Profile: PLANNING

Create or refine a bounded feature slice for:
- <FEATURE ID + NAME>

Do not write production code.

Read:
- relevant requirements from requirements-index.md
- feature-map.md
- open-questions.md
- relevant ADRs
- relevant security sections if triggered
- existing specs/<feature>/spec.md and plan.md if present

Write:
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- traceability scaffold updates if required

spec.md must include:
- purpose
- included scope
- excluded scope
- requirement IDs
- actors/roles
- data and workflow impact
- API/UI behavior where relevant
- validation rules
- security controls
- acceptance criteria
- required tests
- open questions

plan.md must include:
- implementation approach
- files/components likely affected
- migrations/data changes
- tests
- security checks
- rollout/rollback notes
- dependencies and blockers

Return:
- files changed
- scope summary
- dependencies
- blockers
- readiness for implementation
```

## Stage 3 correction

```md
Profile: PLANNING

Correct existing feature specs/plans after review comments.
Do not write production code.
Do not create new slices unless explicitly required.

Read:
- active spec.md and plan.md
- latest review findings
- relevant requirements and traceability
- relevant ADRs/security sections

Fix:
- false requirement ownership
- hidden overlap between slices
- missing excluded scope
- unclear dependencies
- missing security controls
- vague tests
- acceptance criteria that cannot be verified

Return:
- files changed
- mappings changed
- blockers remaining
- whether the slice is ready for implementation
```

## Stage 4: implement one approved slice

```md
Profile: IMPLEMENTATION

Implement only:
- <FEATURE ID + NAME>

Before coding, confirm:
- active spec.md exists
- active plan.md exists
- scope is bounded
- blockers are absent
- security triggers are understood

Read:
- active spec.md and plan.md
- relevant ADRs
- directly relevant code/tests
- relevant security sections only if triggered

Do:
1. Restate exact scope briefly.
2. Implement only approved scope.
3. Add/update tests.
4. Update review.md.
5. Update requirements traceability.
6. Update security traceability if security controls apply.
7. Update workflow state only for meaningful milestone/blocker changes.

Stop if:
- scope is unclear
- implementation requires another slice
- a security decision is ambiguous
- an ADR or requirement conflicts with the plan

Return compact output.
```

## Stage 5: formal review

```md
Profile: REVIEW

Review the current implementation for:
- <FEATURE ID + NAME>

Do not implement new feature scope.

Read:
- active spec.md
- active plan.md
- active review.md
- changed code and tests
- requirements traceability
- security traceability
- relevant ADRs
- relevant security sections if triggered

Check:
- implemented behavior matches spec
- no unauthorized scope expansion
- tests cover acceptance criteria
- security controls are implemented, not only documented
- traceability claims are true
- partial work is marked partial
- deferred work has owner/later slice/open question

Return:
- findings by severity
- required corrections
- accept / not accept recommendation
- traceability corrections needed
```

## Security review

```md
Profile: SECURITY

Perform security review for:
- <FEATURE ID + NAME>

Read:
- governance/security/security-spec.md
- governance/security/security-review-checklist.md
- governance/security/security-traceability.md
- active spec/plan/review
- relevant ADRs
- relevant code/tests

Assess:
- authentication and authorization
- tenant/organization isolation
- file upload, document, import, export handling
- API/integration trust boundaries
- audit logging
- secrets/tokens/encryption
- data integrity and retention
- fail-closed behavior
- tests for abuse cases

Return:
- applicable security controls
- gaps
- required fixes
- tests to add
- accept / block recommendation
```

## Add or change requirement

```md
Profile: REQUIREMENTS

A new or changed requirement has been introduced.
Do not write code.

Classify the request as:
- new requirement
- changed requirement
- replacement
- removal/deprecation

Update:
- requirements-index.md
- open-questions.md if needed
- feature-map.md
- change-log.md
- traceability.md scaffold
- security-traceability.md if security relevance changes

Assess impact on:
- existing requirements
- feature specs
- ADRs
- tests
- security controls
- accepted slices

Return:
- requirement IDs affected
- impact assessment
- recommendation: approve / defer / reject / needs clarification
```

## Accept slice

```md
Profile: REVIEW

Accept the completed slice:
- <FEATURE ID + NAME>

Verify:
- no unresolved HIGH/MEDIUM findings
- review.md reflects final state
- requirements traceability is accurate
- security traceability is accurate where relevant
- deferred work is explicit
- next slice recommendation is clear

Do not write production code.
Update current-workflow-state.md only if active/accepted/next slice changes.

Return:
- acceptance decision
- deferred items
- next slice
```

## Onboard or switch coding agent

```md
We are continuing a governance-driven repository.

Read:
- AGENTS.md
- governance/agent/current-workflow-state.md
- governance/agent/agent-profiles.md
- governance/agent/short-prompts.md

Then select exactly one profile for the requested task.
Read only the additional files allowed by that profile.

Before making changes, report:
- selected profile
- active slice
- exact next action
- blockers if any

Do not implement outside the active approved slice.
```
