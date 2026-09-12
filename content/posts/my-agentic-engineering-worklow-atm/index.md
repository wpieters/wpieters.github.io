---
title: "My Agentic Engineering Workflow ATM"
date: 2026-06-21
description: "A look at what tools I use and how I use them to be more productive as an engineer"
tags: ["ai", "macos", "tips", "productivity", "claude-code", "jetbrains"]
categories: ["tech"]
canonicalURL: "https://wynandpieters.dev/posts/my-agentic-engineering-worklow-atm"
cover:
    image: "posts/my-agentic-engineering-worklow-atm/images/banner.png"
    alt: "Cover image for My Agentic Engineering Workflow"
    caption: ""
---

## Tools

*Full comparisons and context in my [2026 AI tech stack post](/posts/ai-tech-stack-2026). This is just what you need installed to follow the workflow below.*

### Claude Code

If you're new, start with the [cheat sheet](https://awesomeclaude.ai/code-cheatsheet) and [Anthropic best practices](https://code.claude.com/docs/en/best-practices).

**Security — set this up first:**
- [Claude Code Security Hooks](https://github.com/slavaspitsyn/claude-code-security-hooks) — 7-layer prompt injection defence, read guards, canary files
- Lock down your `.env` and any [git-secret](https://sobolevn.me/git-secret/) files in `.claude/settings.local.json` before anything else

**MCP:**
- [Context7](https://context7.com) — library/API docs on demand
- [DeepWiki](https://mcp.deepwiki.com/mcp) — open source repo documentation

**Skills:**
- [Matt Pocock's skill set](https://github.com/mattpocock/skills) — `/grill-me`, `/handoff`, `/improve-codebase-architecture` (covered in detail below)
- [Understand Anything](https://github.com/Egonex-AI/Understand-Anything) — interactive code knowledge graphs
- [Ponytail](https://github.com/DietrichGebert/ponytail) — laziest-senior-dev heuristic, pairs well with `/improve-codebase-architecture`

**Agents:**
- [DocsExplorer](https://github.com/academind/claude-code-course/blob/main/.claude/agents/DocsExplorer.md) — handles docs lookup in a subagent without polluting main context

**Hooks / proxies:**
- [rtk](https://github.com/rtk-ai/rtk) — token reduction proxy, single Rust binary

**UI:**
- [Claude HUD](https://github.com/jarrodwatts/claude-hud) — status bar showing model, context size, active tools and agents

### Other tools

- **JetBrains** — for git, debugging and reviewing Claude's changes; [Claude Code plugin](https://plugins.jetbrains.com/plugin/27310-claude-code-beta-)
- **Warp.dev** — terminal; Warp Oz for hands-off tasks, Claude Code for hands-on

## Process

As I've mentioned in previous posts, my workflow is typically very different from what you'll see in the hype and social media posts. I don't typically work on monorepo, single stack, single language projects. My clients are typically full-on microservices with multiple languages and stacks. And beyond that, I still prefer IDEs over fancy pluggable text-editors, which often means I can't keep all the projects single scoped. 

What this means is that current favourites like [Air](https://air.dev/), [Conductor](https://www.conductor.build/), and [Antigravity](https://antigravity.google/product/antigravity-2) don't work for me.

So I've been solving my own problems, and this process I'm sharing today allows me to employ multiple agents working mostly independently on different repos towards a singular goal. I treat my agents like I would juniors or contractors; trust but verify. I give them tasks, but I have final sign-off.

### Folders and Worktrees

This has been one of the key things I've learned. Let's assume the project has five different repos for different microservices. Some of them are `trunk` based, others use `feature` branches, it doesn't really matter, but we assume there is a default main branch work always goes back to. I keep these in `~/Source/mains`

```
~/Source/mains/
├── CLAUDE.md
├── repoA           <--- frontend
│   ├── CLAUDE.md
│   └── README.md
├── repoB           <--- backend
│   ├── CLAUDE.md
│   └── README.md
├── repoC           <--- backend
│   ├── CLAUDE.md
│   └── README.md
├── repoD           <--- backend
│   ├── CLAUDE.md
│   └── README.md
└── repoE           <--- shared lib
    ├── CLAUDE.md
    └── README.md
```

Each repo has a `CLAUDE.md` file that contains the context and instructions for the agent. This is where I can add any specific instructions or context that the agent needs to know. Ideally that references readme as well as any other specs or ADRs that are relevant to the project.

If you are really lucky, there is a `.understand-anything` folder that can be referenced, but regardless, one of the first things I try and add is a CLAUDE.md in the root which ties all the repos and other CLAUDE.md files together.

### Implementing Features

Alright, so let's assume we have a feature request and we already know that it's going to span multiple repos, so let's say it affects two microservices (B and C) and one shared library (E). I'll start by creating a worktree in `~/Source/worktrees` for the feature (call it feat1) with whatever branch name convention the team follows. This allows me to work on multiple features or bugs at the same time without having to worry about conflicts.

```
~/Source/worktrees/
└── feat1
    ├── repoB
    │   ├── CLAUDE.md
    │   └── README.md
    ├── repoC
    │   ├── CLAUDE.md
    │   └── README.md
    └── repoE
        ├── CLAUDE.md
        └── README.md
```

For example, let's assume there is also a frontend bug fix that needs to happen, which touches repos A and C. I can create another worktree for that feature (call it bugfix1) and work on it in parallel.

```
~/Source/worktrees/
├── bugfix1
│   ├── repoA
│   │   ├── CLAUDE.md
│   │   └── README.md
│   └── repoC
│       ├── CLAUDE.md
│       └── README.md
└── feat1
    ├── repoB
    │   ├── CLAUDE.md
    │   └── README.md
    ├── repoC
    │   ├── CLAUDE.md
    │   └── README.md
    └── repoE
        ├── CLAUDE.md
        └── README.md
```

### Planning and Coordination

Okay, so pretty boring stuff so far, but here comes the magic. Now that I have all the repos set up in my worktree, I can start planning and coordinating the implementation. For this, I usually start with a new `claude` session in the mains directory. Why? Because it has all the context, and if I missed something in my initial assessment, it can tell me what I missed and help me fill in the gaps.

So I start a new session, switch to Plan Mode (because I use the `/model opusplan` mode and I want to use the best for planning), and then start with a `/grill-me` or `/grill-with-docs` if I have a PRD already, and we bash that out until I'm happy with the proposed plan. But since I don't want this session to handle the work, because it's overloaded by context, I give it the following command instead.

> /handoff separate agents will be handling each repo, so create a handoff doc with enough context from the plan for each agent to handle their part independently

This creates a handoff document that I can copy into `~/Source/worktrees/feat1/handoff.md` and then use to coordinate the implementation, without overloading the context of each agent's session. 

This next bit is important; I keep the mains session open and do a `/clear` to start from scratch. This session is my overwatch. It knows the big picture and can coordinate the agents when something comes up.

```
~/Source/
├── mains               <---- Overwatch session runs here
│   ├── CLAUDE.md
│   ├── repoA
│   │   ├── CLAUDE.md
│   │   └── README.md
│   ├── repoB
│   │   ├── CLAUDE.md
│   │   └── README.md
│   ├── repoC
│   │   ├── CLAUDE.md
│   │   └── README.md
│   ├── repoD
│   │   ├── CLAUDE.md
│   │   └── README.md
│   └── repoE
│       ├── CLAUDE.md
│       └── README.md
└── worktrees
    ├── bugfix1
    │   ├── handoff.md
    │   ├── repoA
    │   └── repoC
    └── feat1
        ├── handoff.md  <---- generated by overwatch
        ├── repoB       <---- dev session run here
        ├── repoC
        └── repoE
```

Alright, so with that in mind, let's actually do the thing.

### Execution

For this part, it kinda depends on how complicated the implementation is, but generally, I'll start by creating a new `claude` session for each repo in the worktree. I will make sure it has permissions to access the handoff file. Then, depending on the complexity, for each agent I'll do one of the following.

For simple tasks:

> /goal you have [repo name], have a look at @handoff.md and determine what to implement, and check yourself against criteria set for you

For slightly harder tasks, I switch to Plan Mode:

> you have [repo name], have a look at @handoff.md and determine what to implement, and present a plan for me to review. ask questions where you are unsure.

I don't tend to use `/grill-me` here again, since we've already done that, and the handoff doc should have all the context we need. Once I have the plan, I check if there is any cross coordination needed between the agents, and if so, I will go back to my main overwatch session, and have it do a review for me, since Claude stores all plans in `~/.claude/plans/` by default.

```
~/.claude/plans/
└── you-are-investigating-the-snuggly-sundae.md
```

> two agents are working on [feature we discussed] based on this handover you generated @handoff.md - please review their individual plans and coordinate any cross dependencies. the plans are [@agent1plan.md] and [@agent2plan.md]. update the plan files if needed.

If there were any updates, I'd go back to the agents and have them review the updates and prompt me again before proceeding in Auto Accept mode, which I'm comfortable with since I've already reviewed the plans and have my Claude setup with various security checks and hooks.

### Testing and Validation

I won't bore you too much here, because this is highly dependent on the project and the testing strategy. Generally, I'll have the agents run the unit tests and make sure they pass. If there are any issues, I'll go back to the agents and have them fix them.

For UI things, if Playwright is available, I'll have the agents run using the MCP server, otherwise I just end up testing manually. At the end of the day, I'm still the one responsible for the quality of the code, and I need to make sure it's good before logging my pull/merge request.

The important bit here is actually when something goes wrong. Because we might have a scenario where both agents need the context of the thing that's broken, for review purposes, I'll actually start a new reviewer session in for example `~/Source/worktrees/feat1` and tell it

> different claude agents implemented the feature in @handoff.md, and we are experiencing [bug or issue]. please investigate and fix it. you have access to all the relevant repos, and you can see their plans @~/.claude/plans/[plan name 1].md and @~/.claude/plans/[plan name 2].md if you need the context.

This allows me to use a clean slate to investigate the issue, putting us back in the "good" part of the context window, and have cross repo coordination in case a change needs to be made in more than one place.

```
~/Source/worktrees/
├── bugfix1
│   ├── repoA
│   │   ├── CLAUDE.md
│   │   └── README.md
│   └── repoC
│       ├── CLAUDE.md
│       └── README.md
└── feat1               <---- reviewer session here
    ├── handoff.md
    ├── repoB
    │   ├── CLAUDE.md
    │   └── README.md
    ├── repoC
    │   ├── CLAUDE.md
    │   └── README.md
    └── repoE
        ├── CLAUDE.md
        └── README.md
```

Even if testing went well though, I usually also run this one at least once:

> please review the last git diffs and commits on this repo and compare the implementtion to @handoff.md and highlight anything that might have been missed or not correct

### Improvements

On a side note, a feature I've been playing with but don't quite have working the way I want is `/loop`. This is pretty cool, you can run a prompt or slash command on a recurring interval (e.g. /loop 5m /foo). What I've tried doing is having something in the CLAUDE.md of each repo which says 

> "If you are stuck and need high level questions answered, put your question in a file called @~/Source/mains/feat1-questions.md."

I then, in the overwatch session, have a loop running that checks for new questions and answers them in the same file. When that's done, I can tell the worker agent to look for the answers and continue with its work. I love this idea, but it doesn't quite always work the way I want it to. But still, it's a cool coordination experiment.

Ideally you want just a `/loop 5m look at @~/Source/mains/feat1-questions.md and if your question is answered continue with your work` ... that would be amazing. But alas, no luck getting it working consistently yet.

## Summary

Okay, damn, that was a wordy blog post. Ultimately for me, it comes down to handling my agents the way I would my humans. If I gave two juniors different repos to work on, I'd make sure to have a mid or senior dev to review their work, and a lead or a PM to coordinate their efforts. Ultimately, all this is a just a manual stopgap until I have [CAST](/posts/cast-intro) [working](/posts/cast-first-blood) though.

What I also love about this approach is that it doesn't limit me to just using Claude. Any harnass that can have the skills installed, and that have the security bits in place can be used. I can swap out my IDEs and editors, the harness I use, the LLM model, all of that. The process matters more than the tools.

With that, I hope this gives you some ideas of how you can use Claude Code to be more productive. I'm sure there are better ways to do this, and I'm always looking for feedback and suggestions. I think the closest I've found is probably [Gas Town](https://github.com/gastownhall/gastown) by Steve Yegge, and there is some overlap in the way things are structured. If you have any other suggestions or questions for me, please reach out to me on Twitter or LinkedIn. But in the meantime, happy coding.

---
*This post was inspired by others like the one from [Harper Reed](https://harper.blog/2025/02/16/my-llm-codegen-workflow-atm/) as well as from discussions with my teammates at [NetNineNine](https://netninenine.co.za), and from working on CAST.* 