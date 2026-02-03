# HemSoft Public Skills

Public agent skills for the [skills.sh](https://skills.sh) ecosystem.

## Installation

```bash
npx skills add HemSoft/public-skills
```

Or install individual skills:

```bash
npx skills add HemSoft/public-skills/repo-audit
```

## Available Skills

| Skill | Description |
|-------|-------------|
| [github-issue](skills/github-issue/SKILL.md) | Creates well-structured GitHub Issues via CLI with proper labels and escape-safe formatting |
| [markdown](skills/markdown/SKILL.md) | Markdown linting with markdownlint-cli2, quality enforcement, and best practices |
| [pr-reviewer](skills/pr-reviewer/SKILL.md) | Thorough PR reviews with 3 modes: local report, inline comments, or active fix assistance |
| [repo-audit](skills/repo-audit/SKILL.md) | Audits repositories for documentation drift, stale artifacts, and configuration issues |
| [self-reflection](skills/self-reflection/SKILL.md) | Helps agents troubleshoot difficult tasks and record lessons learned |
| [simplisticate](skills/simplisticate/SKILL.md) | Identifies complexity in code and proposes targeted simplifications with risk assessment |
| [todo](skills/todo/SKILL.md) | Manages TODO.md with status table and context sections for project task tracking |
| [verbiage](skills/verbiage/SKILL.md) | Creates VERBIAGE.md terminology dictionary for consistent project vocabulary |
| [version](skills/version/SKILL.md) | Release version management with SemVer, Keep a Changelog, and git tag integration |

## About skills.sh

Skills are markdown files that extend AI agents with specialized knowledge and capabilities. Learn more at [skills.sh](https://skills.sh).

## License

MIT License - see [LICENSE](LICENSE) for details.
