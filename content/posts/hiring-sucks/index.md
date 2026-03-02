---
title: "Hiring in Tech Sucks: Here's What Actually Worked For Me"
date: 2026-03-02
description: "My experience with hiring in tech across a range of companies and roles"
tags: ["tech", "hiring", "operations", "opinion"]
categories: ["tech"]
canonicalURL: "https://wynandpieters.dev/posts/hiring-sucks/"
cover:
    image: "posts/hiring-sucks/images/banner.png"
    alt: "Cover image for Hiring Sucks"
    caption: ""
---

## The Problem

Hiring has been coming up a lot in some of my social feeds again. 

It's a problem I've experienced, and seen often from both sides of the table. 

And most companies don't get this right.

Do I have all the answers? No. But I do have some thoughts.

## The Trigger

The conversation trigger for this post was:

> "During the developer hiring process, has anyone found an alternative to take-home code tests and live code tests? Companies have stopped using take-home tests as they believe that it's too easy to cheat using LLMs. About 70% of technical hiring managers that I've discussed this with admit that live code (and architecture) tests aren't working, as too many skilled developers produce poor results due to the artificial time limit."

There were some interesting responses, ranging from:

> "The artificial time limit is a real problem for me. I've had a few interviews where I bombed during the interview, but had the answer maybe 30 minutes after when my heart rate had gone down."

to:

> "In other words, using Google/LLM is not cheating, you're just testing the wrong things."

as well as:

> "For a senior, do you really care that they can write FizzBuzz on a time limit anymore? And should a senior be judged with the same stick that lets a junior pass to the next stage of interviews?"

At the end of the day, hiring in tech is broken. Unless you're applying for Google where everything is planet scale, you probably don't need to go all in on the types of details that they do (optimization, algorithms, thinking process). What you probably need to figure out is: (a) can they do the work, and (b) can they fit into your team?

## Addressing the Comments

But before I get into what worked for us, I'd like to address some of those comments.

First, **juniors, mids, and seniors should definitely be held to different standards.** They fulfill different roles and should be tested for those roles. Juniors especially. You're looking for more of an attitude and willingness than anything else. I also tell my people: "I can teach you skills, but I can't teach you excitement, commitment, drive, and to not be an asshole."

