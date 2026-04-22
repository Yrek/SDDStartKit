# Prompt Templates

This file is the practical day-to-day prompt library for the coding agent.
Use these prompts as starting points and adjust only the task-specific parts.

## 1. Universal startup instruction
```md
Read and follow, in order:
1. governance/agent/current-workflow-state.md
2. governance/agent/constitution.md
3. governance/agent/workflow.md
4. governance/agent/short-prompts.md
5. governance/agent/prompt-templates.md

Then read only task-relevant formal sources for the current stage:
- governance/adr/*
- governance/security/security-spec.md
- governance/requirements/master-requirements.md
- governance/requirements/requirements-index.md
- governance/requirements/open-questions.md
- governance/requirements/feature-map.md
- governance/requirements/traceability.md
- governance/security/security-traceability.md

Hard rules:
- no silent assumptions
- no extra scope
- tests are mandatory
- traceability must be updated
- if sources conflict, stop and report
- existing ADRs must be respected
- if security-critical uncertainty exists, fail closed
- one slice at a time
- do not restate accepted history unless directly relevant
- current-workflow-state is operational memory only
- accepted review.md + requirements/security traceability are authoritative
- design references are visual guidance only, never source-of-truth for auth/tenant/security/API contracts
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

### x. Create ADRs
```md
Use the approved architecture output and create draft ADR files for the most important architectural decisions.

Create ADRs under `governance/adr/`.

Prioritize decisions that affect:
- tenant isolation
- authorization model
- audit/event architecture
- document versioning/integrity
- offline sync/conflict handling
- external integration trust boundaries

For each ADR:
- use the ADR template in `governance/adr/`
- include Context
- include Decision
- include Rationale
- include Consequences
- include Related requirements
- include Related security controls

Rules:
- create draft ADRs only
- do not assume they are approved
- do not write production code
- keep decisions consistent with the approved architecture and security specification

At the end, list:
- ADR files created
- decisions that still need human confirmation
- any conflicts or unresolved questions
```

### x. ADR final input before acceptance
```md
Review the approved ADRs and current governance files and tell me whether anything still blocks safe Stage 3 spec generation.

Do not write code.
Do not create specs yet.

Classify findings as:
- blocker
- should resolve first
- safe to defer

Be explicit about:
- authorization model gaps
- unresolved role/scope matrix issues
- unresolved offline/conflict issues
- unresolved document/versioning rules
- unresolved external-access/delegation rules
```

### x. ADR acceptance
```md
The ADR drafts have now been reviewed and accepted.

Do not write production code.
Do not create feature specs yet.

Your task is to reconcile the governance files with the approved ADRs.

Read:
- AGENTS.md
- governance/agent/constitution.md
- governance/agent/workflow.md
- governance/security/security-spec.md
- governance/security/security-traceability.md
- governance/requirements/master-requirements.md
- governance/requirements/requirements-index.md
- governance/requirements/open-questions.md
- governance/requirements/feature-map.md
- governance/requirements/traceability.md
- all ADRs in governance/adr/

Required actions:
1. Identify where the approved ADRs affect:
   - requirements-index.md
   - open-questions.md
   - feature-map.md
   - traceability.md
   - security-traceability.md
2. Update those files where needed.
3. Mark any previously open questions as:
   - resolved by ADR
   - still open
   - deferred
4. Identify any new constraints introduced by the ADRs that feature specs must respect.
5. Confirm the first 3 feature slices to create next.

Rules:
- no production code
- no implementation plans beyond governance reconciliation
- no extra product scope
- do not silently delete history
- if an ADR conflicts with another governance file, update the governance file or flag the conflict explicitly

At the end, report:
- files updated
- open questions resolved
- remaining blockers
- confirmed first 3 feature slices
```

## 5. Create the first 3 feature specs
```md
Execute Stage 3 only.

Use the approved architecture, approved ADRs, reconciled governance files, requirements decomposition, and security mapping.

