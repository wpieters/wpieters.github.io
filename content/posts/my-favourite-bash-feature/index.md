---
title: "My Favourite Bash Feature"
date: 2023-05-04
description: "A quick tip about Bash history expansion that will make you look like a command line ninja"
tags: ["dev-to", "bash", "linux", "tips", "productivity"]
categories: ["tech"]
canonicalURL: "https://dev.to/wynandpieters/my-favourite-bash-feature-i5"
---

Just a quick one here, but I used this a lot today and realised after chatting to some other devs that a lot of people don't know about this trick.

That trick is: [History Expansion](https://www.oreilly.com/library/view/learning-the-bash/1565923472/ch02s06.html)

Let's say you are SSH'd (or SSM'd on AWS if you want to be more secure) onto a Linux box and you want check the status of a service with

```bash
systemctl status cassandra.service
```

You see that the service is exited for some reason, and you want to restart it after determining that's all that is needed.

Do you now go and type out

```bash
systemctl restart cassandra.service
```

or do you want to be 1337 and impress all your friends with

```bash
^status^restart
```

We all know you prefer the latter.

There are some variations on this, I recommend you check out the docs (or `man` pages for the really 1337 among you).

What are some of your favourite Bash tricks? 