Second, **do I care if you can code FizzBuzz in your sleep?** Probably not. But if you say you are a master of SQL, I'm gonna ask you to do FizzBuzz in SQL, because yes, [it's possible](https://www.cathrinewilhelmsen.net/solving-fizzbuzz-using-sql/).

Third, **Google, Stack Overflow, and ChatGPT are not cheating if these are tools that the company allows you to use.** But as I've [said for so long](https://dev.to/wynandpieters/my-love-hate-relationship-with-chatgpt-the-unexpected-cost-of-productivity-76m), these things are more often than not crutches rather than tools. So we should be testing [fundamental knowledge and skills](https://dev.to/wynandpieters/ai-tools-are-we-replacing-skills-or-enhancing-them-28n), not just tool usage (although how they use these things is important too).

One more thing. 

I made a post about [devs vs. engineers](https://dev.to/wynandpieters/why-youre-not-an-engineer-also-why-it-probably-doesnt-matter-egb) some time back, and I mentioned there something I feel is relevant here as well: the role you are hiring for matters, and the experience and background of the candidate does too. Each interview is unique. We spend twice as much time preparing for an interview as we do in the interview. 

Why? Because it all depends. 

![Say the Line Bart meme with context of senior developer saying "It Depends"](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/patbb2hvv28bw4a6lwqb.jpeg)

If I need someone who can build with engineering principles, but my candidate is a self-taught wannabe script kiddie, it's not gonna work, because that's not what I'm hiring for. Prep matters. Role matters. Experience matters. This is why so many candidates that go shotgun approach either end up in bad roles or go through so much rejection.

## My Experience

I've done hundreds of interviews for different companies at different stages of growth with different requirements, from state owned companies to start-ups and scale-ups, with the one exception being large multi-national corporations, where I only have experience on the other side of the table.

That said, I've had the privilege of working with the same people across different companies and industries and company sizes, where we were both involved in hiring, and we developed what I thought was a pretty good system, which tested the most important things. Here are some key takeaways.

### The Three-Person Interview Team

You need at least three people in the room, not only to break consensus, but also because you need a few things.

First, **you need a technical expert**. Someone who can deep-dive into any topic that comes up. That annoying know-it-all in your company (you all know who he is), he is invaluable in spotting the BS. If a candidate can't even speak about the things on their CV, then what's the point? You need to figure that out.

Second, **you need someone who can read people**. Someone who can discern between faking it and stress. Often, an HR person is the right one here, but you'll probably know someone on the team who is just a "people person" and gets along with everyone. They could be useful here.

Lastly, **you need an antagonist**. Someone who will rub this person the wrong way; maybe not as extreme as "Gina looking for new IT security person" way ([reference](https://www.youtube.com/watch?v=FyPDVLSHQqA)), just in a "can this candidate deal with some friction" way. Maybe it's that guy that chews loudly or sniffs constantly because of his allergies, or he plays with his fidget toys while pretending to listen, or they just have that abrasive "I don't agree with you" type personality.

This last one is probably the least necessary, since most candidates are plenty stressed already. Depending on who you are interviewing and what role this is for, you can just another technical person (for technical roles) or another people person (for lead or operation roles).

### Test What They Know, Not What They Don't

You have their CVs and socials and work experience. **You want to know if they're really familiar with what they claim to be**. If they rank themselves expert in Java, you damn well know I'm gonna ask them to explain the different garbage collection methods and memory generations. And if you don't know it, you dug your own grave.

And here of course juniors are tested differently, but hopefully you remember enough of the fundamentals to query them on what they should have just learned. Or best case, they have projects on Github you can dig into. Ask them about those.

If they do well on the things they know, and there's time, we can venture into what they'll be doing and dig into that a little. But that also leads into the next point.

### Questions Are Not Pass/Fail; They're a Conversation

We had standard questions that got increasingly more difficult, and they would test what level someone is at. We use this **to figure out how much onboarding and training they'd need, not whether they fail a test**. We want to assume that every candidate is the one, and we treat them that way. Assume they know what they're doing, until proven otherwise.

The conversation bit is important as well, because **this isn't just about technical competence, but also about communication**, and how they handle pressure. I [posted on LinkedIn](https://www.linkedin.com/posts/wynand-pieters_softwareengineering-ai-futureofwork-activity-7415311087723843584-6bwx?utm_source=share&utm_medium=member_desktop&rcm=ACoAAALRdvUB-ZlGxXp4zNgaZSFrG5hKPM5Z_jU) about how "words matter" and what we say is important, being able to communicate well is a skill we are losing in the online world, and probably losing it faster in the age of AI. Communication is key in teams, especially remote ones, and conversations are how we gauge that.

### Pair Programming Over Whiteboarding

I've always found whiteboard coding to be a pain. The stress gets to people, they miss having their IDE for boilerplate things, some people just study the standard problems and fake their way through it, and it doesn't reflect how we work. The same with take-home assignments. **Unless they'll be working solo and remote, it's not a scenario they'll deal with, so why test it?**

Rather, bring a laptop and pair program. Take them through an already merged PR in a repo that isn't giving away IP or requires an NDA, and just code with them. Solve with them. And this is also where having someone else from the team they're interviewing for available is also useful. Have them actually work with someone they'll be working with. It helps. Not only in showing competence, but also again, in gauging their communication skills.

### Adaptive Question Buckets

We also have a few brain teasers or puzzles ready, and generally as a hiring group we'll figure out where we have some questions left, and then we pick from the buckets. Not sold on problem-solving ability? Choose a brain teaser and see how they think through it. Not sold on their technical skill? Choose an algorithm question. Happy with that and just want to push their boundaries? Here's a real-world scaling problem we solved, how would they approach it.

### The Dealbreaker Question (and What You're Most Proud Of)

Bonus tip, because I got this from a hiring manager and I actually found it to be indispensable. **Right at the end, ask them: "What is your dealbreaker? What would make you quit in the first month?"**

This helps you figure out their motivations, their morals, their ideals, and their values. I've had answers range from "if you get in my way" (immediate no) to "if we're involved in criminal activity" (hell yes). You'll be surprised what people come up with.

Oh, and we also use that question as the signal to cut an interview short. If any of the interviewers throw that in before the time is up, they've given up and it's already a no from them.

An alternative or additional question I almost forgot about is "What's the thing you are most proud of?" Sometimes it's a work thing, sometimes it's a personal thing, and it tells you a lot about them. Because if they are just telling you what they think you want to hear, and you have your people reader in the room to spot it, that tells you a lot too.

### All Yes or No

The last thing we decided, and this is more controversial, is that **unless an interview is "yes" from all three interviewers, it's a no**. Even one "maybe" makes it a no.

And have we missed out on good candidates this way? Yeah, of course. And sometimes we'll never know if we were wrong. But at least in South Africa, it's really hard to get rid of bad hires, so many companies will err on the side of caution. And we've built fantastic teams over the years, and I've loved all the people I've worked with.

At this company in my teams, we've had one poor fit in the team, and I still hate that we couldn't make it work, because the kid was smart, just not a good fit. But it was a hire with two yes's and one maybe. So maybe that tells you everything.

## What Not to Do: Lessons from the Other Side

While drafting this I recalled some anecdotes from an interview that I failed, where I was fortunate to be able to have a conversation with the recruiter afterwards. 

I was interviewing for a very senior and very technical position, and being on the other side of an interview there were a few things in their process that bothered me; mistakes other people can avoid.

**Don't cram 10 questions into a 60-minute interview**. You don't get to have a conversation, and it means you are looking for specific answers and will exclude candidates with good problem-solving skills that need to think about problems.

**If you are interviewing for senior roles, don't "fail" them if they skip the "obvious" things and jump straight into real problems**. If my query is slow, it won't be because of something as trivial as the very first thing everyone looks at. I'm not gonna think about adding indexes to a DB table first, because that is so obvious I never would have created the table without indexes in the first place.

**Be clear about your intent and goal with a question.** Don't say I can do something in whatever language and framework I want, and then complain afterwards that I didn't use the tool you are familiar with or the one your company is currently using.

**Experienced candidates rely on experience. Shocking, I know.** If they keep relating back to what they know and try to tie that in to the problem you are presenting them, that's a good thing. It's a helluva lot better than trying to make stuff up and fumble. Especially if they admit they don't know, and then proceed with "but it sounds similar to this thing, and this is how I solved that, so I would start there." If they have experience, don't go complaining that they "talked about what they've done rather than answer our question."

## Remote vs In-Person

Most of these tips are for in-person interviews, as that was how we did it most of the time. We did hire and interview remote even before the pandemic, and in some ways this made things easier, as we could just use screen sharing for pair programming to get a feel for the candidate. 

Some things are harder, because we can't get a proper feel for how they interact and work with the team, but on the flip side, we can get a good idea how they handle communication and frustration, since these tools often have gremlins (audio dropouts, video freezes, etc.)

In the age of AI and deepfakes there are a ton of other considerations, [mostly around security](https://www.reddit.com/r/cybersecurity/comments/1ihoplk/the_developer_used_ai_to_alter_his_face_during/), and I'm sure I'll write about that another time.

Ultimately I do feel interviews over video calls are harder; there is something to be said about looking someone directly in the eye and gauging their body language, but it's not impossible, and you can still test all the things I mentioned. Especially skills, knowledge and communication. It's just a bit more challenging.

## Discuss

Okay, that was a lot, and yeah, I know this won't work for every person or for every company, but it worked for me and my teams over a few different places with tweaks and iterations.

What do you all feel? I know that there are a lot of opinions going around about the state of the industry and how hard hiring is, with some people blaming the companies and others blaming the candidates (while most of us do actually know it's more nuanced than that, even if we don't admit it).

What processes have you used and what do you agree and disagree with my approach? Keen to hear your thoughts.

---
*I had intended to first post this over on Dev.to, but never actually did. Weird to ask for comments and thoughts when I don't even have a comments section on this site. Also, needs more memes, but my brain is fried. I might edit later.* 
