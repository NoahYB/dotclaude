---
name: usage-stats
description: Show the user's Claude Code usage stats for the current day - prompts sent, sessions, projects, and historical comparison from ~/.claude/
user_invocable: true
---

# Usage Stats

Show the user their Claude Code usage stats for today.

**Announce at start:** "Checking your usage stats for today..."

## Data Sources

1. **`~/.claude/history.jsonl`** — one JSON object per line, each user prompt with `timestamp`, `project`, `sessionId`
2. **`~/.claude/stats-cache.json`** — historical aggregate stats with `dailyActivity`, `dailyModelTokens`, `modelUsage`, `totalSessions`, `totalMessages`
3. **`~/.claude/sessions/`** — JSON files named `{pid}.json` with `sessionId`, `startedAt`, `kind`

## Steps

### 1. Gather Today's Data

Run a Python script to parse `~/.claude/history.jsonl` and compute:

- Total prompts sent today
- Number of unique sessions today
- Prompts broken down by project (use basename of project path)
- Prompts broken down by session
- Time of first and last prompt
- Active hours span

Use the current local date (midnight to now) as the filter window. Convert timestamps from milliseconds.

### 2. Gather Historical Context

Read `~/.claude/stats-cache.json` to get:

- `totalMessages` and `totalSessions` (all-time)
- `dailyActivity` array for comparison (average messages/day, busiest day)
- `longestSession` info

### 3. Present Results

Format the output as a clean summary:

```
## Today's Usage (Apr 1, 2026)

| Metric            | Value |
|-------------------|-------|
| Prompts sent      | 57    |
| Sessions          | 6     |
| Active since      | 12:17 PM |
| Last prompt       | 3:47 PM  |

### By Project
- adaptivereader: 30 prompts
- ~ (home): 27 prompts

### Historical Context
- All-time: X messages across Y sessions
- Daily average: Z messages/day
- Today vs average: above/below average
```

Keep it concise. Use a table for the main metrics, bullets for breakdowns.

## Important

- Timestamps in history.jsonl are in milliseconds since epoch
- Use the local timezone for display
- If stats-cache.json is stale (lastComputedDate is old), note that historical stats may be incomplete
- If no prompts found for today, say so clearly
