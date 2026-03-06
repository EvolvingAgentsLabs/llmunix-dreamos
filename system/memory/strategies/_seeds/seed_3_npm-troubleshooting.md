---
id: seed_3_npm-troubleshooting
version: 1
hierarchy_level: 3
title: NPM Dependency Resolution
trigger_goals: ["npm install error", "ERESOLVE", "peer dependency", "dependency conflict", "package install fail"]
preconditions: ["Node.js project", "package.json exists"]
confidence: 0.5
success_count: 0
failure_count: 0
source_traces: ["seed"]
deprecated: false
---

# NPM Dependency Resolution

## Steps
1. Read the full error message carefully — identify which packages conflict
2. Check `package.json` for the conflicting package versions
3. Check if a `package-lock.json` or `yarn.lock` exists and may be stale
4. Try: `npm install` (clean attempt)
5. If ERESOLVE: check if updating the conflicting package resolves it
6. If peer dependency mismatch: check if a compatible version range exists
7. As a last resort: `npm install --legacy-peer-deps` (note: this skips peer dep validation)
8. Verify the application builds and tests pass after resolution

## Negative Constraints
- Do not blindly use `--force` or `--legacy-peer-deps` without understanding the conflict
- Do not delete `node_modules` and `package-lock.json` as a first step — this is wasteful and may introduce new issues
- Do not downgrade packages without checking if the downgrade breaks other dependencies

## Notes
- `npm ls [package]` shows the dependency tree for a specific package
- `npm outdated` shows which packages have newer versions available
- Consider using `npx npm-check-updates` to identify safe upgrades
