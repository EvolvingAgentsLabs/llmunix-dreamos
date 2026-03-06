---
id: seed_1_project-setup
version: 1
hierarchy_level: 1
title: New Project Setup Pattern
trigger_goals: ["new project", "project setup", "initialize project", "create app", "scaffold"]
preconditions: ["project requirements defined", "tech stack chosen"]
confidence: 0.5
success_count: 0
failure_count: 0
source_traces: ["seed"]
deprecated: false
---

# New Project Setup Pattern

## Steps
1. Create project directory structure following conventions of the chosen framework
2. Initialize version control with `git init` and create `.gitignore`
3. Set up package manager (package.json, requirements.txt, go.mod, etc.)
4. Install core dependencies (framework, linter, test runner)
5. Create minimal configuration files (tsconfig, eslint, prettier, etc.)
6. Write a "hello world" entry point to verify the setup works
7. Run the build/dev command to verify everything compiles
8. Create initial commit with the scaffold

## Negative Constraints
- Do not install unnecessary dependencies upfront — add them as needed
- Do not copy configuration from other projects without understanding each option
- Do not skip the verification step (step 7) — catching setup issues early saves hours

## Notes
- Prefer established project templates (create-next-app, create-vite, etc.) over manual scaffolding when available
- Always check the framework's latest documentation for current best practices
