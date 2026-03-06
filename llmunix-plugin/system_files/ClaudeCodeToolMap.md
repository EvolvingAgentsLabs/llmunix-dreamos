# Claude Code Tool Map — DreamOS Edition

Maps DreamOS cognitive operations to Claude Code's native tool inventory. Updated for the hierarchical trace system and strategy-driven execution model.

## Core Tool Inventory

### File Operations
| Tool | Purpose | Trace Level |
|------|---------|-------------|
| `Read` | Read file contents | L4 |
| `Write` | Create/overwrite files | L3-L4 |
| `Edit` | Precise string replacements | L3-L4 |
| `Glob` | Find files by pattern | L4 |
| `NotebookEdit` | Edit Jupyter notebooks | L3-L4 |

### Search Operations
| Tool | Purpose | Trace Level |
|------|---------|-------------|
| `Grep` | Search file contents by regex | L4 |
| `WebSearch` | Search the web | L2-L3 |
| `WebFetch` | Fetch and process web content | L2-L3 |

### Execution Operations
| Tool | Purpose | Trace Level |
|------|---------|-------------|
| `Bash` | Run shell commands | L3-L4 |
| `Task` | Delegate to sub-agents | L1-L2 |

### LSP Operations
| Tool | Purpose | Trace Level |
|------|---------|-------------|
| `LSP` | Code intelligence (go-to-def, refs, hover) | L4 |

## DreamOS Operation Mappings

### 1. Strategy Query
```
Operation: Find relevant strategies before planning
Tools: Grep → Read
Pattern:
  1. Grep(pattern="keyword", path="system/memory/strategies/", output_mode="files_with_matches")
  2. Read(matching_files) → parse YAML frontmatter → score → rank
```

### 2. Constraint Loading
```
Operation: Load negative constraints before execution
Tools: Read
Pattern:
  1. Read("system/memory/strategies/_negative_constraints.md")
  2. Filter by context relevance to current task
```

### 3. Trace Writing
```
Operation: Log execution trace
Tools: Read → Write (or Edit to append)
Pattern:
  1. Read("system/memory/traces/trace_YYYY-MM-DD.md")  # Check if exists
  2. Write/Edit to append new trace entry
```

### 4. Agent Creation
```
Operation: Create specialized agent for delegation
Tools: Write → Read → Task
Pattern:
  1. Write("projects/[project]/components/agents/[Agent].md", agent_definition)
  2. Read(agent_file)  # Load full definition
  3. Task(subagent_type="general-purpose", prompt=agent_content)
```

### 5. Dream Consolidation
```
Operation: Run 3-phase memory consolidation
Tools: Task
Pattern:
  1. Task(subagent_type="DreamEngineAgent", prompt="Run dream consolidation...")
  # DreamEngineAgent internally uses: Glob, Read, Write, Grep, Bash
```

### 6. Project Initialization
```
Operation: Create project directory structure
Tools: Bash (mkdir)
Pattern:
  1. Bash("mkdir -p projects/[name]/{components/agents,output,memory/{short_term,long_term}}")
```

### 7. Strategy Application
```
Operation: Apply a strategy during execution
Tools: Read
Pattern:
  1. Read the matching strategy file
  2. Extract steps from markdown body
  3. Follow steps in order, creating L3/L4 traces
  4. After completion, update trace with outcome
```

### 8. Strategy Writing (Dream Engine Only)
```
Operation: Write new or updated strategy
Tools: Write
Pattern:
  1. Generate YAML frontmatter + markdown body
  2. Write to system/memory/strategies/level_[N]_[name]/[slug].md
```

## Common Workflow Patterns

### Full Task Execution
```
1. [Grep] Query strategies for relevant patterns
2. [Read] Load negative constraints
3. [Read] Load matching strategy details
4. [Write] Create root trace (L1/L2)
5. [Write/Task] Execute sub-tasks (creating L3/L4 traces)
6. [Write] Update root trace with outcome
7. [Task] Invoke DreamEngineAgent for consolidation
```

### Strategy-Guided Coding
```
1. [Grep] Find strategy matching "build [feature type]"
2. [Read] Load strategy steps
3. For each step:
   a. [Write] Create L3 trace
   b. [Read/Write/Edit/Bash] Execute the step
   c. [Write] Update L3 trace with outcome
4. [Write] Update L2 trace with aggregate outcome
```

### Post-Failure Learning
```
1. [Write] Log failure trace with detailed reason
2. [Task] Invoke DreamEngineAgent
3. DreamEngine Phase 1 (SWS):
   a. [Read] Parse failure traces
   b. [Write] Add new negative constraint
4. DreamEngine Phase 3:
   a. [Write] Update _negative_constraints.md
   b. [Write] Append to _dream_journal.md
```

## Tool Limitations & Workarounds

| Limitation | Workaround |
|-----------|------------|
| `Write` cannot create files in non-existent directories | Use `Bash(mkdir -p)` first |
| `Task` is one-shot (no follow-up) | Include ALL context in the initial prompt |
| `Grep` is single-line by default | Use `multiline: true` for cross-line patterns |
| `Bash` has 2-minute default timeout | Use `timeout` parameter for long operations |
| `Edit` requires unique `old_string` | Include more surrounding context for uniqueness |
