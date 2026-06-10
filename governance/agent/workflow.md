# Agent Workflow

## Overview

This workflow is stage-based and profile-based.

Every task must select exactly one profile from `agent-profiles.md` before work begins.

Stages:

0. Governance/context loading
1. Requirement normalization and decomposition
2. Architecture and solution design
3. Feature slicing and implementation planning
4. Implementation and tests
5. Validation, traceability, and review
6. Change handling and iteration
6A. New requirement/change intake

## Stage 0 - Governance and context loading

Profile: selected based on task.

Required actions:
1. Read `AGENTS.md`.
2. Read `current-workflow-state.md`.
3. Read `agent-profiles.md`.
4. Select exactly one profile.
5. Read the relevant short prompt if routine work.
6. Load only task-relevant files allowed by the selected profile.

Authority rules:
- `current-workflow-state.md` is operational memory only.
- Accepted `review.md` and traceability files are authoritative.
- Approved ADRs and security rules take precedence where applicable.
- If sources conflict, stop and report.

Output:
- selected profile
- active slice or requirement area
- exact next action
- blockers if any

## Stage 1 - Requirement normalization and decomposition

Profile: `REQUIREMENTS`

Goal:
Turn broad business requirements into stable, traceable, implementation-ready requirement records.

Required actions:
- assign stable requirement IDs
- classify type, priority, owner/source, status
- identify dependencies
- identify security relevance
- update open questions
- update feature map
- update traceability scaffold

Outputs:
- updated requirements index
- updated open questions
- updated feature map
- updated change log if applicable
- recommended bounded slices

No production code may be written in this stage.

## Stage 2 - Architecture and solution design

Profile: `PLANNING`

Required inputs:
- normalized requirements
- feature map
- relevant ADRs
- relevant security specification sections
- relevant code only if needed

Required actions:
- identify domain boundaries
- identify tenant/organization boundaries
- identify role/authorization implications
- identify audit/event model implications
- identify document/file/integration boundaries
- identify security review triggers
- propose ADRs where durable decisions are needed

Outputs:
- architecture summary or plan update
- ADR drafts when needed
- risks and blockers
- recommended implementation order

No production code may be written in this stage.

## Stage 3 - Feature slicing and implementation planning

Profile: `PLANNING`

Rules:
- one feature slice per spec folder
- included scope must be explicit
- excluded scope must be explicit
- dependencies must be explicit
- security controls must be linked where relevant
- tests must be planned

Required outputs per feature:
- `specs/<feature>/spec.md`
- `specs/<feature>/plan.md`
- traceability scaffold updates where needed

A slice is not implementation-ready until `spec.md` and `plan.md` are internally consistent.

## Stage 4 - Implementation and tests

Profile: `IMPLEMENTATION`

Rules:
- implement only the approved active slice
- no unrelated refactors
- no extra scope
- tests are mandatory
- update review and traceability
- fail closed on security ambiguity

Required actions:
- confirm active spec and plan
- implement bounded scope
- add/update tests
- update `review.md`
- update requirements traceability
- update security traceability if applicable
- update workflow state only for meaningful active/accepted/blocker/next-slice changes

Stop if implementation requires changing requirements, architecture, or another slice.

## Stage 5 - Validation, traceability, and review

Profile: `REVIEW`, or `SECURITY` when security triggers apply.

Required actions:
- compare implementation to spec and plan
- check tests against acceptance criteria
- check scope drift
- verify requirement traceability
- verify security traceability where relevant
- verify deferred items are explicit
- classify findings by severity

Required outputs:
- updated `review.md`
- corrected traceability files if needed
- accept / not accept recommendation
- unresolved findings and owners

## Stage 6 - Change handling and iteration

Profile: usually `REQUIREMENTS` or `PLANNING`.

Rules:
- do not silently add scope to active implementation
- changed requirements must be registered
- impact must be assessed before implementation
- accepted slices must not be rewritten without explicit change handling

A change may result in:
- updated requirement
- new feature slice
- revised plan
- new ADR
- security review
- deferred item

## Stage 6A - New requirement / change intake

Profile: `REQUIREMENTS`

Goal:
Safely handle new or changed requirements without corrupting existing traceability.

Required actions:
1. Classify as new, changed, replaced, or removed.
2. Assign or update requirement ID and version.
3. Record rationale/source/date/status.
4. Identify impacted requirements, slices, ADRs, tests, and security controls.
5. Decide whether it fits an existing slice or requires a new slice.
6. Update requirements governance files.
7. Update security traceability if relevance changes.
8. Record open questions or blockers.

Hard rules:
- no code in Stage 6A
- no silent replacement of accepted requirements
- no deletion of historical traceability
- no implementation until impact is clear