Do not repeat architecture analysis.
Do not write production code.

Your task is to create the first 3 implementation-ready feature slices.

Read:
- AGENTS.md
- governance/agent/constitution.md
- governance/agent/workflow.md
- governance/agent/prompt-templates.md
- governance/security/security-spec.md
- governance/security/security-traceability.md
- governance/security/security-review-checklist.md
- governance/requirements/master-requirements.md
- governance/requirements/requirements-index.md
- governance/requirements/open-questions.md
- governance/requirements/feature-map.md
- governance/requirements/traceability.md
- all ADRs in governance/adr/

Required actions:
1. Select the first 3 feature slices that should be implemented next.
2. For each selected slice, create:
   - `specs/<feature>/spec.md`
   - `specs/<feature>/plan.md`
3. Keep each slice bounded, reviewable, and suitable for implementation by a coding agent.

Selection rules:
- choose foundational slices first
- choose slices that reduce security and architecture risk
- choose slices that unblock later work
- avoid combining unrelated concerns into one slice
- keep slices small enough to review in one pass

For each `spec.md`, include:
- Goal
- Included scope
- Excluded scope
- Source requirement IDs
- Related security controls
- Actors / roles
- Data model impact
- API / UI behavior
- Validation rules
- Acceptance criteria
- Tests required
- Risks / open questions

For each `plan.md`, include:
- Files to add/change
- Migration / data changes
- Tests to add
- Security checks
- Rollback / risk notes

Rules:
- use approved ADRs as binding input
- use requirement IDs explicitly
- use security controls explicitly
- do not invent scope beyond approved requirements
- actually create or update the files in `specs/`
- do not stop at recommendations only

