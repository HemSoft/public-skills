---
name: repo-audit
description: V1.0 - Audits repositories for documentation/reality drift, stale artifacts, unused config, and cross-reference accuracy. Use when verifying repository health.
license: MIT
---

# Repo Audit

> *"Trust, but verify."* — Ensure your repo reflects reality.

## Core Philosophy

- **Reality over documentation**: What exists matters more than what's written.
- **No assumptions**: Cross-check everything, trust nothing.
- **Actionable findings**: Every issue has a clear fix.

## Audit Depth Levels

| Level | What It Checks | Example |
|-------|----------------|---------|
| **Surface** | Comments ↔ code usage in same file | `.env` comment says "plans, templates" but code only uses `runs/` |
| **Cross-file** | Documentation ↔ folder/file reality | README shows `PLAN.md` but file doesn't exist |
| **Deep** | Type definitions ↔ runtime usage | `CONDUCTOR_PORT` defined but never read by code |

All three levels are checked in every audit.

## Audit Categories

### 1. Documentation Drift

- README directory trees vs actual structure
- Comments describing non-existent behavior
- Outdated architecture diagrams
- Stale "TODO" or "FIXME" with old dates

### 2. Configuration Hygiene

- `.env` variables not used in code
- `.env` comments not matching reality
- `package.json` dependencies not imported
- Config files for removed features

### 3. Artifact Staleness

- Files/folders marked "legacy" or "deprecated" still existing
- Old plan/design documents for implemented features
- Orphan files never imported/referenced
- Empty or placeholder directories

### 4. Cross-Reference Accuracy

- Internal doc links pointing to deleted files
- Type definitions not matching actual usage
- Event names defined but never emitted/handled
- API routes documented but not implemented (or vice versa)

### 5. Terminology Consistency

- Inconsistent naming (e.g., "template" vs "workload")
- Old terminology in comments/docs after rename
- Mixed conventions (camelCase vs snake_case inconsistently)

### 6. Workload Definition Conflicts (YAML-based projects)

- Duplicate workload IDs across different directories
- Workload type mismatches (file location doesn't match declared type)
  - e.g., `type: task` in `workloads/ad-hoc/` directory
  - e.g., `type: ad-hoc` in `workloads/tasks/` directory
- Missing required fields for workload type
  - Ad-hoc requires `prompt` field
  - Tasks require `steps` field
  - Workflows require `steps` and dependency definitions
- Orphaned workload files (defined but never used)

### 7. Environment Configuration Issues

- Placeholder values in `.env` not replaced (e.g., `your-event-key-here`, `test-signing-key`)
- Environment variable comments not matching actual behavior
- Missing environment variable definitions that code depends on
- `.env` variables with values that contradict documented behavior
- Development config values left in files that should be production-ready

## Workflow

### 1. Scan Repository

Analyze:

- All markdown files (README, CHANGELOG, docs/*)
- Configuration files (.env, package.json, tsconfig.json, etc.)
- Source code comments and type definitions
- Folder structure vs documented structure

### 2. Generate Audit Report

Present findings in table format:

```
## Repo Audit Report

| # | Category | Finding | Risk | Recommendation | Action |
|---|----------|---------|------|----------------|--------|
| 1 | Config | `CONDUCTOR_PORT` in .env not used in code | 🟢 | Remove unused variable | Y/n |
| 2 | Docs | README shows `PLAN.md` but file deleted | 🟢 | Update directory tree | Y/n |
| 3 | Artifact | `data/templates/` marked legacy, still exists | 🟡 | Delete folder, update refs | Y/n |
| 4 | Terminology | .env comment says "plans" but feature removed | 🟢 | Update comment | Y/n |
| 5 | Cross-ref | SANITIZATION-STRATEGY.md references PLAN.md | 🟢 | Remove reference | Y/n |

**Legend:**
- Y/n = Yes recommended (press Enter or type 'y' to approve)
- y/N = No recommended (press Enter or type 'n' to skip)
- Risk: 🟢 Low | 🟡 Medium | 🔴 High
```

### 3. Risk Assessment

| Level | Description | Criteria |
|-------|-------------|----------|
| 🟢 Low | Cosmetic, documentation-only, no functional impact | Safe to auto-fix |
| 🟡 Medium | Touches shared config or may affect behavior | Review before applying |
| 🔴 High | Structural change, may break builds/deployments | Manual intervention required |

### 4. User Decision

After presenting report, ask:

```
Enter numbers to SKIP (e.g., "3,5") or press Enter to apply all recommended:
```

- Blank = apply all Y/n recommendations
- Numbers = skip those items
- "all" = apply everything including y/N items
- "none" = skip all, just viewing

### 5. Execute Approved Changes

For each approved item:

1. Make the change
2. Verify no breakage (if applicable)
3. Mark complete in report

### 6. Summary

```
## Audit Complete

✅ Applied: 4 changes
⏭️ Skipped: 1 item (#3 - user declined)
📋 Total findings: 5
```

## Example Findings

| Category | Signal | What To Check |
|----------|--------|---------------|
| Config | `.env` variable | grep for usage in `src/**/*.ts` |
| Docs | Directory tree in README | Compare with `ls -R` output |
| Artifact | "Legacy" or "Deprecated" comments | Check if referenced anywhere |
| Cross-ref | `[link](file.md)` in markdown | Verify file exists |
| Terminology | Comment mentions feature name | Verify feature still exists with that name |
| Workload | Same `id:` in multiple `.yaml` files | Check `workloads/*/` for duplicates |
| Workload | YAML `type:` doesn't match directory | `type: task` in `workloads/ad-hoc/` |
| Workload | Required field missing for type | `type: ad-hoc` without `prompt` field |
| Config | Placeholder value in `.env` | Values like `your-event-key-here`, `test-` prefix |
| Config | Comment says "Set to 0" but code expects 1 | `INNGEST_DEV=0` with comment "for production" |

## Output Format

```text
# 🔍 Repo Audit Report
**Repository**: {repo-name}
**Date**: {YYYY-MM-DD HH:MM}
**Findings**: {count}

| # | Category | Finding | Risk | Recommendation | Action |
|---|----------|---------|------|----------------|--------|
{rows}

**Legend**: Y/n = Yes recommended | y/N = No recommended

---

Enter numbers to SKIP (e.g., "3,5") or press Enter to apply all recommended:
```

---

*"The best code is no code. The best documentation is accurate documentation."*
