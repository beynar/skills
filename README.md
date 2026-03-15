# Skills

A collection of AI agent skills. Skills are packaged instructions, rules, and references that extend AI coding agent capabilities.

## Available Skills

| Skill | Description | Categories | Impact |
|-------|-------------|------------|--------|
| [clean-code](skills/clean-code/) | AI code review & cleanup for TypeScript/Node.js | 13 anti-pattern categories | Empirically grounded |

## What Are Skills?

Skills follow the [Agent Skills format](https://github.com/vercel-labs/agent-skills). Each skill contains:

- `SKILL.md` — Instructions for the agent (required)
- `AGENTS.md` — Compiled reference document
- `rules/` — Individual rule files with signals, tests, and examples
- `references/` — Sub-skill documentation (review, detect, cleanup modes)
- `metadata.json` — Version, abstract, and source references

## Installation

### Claude Code

```bash
# Clone and symlink into your global skills directory
git clone https://github.com/beynar/skills.git
cp -r skills/clean-code ~/.claude/skills/clean-code
```

Or add to a project's `.claude/skills/` directory for project-scoped access.

### Other Agents

Use `AGENTS.md` or `references/system-prompt.md` as a system prompt for any LLM-based code review workflow.

## Research Sources

The clean-code skill is grounded in empirical data, not opinions:

- **CodeRabbit (2025)**: 470 PRs. AI code = 1.7x more issues. Readability 3x worse. I/O 8x worse.
- **GitClear (2025)**: 211M lines. Code clones up 8x. Refactoring down from 25% to <10%.
- **Augment Code (2025)**: 1 in 5 AI samples reference fake libraries. 45% have security flaws.
- **IEEE Spectrum (2026)**: Silent failures documented — AI removes safety checks, produces fake output.

## Contributing

Open an issue or PR. When adding a new rule:

1. Create a `.md` file in `skills/clean-code/rules/` with YAML frontmatter (title, impact, tags)
2. Include: signals, diagnostic test, incorrect example, correct example
3. Update `SKILL.md` rule table and `AGENTS.md` compiled reference

## License

MIT
