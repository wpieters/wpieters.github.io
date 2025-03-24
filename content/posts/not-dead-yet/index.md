---
title: "Not dead yet...💀🎲"
date: 2024-04-20
description: "Development update on technical challenges with Unity URP and version control for So This Is How I DIE"
tags: ["itch-io", "so-this-is-how-i-die", "gamedev", "devlog", "technical-issues"]
canonicalURL: "https://duhblinnza.itch.io/so-this-is-how-i-die/devlog/717988/not-dead-yet"
---

Hey everyone.

I've been wanting to do an update, but was hoping to do so once I had more to share.

I've been working on the game occasionally over the last few weeks, but I've run into a ton of issues, mostly my own fault, but not all of them.

I've started addressing some of the issues that people raised, and this was going well until I tried to make a new build. I mentioned in one the last posts just before I released that upgrading to URP in Unity meant my lighting was broken, and builds took longer because I had to manually bake lighting.

This solved the problem for the release, except on Linux, since I'm building in a VM and that doesn't get passthrough to my GPU, but it worked. Until it didn't. I cannot get a build working after making the latest UI changes, and I'm spending a lot off time learning about the Render Pipelines and reading through forum posts to figure why this is broken now.

The other issues I have is also regarding the URP; I purchased the VFX asset I had my eye on, and that one comes with a separate tool needed to convert from SRP to URP, and this is crashing my Unity on both my Desktop and my Laptop. Again, no idea why. URP is just messing with me.

![Screenshot of Light Baking with 6 days remaining](https://img.itch.zone/aW1nLzE1ODQwMTAzLnBuZw==/original/n5wAP2.png)

On the topic of the Desktop, my destop is more powerful (12 Core/24 Thread Ryzen with RTX 3070) compared to my Laptop (8 Core/16 Thread Ryzen with RTX 3060 Mobile) which I've been using for most of the development. I thought moving to the more powerful rig will help, but then I discovered issue number one; not everything I needed was committed and pushed into version control. 

I eventually decided the safest play was just to commit everything, including assets, and boy oh boy was that a stupid idea. Issue 2; I hit the Bitbucket 4GB repo size limit. This made migrating things much harder, and I eventually setup a weird little Git "mirror server" on my network so everything goes to that machine, which I then push to Bitbucket again, but I learned that maybe my assets that I can simply download from the store should not be part of my VCS...

Okay, and that's where I'm at. I cannot get the VFX to convert and build and URP has broken my lighting and I don't know why. This means that anything I do is not in a releasable state, even though some things work in the Game Preview.

Oh, also fun, trying to run Chrome and Unity at the same time is maxing out my 32GB RAM. Funsies. Guess I'm switching back to Firefox again.

But with that said, I am working on some things and doing my best to figure out what is going wrong so I can put out a new version with some UI improvements within the next week or two.

I hope 🤞

---
*This post was originally published on [itch.io](https://duhblinnza.itch.io/so-this-is-how-i-die/devlog/717988/not-dead-yet)*
