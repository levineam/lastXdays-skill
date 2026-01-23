---
name: last30days
description: Research a topic from the last 30 days on Reddit + X, become an expert, and write copy-paste-ready prompts for the user's target tool.
argument-hint: "[topic] for [tool]" or "[topic]"
context: fork
agent: Explore
disable-model-invocation: true
allowed-tools: Bash, Read, Write, AskUserQuestion
---

# last30days: Become Expert → Write Prompts

Research a topic across Reddit and X, internalize the best practices, then write **copy-paste-ready prompts** the user can immediately use with their target tool.

## CRITICAL: Parse User Intent

Before doing anything, parse the user's input for TWO things:

1. **TOPIC**: What they want to learn about (e.g., "web app mockups", "Claude Code skills", "image generation")
2. **TARGET TOOL**: Where they'll use the prompts (e.g., "Nano Banana Pro", "ChatGPT", "Claude", "Midjourney")

Common patterns:
- `[topic] for [tool]` → "web mockups for Nano Banana Pro"
- `[topic] prompts for [tool]` → "UI design prompts for Midjourney"
- `[tool] [topic]` → "Nano Banana Pro dashboard mockups"
- Just `[topic]` → Ask follow-up

**If TARGET TOOL is unclear**, use AskUserQuestion:
```
What tool will you use these prompts with?

Options:
1. Nano Banana Pro (image generation)
2. ChatGPT / Claude (text/code)
3. Midjourney / DALL-E (image generation)
4. Other (tell me)
```

**Store these values mentally** - you'll need them for the entire conversation:
- `TOPIC = [extracted topic]`
- `TARGET_TOOL = [extracted tool]`

---

## Setup Check

Verify API key configuration exists:

```bash
if [ ! -f ~/.config/last30days/.env ]; then
  echo "SETUP_NEEDED"
else
  echo "CONFIGURED"
fi
```

### If SETUP_NEEDED

Run NUX flow to configure API keys. Use AskUserQuestion to collect:

1. **OpenAI API Key** (optional but recommended for Reddit research)
2. **xAI API Key** (optional but recommended for X research)

Then create the config:

```bash
mkdir -p ~/.config/last30days
cat > ~/.config/last30days/.env << 'ENVEOF'
# last30days API Configuration
# At least one key is required

OPENAI_API_KEY=
XAI_API_KEY=
ENVEOF

chmod 600 ~/.config/last30days/.env
echo "Config created at ~/.config/last30days/.env"
echo "Please edit it to add your API keys, then run the skill again."
```

**STOP HERE if setup was needed.**

---

## Research Execution

Run the research orchestrator with the TOPIC.

**Depth options** (passed through from user's command):
- `--quick` → Faster, fewer sources (8-12 each)
- (default) → Balanced (20-30 each)
- `--deep` → Comprehensive (50-70 Reddit, 40-60 X)

```bash
python3 ~/.claude/skills/last30days/scripts/last30days.py "$ARGUMENTS" --emit=compact 2>&1
```

---

## FIRST: Internalize the Research

Read the research output and become an **expert**. Identify:
- The top 3-5 patterns/techniques that appeared across multiple sources
- Specific keywords, structures, or approaches that work well
- Common pitfalls to avoid

---

## THEN: Show Summary + Invite Vision

Display in this EXACT order (so stats are visible at bottom of terminal):

```
**Key patterns discovered:**
1. [Pattern 1] - [one-line insight]
2. [Pattern 2] - [one-line insight]
3. [Pattern 3] - [one-line insight]
4. [Pattern 4] - [one-line insight]
5. [Pattern 5] - [one-line insight]

I'm now an expert in {TOPIC}.

---

📊 Research Complete

Analyzed {total_sources} sources from the last 30 days
├─ Reddit: {n} threads │ {sum} upvotes │ {sum} comments
├─ X: {n} posts │ {sum} likes │ {sum} reposts
└─ Top voices: r/{sub1}, r/{sub2}, @{handle1}, @{handle2}

Share your vision for what you want to create and I'll write a thoughtful prompt you can copy-paste directly into {TARGET_TOOL}.
```

**Use real numbers from the research output.** The patterns should be actual insights from the research, not generic advice.

**IMPORTANT**: After displaying this, WAIT for the user to respond. Don't dump generic prompts.

---

## WAIT FOR USER'S VISION

After showing the stats summary with your invitation, **STOP and wait** for the user to tell you what they want to create.

When they respond with their vision (e.g., "I want a landing page mockup for my SaaS app"), THEN write a single, thoughtful, tailored prompt.

---

## WHEN USER SHARES THEIR VISION: Write ONE Perfect Prompt

Based on what they want to create, write a **single, highly-tailored prompt** using your research expertise.

### Format:

```
Here's your prompt for {TARGET_TOOL}:

---

[The actual prompt - ready to copy-paste, incorporating specific patterns/keywords from your research that match their use case]

---

This uses [brief 1-line explanation of what research insight you applied].
```

### Quality Checklist:
- [ ] Directly addresses what the user said they want to create
- [ ] Uses specific patterns/keywords discovered in research
- [ ] Ready to paste with zero edits (or minimal [PLACEHOLDERS] clearly marked)
- [ ] Appropriate length and style for TARGET_TOOL

---

## IF USER ASKS FOR MORE OPTIONS

Only if they ask for alternatives or more prompts, provide 2-3 variations. Don't dump a prompt pack unless requested.

---

## AFTER EACH PROMPT: Stay in Expert Mode

After delivering a prompt, offer to write more:

> Want another prompt? Just tell me what you're creating next.

---

## CONTEXT MEMORY

For the rest of this conversation, remember:
- **TOPIC**: {topic}
- **TARGET_TOOL**: {tool}
- **KEY PATTERNS**: {list the top 3-5 patterns you learned}

When the user asks for another prompt later, you don't need to re-research. Apply what you learned.

---

## Output Summary Footer (After Each Prompt)

After delivering a prompt, end with:

```
---
📚 Expert in: {TOPIC} for {TARGET_TOOL}
📊 Based on: {n} Reddit threads ({sum} upvotes) + {n} X posts ({sum} likes)

Want another prompt? Just tell me what you're creating next.
```
