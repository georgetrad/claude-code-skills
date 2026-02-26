# Claude Code Custom Skills

Personal reusable skills (slash commands) for [Claude Code](https://claude.ai/claude-code).

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| [init-claude-docs](init-claude-docs/) | `/init-claude-docs` | Analyzes a codebase and scaffolds `.claude/` project documentation (CLAUDE.md, ARCHITECTURE, CONVENTIONS, TROUBLESHOOTING, and more) |

## Setup

Clone into your Claude Code skills directory:

```bash
git clone https://github.com/georgetrad/claude-code-skills.git ~/.claude/skills
```

Skills are available immediately via `/skill-name` in any Claude Code session.

## Structure

Each skill follows the standard layout:

```
skill-name/
├── SKILL.md              # Main prompt (required)
└── references/           # Supporting files loaded on demand
    └── templates.md
```
