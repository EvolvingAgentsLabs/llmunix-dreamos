# LLMunix DreamOS Plugin

The kernel plugin for the LLMunix DreamOS cognitive operating system. Provides unihemispheric dreaming (parallel, goal-focused dream sessions), bio-inspired memory consolidation, hierarchical trace logging, and strategy-driven execution for Claude Code Desktop.

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

### Recommended: Skip Permissions

DreamOS orchestrates multiple agents, writes traces, and runs parallel dream cycles — all of which trigger frequent permission prompts. For a smooth experience, start Claude Code with the `--dangerously-skip-permissions` flag:

```
claude --dangerously-skip-permissions
```

This allows agents to read, write, and execute without manual approval at each step. Only use this in trusted project directories where you're comfortable with autonomous file operations.

### Start Dreaming

DreamOS supports dolphin-inspired unihemispheric dreaming — dream sessions run in parallel with your work:

```
/llmunix dream                                 # Full sweep
/llmunix dream authentication                  # Goal-focused
/llmunix dream --parallel auth | API | React   # Parallel multi-goal
```

Or set up a scheduled dream task for automatic consolidation. See [Unihemispheric Dreaming](../README.md#unihemispheric-dreaming) in the main README.

## Components

### Agents

| Agent | Role | Biological Analog |
|-------|------|-------------------|
| **SystemAgent** | Executive orchestration, strategy-aware planning, hierarchical task decomposition | Cortex |
| **MemoryAnalysisAgent** | Real-time trace logging, structured event capture, hierarchy level assignment | Hippocampal Encoder |
| **DreamEngineAgent** | Unihemispheric 3-phase memory consolidation: SWS (failure analysis), REM (strategy abstraction), Consolidation (persistence). Supports parallel, goal-focused, and level-focused dreams. | Hippocampus |

### Commands

| Command | Description |
|---------|-------------|
| `/llmunix [goal]` | Full cognitive pipeline: memory query, hierarchical decomposition, execution with trace logging, dream consolidation |
| `/llmunix dream` | Full-sweep dream consolidation on all unprocessed traces |
| `/llmunix dream [keywords]` | Goal-focused dream — only consolidate traces matching keywords |
| `/llmunix dream --parallel [g1] \| [g2] \| [g3]` | Launch parallel dream sessions for multiple goals |
| `/llmunix dream status` | Report memory state: strategies, constraints, unprocessed traces |
| `/llmunix loop` | Generate `/loop` command for recurring full-sweep dreams |
| `/llmunix loop [keywords]` | Generate `/loop` command for goal-focused recurring dreams |
| `/llmunix loop stop` | Output instructions for stopping dream loops |

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
User Goal → SystemAgent queries strategies → Triad Decomposition (min 3 agents)
  → Implementation + Quality + Integration agents execute with trace logging
  → Per-agent dream cycles (min 3, in parallel) → DreamEngineAgent consolidates each
  → Strategies + constraints written to disk → Better next time

Dreaming (runs alongside work):
  /llmunix dream [keywords] → Goal-focused dream in parallel session
  /llmunix loop [interval]  → Generate /loop command for recurring dreams
  /llmunix dream --parallel → Multiple domains simultaneously
```

## Version

3.0.0 — Unihemispheric Dreaming Edition

## Author

[Evolving Agents Labs](https://github.com/EvolvingAgentsLabs)
