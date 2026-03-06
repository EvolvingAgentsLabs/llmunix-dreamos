# Query Memory Tool — DreamOS Edition

Enables searching and retrieving strategies, constraints, and learnings from the DreamOS long-term memory.

## Function Signature

```
QueryMemory(
  query: string,                    # Keywords to search for
  level: 1 | 2 | 3 | 4 | "all",   # Hierarchy level filter
  include_seeds: boolean = true,    # Include seed strategies
  include_deprecated: boolean = false # Include deprecated strategies
)
```

## Implementation (Pure Claude Code Tools)

### Step 1: Find strategy files

```
Glob(pattern: "system/memory/strategies/level_*/**/*.md")
```

If `include_seeds`:
```
Glob(pattern: "system/memory/strategies/_seeds/*.md")
```

### Step 2: Search for keyword matches

```
Grep(
  pattern: "[keyword1|keyword2|keyword3]",
  path: "system/memory/strategies/",
  output_mode: "files_with_matches"
)
```

### Step 3: Read and score matches

For each matching file:
1. `Read` the file
2. Parse YAML frontmatter to extract: `trigger_goals`, `confidence`, `success_count`, `failure_count`, `deprecated`
3. If `deprecated == true` and `include_deprecated == false`: skip
4. Calculate composite score:
   ```
   trigger_score = best_match(query_keywords, trigger_goals)
   success_rate = success_count / max(success_count + failure_count, 1)
   composite = (trigger_score * 0.5) + (confidence * 0.3) + (success_rate * 0.2)
   ```
5. Filter: `composite >= 0.2`

### Step 4: Sort and return

Sort by composite score descending. Return top 5 matches.

## Return Format

```markdown
# Strategy Query Results

## Query: "[original query]"
## Level Filter: [level or "all"]
## Matches Found: [N]

### Match 1: [strategy title] (Score: [composite])
**ID**: [strategy_id]
**Level**: L[1-4]
**Confidence**: [confidence]
**Success Rate**: [success_count]/[total]
**Trigger Goals**: [list]

**Steps**:
1. [step 1]
2. [step 2]

**Constraints**:
- [constraint 1]

---

### Match 2: ...
```

## Constraint Query

To query negative constraints specifically:

```
Read("system/memory/strategies/_negative_constraints.md")
```

Then filter by context keywords matching the current task domain.

## Usage Patterns

### Before Task Planning
```
1. QueryMemory(query="user authentication JWT", level="all")
2. Read constraints
3. Incorporate results into task plan
```

### Before Agent Creation
```
1. QueryMemory(query="[agent's domain]", level=3)
2. Inject matching strategy steps into agent prompt
3. Inject matching constraints into agent prompt
```

### After Task Completion (Pre-Dream)
```
1. QueryMemory(query="[completed task keywords]", level="all")
2. Check if a strategy already exists for this pattern
3. If yes: DreamEngine will merge
4. If no: DreamEngine will create new strategy
```
