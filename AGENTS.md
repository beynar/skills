# Agent Skills Repository

A collection of skills for AI coding agents. Skills are packaged instructions, rules, and references that extend agent capabilities.

## Directory Structure

```
skills/
  <skill-name>/
    SKILL.md          # Skill definition and instructions (required)
    AGENTS.md         # Compiled reference document
    metadata.json     # Version, abstract, references
    rules/            # Individual rule files
    references/       # Sub-skill documentation
```

## Skill Format

Each skill follows the Agent Skills format:

- **SKILL.md**: YAML frontmatter (name, description, license, metadata) followed by markdown instructions. Kept under 500 lines. References point to `rules/` and `references/` for deeper content.
- **Rules**: Individual `.md` files with YAML frontmatter (title, impact, tags) containing signals, diagnostic tests, and before/after code examples.
- **References**: Sub-skill documentation loaded on demand based on the user's request.

## Available Skills

- `clean-code` — AI code review and cleanup toolkit for TypeScript/Node.js. 13 anti-pattern categories, 3 operational modes (review, detect, cleanup).

## Naming Conventions

- Skill directories: `kebab-case`
- Rule files: `kebab-case.md` matching the category name
- Skill definition: always `SKILL.md` (uppercase)
