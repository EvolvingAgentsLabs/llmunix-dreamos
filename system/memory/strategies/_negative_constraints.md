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

### Never list a tool in agent instructions that requires capabilities not in the agent's `tools` field
**Context:** agent definition, plugin configuration
**Severity:** medium
**Learned from:** tr_1741348860000_b3c4
**Dream ID:** dream_20260307_a4f7

### Never duplicate agent definition files across multiple directories without a single-source-of-truth mechanism
**Context:** plugin structure, agent definitions
**Severity:** medium
**Learned from:** tr_1741348860000_b3c4
**Dream ID:** dream_20260307_a4f7

### Never reference non-existent tools or APIs in specification documents
**Context:** system specifications, tool mapping
**Severity:** medium
**Learned from:** tr_1741348920000_d5e6
**Dream ID:** dream_20260307_a4f7

### Never allow multiple specification files to define the same parameter with different values
**Context:** system specifications, configuration consistency
**Severity:** medium
**Learned from:** tr_1741348920000_d5e6
**Dream ID:** dream_20260307_a4f7
