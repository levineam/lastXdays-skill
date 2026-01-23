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

Before doing anything, parse the user's input for:

1. **TOPIC**: What they want to learn about (e.g., "web app mockups", "Claude Code skills", "image generation")
2. **TARGET TOOL** (if specified): Where they'll use the prompts (e.g., "Nano Banana Pro", "ChatGPT", "Midjourney")

Common patterns:
- `[topic] for [tool]` → "web mockups for Nano Banana Pro" → TOOL IS SPECIFIED
- `[topic] prompts for [tool]` → "UI design prompts for Midjourney" → TOOL IS SPECIFIED
- Just `[topic]` → "iOS design mockups" → TOOL NOT SPECIFIED, that's OK

**IMPORTANT: Do NOT ask about target tool before research.**
- If tool is specified in the query, use it
- If tool is NOT specified, run research first, then ask AFTER showing results

**Store the TOPIC** - you'll extract or ask about TARGET_TOOL later:
- `TOPIC = [extracted topic]`
- `TARGET_TOOL = [extracted tool, or "unknown" if not specified]`

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

**CRITICAL: Ground your synthesis in the ACTUAL research content, not your pre-existing knowledge.**

Read the research output carefully. Pay attention to:
- **Exact product/tool names** mentioned (e.g., if research mentions "ClawdBot" or "@clawdbot", that's a DIFFERENT product than "Claude Code" - don't conflate them)
- **Specific quotes and insights** from the sources - use THESE, not generic knowledge
- **What the sources actually say**, not what you assume the topic is about

**ANTI-PATTERN TO AVOID**: If user asks about "clawdbot skills" and research returns ClawdBot content (self-hosted AI agent), do NOT synthesize this as "Claude Code skills" just because both involve "skills". Read what the research actually says.

Identify from the ACTUAL RESEARCH OUTPUT:
- The top 3-5 patterns/techniques that appeared across multiple sources
- Specific keywords, structures, or approaches mentioned BY THE SOURCES
- Common pitfalls mentioned BY THE SOURCES

---

## THEN: Show Summary + Invite Vision

**CRITICAL ORDER**: Display sections in this EXACT sequence (insights FIRST, stats LAST):

```
---
What I learned:

[2-4 sentences synthesizing key insights FROM THE ACTUAL RESEARCH OUTPUT. Quote or paraphrase what the sources said. If sources mention a specific product (ClawdBot, Cursor, etc.), use that name - don't substitute your own knowledge. The synthesis should be traceable back to the research results above.]

---
📊 Research Complete

Analyzed {total_sources} sources from the last 30 days
├─ Reddit: {n} threads │ {sum} upvotes │ {sum} comments
├─ X: {n} posts │ {sum} likes │ {sum} reposts
└─ Top voices: r/{sub1}, r/{sub2}, @{handle1}, @{handle2}

---
Share your vision for what you want to create and I'll write a thoughtful prompt you can copy-paste directly into {TARGET_TOOL or "your tool of choice"}.
```

**Use real numbers from the research output.** The patterns should be actual insights from the research, not generic advice.

**SELF-CHECK before displaying**: Re-read your "What I learned" section. Does it match what the research ACTUALLY says? If the research was about ClawdBot (a self-hosted AI agent), your summary should be about ClawdBot, not Claude Code. If you catch yourself projecting your own knowledge instead of the research, rewrite it.

**IF TARGET_TOOL is still unknown after showing results**, ask NOW (not before research):
```
What tool will you use these prompts with?

Options:
1. [Most relevant tool based on research - e.g., if research mentioned Figma/Sketch, offer those]
2. Nano Banana Pro (image generation)
3. ChatGPT / Claude (text/code)
4. Other (tell me)
```

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
