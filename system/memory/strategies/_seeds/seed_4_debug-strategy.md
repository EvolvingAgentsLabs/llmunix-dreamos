---
id: seed_4_debug-strategy
version: 1
hierarchy_level: 4
title: Systematic Debugging Strategy
trigger_goals: ["debug", "fix bug", "error", "not working", "broken", "investigate issue"]
preconditions: ["bug or error identified", "can reproduce the issue"]
confidence: 0.5
success_count: 0
failure_count: 0
source_traces: ["seed"]
deprecated: false
---

# Systematic Debugging Strategy

## Steps
1. READ the full error message — extract file, line number, error type, and stack trace
2. REPRODUCE the issue — confirm you can trigger it consistently
3. LOCATE the source — navigate to the file and line from the error
4. UNDERSTAND the context — read the surrounding code, check recent changes with `git log -p [file]`
5. HYPOTHESIZE — form a specific theory about what's wrong
6. VERIFY — add a targeted log/breakpoint to confirm or disprove the hypothesis
7. FIX — make the minimal change that addresses the root cause
8. TEST — run the specific test case, then the full test suite
9. CLEAN UP — remove any debug logs added during investigation

## Negative Constraints
- Do not change multiple things at once — isolate variables
- Do not "fix" code you don't understand — understand first, then fix
- Do not suppress errors (try/catch with empty catch) as a "fix"
- Do not assume the first hypothesis is correct — verify before acting

## Notes
- `git bisect` is powerful for finding which commit introduced a regression
- Check if the issue is environment-specific (dev vs prod, OS, Node version)
- Rubber duck debugging: explain the problem out loud — the act of explaining often reveals the answer
