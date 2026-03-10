---
title: "Two AI Agents Walk Into a Codebase"
date: 2026-03-10
description: "They don't talk to each other"
tags: ["ai", "coding", "productivity", "linkedin"]
categories: ["tech"]
canonicalURL: "https://www.linkedin.com/posts/wynand-pieters_two-ai-agents-walk-into-a-codebase-they-activity-7437045079204167680-I5Nj"
cover:
    image: "posts/two-ai-agents-walk-into-a-codebase/images/banner.png"
    alt: "Cover image for Two AI Agents Walk Into a Codebase"
    caption: ""
---

That's the punchline and the problem.

I've run into these two similar scenarios which have had me stumped and annoyed. 

- In one, I'm running Claude Code in both GoLand and PyCharm, trying to integrate one project's API with another project's GRPC service.
- In the other, I have a Scala library that needs updates, and then multiple services that depend on that library.

The hiccup: each instance is completely unaware of the other's existence. 

Change an API contract in one? The other has no idea. Consumer has an error? Can't tell the producer. How do I get the downstream to update when the upstream changes, and how do I report errors back up the chain?

I haven't really had this issue on mono-repos, they have all the context in one place. It's only when working with separate microservices that have me spinning up multiple Claude Codes and trying to orchestrate things between them.

My current fix: Shared files (`/add-dir`) + comprehensive docs + manual prodding. It works, but feels like duct tape. And sometimes I want my agents to work without me.

Better options exist: → Agent Teams (experimental P2P communication) → Headless CLI orchestration → MCP server bridging → Git-based coordination

I've also seen some interesting Github projects that could help, and some new agent tooling from Warp looks cool, but I also suspect just changing how I use folders and repos could help, e.g. each feature has a folder with all the related repos checked out, then a single Claude Code with sub-agents maybe? 

I plan to run some experiments with all of them, then writing a full breakdown blog post with real code, configs, and token costs. But that might take a while.

So...

Question for the group: How are you handling multi-agent coordination across projects? What tools? What's actually working?

Something else I'd really like to get peoples feedback on; testing changes. 

For a lot of my projects I already have Bash helper scripts for testing (e.g. call this API then check the output then call something else or grep this log etc), so adding that to CLAUDE.md helps, but for more complicated microservices where I might want to watch for debug logs in two or three different places, and then check the database... 

How are you instructing your agents to do these things? Skills? Commands? Prompt every time?

Let me know in the comments section on the original LinkedIn post 👇

---
*This post was originally published on [LinkedIn](https://www.linkedin.com/posts/wynand-pieters_two-ai-agents-walk-into-a-codebase-they-activity-7437045079204167680-I5Nj)*