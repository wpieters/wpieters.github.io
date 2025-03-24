---
title: "Local LLMs are about to change big time (probably for the better)"
date: 2025-03-12
description: "Exploring the implications of Apple's new Mac Studio with M3 Ultra for local LLM capabilities"
tags: ["linkedin", "ai", "llm", "apple", "hardware"]
canonicalURL: "https://www.linkedin.com/posts/wynand-pieters_applesilicon-m3ultra-macstudio-activity-7305140487173136384-F73i"
cover:
    image: "posts/local-llms-are-about-to-change-big-time/images/banner.jpeg"
    alt: "Cover image for Local LLMs are about to change big time"
    caption: ""
---

Wow, the new Mac Studios look really impressive. I can't find the post that I saw last night about it, so I can't share that author's thoughts (I thought I commented or reacted at least, apparently not...), but what was interesting is the M3 Ultra goes up to 512GB of unified memory.

512GB...

Of unified memory...

If you've been playing with LLMs locally, and you have Apple Silicon, you'll know that Apple's unified memory is pretty performant, and it's great for running local LLMs because the GPU uses the same system memory as the CPU, which means it can access most if not all of the RAM.

That means that the top spec M3 Ultra can run `llama3.1:405b` without breaking a sweat, since it only needs around 243GB 😲

It can run `deepseek-r1:671b` which needs around 404GB 🤯 Which means you'll still have memory left over to run Chrome 😜

💰 And when you compare the local South African pricing, and see that the 96GB M3 Ultra Mac Studio is on pre-order for R91k, and the 48GB Nvidia RTX 6000 ADA goes for R150k, it seems that Mac Studio clusters will become the more cost effective option for SMBs who want to run and train models in house.

I'd be very excited to see some experiments and performance comparisons.

Not from me, mind, I don't have that kinda cash just hiding in my sofa... But I would love to see it.

Exciting times ahead, methinks.

---
*This post was originally published on [LinkedIn](https://www.linkedin.com/posts/wynand-pieters_applesilicon-m3ultra-macstudio-activity-7305140487173136384-F73i)*
