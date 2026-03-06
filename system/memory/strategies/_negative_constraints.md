# Negative Constraints

Learned anti-patterns and guardrails. These represent hard-won lessons from past failures.
The Dream Engine appends new constraints here during SWS (Slow Wave Sleep) phase.

**Severity Levels:**
- `high`: Always apply, regardless of context. Violating these caused critical failures.
- `medium`: Apply when context matches. Violating these caused significant rework.
- `low`: Consider when context matches. Violating these caused minor inconvenience.

---

### Never overwrite an entire file without reading it first
**Context:** file editing, any project
**Severity:** high
**Learned from:** seed

### Never run `npm install` or `pip install` without checking existing lock files
**Context:** dependency management
**Severity:** medium
**Learned from:** seed

### Never commit sensitive files (.env, credentials, API keys) to version control
**Context:** git operations
**Severity:** high
**Learned from:** seed
