# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About This Repo

This is the optional skills registry for [claude-assistant](https://github.com/nnance/claude-assistant). Skills here extend the assistant with integrations that users can install on demand. Only add skills here that are optional — core skills (memory, scheduling, slack-messaging) live in the main repo.

## Design Principles

1. **Prompt-driven over deterministic code** — Skill behavior should be shaped by prompts and instructions, not hardcoded logic. Give the agent autonomy to decide what to do. Code (scripts, CLIs) should be limited to API interactions exposed as commands the agent invokes.
2. **CLIs over MCP tools** — Expose capabilities as CLI commands that skills invoke via shell scripts or `npx tsx`. This supports progressive disclosure — the agent discovers tools through skills as needed.
3. **Keep it simple and DRY** — Minimal dependencies. Prefer shell scripts for macOS integrations. Only add a runtime (Node, Python) when it provides significant value.

## Skill Authoring

### Structure

Each skill lives in `skills/<name>/` and must contain a `SKILL.md` file:

```
skills/
└── my-skill/
    ├── SKILL.md          # Required: skill instructions
    ├── scripts/          # Optional: shell scripts
    │   └── action.sh
    └── references/       # Optional: reference docs linked from SKILL.md
        └── examples.md
```

### SKILL.md Format

```markdown
---
name: managing-<thing>
description: "One sentence in third person. Use when the user says X, Y, Z. Triggers on mentions of 'keyword'."
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
---

# Title

Instructions for the agent...
```

**Frontmatter rules:**
- `name`: Use gerund form (e.g., `managing-vault`, not `vault`)
- `description`: Third person, include trigger phrases. This is what the assistant uses to decide when to invoke the skill.
- `allowed-tools`: List only the tools the skill needs

### Writing Good Instructions

- Write instructions the agent follows, not documentation for humans
- Be explicit about command signatures, output formats, and error handling
- Include decision logic (when to take action vs. ask for clarification)
- Reference sub-documents with relative links: `[tags](references/tags.md)`
- Add worked examples in a `references/examples.md` file for complex skills

### Shell Scripts

- Use `#!/bin/bash` and `set -e`
- Return JSON on success; include `{"error": "..."}` with exit code 1 on failure
- Source a `_common.sh` for shared utilities if multiple scripts exist
- Make scripts executable: `chmod +x scripts/*.sh`

## Adding a Skill

1. Create `skills/<name>/SKILL.md` following the format above
2. Add any supporting scripts or reference files
3. Update `README.md` — add a row to the overview table and a full detail section
4. Open a pull request
