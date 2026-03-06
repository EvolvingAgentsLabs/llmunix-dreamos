---
id: seed_2_git-workflow
version: 1
hierarchy_level: 2
title: Git Branch Workflow
trigger_goals: ["git workflow", "branching", "feature branch", "pull request", "merge"]
preconditions: ["git repository initialized", "remote configured"]
confidence: 0.5
success_count: 0
failure_count: 0
source_traces: ["seed"]
deprecated: false
---

# Git Branch Workflow

## Steps
1. Ensure you are on the main/master branch and it is up to date: `git pull origin main`
2. Create a descriptive feature branch: `git checkout -b feature/[descriptive-name]`
3. Make changes in logical, atomic commits (one concern per commit)
4. Write clear commit messages: imperative mood, explain WHY not just WHAT
5. Push the branch: `git push -u origin feature/[name]`
6. Create a pull request with a summary of changes and test plan
7. Address review feedback in new commits (do not force-push during review)
8. Squash merge into main when approved

## Negative Constraints
- Never force-push to main/master branch
- Never commit directly to main without a PR (except for trivial fixes in solo projects)
- Never use `git add .` without reviewing `git status` first — you may stage unwanted files
- Never amend a published commit that others may have pulled

## Notes
- For hotfixes, branch from main, fix, and create an expedited PR
- Use conventional commits (feat:, fix:, docs:, refactor:) when the project follows that convention
