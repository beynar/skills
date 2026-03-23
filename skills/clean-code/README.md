# Clean Code Plugin

AI code reviewer and cleanup toolkit for TypeScript/Node.js. Reviews AI-generated code for 13 empirically-grounded anti-pattern categories and produces review reports, diagnostic scans, or cleaned rewrites.

## Commands

| Command | Description |
|---------|-------------|
| `/review [file]` | Full review: structured report + cleaned rewrite |
| `/slop [file]` | Quick scan: severity table + slop score (1-10), no rewrite |
| `/cleanup [file]` | Mechanical cleanup: 9-pass pipeline, just the fixed code |

## Skills

| Skill | Triggers on |
|-------|-------------|
| **code-review** | "review this code", "audit code quality", "check before merging" |
| **slop-detector** | "detect slop", "how bad is this", "rate this code" |
| **code-cleanup** | "clean up this code", "strip the slop", "just fix it" |

## What It Catches

13 anti-pattern categories grounded in empirical research:

- **Trojan Output** — silent failures, fake output, removed safety checks
- **Error Amnesia** — swallowed errors, empty catches
- **Walking Dead** — unused imports, variables, functions
- **Phantom Import** — hallucinated APIs, deprecated methods
- **Phantom Abstractions** — wrappers, singletons, Context Bags, passthrough generators
- **Monolith Instinct** — workflows dumped in one function; bad decomposition when forced to split
- **Prophetic Engineering** — over-engineering, ceremony interfaces
- **Clone Army** — duplicated logic (8x increase per GitClear)
- **Speed Trap** — O(n^2) loops, N+1 queries, sequential awaits
- **Nesting Instinct** — deep control flow that should be flat
- **Identity Crisis** — vague naming hiding intent
- **Type System Cosplay** — TypeScript features adding complexity, not safety
- **Comment Theater** — comments restating the code

## Research Sources

- CodeRabbit (2025): 470 PRs, AI code = 1.7x more issues
- GitClear (2025): 211M lines, code clones up 8x
- Augment Code (2025): 1 in 5 AI samples reference fake libraries
- IEEE Spectrum (2026): silent failures documented
