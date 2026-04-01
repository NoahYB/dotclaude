---
name: usage-stats
description: Show the user's Claude Code usage stats for the current day - prompts sent, sessions, projects, token usage, and estimated API cost from ~/.claude/
user_invocable: true
---

# Usage Stats

Show the user their Claude Code usage stats for today with token breakdown and cost estimates.

**Announce at start:** "Checking your usage stats for today..."

## Data Sources

1. **`~/.claude/history.jsonl`** — one JSON object per line, each user prompt with `timestamp`, `project`, `sessionId`
2. **`~/.claude/stats-cache.json`** — historical aggregate stats with `dailyActivity`, `dailyModelTokens`, `modelUsage`, `totalSessions`, `totalMessages`
3. **`~/.claude/projects/*/<sessionId>.jsonl`** — conversation logs containing assistant messages with `message.usage` (input_tokens, output_tokens, cache_read_input_tokens, cache_creation_input_tokens) and `message.model`

## Steps

### 1. Run a single Python script

Run a single Python script that does ALL of the following in one pass:

**From history.jsonl** — filter to today's local date (midnight to now, timestamps are ms since epoch):
- Total prompts sent today
- Unique session IDs
- Prompts per project (use basename, show home dir as "~ (home)")
- Timestamp of first and last prompt

**From project .jsonl files** — for each of today's session IDs, walk `~/.claude/projects/` to find matching `<sessionId>.jsonl` files. Parse assistant messages where `message.usage` exists:
- Aggregate tokens per model: input_tokens, output_tokens, cache_read_input_tokens, cache_creation_input_tokens
- Count total API requests

**From stats-cache.json**:
- totalMessages, totalSessions
- dailyActivity for average/busiest day comparison

### 2. Compute estimated API cost

Use these rates (per million tokens) for cost estimation:

| Model | Input | Output | Cache Write | Cache Read |
|-------|-------|--------|-------------|------------|
| claude-opus-4-6 | $15 | $75 | $18.75 | $1.50 |
| claude-sonnet-4-6 | $3 | $15 | $3.75 | $0.30 |
| claude-sonnet-4-5-20250929 | $3 | $15 | $3.75 | $0.30 |
| claude-haiku-4-5-20251001 | $0.80 | $4 | $1 | $0.08 |

For any model containing "opus" use opus rates, "sonnet" use sonnet rates, "haiku" use haiku rates.

### 3. Present results with style

Use box-drawing characters and visual flair. Format like this:

```
╔══════════════════════════════════════════════════════════╗
║              CLAUDE CODE DAILY REPORT                    ║
║              <date> — <first_time> to <last_time>       ║
╚══════════════════════════════════════════════════════════╝

┌─ ACTIVITY ──────────────────────────────────────────────┐
│  <N> prompts  ·  <N> sessions  ·  <N> API requests      │
│                                                          │
│  <project_a>  ████████████████░░░░░░░░░░  <N> (<pct>%)  │
│  <project_b>  ███████████████░░░░░░░░░░░  <N> (<pct>%)  │
└──────────────────────────────────────────────────────────┘

┌─ TOKENS & COST ────────────────────────────────────────-┐
│  Model: <model_name>                                     │
│  Tier:  <TIER> pricing                                   │
│                                                          │
│  Input          <tokens>    ░░░░░░░░░░░░░░  $<cost>     │
│  Output         <tokens>    ████████░░░░░░  $<cost>     │
│  Cache Write    <tokens>    ██████████████  $<cost>     │
│  Cache Read     <tokens>    ██████████████  $<cost>     │
│                                                          │
│                                  TOTAL      $<total>    │
└──────────────────────────────────────────────────────────┘

┌─ HISTORICAL CONTEXT ────────────────────────────────────┐
│  All-time: <N> messages across <N> sessions              │
│  Daily avg: <N> msgs/day                                 │
│  Today vs avg: ▼/▲ below/above average                   │
│  Busiest day: <date> (<N> msgs)                          │
└──────────────────────────────────────────────────────────┘
```

Use filled blocks (█) and empty blocks (░) for bar charts. Scale bars proportionally. Right-align dollar amounts. Use commas in large numbers.

## Important

- Timestamps in history.jsonl are in milliseconds since epoch
- Use the local timezone for display
- If stats-cache.json is stale (lastComputedDate is old), note it
- If no prompts found for today, say so clearly
- The bar chart for projects should be proportional to their prompt counts
- The bar chart for token categories should be proportional to their dollar cost
