# LLMunix DreamOS

**A Bio-Inspired Cognitive Operating System for Claude Code & Claude Cowork**

> *"What if your AI assistant remembered what worked, learned from what failed, and got smarter every time you used it?"*

DreamOS transforms Claude from a **stateless assistant that forgets everything between sessions** into a **learning system** that accumulates strategies, avoids past mistakes, and improves with every interaction. It brings the algorithmic rigor of hierarchical memory consolidation — proven in physical robotics — to the pure-markdown plugin ecosystem of Claude Code and Claude Cowork.

---

## Table of Contents

- [The Problem](#the-problem)
- [The Solution](#the-solution-dreamos)
- [What DreamOS Adds to Claude Code](#what-dreamos-adds-to-claude-code)
- [What DreamOS Adds to Claude Cowork](#what-dreamos-adds-to-claude-cowork)
- [Architecture Overview](#architecture-overview)
- [Installation](#installation)
  - [Claude Code (Terminal)](#option-1-claude-code-terminal)
  - [Claude Cowork (Desktop)](#option-2-claude-cowork-desktop)
- [Tutorial: Using DreamOS with Claude Code](#tutorial-using-dreamos-with-claude-code)
- [Tutorial: Using DreamOS with Claude Cowork](#tutorial-using-dreamos-with-claude-cowork)
- [Directory Structure](#directory-structure)
- [How the Dream Engine Works](#how-the-dream-engine-works)
- [Seed Strategies](#seed-strategies)
- [Strategy Format Reference](#strategy-format-reference)
- [Provenance](#provenance)
- [License](#license)

---

## The Problem

AI coding assistants today suffer from **cognitive amnesia**. Every time you start a new session with Claude Code or Claude Cowork, the slate is wiped clean:

**1. No memory of what worked.** You solved a tricky React hydration bug last week using a specific pattern. Today, facing the same issue, Claude starts from scratch — suggesting the same wrong approaches you already tried and rejected.

**2. No memory of what failed.** Last month, Claude tried to `npm install --force` and broke your lock file. Today, facing a similar dependency conflict, it suggests the exact same destructive approach.

**3. No accumulation of expertise.** You've been building Express APIs for months. Claude has helped you create 15 endpoints. But it still doesn't "know" your project's patterns — it doesn't remember that you always use Zod for validation, that your error handler expects a specific format, or that your team uses a particular middleware ordering.

**4. No strategic planning.** Claude treats every task as if it's the first one. It doesn't decompose complex goals into hierarchical sub-tasks, doesn't track which sub-tasks depend on others, and doesn't learn from the outcomes of multi-step plans.

**5. Repeated mistakes across sessions.** Without persistent memory, the same anti-patterns reappear. The same debugging rabbit holes get explored. The same time gets wasted.

This problem exists because **Claude Code and Claude Cowork are stateless by design**. Their built-in memory (`CLAUDE.md`, auto-memory) captures basic preferences but lacks the structure to store *strategies*, *anti-patterns*, *confidence scores*, or *hierarchical task decompositions*. There is no mechanism to analyze past sessions, extract what worked, discard what didn't, and synthesize reusable knowledge.

### The Cost of Forgetting

| Scenario | Without DreamOS | With DreamOS |
|----------|----------------|--------------|
| Setting up a new Express API | Claude guesses your conventions every time | Claude applies `strat_3_api-endpoint` with your team's patterns |
| Hitting an npm ERESOLVE error | Claude suggests `--force` (again) | `_negative_constraints.md` blocks the destructive approach; `seed_3_npm-troubleshooting` provides the proven fix |
| Debugging a failing test | Claude tries random changes | `seed_4_debug-strategy` enforces systematic isolation |
| Second week on a project | Same quality as day one | Strategies evolved from 10+ successful traces guide better plans |

---

## The Solution: DreamOS

DreamOS is a **Claude Code / Claude Cowork plugin** that implements a biologically-inspired cognitive architecture. It is based on three innovations, originally proven in [RoClaw](https://github.com/EvolvingAgentsLabs/RoClaw) — a physical robot that learns to navigate rooms by consolidating motor traces into reusable strategies during "sleep" phases.

### 1. Hierarchical Task Decomposition (L1-L4)

Every task is decomposed into a 4-level cognitive hierarchy, mirroring neocortical organization:

| Level | Name | Software Equivalent | Example |
|-------|------|---------------------|---------|
| **L1** | GOAL | Epic / Project | "Build authentication system" |
| **L2** | ARCHITECTURE | Strategy / Design | "Implement JWT with refresh tokens" |
| **L3** | TACTICAL | Module / Task | "Write middleware for token validation" |
| **L4** | REACTIVE | Command / Tool Call | `npm install jsonwebtoken` |

Traces at each level link to their parent, forming a tree that the Dream Engine can analyze for patterns.

### 2. Execution Traces (Short-Term Memory)

Every significant action is logged as a structured trace with: hierarchy level, parent linkage, goal, applied strategy, outcome (SUCCESS/FAILURE/PARTIAL), confidence score, and action details. These traces are the raw material for learning.

### 3. Dream Engine (3-Phase Memory Consolidation)

After task completion, the Dream Engine runs a bio-inspired consolidation cycle:

| Phase | Biological Analog | What It Does |
|-------|-------------------|-------------|
| **Phase 1: SWS** | Slow Wave Sleep | Replays failures. Extracts **negative constraints** — things to NEVER do again. |
| **Phase 2: REM** | REM Sleep | Abstracts successful patterns into **reusable strategies** with confidence scores. Merges new evidence into existing strategies. |
| **Phase 3: Consolidation** | Memory Writing | Writes strategies and constraints to disk. Updates the dream journal. Prunes old traces. |

---

## What DreamOS Adds to Claude Code

Claude Code is a powerful terminal-based AI coding tool. DreamOS extends it with:

| Capability | Claude Code Alone | Claude Code + DreamOS |
|-----------|-------------------|----------------------|
| **Memory** | `CLAUDE.md` + auto-memory (preferences only) | Hierarchical strategies with confidence scores, negative constraints, dream journal |
| **Learning** | None between sessions | Dream Engine extracts strategies from successes and constraints from failures |
| **Planning** | Ad-hoc, flat task decomposition | 4-level hierarchical decomposition (L1-L4) with strategy injection |
| **Error prevention** | None | Negative constraints loaded before every task; high-severity constraints always enforced |
| **Task logging** | No structured logging | Hierarchical traces with parent-child linking, outcomes, and confidence |
| **Knowledge reuse** | Manual copy-paste | Automatic strategy matching via trigger keywords and composite scoring |
| **Sub-agent coordination** | Built-in Task tool | Strategy-aware agents that query memory before acting |

### Concrete Example: What Changes

**Without DreamOS** — You ask Claude Code to "add user registration endpoint":
1. Claude improvises a plan from scratch
2. May or may not follow your project conventions
3. If it fails, the failure is forgotten
4. Next time, it starts over

**With DreamOS** — Same request:
1. Claude checks `_negative_constraints.md` — avoids known anti-patterns
2. Claude searches strategies — finds `seed_3_api-endpoint` (REST API creation)
3. Claude follows the strategy steps, adapted to your project
4. Actions are logged as L3 traces with parent linking
5. After completion, Dream Engine consolidates the experience
6. Next time: higher-confidence strategy, even better execution

---

## What DreamOS Adds to Claude Cowork

Claude Cowork is Anthropic's desktop application that brings Claude Code's agentic capabilities to knowledge work beyond coding. It can read/write local files, coordinate sub-agents, and run long tasks autonomously. DreamOS supercharges it with:

| Capability | Cowork Alone | Cowork + DreamOS |
|-----------|-------------|-----------------|
| **Session memory** | No memory across sessions | Strategies, constraints, and journal persist on disk |
| **Background learning** | None | DreamEngineAgent consolidates while you work |
| **Scheduled consolidation** | Not possible | Schedule `/llmunix dream` to run nightly via Cowork's scheduled tasks |
| **Skill evolution** | Static skills from plugins | Strategies evolve with every task — confidence scores update, new patterns emerge |
| **Team knowledge** | Each user starts from zero | Shared `system/memory/strategies/` directory = team-wide accumulated knowledge |
| **Failure prevention** | None | Negative constraints prevent repeating costly mistakes |

### The Cowork Advantage: Asynchronous Dreaming

Cowork's killer feature for DreamOS is **background execution**. While you code:

```
You (working) ──────────────────────────────────────────────>
                    │                          │
                    ▼                          ▼
         DreamEngine (background)    DreamEngine (background)
         Consolidates morning        Consolidates afternoon
         traces into strategies      traces into strategies
```

The DreamEngineAgent runs as a background Cowork task, continuously processing your traces and updating strategies — without interrupting your workflow.

---

## Architecture Overview

DreamOS adapts the `llmunix-core` Dream Engine from RoClaw (a physical robot) to software development:

| RoClaw (Robotics) | DreamOS (Software) |
|---|---|
| Execution traces (bytecodes) | Session logs (tool calls, bash, file ops) |
| Motor hierarchy (L1 Goal → L4 Motor) | Task hierarchy (L1 Epic → L4 Command) |
| Negative constraints (collision warnings) | Anti-patterns (don't overwrite files, don't skip tests) |
| Bytecode (6-byte binary) | Claude Code tool calls |
| Semantic Map (topological room graph) | Codebase map (file dependencies) |
| Strategy (doorway approach pattern) | Strategy (TDD workflow, API creation pattern) |
| Cortex (goal planner) | SystemAgent |
| Cerebellum (motor control) | Executing sub-agents |
| Hippocampus (memory consolidation) | DreamEngineAgent |

### Three Core Agents

| Agent | Biological Role | Function |
|-------|----------------|----------|
| **SystemAgent** | Cortex | Executive orchestrator — decomposes goals, queries strategies, delegates to specialized agents |
| **MemoryAnalysisAgent** | Hippocampal Encoder | Real-time trace logger — captures execution events as structured hierarchical traces |
| **DreamEngineAgent** | Hippocampus | Memory consolidator — runs the 3-phase SWS/REM/Consolidation cycle |

### The Dream Cycle

```
Execution (awake)          Dream (consolidation)         Next Execution (smarter)
┌──────────────┐          ┌──────────────────────┐       ┌──────────────────┐
│ User tasks   │  traces  │ Phase 1: SWS         │       │ Load strategies  │
│ Tool calls   │ ──────>  │  - Analyze failures  │       │ Load constraints │
│ Outcomes     │          │  - Extract anti-pats  │       │ Apply to planning│
│ Successes    │          │                      │       │ Better execution │
│ Failures     │          │ Phase 2: REM         │       │ Fewer mistakes   │
└──────────────┘          │  - Abstract patterns │       └──────────────────┘
                          │  - Create strategies │              ↑
                          │  - Merge evidence    │              │
                          │                      │        strategies
                          │ Phase 3: Consolidate │        constraints
                          │  - Write to disk     │ ───────────┘
                          │  - Update journal    │
                          │  - Prune old traces  │
                          └──────────────────────┘
```

---

## Installation

### Prerequisites

- **Claude Pro, Max, Team, or Enterprise** subscription
- **Claude Code** (for terminal usage): Install with `curl -fsSL https://claude.ai/install.sh | bash`
- **Claude Desktop** (for Cowork usage): Download from [claude.com/download](https://claude.com/download)

### Option 1: Claude Code (Terminal)

#### Method A: Install as a Plugin (Recommended)

```bash
# 1. Clone the repository
git clone https://github.com/EvolvingAgentsLabs/llmunix-dreamos.git

# 2. Start Claude Code with the plugin
cd your-project
claude --plugin-dir /path/to/llmunix-dreamos/llmunix-plugin
```

To install permanently (available in all sessions):

```bash
# Add the marketplace
claude plugin marketplace add evolving-agents-labs/llmunix-dreamos

# Install the plugin
claude plugin install llmunix-plugin@llmunix-dreamos
```

Or install at project scope (shared with your team via git):

```bash
claude plugin install llmunix-plugin@llmunix-dreamos --scope project
```

#### Method B: Manual Setup (Standalone Project)

If you want DreamOS as the foundation for a new project:

```bash
# 1. Clone it as your project root
git clone https://github.com/EvolvingAgentsLabs/llmunix-dreamos.git my-project
cd my-project

# 2. Start Claude Code — CLAUDE.md loads automatically
claude
```

The `CLAUDE.md` at the root instructs Claude to use the DreamOS cognitive architecture. The agents in `.claude/agents/` are auto-discovered by Claude Code.

#### Method C: Add to an Existing Project

```bash
# 1. Copy the system memory into your project
cp -r /path/to/llmunix-dreamos/system ./system
cp -r /path/to/llmunix-dreamos/.claude ./

# 2. Append DreamOS instructions to your existing CLAUDE.md
cat /path/to/llmunix-dreamos/CLAUDE.md >> ./CLAUDE.md

# 3. Start Claude Code
claude
```

### Option 2: Claude Cowork (Desktop)

#### Step 1: Prepare the Plugin Directory

```bash
# Clone the repository
git clone https://github.com/EvolvingAgentsLabs/llmunix-dreamos.git
```

#### Step 2: Open in Cowork

1. Open **Claude Desktop**
2. Switch to the **Cowork** tab
3. Click **"Work in a folder"** and select the `llmunix-dreamos` directory (or your project that contains the DreamOS files)

#### Step 3: Configure Global Instructions (Optional)

1. Go to **Settings > Cowork**
2. Click **"Edit"** next to **Global instructions**
3. Add:
   ```
   Before starting any complex task, check system/memory/strategies/_negative_constraints.md
   for anti-patterns to avoid. Search system/memory/strategies/ for relevant strategies.
   After completing complex tasks, run the DreamEngineAgent for memory consolidation.
   ```
4. Click **"Save"**

#### Step 4: Install as a Cowork Plugin

1. In the Cowork sidebar, click **"Customize"**
2. Click **"Browse plugins"** or upload the plugin ZIP
3. Alternatively, point Cowork to the `llmunix-plugin/` directory

### Verify Installation

After installation, test that DreamOS is active:

```
# In Claude Code terminal:
What strategies do you have loaded? Check system/memory/strategies/_seeds/

# Or use the /llmunix command:
/llmunix List all available seed strategies
```

Claude should read the seed strategies and report 6 bootstrap strategies.

---

## Tutorial: Using DreamOS with Claude Code

### Level 1: Passive Mode (Zero Effort)

Once DreamOS is installed, it works **automatically** through `CLAUDE.md` instructions. You don't need to learn any new commands.

**What happens behind the scenes:**

1. You ask Claude to do something: *"Add a login endpoint to the API"*
2. Claude reads `CLAUDE.md` → knows to check strategies first
3. Claude searches `system/memory/strategies/` → finds `seed_3_api-endpoint`
4. Claude follows the strategy steps while adapting to your project
5. Claude logs traces to `system/memory/traces/trace_2026-03-06.md`
6. After completion, Claude invokes the DreamEngineAgent to consolidate

**You'll notice:**
- Claude mentions checking constraints before acting
- Claude references strategy IDs when it finds a match
- Trace files appear in `system/memory/traces/`
- Over time, new `.md` files appear in `system/memory/strategies/level_*/`

### Level 2: Active Mode — Using `/llmunix`

The `/llmunix` command triggers the full cognitive pipeline with explicit orchestration.

#### Example 1: Simple Task

```
/llmunix Fix the login form validation bug in src/components/LoginForm.tsx
```

**What DreamOS does:**
1. Queries strategies → finds `seed_4_debug-strategy` (Systematic Debugging)
2. Checks constraints → loads 3 seed constraints
3. Follows the debug strategy: read error → reproduce → locate → hypothesize → verify → fix → test
4. Logs an L3 TACTICAL trace with actions and outcome
5. Runs dream consolidation → may create a new constraint if the bug was caused by an anti-pattern

#### Example 2: Complex Multi-Step Project

```
/llmunix Build a complete REST API with user authentication, using Express, JWT, and PostgreSQL
```

**What DreamOS does:**
1. Creates an L1 GOAL trace: "Build REST API with auth"
2. Decomposes into L2 sub-tasks:
   - Design database schema (L2 ARCHITECTURE)
   - Set up Express project structure (L2 ARCHITECTURE)
   - Implement user model (L3 TACTICAL)
   - Implement auth endpoints (L3 TACTICAL)
   - Add JWT middleware (L3 TACTICAL)
   - Write tests (L3 TACTICAL)
3. For each sub-task:
   - Queries matching strategies
   - Creates specialized agents if needed
   - Executes with trace logging
   - Links child traces to parent
4. After completion, runs full dream consolidation
5. Reports: strategies applied, new strategies learned, constraints discovered

#### Example 3: Triggering Manual Dream Consolidation

After a long coding session where you didn't use `/llmunix` but traces accumulated:

```
Run the DreamEngineAgent to consolidate today's traces
```

Or more explicitly:

```
Invoke the DreamEngineAgent sub-agent. Tell it to process all traces in
system/memory/traces/ since the last dream journal entry.
```

### Level 3: Power User — Inspecting and Managing Memory

#### View your strategies:

```
Show me all strategies in system/memory/strategies/ with their confidence scores
```

#### View constraints:

```
Read system/memory/strategies/_negative_constraints.md and summarize the high-severity ones
```

#### View the dream journal:

```
Read system/memory/strategies/_dream_journal.md — what did the system learn recently?
```

#### View today's traces:

```
Read system/memory/traces/trace_2026-03-06.md and summarize the outcomes
```

#### Manually create a strategy:

If you know a pattern is valuable but don't want to wait for the Dream Engine:

```
Create a new L3 strategy in system/memory/strategies/level_3_tactical/ for
"React component testing with React Testing Library". Include steps for
rendering, querying, asserting, and snapshot testing.
```

#### Deprecate a bad strategy:

```
Update the strategy in system/memory/strategies/level_3_tactical/some-strategy.md
to set deprecated: true in the frontmatter. It keeps suggesting the wrong approach.
```

### Level 4: Team Usage with Git

DreamOS strategies are just Markdown files. Share them with your team:

```bash
# Add strategies to version control
git add system/memory/strategies/
git commit -m "Add DreamOS strategies from sprint 12"
git push

# Team members get accumulated knowledge when they pull
git pull  # Now everyone has the team's learned strategies
```

The `system/memory/traces/` directory is gitignored by default (traces are volatile, personal session logs).

---

## Tutorial: Using DreamOS with Claude Cowork

Cowork runs on the Claude Desktop app and excels at long-running, file-centric tasks. DreamOS leverages Cowork's unique capabilities for **asynchronous dreaming** and **scheduled consolidation**.

### Getting Started in Cowork

#### Step 1: Start a DreamOS-Powered Task

1. Open Claude Desktop → switch to **Cowork** tab
2. Select your project folder (containing the DreamOS `system/` directory)
3. Describe your task:

```
Using the DreamOS system in this project: Build a data pipeline that reads
CSV files from the input/ folder, validates the data, transforms it, and
writes cleaned output to output/. Check strategies and constraints first.
```

4. Claude will:
   - Read `CLAUDE.md` and recognize DreamOS
   - Check `system/memory/strategies/_negative_constraints.md`
   - Search for matching strategies
   - Create a hierarchical plan with L1/L2/L3 decomposition
   - Execute while logging traces
   - Run dream consolidation when done

#### Step 2: Monitor Progress

While Claude works:
- The **sidebar** shows real-time progress across sub-agents
- Traces accumulate in `system/memory/traces/`
- You can jump in to redirect: *"Don't use pandas, use polars instead"*
- Claude logs the course correction as a trace (potentially becoming a future constraint)

#### Step 3: Review Consolidated Results

After the task completes:
- Check `system/memory/strategies/_dream_journal.md` for what was learned
- Check `system/memory/strategies/level_*/` for any new strategies
- Check `system/memory/strategies/_negative_constraints.md` for new anti-patterns

### Advanced Cowork Usage: Background Dreaming

Cowork's most powerful feature for DreamOS is running the DreamEngineAgent in the background while you continue working.

#### Method 1: Explicit Background Consolidation

While working on a task, ask Claude:

```
In the background, run the DreamEngineAgent to consolidate all traces from today.
Don't interrupt my current task — just process the traces and update strategies.
```

Claude spawns a background sub-agent that:
1. Reads traces from `system/memory/traces/`
2. Runs SWS → REM → Consolidation
3. Writes new strategies and constraints
4. You see the results next time you start a task

#### Method 2: Scheduled Dream Cycles

Use Cowork's scheduled tasks for automatic nightly consolidation:

1. In Cowork, type `/schedule`
2. Describe the task:
   ```
   Run the DreamOS dream consolidation cycle. Read all traces in
   system/memory/traces/ newer than the last entry in
   system/memory/strategies/_dream_journal.md. Execute the 3-phase
   Dream Engine: analyze failures for constraints, abstract successes
   into strategies, write results and prune old traces.
   ```
3. Set cadence: **Daily** (or **Weekdays** for work projects)
4. Select the project folder
5. Click **"Schedule"**

Now, every night while your computer is on and Claude Desktop is open, DreamOS automatically consolidates your day's work into strategies.

#### Method 3: End-of-Session Dreaming

Create a habit: before closing Claude Desktop, run:

```
Dream time! Consolidate everything from today's session.
```

### Cowork Plugin: Using Slash Commands

Once the plugin is installed, use these commands in Cowork:

| Command | What It Does |
|---------|-------------|
| `/llmunix [goal]` | Full cognitive pipeline — strategy query, decomposition, execution, dreaming |
| `/llmunix dream` | Run dream consolidation on recent traces |
| `/llmunix status` | Show current memory stats (strategies count, constraints, last dream date) |

### Cowork Team Workflow

For teams using Claude Cowork with DreamOS:

1. **Shared strategies directory**: Keep `system/memory/strategies/` in a shared repository or synced folder
2. **Individual traces**: Each team member's `system/memory/traces/` stays local
3. **Merged dreams**: After each team member's dream consolidation, review and commit new strategies to the shared repo
4. **Growing team brain**: Over weeks, the team accumulates a rich strategy library that new members immediately benefit from

### Example: Full Cowork Session

Here's what a productive DreamOS + Cowork session looks like:

```
Morning:
1. Open Claude Desktop → Cowork tab → select project folder
2. Claude loads CLAUDE.md → sees DreamOS → checks last dream journal entry
3. You: "Build the user settings page with profile editing and password change"
4. Claude queries strategies → finds seed_3_api-endpoint and seed_2_git-workflow
5. Claude decomposes into L1 goal → L2 architecture → L3 tactical tasks
6. Claude creates feature branch (applying git-workflow strategy)
7. Claude builds the feature, logging traces at each level
8. Task complete → DreamEngineAgent runs consolidation
9. New strategy created: "strat_3_settings-page" (confidence: 0.5)

Afternoon:
10. You: "Now add email notification when password changes"
11. Claude queries strategies → finds the NEW "strat_3_settings-page"
12. Claude also loads constraints → avoids a mistake from the morning session
13. Builds on the morning's work with accumulated knowledge
14. Another dream cycle → strategy updated to version 2, confidence: 0.55

Next week:
15. Similar task on another project → "strat_3_settings-page" transfers over
16. Confidence now 0.7 after multiple successes → Claude follows it closely
```

---

## Directory Structure

```
llmunix-dreamos/
├── .claude/
│   ├── agents/                       # Agent definitions for Claude Code
│   │   ├── SystemAgent.md            # Cortex: orchestration & planning
│   │   ├── MemoryAnalysisAgent.md    # Encoder: trace logging
│   │   └── DreamEngineAgent.md       # Hippocampus: 3-phase consolidation
│   └── settings.local.json           # Permission configuration
├── .claude-plugin/
│   └── marketplace.json              # Marketplace config (for distribution)
├── llmunix-plugin/                   # Plugin package
│   ├── .claude-plugin/
│   │   └── plugin.json               # Plugin manifest (v2.0.0)
│   ├── agents/                       # Same 3 agents (plugin scope)
│   │   ├── SystemAgent.md
│   │   ├── MemoryAnalysisAgent.md
│   │   └── DreamEngineAgent.md
│   ├── commands/
│   │   └── llmunix.md                # /llmunix kernel command
│   └── system_files/                 # Reference specifications
│       ├── ClaudeCodeToolMap.md       # Tool → operation mapping
│       ├── MemoryTraceManager.md      # Trace schema & lifecycle
│       ├── QueryMemoryTool.md         # Strategy retrieval algorithm
│       └── SmartMemory.md             # Memory architecture spec
├── system/
│   └── memory/
│       ├── strategies/                # Long-term memory (persistent)
│       │   ├── level_1_epics/         # Project-level patterns
│       │   ├── level_2_architecture/  # Design strategies
│       │   ├── level_3_tactical/      # Module-level tactics
│       │   ├── level_4_reactive/      # Command-level patterns
│       │   ├── _seeds/                # Bootstrap strategies (6 seeds)
│       │   ├── _negative_constraints.md  # Anti-patterns (what NOT to do)
│       │   └── _dream_journal.md      # Consolidation history
│       └── traces/                    # Short-term memory (volatile, gitignored)
│           └── trace_YYYY-MM-DD.md    # Daily trace files
├── CLAUDE.md                          # Kernel instructions
├── .gitignore
├── LICENSE                            # Apache 2.0
└── README.md                          # This file
```

---

## How the Dream Engine Works

The DreamEngineAgent executes three sequential phases, inspired by how biological brains consolidate memories during sleep.

### Phase 1: Slow Wave Sleep (SWS) — Failure Analysis

**Input**: Trace sequences with `outcome = FAILURE` or `outcome = PARTIAL` (score < 0.3)

**Process**:
1. Parse all traces newer than the last dream timestamp
2. Group traces into sequences by parent-child relationships
3. Score each sequence: `(confidence * 0.4) + (outcome_score * 0.4) + (recency * 0.2)`
4. For each failure sequence:
   - What was the goal?
   - What strategy was applied (if any)?
   - What specific actions caused the failure?
   - Was there a pattern of repeated retries?
5. Extract negative constraints with severity ratings

**Output**: New entries for `_negative_constraints.md`

**Example**: A task to update a Next.js version failed because peer dependencies broke:
```markdown
### Never update Next.js major version without checking peer dependencies first
**Context:** Next.js, dependency management
**Severity:** high
**Learned from:** tr_1709736622000_a3f2
```

### Phase 2: REM Sleep — Strategy Abstraction

**Input**: Trace sequences with `outcome = SUCCESS` or high-scoring `PARTIAL`

**Process**:
1. Group successful sequences by hierarchy level (L1-L4)
2. For each sequence:
   - Compress the action list (run-length encoding for repetitive patterns)
   - Search existing strategies for trigger keyword overlap (>50%)
   - **If match exists**: Merge new evidence into the strategy (`version++`, `confidence += 0.05`)
   - **If no match**: Create a new strategy with `confidence: 0.5`
3. Deprecation check: if any strategy has `failure_count > success_count * 2` (minimum 3 attempts), mark as `deprecated: true`

**Output**: New and updated strategy files in `system/memory/strategies/level_*/`

### Phase 3: Consolidation — Persistence

**Process**:
1. Write all new/updated strategies to disk
2. Write new constraints to `_negative_constraints.md`
3. Append summary to `_dream_journal.md`
4. Prune trace files older than 7 days

**Example journal entry**:
```markdown
## 2026-03-06T22:00:00.000Z
- Traces processed: 12
- Sequences analyzed: 5
- Strategies created: 1
- Strategies updated: 2
- Constraints learned: 1
- Traces pruned: 3

Learned a new pattern for React form validation with Zod schemas.
Updated the API endpoint strategy with improved error handling.
Added constraint: never skip TypeScript strict mode on new projects.
```

### Strategy Scoring Algorithm

When searching for strategies to apply, DreamOS uses composite scoring (adapted from RoClaw's `StrategyStore`):

```
composite = (trigger_match * 0.5) + (confidence * 0.3) + (success_rate * 0.2)
```

| Factor | Weight | Calculation |
|--------|--------|-------------|
| Trigger Match | 50% | Exact keyword match (1.0), substring (0.7), single word overlap (0.4) |
| Confidence | 30% | Strategy's confidence field (0.0-0.95) |
| Success Rate | 20% | `success_count / (success_count + failure_count)` |

Strategies with composite score < 0.2 are filtered out. Top 3 matches are returned.

---

## Seed Strategies

DreamOS ships with 6 bootstrap strategies to solve the cold-start problem (no prior execution history):

| ID | Level | Title | Trigger Keywords |
|----|-------|-------|-----------------|
| `seed_1_project-setup` | L1 | New Project Setup Pattern | new project, initialize, scaffold |
| `seed_2_git-workflow` | L2 | Git Branch Workflow | git, branching, pull request, merge |
| `seed_3_npm-troubleshooting` | L3 | NPM Dependency Resolution | npm install error, ERESOLVE, peer dependency |
| `seed_3_tdd-workflow` | L3 | Test-Driven Development | write tests, TDD, unit test |
| `seed_3_api-endpoint` | L3 | REST API Endpoint Creation | create endpoint, API route, backend |
| `seed_4_debug-strategy` | L4 | Systematic Debugging | debug, fix bug, error, not working |

Seeds start with `confidence: 0.5`. They evolve through use — the Dream Engine updates their confidence and adds new steps as it processes traces. Seeds are **never deleted** (stored in `_seeds/`), but evolved versions are written to the `level_*/` directories.

---

## Strategy Format Reference

Strategies are Markdown files with YAML frontmatter:

```yaml
---
id: strat_3_express-middleware      # Unique ID: strat_[level]_[slug]
version: 2                          # Incremented by Dream Engine on merge
hierarchy_level: 3                  # 1=GOAL, 2=ARCH, 3=TACTICAL, 4=REACTIVE
title: Express Middleware Pattern    # Human-readable name
trigger_goals:                      # Keywords for matching (3-5 recommended)
  - middleware
  - express
  - request pipeline
preconditions:                      # What must be true to apply
  - Express.js project
confidence: 0.78                    # 0.0-0.95, updated by Dream Engine
success_count: 5                    # Times applied successfully
failure_count: 1                    # Times applied and failed
source_traces:                      # Trace IDs that contributed
  - tr_1709736622000_a3f2
  - tr_1709822000000_b4c1
deprecated: false                   # true = no longer recommended
---

# Express Middleware Pattern

## Steps
1. Create middleware file in src/middleware/
2. Export a function with (req, res, next) signature
3. Implement the middleware logic
4. Add error handling with next(error)
5. Register in app.ts/app.js in correct order
6. Write tests with supertest

## Negative Constraints
- Never call next() after sending a response
- Never modify req.body without cloning first

## Notes
- Middleware order matters: auth before validation before business logic
- Use express-async-errors to catch async middleware failures
```

---

## Provenance

DreamOS stands on the shoulders of two projects:

- **[RoClaw](https://github.com/EvolvingAgentsLabs/RoClaw)**: The hierarchical memory architecture, Dream Engine 3-phase algorithm, strategy scoring with composite weights, negative constraints with severity levels, trace hierarchy (L1-L4), and the core+adapter pattern. Originally built for a physical robot that learns to navigate rooms.

- **[LLMunix Marketplace](https://github.com/EvolvingAgentsLabs/llmunix-marketplace)**: The pure-markdown plugin architecture, agent definitions with YAML frontmatter, the `/llmunix` kernel command, SmartMemory system, and the philosophy that everything should be inspectable markdown — no binary dependencies, no databases.

---

## License

Apache License 2.0 — See [LICENSE](LICENSE)

Built by [Evolving Agents Labs](https://github.com/EvolvingAgentsLabs)
