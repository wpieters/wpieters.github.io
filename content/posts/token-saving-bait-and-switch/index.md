---
title: "Token Saving Bait and Switch"
date: 2026-04-15
description: "Token savings are not what you think they are..."
tags: ["linkedin", "tech", "ai", "future-of-tech", "opinion"]
categories: ["tech"]
canonicalURL: "https://www.linkedin.com/posts/wynand-pieters_ai-claudecode-tokenoptimization-activity-7449705482128257024-1jYd"
cover:
    image: "posts/token-saving-bait-and-switch/images/banner.jpg"
    alt: "Cover image for Token Saving Bait and Switch"
    caption: ""
---

Just because your fancy new Claude Code plugin prints out "50% tokens saved" in the logs doesn't mean you actually got them.

I was playing around with Caveman Compression last week and tried to get the `UserPromptSubmit` hook to rewrite prompts using the tool for savings across the board. Turns out? The hook doesn't allow it.

Probably for security reasons; can you imagine the injection attacks if any random plugin could hook into and replace your prompts?

Truth is, you can only add additional context. Which actually increases token usage, not decreases it.

And there are a lot of public repos on GitHub using this technique, claiming token savings. But with some very basic testing yourself, you can prove this doesn't actually work. Just modify the hook, and watch Claude ignore it.

I'm shocked that people aren't making a bigger deal about this.

Saving tokens is like the current meta for agentic engineering, right? So how is it that so few people are actually doing it right? Not even `PostToolUse` or `SubagentStop` events really let you rewrite outputs.

I see only two logical approaches left:
- Don't run Claude Code interactively - pipe already-compressed input into `claude -p` and compress the output after (this defeats the point of interactive agents, but works for other tools too)
- Have a proxy layer between Claude Code and the cloud models using ANTHROPIC_BASE_URL to point to the proxy and handle compression/decompression there (but I don't think you can make this work with a subscription, only API keys)

Neither of these are particularly elegant or practical for most use cases.

So question for the class: What are you doing to save tokens? And how confident are you that it actually works?

Because I'm seeing a lot of plugins claiming savings that don't materialize, techniques that increase tokens while claiming to reduce them, and hype without verification.

If you've actually measured and verified token savings in Claude Code (or any AI agent tool), I'd genuinely love to hear how you're doing it.

Otherwise, maybe we should stop treating "token compression" plugins as gospel and start asking for receipts.

.

.

.

#AI #ClaudeCode #TokenOptimization #AIAgents #SoftwareDevelopment #TechSkepticism #DevTools #AIProductivity #TechReality #Anthropic

---

EDIT: LOL. Of course. Now that I've posted about it, now I get all the other LinkedIn posts about how the token savings are a lie. Of course, the reason I asked the question is because it's never binary, there are things that work. The `rtk-ai/rtk` repo uses PreToolUse and seems to help, using Caveman Compression on my CLAUDE.md and SPEC.md and similar files helps minimise the repeating token usage, which adds up over time. But there's gotta be more we can do.

---
*This post was originally published on [LinkedIn](https://www.linkedin.com/posts/wynand-pieters_ai-claudecode-tokenoptimization-activity-7449705482128257024-1jYd) and prompted by [my own more detailed post](/posts/the-token-saving-cake-is-a-lie/)* 