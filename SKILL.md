---
name: last-x-days
description: "Research trending discussions, recent news, and social media activity on any topic across Reddit, X, and the web within a configurable time window (1–365 days). Use when the user wants to find what people are saying about a topic, track recent trends, or gather community sentiment from a specific time range."
argument-hint: '"<days> [topic]" or "[topic]" (defaults to 30 days)'
context: fork
agent: Explore
disable-model-invocation: true
allowed-tools: Bash, Read, Write, AskUserQuestion, WebSearch
---

# last-x-days: Research Any Topic from a Custom Time Range

Gathers posts, threads, and articles from Reddit, X, and the web, then summarizes key themes, engagement metrics, and notable discussions for a user-specified time window.

## Usage

```
last-x-days 7 "AI tools"           # Last 7 days
last-x-days 14 "Claude Code"       # Last 2 weeks
last-x-days 3 "breaking news"      # Last 3 days
last-x-days "best prompts"         # Defaults to 30 days
```

## Time Range

- **Minimum:** 1 day
- **Maximum:** 365 days
- **Default:** 30 days (if no number specified)

## Research Execution

Run the research script:
```bash
python3 ~/.claude/skills/lastXdays/scripts/lastXdays.py <days> "$TOPIC" --emit=compact 2>&1
```

Examples:
```bash
python3 ~/.claude/skills/lastXdays/scripts/lastXdays.py 7 "AI tools" --emit=compact
python3 ~/.claude/skills/lastXdays/scripts/lastXdays.py 14 "best practices" --quick
python3 ~/.claude/skills/lastXdays/scripts/lastXdays.py 3 "breaking news" --deep
```

## Options

- `--quick` — Faster, fewer sources (8-12 each)
- `--deep` — Comprehensive (50-70 Reddit, 40-60 X)
- `--emit=MODE` — Output: compact|json|md|context|path
- `--sources=MODE` — Source: auto|reddit|x|both
- `--debug` — Verbose logging

## Output

Results are written to `~/.local/share/last30days/out/` and include:

- `report.md` — Full human-readable report with ranked posts, engagement scores, and synthesized best practices
- `report.json` — Normalized data with relevance, recency, and engagement scores per item
- `last30days.context.md` — Compact reusable snippet for embedding research into other skills

## Dependencies

Requires at least one API key configured in `~/.config/last30days/.env`:
- `OPENAI_API_KEY` — for Reddit research via OpenAI web search
- `XAI_API_KEY` — for X research via xAI live search (optional but recommended)

If the script fails with a missing-key error, verify the `.env` file exists and contains valid keys.
