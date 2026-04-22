Hey @levineam 👋

I ran your skills through `tessl skill review` at work and found some targeted improvements. Here's the full before/after:

| Skill | Before | After | Change |
|-------|--------|-------|--------|
| last-x-days | 64%* | 90% | +26% |

*\*The original SKILL.md had a YAML frontmatter parsing error (`argument-hint` value with unescaped special characters) and a non-kebab-case name (`lastXdays`), which caused the review to fail outright (10%). I fixed the YAML syntax and renamed to `last-x-days` to get a valid baseline of 64%, which is the "before" score used above.*

![Skill Review Score Card](score_card.png)

<details>
<summary>What changed</summary>

- **Fixed YAML frontmatter**: Properly quoted the `argument-hint` value and `description` to resolve parsing errors
- **Renamed skill to kebab-case**: `lastXdays` → `last-x-days` to follow naming conventions
- **Expanded description**: Added explicit "Use when..." clause, natural trigger terms (trending, recent news, community sentiment), and specific output actions
- **Removed redundant section**: Dropped "How It Works" — the usage examples and execution commands already convey this
- **Concrete output docs**: Replaced vague "Same format as last30days" with actual output file paths and descriptions from the SPEC
- **Added dependency guidance**: Documented required API keys and what to check if the script fails

</details>

Honest disclosure — I work at @tesslio where we build tooling around skills like these. Not a pitch - just saw room for improvement and wanted to contribute.

Want to self-improve your skills? Just point your agent (Claude Code, Codex, etc.) at [this Tessl guide](https://docs.tessl.io/evaluate/optimize-a-skill-using-best-practices) and ask it to optimize your skill. Ping me - [@yogesh-tessl](https://github.com/yogesh-tessl) - if you hit any snags.

Thanks in advance 🙏
