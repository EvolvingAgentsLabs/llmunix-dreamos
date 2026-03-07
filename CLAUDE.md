# LLMunix DreamOS Kernel

You are operating within **LLMunix DreamOS**, a bio-inspired cognitive operating system for software development. This system brings the algorithmic rigor of hierarchical memory consolidation (proven in robotics) to Claude Code and Claude Cowork workflows.

## Core Architecture

DreamOS implements a **4-level cognitive hierarchy** adapted from neuroscience:

| Level | Name | Software Equivalent | Example |
|-------|------|---------------------|---------|
| L1 | GOAL | Epic / Project | "Build authentication system" |
| L2 | ARCHITECTURE | Strategy / Design | "Implement JWT with refresh tokens" |
| L3 | TACTICAL | Module / Task | "Write middleware for token validation" |
| L4 | REACTIVE | Command / Tool Call | `Bash: npm install jsonwebtoken` |

## Before Starting Any Complex Task

1. **Check negative constraints** to avoid past mistakes:
   - Read `system/memory/strategies/_negative_constraints.md`
   - Apply ALL constraints with severity `high` unconditionally
   - Apply `medium` constraints when context matches

2. **Search for relevant strategies** before planning:
   - Use `Grep` on `system/memory/strategies/` for keywords related to the user's goal
   - If a strategy with `confidence >= 0.5` matches, APPLY its steps
   - If multiple strategies match, prefer the one with highest `success_count`

3. **Check the dream journal** for recent learnings:
   - Read `system/memory/strategies/_dream_journal.md` for the last 3 entries

## During Task Execution

### Trace Logging
All significant actions MUST be logged as traces. Use the following format for trace files in `system/memory/traces/`:

**File naming**: `trace_YYYY-MM-DD.md` (one file per day, append new traces)

**Trace format**:
```markdown
### Time: [ISO 8601]
**Trace ID:** tr_[timestamp]_[random]
**Level:** [1-4]
**Parent:** [parent_trace_id or null]
**Goal:** [what was being attempted]
**Strategy:** [strategy_id if one was applied, or null]
**Outcome:** [SUCCESS | FAILURE | PARTIAL | ABORTED | UNKNOWN]
**Reason:** [brief explanation of outcome]
**Duration:** [time in ms if measurable]
**Confidence:** [0.0-1.0]

**Actions:**
1. [Tool: action description] -> [result summary]
2. [Tool: action description] -> [result summary]
---
```

### Hierarchy Rules
- L1 traces wrap entire project/epic executions
- L2 traces wrap architectural decisions and design phases
- L3 traces wrap individual module/feature implementations
- L4 traces wrap individual tool calls and commands
- Every L2+ trace MUST reference a parent trace ID

## After Completing a Complex Task

Invoke the `DreamEngineAgent` sub-agent using the `Task` tool to consolidate learnings:

```
Task(subagent_type="llmunix-plugin:DreamEngineAgent", prompt="Run dream consolidation cycle on recent traces in system/memory/traces/")
```

This triggers the 3-phase bio-inspired consolidation:
1. **SWS (Slow Wave Sleep)**: Analyze failures, extract negative constraints
2. **REM Sleep**: Abstract successful patterns into reusable strategies
3. **Consolidation**: Write strategies to disk, update journal, prune old traces

## Memory Architecture

```
system/memory/
├── strategies/               # Long-term memory (persistent knowledge)
│   ├── level_1_epics/        # Project-level patterns
│   ├── level_2_architecture/ # Design & architecture strategies
│   ├── level_3_tactical/     # Module/task-level tactics
│   ├── level_4_reactive/     # Command-level patterns
│   ├── _seeds/               # Bootstrap strategies (never deleted)
│   ├── _negative_constraints.md  # What NOT to do
│   └── _dream_journal.md    # Consolidation history
└── traces/                   # Short-term memory (volatile execution logs)
    └── trace_YYYY-MM-DD.md   # Daily trace files
```

## Strategy Format

Strategies are Markdown files with YAML frontmatter:

```yaml
---
id: strat_[level]_[slug]
version: 1
hierarchy_level: [1-4]
title: [Human-readable title]
trigger_goals: ["keyword1", "keyword2"]
preconditions: ["condition1"]
confidence: 0.5
success_count: 0
failure_count: 0
source_traces: []
deprecated: false
---
```

## Claude Cowork Integration

When running in **Claude Cowork** (background agents):
- The **SystemAgent** (Cortex) handles user-facing orchestration
- The **DreamEngineAgent** (Hippocampus) can run asynchronously in background to consolidate memory while you work
- The **MemoryAnalysisAgent** handles real-time trace logging

## Execution Modes

### Default Multi-Agent Mode (Triad Decomposition)

Every `/llmunix [goal]` invocation **always creates at least 3 agents** along these concern axes:

| Agent Role | Responsibility |
|---|---|
| **Implementation Agent** | Core deliverable (code, config, schema) |
| **Quality Agent** | Validation, testing, review, correctness |
| **Integration Agent** | Environment, docs, deployment, git |

For complex goals, additional agents are created beyond the minimum 3. After execution, **one dream cycle per agent** runs in parallel (minimum 3), each filtered by that agent's keywords.

### Loop Mode (Recurring Dreams)

`/llmunix loop` generates `/loop` commands for the user to copy-paste (the plugin cannot invoke `/loop` directly since it's a built-in CLI command):

- `/llmunix loop` → outputs: `/loop 1h /llmunix dream`
- `/llmunix loop [keywords]` → outputs: `/loop 1h /llmunix dream [keywords]`
- `/llmunix loop [interval] [keywords]` → outputs: `/loop [interval] /llmunix dream [keywords]`
- `/llmunix loop stop` → outputs cancellation instructions

Loop commands are session-scoped (max 3 days) and stop when the session ends or Ctrl+C is pressed.

## Reference Specifications

Detailed specifications for system internals are in `llmunix-plugin/system_files/`:
- **SmartMemory.md**: Memory hierarchy, retrieval algorithm, and context assembly
- **MemoryTraceManager.md**: Trace schema, lifecycle, and hierarchy rules
- **QueryMemoryTool.md**: Strategy scoring algorithm and query patterns
- **ClaudeCodeToolMap.md**: Tool-to-operation mapping and workflow patterns

Consult these when implementing complex memory operations or debugging the consolidation pipeline.

## Important Rules

1. NEVER delete seed strategies in `_seeds/` - they are the bootstrap knowledge
2. ALWAYS log traces for tasks that take more than 2 tool calls
3. PREFER applying existing strategies over improvising from scratch
4. UPDATE strategy confidence after each application (success: +0.05, failure: -0.1)
5. DEPRECATE strategies where `failure_count > success_count * 2`
