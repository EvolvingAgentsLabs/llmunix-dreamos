---
id: seed_3_tdd-workflow
version: 1
hierarchy_level: 3
title: Test-Driven Development Workflow
trigger_goals: ["write tests", "testing", "TDD", "unit test", "test first", "test coverage"]
preconditions: ["test framework configured", "project builds successfully"]
confidence: 0.5
success_count: 0
failure_count: 0
source_traces: ["seed"]
deprecated: false
---

# Test-Driven Development Workflow

## Steps
1. Understand the requirement: what should the code DO? (not how)
2. Write a failing test that describes the expected behavior
3. Run the test — confirm it fails for the RIGHT reason (not a syntax error)
4. Write the MINIMUM code to make the test pass
5. Run the test — confirm it passes
6. Refactor the code while keeping tests green
7. Repeat for the next requirement
8. Run the full test suite to ensure no regressions

## Negative Constraints
- Do not write implementation before the test — the test defines the contract
- Do not write overly complex tests — test behavior, not implementation details
- Do not mock everything — only mock external dependencies (APIs, databases, file system)
- Do not skip the "failing for the right reason" check — false greens hide bugs

## Notes
- For existing code without tests, start by writing tests for the current behavior before refactoring
- Aim for test names that read like specifications: "should return 401 when token is expired"
- Integration tests > unit tests for critical paths (auth, payments, data persistence)
