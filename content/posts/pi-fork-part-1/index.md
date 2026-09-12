---
title: "Further Down the Token Rabbit Hole"
date: 2026-09-12
tags: ["ai", "tools", "claude", "coding", "llm", "pi", "opencode", "fable", "astra"]
categories: ["tech"]
description: "What would a real solution look like? And what if I didn't even have to build it?"
canonicalURL: "https://wynandpieters.dev/posts/pi-fork-part-1/"
cover:
    image: "posts/pi-fork-part-1/images/banner.png"
    alt: "Cover image for Further Down the Token Rabbit Hole"
    caption: ""
---

## Background

Okay, so this post is a follow up to a few different posts, but most notably [The Token Saving Cake is a Lie](/posts/the-token-saving-cake-is-a-lie/). Before reading this one, I highly recommend checking that out first.

Go ahead. I'll wait.

![Mr Bean Still Waiting GIF](images/waiting.gif)

Back? Good. Let's continue.

## Headroom

So I said that the solution is probably something like [Lynkr](/posts/the-token-saving-cake-is-a-lie/#what-lynkr-actually-does), but before building the integration, I discovered [Headroom](https://github.com/headroomlabs-ai/headroom) (45k stars, Python, its own trained compression model, ships RTK as a component). It does everything the Lynkr plan described, supposedly better. Set `ANTHROPIC_BASE_URL=http://localhost:8787`, run `headroom wrap claude`, done.

I used it in production for two weeks.

In the first six days I burned through $50 of extra usage credits. My normal burn rate is $25 over two to three months.

Anthropic's usage dashboard told the story:
- 31% of usage at >150k context
- 42% of usage from sessions active 8+ hours

Neither of those should be possible given how aggressively I manage sessions. I use `/compact` and `/clear` constantly. No task runs for 8 hours.

The only variables that changed were Sonnet 5 becoming the default model, and Headroom.

I uninstalled Headroom. One session later, back under 50% of my session limit on an intense coding task.

The culprit was almost certainly Headroom's memory and cross-session injection system, along with the Serena MCP it installed without my consent. Between whatever it was adding to every request and routing through Serena was far larger than what the compression was saving. A tool that compresses tokens while injecting enough cross-session memory to create 150k context windows is not saving tokens.

## The Real Problem

The Headroom experience clarified something that should have been obvious earlier: all of these external tools — Headroom, Lynkr, the proxy approach in general — are operating blind. They can see the API traffic but they can't see why it's happening. They don't know your session lifecycle. They don't know that `/clear` was supposed to reset state. They make inferences and inject context based on those inferences, and those inferences can be catastrophically wrong.

Routing that knows a request is part of a plan phase versus an execution phase versus a cleanup phase, *because it manages those phases*, is categorically different from routing that infers it by sniffing system prompts and counting tools.

The token problem can't be solved from outside the harness. It has to be inside.

## The Harness Problem

This is also when I paid attention to a number from a local AI setup video I'd half-watched (and can't find the link for, I'll edit the post if I find it again):

> Claude Code sends roughly 24,000 tokens of instructions before your question even arrives. OpenCode around 14,000. Pi about 1,000.

That 24k overhead is per-request, on every tool call, in every turn. For a frontier model it's a rounding error. For a local model handling subagent explore tasks, it's the whole budget. You can't route to a local model inside a harness designed for a model that can absorb 24k tokens of preamble and still reason clearly.

So I looked at OpenCode and Pi. OpenCode actively maintained, good community. Pi has 102k stars, MIT licensed, TypeScript monorepo with genuinely clean package separation: `pi-ai` (unified multi-provider LLM API), `pi-agent-core` (agent runtime), `pi-coding-agent` (CLI). The `chord` package is a service composition and plugin runtime.

Pi won.

## What Spotify Figured Out (The Right Mental Model)

Around the same time I found a [Spotify engineering post](https://engineering.atspotify.com/2026/9/portal-by-spotify-cut-my-claude-code-token-usage-by-90) about their internal Portal tool achieving 90% token reduction on Claude Code sessions.

The method wasn't compression. It was delegation.

A `PreToolUse` hook blocks file reads above a configurable line threshold and redirects Claude to a cheap worker model (Gemini Flash) that reads the files, answers the specific question Claude had, and returns structured bullets. Claude sees the answer, never the files. The corpus never enters Claude's context.

That's not a compression problem. It's a routing problem. Don't compress the 5,000 line Java file — don't send it to Claude at all. Have something cheaper answer the question and give Claude the result.

I'd been trying to solve compression. The actual opportunity is delegation.

Now. I will say this. I originally had this idea a few weeks after the previous blog post. But I sat on it. Because all my time went into CAST or client work. In the meantime, tools like Warp also brought [Custom Routers](https://docs.warp.dev/agents/inference/custom-routers/) to their Oz platform and built right into the terminal.

This validates everything I thought of, but never prioritised.

## The Plan

A fork of Pi with a local model complexity router built into `pi-agent-core` as a first-class feature.

The architecture is simple: before dispatching a request to the configured model, a small local model (configurable, e.g. a 3B Llama on Ollama) scores the request as `simple`, `medium`, `complex`, or `reasoning`. That score maps to a configured model tier. The scorer classifies, it doesn't complete — the whole call should be under 100ms with a small enough model.

```json
{
  "router": {
    "scorer": {
      "provider": "ollama",
      "model": "llama3.2:3b",
      "baseUrl": "http://localhost:11434"
    },
    "tiers": {
      "simple":    { "provider": "ollama",    "model": "llama3.2:8b" },
      "medium":    { "provider": "anthropic", "model": "claude-sonnet-5" },
      "complex":   { "provider": "anthropic", "model": "claude-sonnet-5" },
      "reasoning": { "provider": "anthropic", "model": "claude-opus-5" }
    },
    "fallback": "medium"
  }
}
```

Everything configurable. No hardcoded model names. The local model for scoring isn't mandatory — if Ollama isn't running the fallback tier kicks in.

I ran the same handoff doc through two frontier models — Fable 5.1 via Claude Code on my Pro subscription, and GPT-6 Astra via OpenCode on OpenRouter. Same brief, same Pi codebase, different models and harnesses.

## Results

If your are curious about the whole process, you can watch it here:

{{< youtube BvcvFPNsVBk >}}

### Build stats

| | Fable 5.1 (Claude Code) | GPT-6 Astra (OpenCode) |
|--|--|--|
| Effort level | Medium | Medium |
| Time to complete | ~7.5 minutes | ~11 minutes |
| Cost | $3.14 (extra usage credits) | $7.43 (OpenRouter budget) |

Fable finished 32% faster and cost 58% less. That cost difference is partly model pricing and partly the harness — Claude Code's 24k token system prompt overhead versus Pi's ~1k means Astra was spending more tokens per turn just on preamble.

### What each model shipped

Both implementations hit the same pre-existing issue in the Pi codebase: a typecheck error in `packages/ai/src/api/google-shared.ts` caused the `pi-ai` build to fail. Neither model touched that file — it's a pre-existing upstream issue, `npm run check` blocked on a missing `FinishReason.TOO_MANY_TOOL_CALLS` handler. Astra specifically noted it and flagged which other checks still passed. Both models correctly identified it as not their problem and worked around it.

On the routing implementation itself:

**Fable 5.1** reported live scoring against LM Studio with `google/gemma-4-e4b` — all five sample prompts correctly classified at roughly 150ms per call. Clean.

**GPT-6 Astra** reported 32 focused tests passed including live scoring and completion through LM Studio with the same model. More test coverage, more time, more money.

### Implementation comparison

I had an agent do a diff-level review of both implementations against the handoff spec. The short version: both wrapped the same seam — the `streamFn` passed into the Agent, which the agent loop calls on every turn including tool continuations. Everything else diverged.

| | Fable 5.1 | GPT-6 Astra |
|--|--|--|
| New source | 106 lines, 1 file | 286 lines, 2 files |
| Tests | 60 lines, 4 cases, unit only | 456 lines, 9 cases, unit + mocked SDK integration + opt-in live LM Studio |
| Docs | 26 lines in settings.md | 121-line dedicated doc + one settings.md row |
| Touched packages | `coding-agent` only | `agent` and `coding-agent` |

**Where the router lives** is the most architecturally significant difference. Fable keeps everything in `coding-agent`, wrapping the existing `streamFn` closure. Astra puts the core router in `pi-agent-core` and exports it from the package index, with a thin `coding-agent` adapter on top. Astra's implementation is reusable by any Agent consumer. Fable's is CLI-only.

**Scorer input** is the most consequential runtime difference. Fable sends the last 6 messages truncated to 2000 chars each. Astra sends the full transcript as JSON with images replaced by `[image]`. Astra's approach is more informed but will blow the local model's context window on long sessions — which is exactly when you'd want routing most. Fable's truncation ceiling is a deliberate and probably more reliable choice in practice.

**Prompt injection protection** is something Astra thought of and Fable didn't. Astra's scorer prompt explicitly tells the local model not to follow instructions found inside the serialized conversation. That's a non-obvious security consideration worth keeping regardless of which implementation ends up in production.

**Output parsing** reflects a meaningful philosophy difference. Fable does a loose regex for the first tier word anywhere in the reply. Astra requires the trimmed lowercased reply to exactly equal a tier word, and rejects any non-stop stop reason. Astra's will fail more gracefully on a confused local model; Fable's will more often return *something*.

**Failure degradation** follows the same pattern. Fable never throws — missing scorer, missing tier model, disabled flag all fall through to the session model. Astra throws at startup for config errors, and synthesises an error AssistantMessage at request time if a backend fails. Astra's behaviour is more correct. Fable's is less surprising.

**Session model handling** is the difference most likely to matter day-to-day. Fable leaves `/model` working but effectively ignores it when routing is enabled — the swap only happens at dispatch time. Astra replaces the session's initial model with the fallback tier and documents that routing overrides manual selection. Astra is more honest. Fable is less surprising.

**Neither implementation reclassifies across tool continuations.** If a request starts as `simple` but turns into a complex multi-file refactor mid-session because the initial read surfaced something unexpected, both stay on the simple tier for the duration. That's acceptable for a POC but it's the most likely source of "why did it use the cheap model for this?" in real use.

Both implementations explicitly rejected `chord` as the insertion point — Astra's doc records why: chord has no LLM pre-request hook, and `before_provider_request` fires after the payload is already built.

### What this tells me

The handoff asked for a POC. Fable delivered exactly that — minimal, clean, 106 lines. Astra delivered something closer to a beta feature at roughly three times the code, with validation, timeouts, and scope the handoff listed as optional or out of scope.

The more interesting data point than the implementation differences is the cost split to produce them: $3.14 on Claude Code vs $7.43 on OpenCode/OpenRouter for the same task at the same effort level. This shows the choice of harness matters. I wonder how Pi would have performed... Maybe I need to let another model attempt this as well.

That said, the path forward is probably a merge: Fable's architecture and minimalism, with Astra's timeout handling, config validation, and prompt injection guard folded in. That's 2-3 hours of work rather than starting from scratch with either.

*Routing accuracy benchmarks to follow once I've run both implementations against real sessions.*

### A telling footnote

When I asked both models to commit and push using conventional commits, their responses were revealing.

Astra immediately went and fixed the pre-existing `google-shared.ts` typecheck error before committing — it was not asked to, it just decided that pushing broken code was not acceptable and handled it. The fix was the same error handler both models had identified earlier.

Fable ignored the pre-existing error, committed cleanly, and pushed. When I explicitly asked it to fix things too, it went down a rabbit hole exploring the problem before eventually landing on the same error handler Astra had added in one shot.

Neither behaviour is wrong exactly. Fable did what it was asked. Astra did what was needed. The difference in disposition is the same one that showed up in the implementations — Fable optimises for the minimal correct answer, Astra optimises for the outcome being actually done.

Make of that what you will.

---

*I've pushed the branches to https://github.com/PiForgeZA/pi/ for review. I am in the process of manually reviewing and comparing them for a follow up post with the focus being on Fable vs Astra, similar to my previous [harness comparison](/posts/claude-code-vs-warp-dev-vs-junie) post.*