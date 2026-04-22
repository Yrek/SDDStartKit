# Agent Design

## 1. Purpose
This repository uses a repo-native agent workflow so each session can operate from local governance files instead of reconstructing context from scratch.

Primary goals:
- consistency across sessions and tools
- lower token use through targeted file loading
- safer slice execution with explicit boundaries
- easier handoff between Codex/Claude/other agents

This design is operational and governance-first, aligned to [AGENTS.md](../../AGENTS.md), [constitution.md](./constitution.md), [workflow.md](./workflow.md), and [short-prompts.md](./short-prompts.md).

## 2. Source-of-truth precedence
Formal truth for slice status and coverage is:
1. accepted `specs/*/review.md`
2. [traceability.md](../requirements/traceability.md)
3. [security-traceability.md](../security/security-traceability.md)

[current-workflow-state.md](./current-workflow-state.md) is lightweight operational memory only.

Conflict rule:
- if operational memory conflicts with formal review/traceability records, formal review/traceability wins
- if formal sources conflict with each other, stop and report conflict before proceeding

## 3. Startup sequence
Default startup:
1. read [current-workflow-state.md](./current-workflow-state.md)
2. read [constitution.md](./constitution.md)
3. read [workflow.md](./workflow.md)
4. read [short-prompts.md](./short-prompts.md)
5. load only files needed for the current mode/slice

Minimal-file rule:
- read only directly relevant spec/plan/review, ADRs, and traceability rows for the active task
- do not load unrelated feature slices unless a dependency/blocker requires it

History rule:
- do not reconstruct or restate full repo history for routine work
- only expand historical reconstruction when required for conflict resolution, formal review, or acceptance validation

## 4. Supported operating modes
Modes are aligned to [short-prompts.md](./short-prompts.md).

### `readiness-check`
- purpose: decide if a slice is ready for Stage 4
- required inputs: current workflow state, target slice spec/plan, relevant ADRs/traceability, existing review for the slice
- expected outputs: readiness status, blockers, exact next action
- stop conditions: slice is ready, blocked, or needs correction/review first

### `stage4-implement`
- purpose: implement one approved slice only
- required inputs: current workflow state, slice spec/plan, relevant ADRs, implementation files, required traceability targets
- expected outputs: code/tests for the slice plus updated review/traceability
- stop conditions: scope complete, blocked by unresolved dependency/security ambiguity, or unapproved scope request

### `finish-slice`
- purpose: complete interrupted in-progress slice work without restart
- required inputs: current workflow state, slice spec/plan/review, in-progress implementation/tests, traceability files if pending
- expected outputs: completed remaining items and readiness-for-review status
- stop conditions: remaining items finished, or blocker found that requires explicit escalation

### `formal-review`
- purpose: run governance-based review for a slice
- required inputs: slice spec/plan/review, implementation/tests, relevant ADRs, both traceability files, current workflow state
- expected outputs: findings by severity, required corrections, accept/not-accept recommendation
- stop conditions: findings set finalized and decision recorded

### `doc-only-correction`
- purpose: fix documentation/traceability/review metadata without changing production code
- required inputs: current workflow state, affected review and traceability records, latest review findings
- expected outputs: corrected docs and updated acceptance-readiness statement
- stop conditions: all named doc issues resolved or conflict requiring formal review

### `code-test-correction`
- purpose: fix only approved review findings in code/tests
- required inputs: current workflow state, slice spec/plan/review, latest review findings, relevant code/tests
- expected outputs: bounded corrections, updated tests, updated review/traceability if impacted
- stop conditions: all listed findings resolved or blocked by architecture/security decision

### `accept`
- purpose: finalize slice acceptance decision
- required inputs: latest review with no unresolved blocking findings, accurate traceability, current workflow state
- expected outputs: acceptance decision, deferred item list, recommended next slice
- stop conditions: accepted or explicitly not accepted with blockers

### `register-slice`
- purpose: register and define a new bounded slice without implementation
- required inputs: current workflow state, requirements/feature map/traceability/security-traceability, relevant ADRs
- expected outputs: governance updates plus new slice spec/plan with clear included/excluded scope
- stop conditions: slice definition complete or blocked by unresolved requirement/architecture questions

### `frontend-continuation` (when relevant)
- purpose: complete approved frontend scope in-slice or in a named continuation slice
- required inputs: current workflow state, slice spec/plan/review, frontend references, relevant security/permission rules
- expected outputs: frontend implementation/testing status and updated review/traceability as needed
- stop conditions: approved frontend scope complete, explicitly deferred to named slice, or blocked by required backend decision

## 5. Slice discipline
- one slice at a time
- no silent scope expansion
- fail closed on security ambiguity
- accepted slices are not reopened without explicit reason and documented trigger
- doc-only fixes do not trigger full re-review unless explicitly required by governance/reviewer decision

## 6. Frontend policy
- frontend work can be in-slice or in an explicitly named continuation slice
- design references are visual guidance only
- auth, tenant logic, security controls, and API contracts must come from specs/ADRs/security governance, not HTML/mock/design artifacts
- if a slice spec says frontend required: `Yes`, frontend must be implemented in that slice or explicitly deferred to a named follow-up slice in review/traceability records

## 7. State maintenance policy
Update [current-workflow-state.md](./current-workflow-state.md) when meaningful milestones occur:
- slice accepted
- active slice changed
- blocker discovered or cleared
- recommended next slice changed

State entry rules:
- keep entries concise and operational
- cite the formal record used for each state change
- do not mark acceptance or coverage from assumptions
- do not duplicate full review history in this file

## 8. Review and acceptance policy
Acceptance threshold:
- unresolved HIGH or MEDIUM findings block acceptance

Full formal review is required when:
- production code or tests changed for a slice
- scope interpretation changed
- security-relevant behavior changed

Doc-only correction is enough when:
- only review/traceability/governance wording needs correction
- no production behavior or test evidence changed

Acceptance blockers include:
- unresolved high/medium findings
- missing required tests
- inaccurate or incomplete requirement/security traceability
- hidden scope creep or unresolved security ambiguity

Deferred work vs hidden incompleteness:
- deferred work must be explicitly listed with owner/target slice
- anything required for current slice acceptance but not done is incompleteness, not deferral

## 9. Tool handoff policy
All tools/agents (Codex, Claude, others) resume from the same startup sequence and source-of-truth rules.

Use a lightweight handoff block in session outputs:
- active slice
- current mode
- blockers
- next action
- formal records touched

Handoff rules:
- avoid replaying full history; load only current state + relevant formal files
- never treat prior chat narrative as authority over repo records
- if handoff notes conflict with formal files, formal files win

## 10. Future automation path
This pattern can evolve incrementally:
1. add a small validation script to check required governance files are present before mode execution
2. add traceability consistency checks (review IDs vs requirements/security matrices)
3. add a mode launcher wrapper that selects minimal file sets by mode
4. add pre-commit or CI checks for slice discipline rules (one-slice scope, required review/traceability updates)
5. later introduce an orchestrator that reads `current-workflow-state.md` but always validates against formal records before action

Automation must preserve current governance-first authority and conflict rules.
