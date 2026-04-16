# Prompt Templates

This file is the practical day-to-day prompt library for the coding agent.
Use these prompts as starting points and adjust only the task-specific parts.

## 1. Universal startup instruction
```md
Read and follow, in order:
1. governance/adr/*
2. governance/security/security-spec.md
3. governance/agent/constitution.md
4. governance/agent/workflow.md
5. governance/agent/prompt-templates.md
6. governance/requirements/master-requirements.md
7. governance/requirements/requirements-index.md
8. governance/requirements/open-questions.md
9. governance/requirements/feature-map.md
10. governance/requirements/traceability.md
11. governance/security/security-traceability.md

Hard rules:
- no silent assumptions
- no extra scope
- tests are mandatory
- traceability must be updated
- if sources conflict, stop and report
- existing ADRs must be respected
- if security-critical uncertainty exists, fail closed
```

## 2. Stage 0 + Stage 1 review only
```md
Execute Stage 0 and Stage 1 only.

Do not write code.

Tasks:
1. verify the governance pack
2. refine the requirements index
3. refine open questions
4. refine the feature map
5. refine the traceability scaffold
6. identify where the security spec must be linked

Deliver:
- context summary
- ambiguities/conflicts/missing information
- proposed updates to governance files
- recommendation for the first 3 implementation slices

Constraints:
- no code changes
- no migrations
- no implementation beyond analysis and refinement
```

## 3. Stage 1 decomposition only
```md
Do not write code.

Analyze the master requirements and produce or refine:
- requirements-index
- open-questions
- feature-map
- traceability scaffold

For each requirement:
- assign or verify stable ID
- classify type and priority
- identify dependencies
- identify security relevance
- identify whether the security spec applies

Report ambiguities, conflicts, and missing information explicitly.
```

## 4. Stage 2 architecture only
```md
Do not write feature code yet.

Use the normalized requirements, ADRs, and security spec to propose architecture.

Include:
- domain boundaries
- entities and relationships
- trust boundaries
- tenant isolation design
- authorization approach
- offline/sync design
- document/versioning model
- audit/event model
- export/reporting boundary considerations
- security-critical design decisions
- ADR proposals where needed

Output:
1. architecture summary
2. key decisions
3. unresolved risks/questions
4. recommended feature implementation order
5. suggested ADRs to add
```

## 5. Create the first 3 feature specs
```md
Using the approved architecture, approved requirements decomposition, approved feature map, applicable ADRs, and security specification, create the first 3 feature slices that should be implemented.

For each slice:
- create specs/<feature>/spec.md
- create specs/<feature>/plan.md

Requirements:
- include source requirement IDs
- include related security controls
- define included scope and excluded scope
- define acceptance criteria
- define tests required
- list dependencies
- list open questions that remain
- keep each slice bounded and reviewable
- do not invent scope beyond approved requirements and architecture
```

## 6. Implement one approved feature slice only
```md
Implement only the approved feature slice.

Before coding, read:
- governance/adr/*
- governance/security/security-spec.md
- governance/agent/constitution.md
- governance/requirements/requirements-index.md
- governance/requirements/feature-map.md
- governance/requirements/traceability.md
- the relevant spec and plan in specs/<feature>/

Your task:
1. restate exact slice scope
2. list files you will change
3. list tests you will add
4. list relevant security controls
5. then implement only that slice

Constraints:
- no unrelated refactors
- no extra features
- preserve tenant boundaries
- preserve traceability
- update tests
- update governance/requirements/traceability.md
- update governance/security/security-traceability.md
- create or update specs/<feature>/review.md

At the end, report:
- implemented requirement IDs
- implemented security controls
- files changed
- tests added
- remaining gaps or assumptions
```

## 7. Review a patch or PR against governance
```md
Review the current changes against:
- governance/agent/constitution.md
- governance/agent/workflow.md
- governance/security/security-spec.md
- governance/requirements/requirements-index.md
- governance/requirements/traceability.md
- the relevant feature spec and plan
- ADRs

Check for:
- requirement coverage
- unauthorized scope expansion
- missing tests
- security control gaps
- architectural drift
- traceability updates
- assumptions not documented

Output:
- findings by severity
- violated rules if any
- missing tests
- missing traceability
- recommendation: approve / needs changes
```

## 8. Add a new requirement safely
```md
A new requirement or feature request has been introduced.

Do not write code.

Tasks:
1. Determine whether this is:
   - a new requirement
   - a change to an existing requirement
   - a replacement of an existing requirement
   - a removal/deprecation request
2. Assign or update requirement ID and version.
3. Add/update requirement metadata:
   - title
   - category
   - priority
   - status
   - owner
   - source
   - rationale
   - changed date
   - changed by
   - change reason
4. Identify:
   - impacted existing requirements
   - impacted feature specs
   - impacted modules
   - impacted ADRs
   - impacted tests
   - impacted security controls
5. Decide whether the change:
   - fits an existing feature slice
   - requires a new feature spec
   - requires architecture review
   - requires security review
6. Update:
   - governance/requirements/requirements-index.md
   - governance/requirements/open-questions.md if needed
   - governance/requirements/feature-map.md
   - governance/requirements/traceability.md
   - governance/security/security-traceability.md if relevant
7. Produce a short impact assessment and a recommendation:
   - approve
   - defer
   - reject
   - needs clarification
```

## 9. Security review for a feature
```md
Perform a security review for the feature in specs/<feature>/.

Review against:
- governance/security/security-spec.md
- governance/security/security-review-checklist.md
- governance/security/security-traceability.md
- relevant ADRs
- relevant requirements

Check:
- authn/authz impact
- tenant separation
- file handling
- audit logging
- export visibility
- offline/local storage
- secrets and token handling
- data integrity / versioning / non-repudiation

Output:
- applicable security controls
- gaps
- recommended fixes
- tests to add
- architecture review triggers if any
```

## 10. Architecture review trigger prompt
```md
A proposed change may affect core architecture.

Do not implement it yet.

Assess whether the change affects:
- tenant boundary model
- role/authorization model
- offline/sync behavior
- document/versioning model
- audit/event model
- export/report visibility
- API/integration boundaries
- data retention/archival
- core workflow states

If yes:
- describe the architectural impact
- list impacted requirements
- list impacted security controls
- propose ADR updates or new ADRs
- recommend whether implementation should pause pending architecture approval
```

## 11. Refine traceability after implementation
```md
Update traceability after the current implementation.

Tasks:
1. map changed code files to requirement IDs
2. map changed code files to security controls
3. identify tests covering each requirement/control
4. update:
   - governance/requirements/traceability.md
   - governance/security/security-traceability.md
   - specs/<feature>/review.md
5. list any uncovered or partially covered requirements

Do not add new features while doing this.
```
