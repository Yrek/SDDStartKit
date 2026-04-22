@AGENTS.md

## Claude Code
- Follow the same governance-first workflow as defined in AGENTS.md.
- Read `governance/agent/current-workflow-state.md` first for operational context.
- Use `governance/agent/short-prompts.md` for routine slice work where applicable.
- Before making code changes, summarize the current slice scope and blockers.
- Do not implement outside the approved feature slice.
- Always update review.md and traceability files after implementation.
- Keep work to one slice at a time.
- Read only files needed for the current task.
- Do not restate accepted history unless directly relevant.
- Doc-only governance/instruction fixes do not require full slice re-review unless explicitly required.
- Treat design references as visual guidance only, never as the source of truth for auth, tenant logic, security controls, or API contracts.
- `governance/agent/current-workflow-state.md` is lightweight memory only. Accepted `review.md` + requirements/security traceability files remain authoritative on conflicts.