At the end, report:
- which 3 slices were selected
- why they were selected first
- which files were created
- any blockers that still prevent implementation
```

## x. Technical architecture prompt
´´´md
Do not write production code.

Use the approved business architecture, accepted ADRs, requirements, and security specification to propose the Azure technical platform architecture for this project.

Focus on:
- frontend hosting choice
- backend hosting choice
- database choice
- blob/document storage choice
- offline synchronization backend implications
- reporting/export generation placement
- identity/auth integration
- networking and isolation needs
- cost-sensitive MVP choices
- scaling and operational tradeoffs

Compare realistic Azure options where relevant, such as:
- Azure Static Web Apps vs App Service
- Azure Functions Flex Consumption vs Premium vs Dedicated/App Service plan
- Azure Database for PostgreSQL Flexible Server vs Azure SQL Database
- Storage Account / Blob Storage usage for documents and media

Output:
1. recommended Azure architecture for MVP
2. tradeoffs and rejected alternatives
3. decisions that should become ADRs
4. dependencies this creates for current feature specs
5. any blockers before implementation continues

Rules:
- align with security-spec.md
- align with tenant isolation and audit requirements
- align with document/versioning and offline requirements
- prefer simple, cost-conscious MVP choices unless requirements force otherwise
´´´
### x. Used stage 3 prompt
´´´md
Execute Stage 3 only.

Use the approved architecture, accepted ADRs, reconciled governance files, requirements decomposition, and security mapping.

Do not repeat architecture analysis.
Do not write production code.

Your task is to create the 3 confirmed implementation-ready feature slices:

- FEAT-001 Tenant, organization, user, and role foundation
- FEAT-002 Project, object, and location structure
- FEAT-008 Comments, history, and audit-event foundation

For each selected slice, create:
- `specs/<feature>/spec.md`
- `specs/<feature>/plan.md`

Required inputs:
- AGENTS.md
- governance/agent/constitution.md
- governance/agent/workflow.md
- governance/agent/prompt-templates.md
- governance/security/security-spec.md
- governance/security/security-traceability.md
- governance/security/security-review-checklist.md
- governance/requirements/master-requirements.md
- governance/requirements/requirements-index.md
- governance/requirements/open-questions.md
- governance/requirements/feature-map.md
- governance/requirements/traceability.md
- all ADRs in governance/adr/

For each `spec.md`, include:
- Goal
- Included scope
- Excluded scope
- Source requirement IDs
- Related security controls
- Actors / roles
- Data model impact
- API / UI behavior
- Validation rules
- Acceptance criteria
- Tests required
- Risks / open questions

For each `plan.md`, include:
- Files to add/change
- Migration / data changes
- Tests to add
- Security checks
- Rollback / risk notes

Rules:
- use accepted ADRs as binding input
- use requirement IDs explicitly
- use security controls explicitly
- keep each slice bounded and reviewable
- do not invent scope beyond approved requirements
- actually create or update the files in `specs/`
- do not stop at recommendations only

At the end, report:
- files created
- requirement IDs covered by each slice
- security controls covered by each slice
- any blockers that still prevent implementation
´´´
### x. Review created specs
´´´md
Review the newly created Stage 3 slice files for:

- scope clarity
- boundedness
- requirement coverage
- security control coverage
- test completeness
- hidden scope creep
- overlap between slices
- missing exclusions
- missing risks or open questions

Do not write production code.

For each of the 3 slices:
1. say whether it is ready for implementation
2. list any corrections needed
3. identify whether the slice is too broad, too narrow, or well-sized

At the end:
- recommend which slice should be implemented first
- list any edits that should be made before implementation starts
´´´

### x. Cleanup specs after comments
´´´md
Apply a Stage 3 correction pass to the existing feature slice specs and plans.

Do not write production code.

Your task is to update the current Stage 3 artifacts based on the review findings.

Read:
- AGENTS.md
- governance/agent/constitution.md
- governance/agent/workflow.md
- governance/security/security-spec.md
- governance/security/security-traceability.md
- governance/requirements/requirements-index.md
- governance/requirements/feature-map.md
- governance/requirements/traceability.md
- all ADRs in governance/adr/
- the existing specs and plans for FEAT-001, FEAT-002, and FEAT-008
- the Stage 3 review findings

Required changes:
1. Fix requirement ownership and traceability for FEAT-008:
   - remove or mark partial any requirement IDs that FEAT-008 does not fully implement
   - especially correct REQ-AUDIT-003/004/005 if they exceed FEAT-008 scope

2. Clarify audit ownership boundaries:
   - FEAT-008 owns audit store/query/taxonomy/foundation
   - FEAT-001 and FEAT-002 only emit audit events through shared interfaces
   - make this explicit in spec.md and plan.md where relevant

3. Narrow FEAT-001 scope:
   - keep tenant/org/user/role/authz/delegation foundation
   - avoid accidental infrastructure/backup/resilience implementation creep
   - if resilience/policy settings remain, clearly mark them as policy/configuration only

4. Tighten FEAT-002 boundaries:
   - restrict UI scope to minimal structure CRUD/list behavior
   - exclude inspection-specific metadata semantics that belong to FEAT-003
   - avoid audit-foundation ownership

5. Improve test specificity:
   - FEAT-001: delegated link traceability, recipient email, IP, expiry/null-expiry, multi-use behavior as applicable
   - FEAT-008: retention/cleanup guardrails, role-scoped query visibility, immutability protections

Rules:
- do not create new feature slices in this pass
- do not write production code
- do not expand scope
- update the existing files in place
- keep all slices bounded and reviewable

At the end, report:
- files updated
- requirement mappings changed
- ownership boundaries clarified
- whether FEAT-001 is now ready for implementation
´´´

## x. Stage 3 correction prompt
´´´md
Apply a Stage 3 correction pass to the current feature slice specifications and plans.

Do not write production code.
Do not create new architecture unless a blocker requires it.
Do not create new feature slices unless explicitly required by the review findings.

Read first:
1. AGENTS.md
2. governance/agent/constitution.md
3. governance/agent/workflow.md
4. governance/security/security-spec.md
5. governance/security/security-traceability.md
6. governance/security/security-review-checklist.md
7. governance/requirements/master-requirements.md
8. governance/requirements/requirements-index.md
9. governance/requirements/open-questions.md
10. governance/requirements/feature-map.md
11. governance/requirements/traceability.md
12. all ADRs in governance/adr/
13. the current `specs/<feature>/spec.md` and `specs/<feature>/plan.md` files
14. the latest review findings for those slices

Your task is to correct the existing Stage 3 artifacts so they are safe and ready for implementation.

Required actions:
1. Fix requirement ownership and traceability:
   - remove, narrow, or mark partial any requirement IDs that a slice does not fully implement
   - ensure requirement mappings are truthful and bounded

2. Fix ownership boundaries between slices:
   - clarify which slice owns the foundation/infrastructure/shared capability
   - clarify which slices only consume or emit through shared interfaces
   - remove hidden overlap and dependency confusion

3. Narrow slice scope where needed:
   - remove accidental multi-feature scope
   - tighten included scope
   - strengthen excluded scope
   - keep slices bounded and reviewable

4. Improve implementation boundaries:
   - prevent UI, workflow, data, or infrastructure concerns from leaking into the wrong slice
   - keep slices aligned with approved architecture and ADRs

5. Improve test specificity:
   - replace overly generic tests with concrete edge cases and abuse-case coverage where relevant
   - ensure security-relevant behaviors have explicit tests

6. Update files in place:
   - modify the existing `spec.md` and `plan.md` files
   - update traceability-related references if required
   - do not leave review findings unresolved if they can be corrected in this pass

Rules:
- no production code
- no extra scope
- no hidden architectural changes
- no false traceability claims
- no silent assumptions
- if a review finding cannot be resolved without a new ADR or architecture decision, flag it explicitly as a blocker
- keep slices implementation-ready, not just conceptually cleaner

At the end, report:
- files updated
- requirement mappings changed
- ownership boundaries clarified
- tests made more specific
- blockers still remaining
- which slices are now ready for implementation
´´´
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

## 12. feature implementations
´´´md
Execute Stage 4 only for the approved feature slice:

- <FEATURE ID + NAME>

Read first:
- AGENTS.md
- governance/agent/constitution.md
- governance/agent/workflow.md
- governance/security/security-spec.md
- governance/security/security-traceability.md
- governance/security/security-review-checklist.md
- governance/requirements/requirements-index.md
- governance/requirements/traceability.md
- all ADRs in governance/adr/
- specs/<feature>/spec.md
- specs/<feature>/plan.md

Do not implement any other slice.
Do not refactor unrelated code.

Required actions:
1. Restate exact scope.
2. List files to add/change.
3. List tests to add.
4. List security controls to enforce.
5. Implement only this slice.
6. Update:
   - specs/<feature>/review.md
   - governance/requirements/traceability.md
   - governance/security/security-traceability.md

Rules:
- no extra scope
- no unrelated refactors
- tests are mandatory
- fail closed on security ambiguity
- if blocked by another slice/ADR/decision, stop and report it

At the end, report:
- requirement IDs implemented
- security controls implemented
- files changed
- tests added
- remaining risks or gaps
´´´


## Switch / onboard agent prompt
´´´md
We are continuing an existing governance-driven implementation workflow.

You must follow:
- governance/agent/current-workflow-state.md
- AGENTS.md
- governance/agent/constitution.md
- governance/agent/workflow.md
- governance/agent/short-prompts.md
- governance/security/security-spec.md
- governance/security/security-traceability.md
- governance/requirements/traceability.md
- all ADRs
- the current approved feature spec and plan

Rules:
- one slice at a time
- no extra scope
- no unrelated refactors
- fail closed on security ambiguity
- tests are mandatory
- update review.md and traceability files
- if blocked, stop and report instead of guessing
- if current-workflow-state conflicts with accepted review/traceability, accepted review/traceability wins

First, summarize the current repo state and identify the exact next step for the current slice before making any code changes.
´´´
