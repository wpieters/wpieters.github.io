---
title: "My weird (yet useful) streaming setup"
date: 2026-04-01
draft: true
description: "How I solved an issue I think people have but don't talk about"
tags: ["streaming", "setup", "productivity"]
categories: ["tech"]
canonicalURL: ""
cover:
    image: "posts/my-weird-streaming-setup/images/banner.jpg"
    alt: "Cover image for My weird streaming setup"
    caption: ""
---

## Intro

I've been streaming on and off for a while now, and I've been using a lot of different setups, but I think I've finally found the one that works for me.

The biggest issue I always seem to have is, how do I get a nice fancy streaming setup that works for Youtube and Twitch, with alerts and integrations and face cam and chat scenes vs gaming scenes, and so on and so forth, while also getting a clean and professional and high quality recording for my editing vids on my YT channel?

I suspect a lot of people use software that comes with their capture card, maybe a fancy one that records seperate from OBS? Or a capture card that records direct to SD or something? I've never been able to get that to work, probably because I have a cheap-ass Avermedia Live Gamer Mini and their software sucks. So, I always used to Record and Stream in OBS, sure with different quality settings, but the recording always had the streaming overlays and noise.

And then that just feels weird when I have to edit it. Also, that didn't work when I wanted to record something on the streaming desktop itself; piping it's own output into a capture card and viewing the output on the streaming desktop, that just felt like a weird inception. So what is a man to do?

## OBS Portable Mode

Enter portable mode. Which allows me to run two instances of OBS. Simple right? Never overcomplicate your problems 😉

Using one OBS instance with VOD specific scenes and high quality recording settings, and another OBS instance with streaming specific scenes and lower quality recording settings, I can have a clean recording for my editing vids and a streaming experience that works for both Youtube and Twitch.

But, there is an issue here, and I'm not sure if this was an OBS issue or a Windows thing, but I couldn't use the same capture card for both instances. So I _could_ just get another capture card, use an HDMI splitter, and then share things that way, but sheesh, wasn't I just complaining about needlessly overcomplicating things?

This is where the OBS Virtual Webcam setting is useful. My recording OBS configures a virtual webcam, and my streaming OBS uses that virtual webcam as the input. Simple. For some reason the capture cards audio can go to different instances, it's just the video that fails, with the OBS that was opened last just getting a black screen.

But there is still a problem...

## Encoders and cheap-ass GPUs

I was using the same GPU for both OBS instances, and I was using the same encoder for both. This was a problem, because the recording OBS needed to encode at a higher quality, and the streaming OBS needed to encode at a lower quality. This was putting strain on my GPU, which in this case was the RTX 3060 Mobile in my laptop. 
