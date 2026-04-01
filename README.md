# dotclaude

Custom skills for [Claude Code](https://claude.ai/code).

## Skills

### `/usage-stats`

Shows your Claude Code usage stats for the current day — prompts sent, sessions, projects worked on, and historical comparison.

Parses data from `~/.claude/history.jsonl` and `~/.claude/stats-cache.json`.

## Installation

Copy any skill folder into `~/.claude/skills/`:

```bash
cp -r skills/usage-stats ~/.claude/skills/
```

Then invoke with `/usage-stats` in any Claude Code session.
