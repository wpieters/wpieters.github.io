---
title: "Slow and steady wins the race?"
date: 2025-02-24
description: "Progress update on UI tweaks and VFX animations for skills in So This Is How I DIE"
tags: ["itch-io", "so-this-is-how-i-die", "gamedev", "devlog"]
canonicalURL: "https://duhblinnza.itch.io/so-this-is-how-i-die/devlog/892967/slow-and-steady-wins-the-race"
---

Hey folks. Just a quick update that I finally have everything configured and running again on my new laptop. I've also just come off a crazy amount of overtime, so hopefully these updates will become more frequent.

I'm still mostly working on UI tweaks, and adding VFX to the skills. The attached video shows the first attempts at the latter, with the healing animation mostly working (bar some timing issues) and the blades effect for the ultimate (which needs to move and scale a bit, not super visible).

{{< youtube 7P9zLT1VaGo >}}

The VFX was trickier than I thought; I had a lot of issues getting them to work with URP, despite the creators sharing a tool for converting things. Now that it's working I can more easily start playing around with things. I realised that I need to make some more changes to how I invoke them though, since they tend to be roughly in the right place based on the target, but some effects need to be angled correctly, and some AoE effects need to be resized and lifted up above the enemies.

Digging down into this further I'm also realizing how poorly designed some of my functions are. The timing issues with animations and damage are getting worse, and sound effects are triggered too early or too late... Definitely finding where my skills are lacking and more experimentation is needed.

---
*This post was originally published on [itch.io](https://duhblinnza.itch.io/so-this-is-how-i-die/devlog/892967/slow-and-steady-wins-the-race)*
