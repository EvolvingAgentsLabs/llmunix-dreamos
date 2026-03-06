# Memory Trace Manager — DreamOS Edition

Coordinates the lifecycle of hierarchical memory traces within the DreamOS cognitive architecture. This is the specification that all agents must follow when creating, linking, and managing traces.

## Trace Schema

Every trace entry MUST contain these fields:

```yaml
trace_id: string          # Unique: tr_[unix_ms]_[4hex]
hierarchy_level: 1-4       # L1=GOAL, L2=ARCHITECTURE, L3=TACTICAL, L4=REACTIVE
parent_trace_id: string?   # null for root traces, parent ID for children
timestamp: string          # ISO 8601 (e.g., 2026-03-06T14:30:22.000Z)
goal: string               # What was being attempted
strategy_id: string?       # ID of applied strategy (null if none)
outcome: enum              # SUCCESS | FAILURE | PARTIAL | ABORTED | UNKNOWN
outcome_reason: string     # Brief explanation of the outcome
duration_ms: number?       # Execution time (null if not measurable)
confidence: number         # 0.0-1.0 estimated quality
actions: ActionEntry[]     # List of actions taken
```

### ActionEntry Schema

```yaml
timestamp: string          # ISO 8601
tool: string               # Tool name (Read, Write, Bash, Task, Grep, etc.)
description: string        # What the action did
result_summary: string     # Brief outcome ("found 3 files", "test passed", "error: ENOENT")
```

## Trace Hierarchy

Traces form a tree structure via `parent_trace_id`:

```
L1 GOAL: "Build auth system"
├── L2 ARCHITECTURE: "Design JWT flow"
│   ├── L3 TACTICAL: "Create token service"
│   │   ├── L4 REACTIVE: Write src/auth/token.ts
│   │   └── L4 REACTIVE: Write tests/auth/token.test.ts
│   └── L3 TACTICAL: "Create middleware"
│       └── L4 REACTIVE: Write src/middleware/auth.ts
└── L2 ARCHITECTURE: "Design user model"
    └── L3 TACTICAL: "Create User schema"
        └── L4 REACTIVE: Write src/models/user.ts
```

## Trace File Format (Markdown)

**Location**: `system/memory/traces/trace_YYYY-MM-DD.md`
**Mode**: Append-only (one file per day)

```markdown
### Time: 2026-03-06T14:30:22.000Z
**Trace ID:** tr_1709736622000_a3f2
**Level:** 3
**Parent:** tr_1709736000000_b1c4
**Goal:** Create JWT token validation middleware
**Strategy:** strat_3_express-middleware
**Outcome:** SUCCESS
**Reason:** Middleware created and tests pass
**Duration:** 45000
**Confidence:** 0.85

**Actions:**
1. [Read: src/auth/token.ts] -> Found existing token service with sign/verify methods
2. [Write: src/middleware/auth.ts] -> Created auth middleware with Bearer token extraction
3. [Write: tests/middleware/auth.test.ts] -> Created 5 test cases
4. [Bash: npm test -- --testPathPattern=auth] -> All 5 tests pass
---
```

## Trace Linking Patterns

### Sequential (Task B follows Task A)
```
Trace A (outcome: SUCCESS) → Trace B (parent: A.id)
```

### Hierarchical (Task B is subtask of Task A)
```
Trace A (L2) → Trace B (L3, parent: A.id)
```

### Dependency (Task B requires output from Task A)
```
Trace A (outcome: SUCCESS, output: file.ts)
Trace B (parent: A.parent, actions include Read: file.ts)
```

## Trace Lifecycle

### Phase 1: Active Execution
- Traces created in real-time during task execution
- Written to daily trace files
- Actions appended as they complete

### Phase 2: Dream Consolidation
- DreamEngineAgent reads traces from `system/memory/traces/`
- Groups into sequences by parent-child relationships
- Analyzes for strategies (successes) and constraints (failures)
- Writes results to `system/memory/strategies/`

### Phase 3: Pruning
- Trace files older than 7 days are deleted by the DreamEngine
- Strategies persist indefinitely in `system/memory/strategies/`
- Dream journal preserves a summary of what was learned

## Outcome Aggregation

When a parent trace has multiple children:

| Children Outcomes | Parent Outcome |
|-------------------|----------------|
| All SUCCESS | SUCCESS |
| All FAILURE | FAILURE |
| Mix (any SUCCESS + any FAILURE) | PARTIAL |
| Any ABORTED | ABORTED |
| All UNKNOWN | UNKNOWN |

## Confidence Calibration Guidelines

| Scenario | Confidence |
|----------|------------|
| Clean execution, zero retries, all tests pass | 0.9-1.0 |
| Completed with 1-2 minor adjustments | 0.7-0.8 |
| Completed after significant debugging | 0.5-0.6 |
| Barely completed, many workarounds | 0.3-0.4 |
| Mostly failed, trivial progress only | 0.1-0.2 |

## Action Compression

For traces with many repetitive actions, compress using run-length notation:

**Instead of**:
```
1. [Read: src/components/Button.tsx] -> Component found
2. [Read: src/components/Input.tsx] -> Component found
3. [Read: src/components/Modal.tsx] -> Component found
...
```

**Write**:
```
1. [Read: scanned 12 components in src/components/] -> All components follow consistent pattern
```

## Tool-to-Action Mapping

| Claude Code Tool | DreamOS Action Category | Trace Level |
|------------------|------------------------|-------------|
| Read, Glob, Grep | Discovery / Analysis | L4 |
| Write, Edit | Creation / Modification | L3-L4 |
| Bash (tests) | Validation | L3-L4 |
| Bash (build/deploy) | Execution | L2-L3 |
| Task (agent delegation) | Orchestration | L1-L2 |
| WebSearch, WebFetch | Research | L2-L3 |
