# Short Prompts

Use these prompts for routine work. They are intentionally compact.

Before using any prompt, select exactly one profile from `governance/agent/agent-profiles.md` and load only the files allowed by that profile.

Default response format:

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

---

## readiness-check

```md
Profile: REVIEW
Task: Check whether <FEATURE ID + NAME> is ready for implementation.

Read minimal context:
- AGENTS.md
- governance/agent/current-workflow-state.md
- governance/agent/agent-profiles.md
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- specs/<feature>/review.md if present
- directly relevant ADRs/traceability only if needed

Do not write production code.

Return:
- ready / not ready
- blockers
- exact next action
```

---

## requirements-normalize

```md
Profile: REQUIREMENTS
Task: Normalize requirements for <MODULE OR FEATURE>.

Read:
- master requirements
- requirements index
- open questions
- feature map
- change log
- relevant ADRs only

Do:
- assign stable requirement IDs
- classify priority/type/security relevance
- identify dependencies and conflicts
- update requirements governance files

Do not write code or feature plans.

Return changed files, blockers, and recommended slices.
```

---

## plan-slice

```md
Profile: PLANNING
Task: Create or refine spec/plan for <FEATURE ID + NAME>.

Read:
- relevant requirements
- feature map
- relevant ADRs
- relevant security sections if triggered
- existing spec/plan if present

Write:
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- ADR draft only if required

Do not write production code.

Ensure:
- included scope
- excluded scope
- dependencies
- tests
- security controls
- acceptance criteria
```

---

## implement-slice

```md
Profile: IMPLEMENTATION
Task: Implement <FEATURE ID + NAME> only.

Read:
- AGENTS.md
- current workflow state
- agent profiles
- active spec.md and plan.md
- relevant ADRs
- relevant code/tests
- security sections only if triggered

Do:
- implement only approved scope
- add/update tests
- update review.md
- update traceability

Stop if:
- scope is unclear
- dependency is missing
- security ambiguity exists
- change would touch another slice
```

---

## finish-slice

```md
Profile: IMPLEMENTATION
Task: Continue interrupted work on <FEATURE ID + NAME>.

Read:
- active spec/plan/review
- current workflow state
- changed files for this slice
- traceability only if it was part of the interrupted work

Do:
- identify remaining code/tests/docs
- finish only remaining approved work
- update review/traceability

Do not restart or expand scope.
```

---

## review-slice

```md
Profile: REVIEW
Task: Review <FEATURE ID + NAME> against governance.

Read:
- active spec/plan/review
- changed files and tests
- traceability
- relevant ADRs
- security sections if triggered

Check:
- scope drift
- requirement coverage
- security coverage
- missing tests
- false traceability
- unresolved blockers

Return findings by severity and accept / not accept.
```

---

## security-review

```md
Profile: SECURITY
Task: Security-review <FEATURE ID + NAME>.

Read:
- security spec/checklist/traceability
- active spec/plan/review
- relevant ADRs
- relevant code/tests

Check:
- authn/authz
- tenant isolation
- file/upload/export/import
- API/integration boundaries
- audit logging
- secrets/tokens
- fail-closed behavior

Return applicable controls, gaps, tests, and blockers.
```

---

## doc-only-correction

```md
Profile: REVIEW
Task: Apply documentation-only correction for <FEATURE ID + NAME>.

Read only affected governance/review/traceability files.

Do not change production code or tests.
Do not expand scope.

Return changed docs and whether acceptance is now possible.
```

---

## accept-slice

```md
Profile: REVIEW
Task: Accept <FEATURE ID + NAME> if ready.

Verify:
- no unresolved HIGH/MEDIUM findings
- review.md is final
- traceability is accurate
- deferred items are explicit
- next slice is recommended

Do not write production code.

Update workflow state if acceptance changes active/next slice.
```

---

## register-change

```md
Profile: REQUIREMENTS
Task: Register new or changed requirement.

Classify:
- new / changed / replaced / removed
- impacted requirements
- impacted slices
- impacted ADRs
- impacted tests
- impacted security controls

Update requirements governance files.

Do not write code.
Return impact assessment and recommendation.
```
