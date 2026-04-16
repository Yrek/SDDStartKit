# Specs Folder

The `specs/` folder contains the working specs for bounded feature slices.

This is where the project moves from:
- large business requirements
to
- small approved implementation slices

## What belongs here

Each feature slice gets its own folder, for example:

- `specs/feature-001-tenant-role-foundation/`
- `specs/feature-005-points-markers/`
- `specs/feature-012-documents-versioning/`

Inside each feature folder you normally keep:

- `spec.md` — what the feature is supposed to do
- `plan.md` — how it will be implemented
- `review.md` — what was actually implemented, tested, and what gaps remain

## Why this folder exists

The master requirements document is too large and broad to code from directly without drift.

The `specs/` folder solves that by turning the large requirements set into:
- bounded slices
- explicit scope
- linked requirement IDs
- linked security controls
- clear acceptance criteria
- required tests

This makes it easier for both humans and coding agents to work safely and review changes.

## Purpose of each file

### `spec.md`
This is the approved feature contract.

It should define:
- goal
- included scope
- excluded scope
- source requirement IDs
- related security controls
- actors / roles
- data model impact
- API / UI behavior
- validation rules
- acceptance criteria
- tests required
- risks / open questions

The agent should not implement beyond this scope.

### `plan.md`
This is the implementation plan for the feature.

It should define:
- files to add/change
- migrations or data changes
- tests to add
- security checks
- rollback or risk notes

The agent should use this before writing code.

### `review.md`
This is the post-implementation review artifact.

It should define:
- implemented requirement IDs
- implemented security controls
- files changed
- tests added
- remaining gaps
- risks
- follow-up actions

This helps keep traceability honest.

## When to create a new spec folder

Create a new feature folder when:
- a requirement cluster is approved for implementation
- a new requirement does not fit an existing feature slice
- a change is large enough to need separate planning and review
- architecture/security review says the work should be isolated

Do not mix multiple unrelated features into one spec folder.

## Relationship to governance

The `specs/` folder is downstream from governance.

The flow is:

1. `governance/requirements/master-requirements.md`
2. `governance/requirements/requirements-index.md`
3. `governance/requirements/feature-map.md`
4. `specs/<feature>/spec.md`
5. `specs/<feature>/plan.md`
6. implementation
7. `specs/<feature>/review.md`
8. traceability updates

So:
- governance defines what must be respected
- specs define what exact slice is currently being built

## Practical rule

A coding agent should usually **not** implement directly from the master requirements file.

It should implement from an approved `specs/<feature>/spec.md` and `plan.md`, while still respecting governance and ADRs.
