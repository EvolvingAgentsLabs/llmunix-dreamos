# LLMunix DreamOS Plugin

The kernel plugin for the LLMunix DreamOS cognitive operating system. Provides bio-inspired memory consolidation, hierarchical trace logging, and strategy-driven execution for Claude Code and Claude Cowork.

## Quick Start

### Claude Code (Terminal)

```bash
# Run with the plugin
claude --plugin-dir /path/to/llmunix-dreamos/llmunix-plugin

# Or install permanently
claude plugin marketplace add evolving-agents-labs/llmunix-dreamos
claude plugin install llmunix-plugin@llmunix-dreamos
```

### Claude Cowork (Desktop)

1. Open Claude Desktop → Cowork tab
2. Select a folder containing the DreamOS `system/` directory
3. The plugin activates automatically when `CLAUDE.md` is detected

## Components

### Agents

| Agent | Role | Biological Analog |
|-------|------|-------------------|
| **SystemAgent** | Executive orchestration, strategy-aware planning, hierarchical task decomposition | Cortex |
| **MemoryAnalysisAgent** | Real-time trace logging, structured event capture, hierarchy level assignment | Hippocampal Encoder |
| **DreamEngineAgent** | 3-phase memory consolidation: SWS (failure analysis), REM (strategy abstraction), Consolidation (persistence) | Hippocampus |

### Commands

| Command | Description |
|---------|-------------|
| `/llmunix [goal]` | Full cognitive pipeline: memory query, hierarchical decomposition, execution with trace logging, dream consolidation |

### System Files

Reference specifications used by the agents:

| File | Purpose |
|------|---------|
| `SmartMemory.md` | Memory architecture: short-term (traces) vs long-term (strategies), section priorities, retrieval algorithm |
| `MemoryTraceManager.md` | Trace schema (hierarchy levels, outcomes, actions), file format, linking patterns, lifecycle |
| `QueryMemoryTool.md` | Strategy retrieval: keyword matching, composite scoring (trigger 50% + confidence 30% + success rate 20%) |
| `ClaudeCodeToolMap.md` | Maps Claude Code tools (Read, Write, Bash, Task, etc.) to DreamOS cognitive operations |

## How It Works

```
User Goal → SystemAgent queries strategies → Decomposes hierarchically
  → Executes with trace logging → DreamEngineAgent consolidates
  → Strategies + constraints written to disk → Better next time
```

## Version

2.0.0 — DreamOS Edition

## Author

[Evolving Agents Labs](https://github.com/evolving-agents-labs)
