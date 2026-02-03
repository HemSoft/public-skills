---
name: verbiage
description: Creates and manages VERBIAGE.md terminology dictionary in repository root. Use to define project-specific terms and maintain consistent vocabulary.
---

# Verbiage

Manage repository terminology dictionary.

## Purpose

Every project develops its own vocabulary - terms that have specific meaning within the codebase. This skill creates and maintains a `VERBIAGE.md` file that serves as a living glossary for your project.

## Usage

- **No parameters**: Create empty VERBIAGE.md with table structure
- **With terms**: Add specified term definitions

## File Location

`{repo-root}/VERBIAGE.md`

## Format

```markdown
# Verbiage

Project terminology dictionary.

| Term | Definition |
|------|------------|
| Activity Bar | Vertical icon strip on the far left of the app |
| Workload | A unit of work that can be scheduled and executed |
| Conductor | The orchestration service that manages workload execution |
```

## When to Use

- When a term has specific meaning in your project that differs from common usage
- When onboarding new team members who need to learn project vocabulary
- When documenting APIs or user-facing features with domain-specific terms
- When you find yourself repeatedly explaining what a term means in PRs or docs

## Actions

### Create VERBIAGE.md

1. Check if `VERBIAGE.md` exists in repo root
2. If not, create with header and empty table
3. Inform user the file is ready for terms

### Add Terms

When user provides terms:

- `"Activity Bar: Vertical icon strip"` → Add as new row
- Multiple terms can be added at once
- Keep entries alphabetically sorted

### Update Terms

When a term's meaning evolves:

1. Find the existing term in the table
2. Update the definition
3. Preserve alphabetical order

## Example Workflow

```
User: "Add verbiage: Workload - A unit of work that can be scheduled"

Agent:
1. Opens or creates VERBIAGE.md
2. Adds row: | Workload | A unit of work that can be scheduled |
3. Sorts table alphabetically
4. Confirms addition
```

## Best Practices

- **Be concise** - Definitions should be one sentence when possible
- **Be specific** - Explain what the term means *in this project*
- **Update regularly** - Add terms as they emerge during development
- **Link from docs** - Reference VERBIAGE.md from README or CONTRIBUTING
