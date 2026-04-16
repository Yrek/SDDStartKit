# Agent Workflow

## Overview
Development must follow staged execution:

0. Governance and context loading
1. Requirement normalization and decomposition
2. Architecture and solution design
3. Feature slicing and implementation planning
4. Implementation and tests
5. Validation, traceability, and review
6. Change handling and iteration
6A. New requirement / change intake

---

## Stage 0 – Governance and context loading

### Inputs
- `governance/requirements/master-requirements.md`
- `governance/security/security-spec.md`
- `governance/adr/`
- existing codebase
- `governance/agent/constitution.md`

### Required actions
1. Load and summarize governing documents.
2. Identify ADRs and architectural constraints.
3. Identify security rules affecting design, auth, storage, logging, exports, offline, and tenant isolation.
4. Refuse to proceed with architecture or implementation until these are loaded.

### Outputs
- context summary
- architectural constraints
- security constraints

---

## Stage 1 – Requirement normalization and decomposition

### Goal
Convert the large requirements document into atomic, traceable, implementation-ready requirements.

### Required actions
1. Extract all requirements into atomic entries.
2. Assign stable requirement IDs.
3. Classify each item.
4. Mark priority.
5. Detect ambiguity, conflict, duplication, and dependencies.
6. Group requirements into candidate feature slices.
7. Create a first traceability scaffold.

### Required outputs
- `governance/requirements/requirements-index.md`
- `governance/requirements/open-questions.md`
- `governance/requirements/feature-map.md`
- `governance/requirements/traceability.md`

---

## Stage 2 – Architecture and solution design

### Required inputs
- normalized requirements
- open questions
- feature map
- security spec
- ADRs

### Required actions
1. Identify bounded domains/modules.
2. Propose core entities and relationships.
3. Define trust and authorization boundaries.
4. Define tenant isolation strategy.
5. Define offline sync model.
6. Define document/versioning model.
7. Define audit/event model.
8. Map decisions to requirements and security controls.

### Outputs
- architecture overview
- domain model
- data model draft
- trust boundary model
- security design notes
- ADR proposals if needed

---

## Stage 3 – Feature slicing and implementation planning

### Rules
A feature slice must be:
- small enough to review in one pass
- testable end-to-end
- traceable to requirement IDs
- traceable to security controls if relevant
- bounded in scope

### Required outputs per feature
- `specs/<feature>/spec.md`
- `specs/<feature>/plan.md`

---

## Stage 4 – Implementation and tests

### Rules
1. Read ADRs, security spec, feature spec, and plan before writing code.
2. Do not implement features not listed in the approved spec.
3. Follow least privilege and secure defaults.
4. Keep tenant and authorization checks centralized and explicit.
5. Generate/update tests as part of implementation.

---

## Stage 5 – Validation, traceability, and review

### Required actions
1. Map code and tests back to requirement IDs.
2. Map code and tests back to security controls.
3. List uncovered and partially covered requirements.
4. Flag deviations from architecture, ADRs, or security spec.

### Required outputs
- updated `governance/requirements/traceability.md`
- updated `governance/security/security-traceability.md`
- `specs/<feature>/review.md`

---

## Stage 6 – Change handling and iteration

### Rules
1. New requirements get new IDs.
2. Changed requirements get version updates.
3. Removed requirements are marked deprecated/removed, not silently deleted.
4. Changed requirements must update:
   - traceability
   - impacted feature specs
   - impacted tests
   - impacted ADRs
   - impacted security mapping

---

## Stage 6A – New requirement / change intake

### Goal
Add new features or changed requirements in a controlled way without bypassing traceability, architecture review, or security review.

### Required actions
1. Determine whether the request is:
   - a new requirement
   - a change to an existing requirement
   - a replacement of an existing requirement
   - a removal/deprecation request
2. Assign or update requirement ID and version.
3. Add or update metadata.
4. Identify impacted requirements, feature specs, modules, ADRs, tests, and security controls.
5. Decide whether the change:
   - fits an existing feature slice
   - requires a new feature spec
   - requires architecture review
   - requires security review
6. Update all relevant requirement and traceability files.
7. Produce a short impact assessment and recommendation.

### Hard rules
- no silent overwrite of existing requirements
- no deletion without marking superseded/removed
- preserve traceability history
- if security-relevant, link to the security spec explicitly
- if architecture is affected, flag for Stage 2 review before implementation
