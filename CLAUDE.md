@AGENTS.md

## Claude Code usage

Follow `AGENTS.md` exactly.

Before work:
- read `governance/agent/current-workflow-state.md`
- read `governance/agent/agent-profiles.md`
- select exactly one profile
- use `governance/agent/short-prompts.md` for routine work
- read only profile-allowed files

Routine implementation must not load the full governance pack.

Do not implement outside the approved active feature slice.
Do not change requirements, global governance, or security rules unless explicitly asked.
Always update review and traceability after implementation.

Use compact output:

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
