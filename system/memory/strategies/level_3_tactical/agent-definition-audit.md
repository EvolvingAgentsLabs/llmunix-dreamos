---
id: strat_3_agent-definition-audit
version: 1
hierarchy_level: 3
title: Agent Definition Consistency Audit
trigger_goals: ["agent", "tools", "audit", "definition", "permissions"]
preconditions: ["Agent definition files exist in one or more locations", "Tool permission configuration exists (e.g., settings.local.json)"]
confidence: 0.5
success_count: 1
failure_count: 0
source_traces: ["tr_1741348860000_b3c4", "tr_1741348980000_f7g8"]
deprecated: false
---

# Agent Definition Consistency Audit

A tactical procedure for verifying that agent definition files are internally consistent, have correct tool grants, and are not duplicated across locations.

## Steps
1. Enumerate all agent definition files across all known locations (e.g., `.claude/agents/`, `llmunix-plugin/agents/`, any other plugin dirs)
2. For each agent, read its definition and extract the `tools` list
3. Scan the agent's instructions for any operations that imply tool usage (e.g., "delete files" implies Bash, "append to file" implies Edit or Read+Write)
4. Compare implied tool needs against the declared `tools` list -- flag any gaps
5. If agent definitions exist in multiple locations, diff them to detect drift
6. Check the settings/permissions file (e.g., `settings.local.json`) for tool permission grants and verify they cover agent needs
7. Report all findings: missing tools, duplicated definitions, permission gaps

## Negative Constraints
- Do NOT assume an agent can use a tool just because it is mentioned in instructions -- verify it is in the `tools` list
- Do NOT silently fix duplicate definitions by picking one -- flag the duplication for an architectural decision
- Do NOT grant overly broad permissions (e.g., `Bash(*)`) to fix a narrow tool gap

## Notes
- The Write tool overwrites entire files; if an agent needs to append (like MemoryAnalysisAgent logging traces), it must either use Edit or document a Read-then-Write-with-append pattern
- Empty strategy directories need .gitkeep files to survive git clone
- Permission patterns like `Bash(date:*)` may be needed for timestamp operations
