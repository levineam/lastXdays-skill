# lastXdays - Claude Code Skill

**Research any topic from the last X days on Reddit + X + Web with configurable time range.**

*Inspired by and based on [last30days-skill](https://github.com/mvanhorn/last30days-skill) by mvanhorn.*

## What It Does

The AI world moves fast, and 30 days might be too much or too little depending on your research needs. **lastXdays** takes the powerful research capabilities of the original last30days skill and adds **configurable time windows** - research from the last 3 days, 7 days, 14 days, or any custom range up to 365 days.

Like the original last30days, this skill:
- **Researches** - Scans Reddit and X for discussions from your specified time window
- **Synthesizes** - Identifies patterns, best practices, and what actually works
- **Delivers** - Either writes copy-paste-ready prompts for your target tool, or gives you expert-level answers

## Key Enhancement: Configurable Time Range

Unlike the fixed 30-day window, lastXdays lets you specify exactly how far back to research:

```
lastXdays 3 "breaking news"          # Last 3 days for hot topics
lastXdays 7 "AI tools"               # Last week for recent developments  
lastXdays 14 "Claude Code skills"    # Last 2 weeks for broader patterns
lastXdays 90 "startup ideas"         # Last 3 months for trend analysis
lastXdays "best prompts"             # Defaults to 30 days if no number
```

**Time Range Options:**
- **Minimum:** 1 day
- **Maximum:** 365 days  
- **Default:** 30 days (maintains compatibility with last30days)

## Installation

```bash
# Clone the repo
git clone https://github.com/levineam/lastXdays-skill.git ~/.claude/skills/lastXdays

# Add your API keys
mkdir -p ~/.config/last30days
cat > ~/.config/last30days/.env << 'EOF'
OPENAI_API_KEY=sk-...
XAI_API_KEY=xai-...
EOF
chmod 600 ~/.config/last30days/.env
```

## Usage

```
lastXdays <days> [topic]
lastXdays <days> [topic] for [tool]
lastXdays [topic]                    # defaults to 30 days
```

**Examples:**
- `lastXdays 7 "AI tools"`
- `lastXdays 3 "breaking tech news"`
- `lastXdays 14 "best practices for Claude Code"`
- `lastXdays 90 "startup funding trends"`
- `lastXdays "prompting techniques"` (uses 30 days)

## Use Cases by Time Range

**1-3 days:** Breaking news, hot takes, immediate reactions
**7 days:** Weekly trends, recent tool updates, fresh releases  
**14-30 days:** Established patterns, mature discussions, best practices
**60-90 days:** Seasonal trends, quarterly patterns, long-term shifts
**180-365 days:** Annual trends, major paradigm shifts, historical context

## Technical Details

Same robust architecture as last30days:
- OpenAI API with web search for Reddit research
- xAI API with live X search  
- Real Reddit thread enrichment for engagement metrics
- Scoring algorithm weighing recency, relevance, and engagement

**Options:**
- `--quick` - Faster research with fewer sources (8-12 each)
- `--deep` - Comprehensive research (50-70 Reddit, 40-60 X)
- `--sources=reddit|x|both` - Source selection
- `--debug` - Verbose logging

## Requirements

- OpenAI API key (for Reddit research)
- xAI API key (for X research, optional but recommended)

At least one API key is required.

## Attribution

This skill is **inspired by and based on** the original [last30days-skill](https://github.com/mvanhorn/last30days-skill) by [@mvanhorn](https://github.com/mvanhorn). The core research methodology, API integration patterns, and synthesis approach all originate from that excellent work.

**Key differences in lastXdays:**
- Configurable time range (1-365 days vs fixed 30 days)
- Flexible argument parsing for time specification
- Maintains full backward compatibility with 30-day default

All credit for the foundational research architecture goes to the original last30days team. This fork simply adds time configurability to meet different research needs.

## License

MIT License