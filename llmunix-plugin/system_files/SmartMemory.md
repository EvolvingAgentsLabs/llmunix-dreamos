# Smart Memory — DreamOS Edition

Defines the persistent knowledge layer and memory hierarchy for the DreamOS cognitive architecture. Adapted from RoClaw's section-based memory system with a domain-agnostic adapter for software development.

## Memory Architecture

```
system/memory/
├── strategies/                    # LONG-TERM MEMORY (persistent)
│   ├── level_1_epics/             # Project-level patterns
│   ├── level_2_architecture/      # Design & architecture strategies
│   ├── level_3_tactical/          # Module/task-level tactics
│   ├── level_4_reactive/          # Command-level patterns
│   ├── _seeds/                    # Bootstrap strategies (immutable)
│   ├── _negative_constraints.md   # Anti-patterns & guardrails
│   └── _dream_journal.md         # Consolidation history
└── traces/                        # SHORT-TERM MEMORY (volatile)
    └── trace_YYYY-MM-DD.md        # Daily execution logs
```

## Memory Types

### Short-Term Memory (Traces)

**Nature**: Volatile, ephemeral, raw
**Location**: `system/memory/traces/`
**Lifecycle**: Created during execution → consumed by Dream Engine → pruned after 7 days
**Properties**:
- Granular: Individual execution events with full context
- Chronological: Timestamped for sequence reconstruction
- Hierarchical: Linked via parent-child trace IDs
- Complete: Contains goals, actions, outcomes, confidence scores

### Long-Term Memory (Strategies)

**Nature**: Persistent, abstracted, reusable
**Location**: `system/memory/strategies/`
**Lifecycle**: Created by Dream Engine → queried before tasks → updated with new evidence
**Properties**:
- Abstract: Generalized patterns, not specific instances
- Hierarchical: Organized by cognitive level (L1-L4)
- Scored: Confidence and success/failure counts for reliability
- Queryable: YAML frontmatter with trigger_goals for efficient retrieval
- Evolvable: Version-tracked, merged with new evidence over time

### Seed Memory (Bootstrap Knowledge)

**Nature**: Immutable, foundational
**Location**: `system/memory/strategies/_seeds/`
**Lifecycle**: Created at system initialization → never modified or deleted
**Properties**:
- Provides cold-start knowledge for common software development patterns
- Low initial confidence (0.5) — must be validated through use
- Serve as templates for Dream Engine to create evolved strategies

## Memory Operations

| Operation | Agent | Input | Output |
|-----------|-------|-------|--------|
| **Write Trace** | MemoryAnalysisAgent | Execution event | Trace entry in daily file |
| **Read Traces** | DreamEngineAgent | Time window | Parsed trace sequences |
| **Consolidate** | DreamEngineAgent | Trace sequences | Strategies + constraints |
| **Query Strategy** | SystemAgent, Any | Goal keywords | Matching strategies sorted by score |
| **Apply Strategy** | Any executing agent | Strategy steps | Guided execution |
| **Update Strategy** | DreamEngineAgent | New evidence | Updated version, confidence |
| **Read Constraints** | SystemAgent, Any | Context | Applicable anti-patterns |
| **Write Constraint** | DreamEngineAgent | Failure analysis | New constraint entry |

## Strategy Retrieval Algorithm

When searching for strategies (adapted from RoClaw's composite scoring):

```
1. Extract keywords from the goal
2. For each strategy in level_*/:
   a. Match trigger_goals against keywords
      - Exact match: trigger_score = 1.0
      - Substring match: trigger_score = 0.7
      - Single word overlap: trigger_score = 0.4
      - No match: skip
   b. Calculate composite score:
      composite = (trigger_score * 0.5) + (confidence * 0.3) + (success_rate * 0.2)
      where success_rate = success_count / (success_count + failure_count)
   c. Filter: composite >= 0.2
3. Sort by composite score descending
4. Return top 5 matches
```

## Context Assembly

Before executing a task, the system assembles context from memory (inspired by RoClaw's `getFullContext()`):

```
Full Context Assembly:
├── [Priority 10] Negative Constraints (always loaded)
├── [Priority 20] Matched Strategies (loaded if query matches)
├── [Priority 30] Recent Dream Journal (last 3 entries)
├── [Priority 40] Project-Specific Learnings (if in project context)
└── [Priority 50] Seed Strategies (fallback if no learned strategies)
```

## Memory Section Registration

Each memory section is defined by:

```yaml
name: string        # Section identifier
heading: string     # Display heading (e.g., "## Negative Constraints")
source: string      # File path or glob pattern
priority: number    # Lower = loaded first (10, 20, 30...)
required: boolean   # Whether this section must be present
```

### Default Sections for Software Development

| Section | Priority | Source | Required |
|---------|----------|--------|----------|
| Constraints | 10 | `strategies/_negative_constraints.md` | Yes |
| Active Strategies | 20 | `strategies/level_*/` (query-matched) | No |
| Dream Journal | 30 | `strategies/_dream_journal.md` | No |
| Seed Strategies | 50 | `strategies/_seeds/` | No |

## Memory Hygiene

1. **Trace Pruning**: Traces older than 7 days are deleted during dream consolidation
2. **Strategy Deprecation**: Strategies with `failure_count > success_count * 2` (min 3 attempts) are deprecated
3. **Constraint Deduplication**: Similar constraints are merged, not duplicated
4. **Journal Trimming**: Dream journal entries older than 30 days can be summarized into a single "epoch summary"
5. **Seed Preservation**: Files in `_seeds/` are NEVER modified or deleted
