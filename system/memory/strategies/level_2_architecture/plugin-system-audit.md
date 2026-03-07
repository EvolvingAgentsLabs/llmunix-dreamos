---
id: strat_2_plugin-system-audit
version: 1
hierarchy_level: 2
title: Plugin and System File Audit
trigger_goals: ["audit", "plugin", "agent tools", "system files", "code review"]
preconditions: ["Plugin or system project with agent definitions", "Access to strategy and specification files"]
confidence: 0.5
success_count: 1
failure_count: 0
source_traces: ["tr_1741348800000_a1b2", "tr_1741348860000_b3c4", "tr_1741348920000_d5e6", "tr_1741348980000_f7g8", "tr_1741349100000_h9i0"]
deprecated: false
---

# Plugin and System File Audit

A systematic approach to auditing plugin configurations, agent definitions, system specification files, and project structure for inconsistencies, missing capabilities, and structural issues.

## Steps
1. Load negative constraints and dream journal for context before starting
2. Search for existing strategies matching the audit goal to avoid redundant work
3. Read all agent definition files and diff across locations for duplicates or drift
4. Read all system specification files and cross-reference shared parameters for consistency (e.g., top-N values, tool names)
5. Read the command handler and primary configuration files (CLAUDE.md, settings) to verify completeness
6. Check structural concerns: .gitkeep in empty directories, file permissions, tool permission grants in settings
7. Catalogue all issues found, classifying by severity (tool mismatch > consistency > structural)
8. Implement fixes in priority order: tool capability gaps first, then cross-file consistency, then structural hygiene
9. Flag issues that require architectural decisions (e.g., deduplication strategy, config weight reduction) for future work rather than applying premature fixes

## Negative Constraints
- Do NOT fix architectural issues (like deduplicating agent definitions) without a design decision -- flag them instead
- Do NOT edit seed strategies in _seeds/ even if they contain issues -- document the issue and fix downstream
- Do NOT assume tool names from other platforms exist in the current runtime -- verify against the actual tool inventory

## Notes
- This strategy was extracted from the first real audit of the LLMunix DreamOS plugin itself
- The audit found 9 issues and fixed 6; the remaining 3 required architectural decisions
- Key finding: agent tool lists must exactly match the capabilities referenced in their instructions
- Key finding: specification files that are never referenced at runtime provide no value -- they must be linked from CLAUDE.md or agent definitions
