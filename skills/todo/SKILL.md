---
name: todo
description: V1.0 - Manages TODO.md file in repository root with status table and context sections for tracking project tasks, ideas, and plans.
license: MIT
---

# To-Do Manager

Manages TODO.md files in repository roots for tracking project tasks, ideas, and plans.

## When to Use

Activate when the user wants to:

- Create a new TODO.md file in a project
- Update existing TODO items
- Add new tasks, ideas, or plans to a project
- View or organize project to-do lists
- Track project status across multiple repositories

## File Location

Creates or updates `TODO.md` in the root directory of the current repository.

## Structure

### Table Format

The TODO.md file MUST begin with a markdown table using this format:

```markdown
| Status | Priority | Task | Notes |
|--------|----------|------|-------|
| 🚧 | High | Task description | Additional context |
| ⏸️ | Medium | Another task | More details |
| ✅ | Low | Completed item | Done! |
```

### Status Icons

Use these standard status markers:

- `📋` - To Do (not started)
- `🚧` - In Progress (actively working)
- `⏸️` - Blocked/Paused (waiting on something)
- `✅` - Done (completed)
- `❌` - Won't Do (cancelled/deprioritized)

### Priority Levels

- `High` - Critical, needs immediate attention
- `Medium` - Important but not urgent
- `Low` - Nice to have, future work

### Elaboration Sections

Below the table, users can add freeform sections for additional context:

```markdown
## Context

Background information about the project or specific tasks.

## Ideas

Brainstorming and future possibilities.

## Completed

Detailed notes on finished work and outcomes.

## Notes

Any other relevant information.
```

## Operations

### Create New TODO.md

When creating a new file:

1. Verify we're in a git repository (check for .git directory)
2. Create TODO.md in the repository root
3. Add initial table structure with example row
4. Add placeholder sections if requested

### Update Existing TODO.md

When updating:

1. Read the existing TODO.md
2. Parse the table (preserve all existing rows)
3. Add/update/remove rows as requested
4. Maintain table formatting
5. Preserve all content below the table

### Guidelines

- **Always preserve the table at the top** - Never move or remove it
- **Keep the table concise** - Use the elaboration sections for lengthy details
- **Order by status** - Remaining items (📋, 🚧, ⏸️) at top, completed items (✅, ❌) at bottom
- **Link to details** - If a remaining item has a detailed section below, make the Task column a markdown anchor link (e.g., `[Task Name](#task-name)`)
- **Completed items are summary only** - Remove detailed specs for completed items; keep just a brief note in the Notes column with completion date
- **Be specific** - Task descriptions should be actionable ("Add user authentication" not "Auth stuff")

## Example TODO.md

```markdown
# Project TODO

| Status | Priority | Task | Notes |
|--------|----------|------|-------|
| 🚧 | High | [Implement authentication](#implement-authentication) | Using Next-Auth |
| 📋 | High | [Set up database schema](#set-up-database-schema) | PostgreSQL with Prisma |
| 📋 | Medium | Create landing page | Use shadcn/ui components |
| ⏸️ | Low | [Add dark mode support](#add-dark-mode-support) | Blocked: waiting on design |
| ✅ | High | Configure CI/CD pipeline | GitHub Actions (2026-01-15) |
| ✅ | Medium | Set up linting | ESLint + Prettier (2026-01-10) |

## Progress

**Completed: 2 / 6** (33%)

---

## Remaining Items

### Implement authentication

**Location**: `src/auth/`

**Problem**: Need secure user authentication with social login support.

**Proposed Solution**: Use Next-Auth with GitHub and Google providers.

---

### Set up database schema

**Location**: `prisma/schema.prisma`

**Problem**: Need to define data models for users, projects, and tasks.

**Proposed Solution**: Use Prisma ORM with PostgreSQL.

---

### Add dark mode support

**Location**: `src/styles/`

**Problem**: Users want dark mode option.

**Status**: Blocked waiting on design team's color palette.
```

## Workflow

1. **User requests to-do action** (create, update, view)
2. **Determine repository root** (search for .git directory)
3. **Check for existing TODO.md**
4. **Perform requested operation**:
   - Create: Generate new file with template
   - Update: Modify table while preserving structure
   - View: Display current contents
5. **Confirm changes** with user

## Anti-Patterns

- Don't create TODO.md outside of repository roots
- Don't remove or restructure the table without user consent
- Don't use inconsistent status icons
- Don't add vague or non-actionable tasks
- Don't keep detailed specs for completed items - summarize in Notes column
- Don't put completed items above remaining items in the table
- Don't forget anchor links for items that have detailed sections
