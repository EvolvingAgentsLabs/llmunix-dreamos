# LLMunix DreamOS Plugin

The kernel plugin for the LLMunix DreamOS cognitive operating system. Provides bio-inspired memory consolidation, hierarchical trace logging, strategy-driven execution, and scheduled dream consolidation for Claude Code Desktop.

## Quick Start

### Install via Claude Code Desktop

1. Open **Claude Desktop** and switch to the **Code** tab
2. Click the **+** button next to the prompt box and select **Plugins**
3. Select **Add plugin** and search for `llmunix-dreamos`
4. Install the plugin

Or ask Claude directly in any Code session:

```
Please install the llmunix-dreamos plugin from EvolvingAgentsLabs globally
```

### Set Up Scheduled Dream Consolidation

After installing the plugin, set up automatic nightly dream consolidation:

1. Click **Schedule** in the sidebar, then **+ New task**
2. Configure the task (see [Scheduled Dream Consolidation](../README.md#scheduled-dream-consolidation) in the main README)

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

2.1.0 — Claude Code Desktop Edition

## Author

[Evolving Agents Labs](https://github.com/EvolvingAgentsLabs)
