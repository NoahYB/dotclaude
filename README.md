# dotclaude

Custom skills for [Claude Code](https://claude.ai/code).

## Skills

### `/usage-stats`

Shows your Claude Code usage stats for the current day — prompts sent, sessions, projects worked on, and historical comparison.
<img width="745" height="1107" alt="Screenshot 2026-04-01 at 5 01 30 PM" src="https://github.com/user-attachments/assets/a2520c2f-c796-4f8d-80f5-59e46266a85b" />

Parses data from `~/.claude/history.jsonl` and `~/.claude/stats-cache.json`.

### `/spec-writer`

Helps write clear, complete implementation specs before handing work to Claude Code. Walks you through scoping, drafts a structured spec (acceptance criteria, data shapes, decisions, implementation steps), and challenges it for gaps — so Claude Code doesn't silently guess wrong.

## Installation

Copy any skill folder into `~/.claude/skills/`:

```bash
cp -r skills/usage-stats ~/.claude/skills/
cp -r skills/spec-writer ~/.claude/skills/
```

Then invoke with `/usage-stats` or `/spec-writer` in any Claude Code session.
