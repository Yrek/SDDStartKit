# Short Prompts

These are compact prompts for routine slice work.
Use them together with `governance/agent/current-workflow-state.md`.

---

## 1. Readiness check

```md
Check whether <FEATURE ID + NAME> is ready for implementation.

Do not write production code.

Read only:
- governance/agent/current-workflow-state.md
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- any existing specs/<feature>/review.md
- directly relevant ADRs and traceability files

Tasks:
1. Determine whether the slice is:
   - approved and ready for implementation
   - needs spec/plan correction first
   - needs review first
   - blocked by another slice/decision
2. Identify only direct blockers or stale assumptions.
3. Recommend the exact next action.

Output:
- readiness status
- blocker(s) if any
- recommended next action
```

---

## 2. Stage 4 implementation

```md
Execute Stage 4 only for:

- <FEATURE ID + NAME>

Do not implement any other slice.
Do not refactor unrelated code.

Read only:
- governance/agent/current-workflow-state.md
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- directly relevant ADRs
- required traceability files only if coverage must be updated

Required actions:
1. Restate exact scope briefly.
2. Implement only this slice.
3. Add/update tests.
4. Update review.md and traceability files.

Rules:
- no extra scope
- tests are mandatory
- fail closed on security ambiguity
- if blocked, stop and report

At the end, report:
- files changed
- tests added
- requirement IDs implemented
- security controls implemented
- remaining risks or gaps
```

---

## 3. Finish interrupted slice

```md
The slice was already in progress and the last session stopped mid-task.

Do not restart implementation from scratch.
Do not add new feature scope.

Read only:
- governance/agent/current-workflow-state.md
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- specs/<feature>/review.md if present
- the current implementation files for this slice
- traceability files only if governance updates are the interrupted part

Tasks:
1. Inspect current slice state.
2. Identify what remains:
   - code
   - tests
   - review.md
   - requirements traceability
   - security traceability
3. Finish only the remaining work.
4. Report whether the slice is ready for review.

Output:
- files changed
- what was completed
- whether the slice is ready for review
```

---

## 4. Formal review

```md
Rerun the formal review for:

- <FEATURE ID + NAME>

Do not write production code.

Read only:
- governance/agent/current-workflow-state.md
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- specs/<feature>/review.md
- directly relevant ADRs
- requirements traceability
- security traceability
- implementation and tests for this slice

Check:
- scope drift
- requirement coverage
- security control coverage
- missing tests
- fail-closed behavior
- overlap with other slices
- whether partial implementations are correctly marked
- whether review.md and traceability are accurate

Output:
- findings by severity
- required corrections
- whether the slice is ready to accept
- whether remaining items must move explicitly to later slices
```

---

## 5. Doc-only correction

```md
Apply a documentation-only correction pass to:

- <FEATURE ID + NAME>

Do not change production code.
Do not change tests.

Read only:
- governance/agent/current-workflow-state.md
- specs/<feature>/review.md
- requirements traceability
- security traceability
- latest review/acceptance output

Tasks:
1. Fix only the named documentation/traceability issues.
2. Do not expand scope.
3. Report whether the slice is now ready for acceptance.
```

---

## 6. Code/test correction

```md
Apply a bounded correction pass to:

- <FEATURE ID + NAME>

Do not add new feature scope.
Do not refactor unrelated code.

Read only:
- governance/agent/current-workflow-state.md
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- specs/<feature>/review.md
- latest review findings
- directly relevant implementation/test files

Tasks:
1. Fix only the listed review findings.
2. Add/update tests where required.
3. Update review.md/traceability if needed.
4. Report whether the slice is ready for re-review.
```

---

## 7. Acceptance

```md
The current feature slice has passed review and is ready for acceptance.

Do not write production code.

Tasks:
1. Confirm there are no remaining HIGH or MEDIUM findings.
2. Verify review.md reflects the final accepted state.
3. Verify requirements traceability and security traceability are accurate.
4. List deferred items and their owners.
5. Confirm the feature is ready to accept.
6. Recommend the next approved slice.

Output:
- acceptance decision
- deferred items and owners
- next slice to implement
```

---

## 8. Register a new slice

```md
Do not write production code yet.

We need to register and define a new slice:

- <FEATURE ID + NAME>

Read only:
- governance/agent/current-workflow-state.md
- requirements-index
- feature-map
- traceability
- security-traceability
- directly relevant ADRs
- directly relevant accepted slice reviews/specs

Tasks:
1. Register the new slice in governance where needed.
2. Create spec.md and plan.md.
3. Keep the slice bounded and explicit about dependencies and exclusions.

Output:
- governance files updated
- files created
- exact included scope
- exact excluded scope
- why this slice should be next
```

---

## 9. Frontend continuation implementation

```md
Implement the approved frontend scope for:

- <FEATURE ID + NAME>

Do not reopen backend scope unless a real blocker requires a minimal change.

Read only:
- governance/agent/current-workflow-state.md
- specs/<feature>/spec.md
- specs/<feature>/plan.md
- specs/<feature>/review.md if present
- relevant frontend design references

Rules:
- frontend only unless blocked
- design references are visual guidance only
- no extra business scope
- preserve permission-aware UI behavior from governance/specs

At the end, report:
- frontend files/components changed
- tests added
- whether frontend scope is now implemented
```
