---
title: "Claude 3.7 Impresses Me"
date: 2025-03-05
tags: 
    - AI
    - LLM
    - Machine Learning
    - Technology
    - Programming
    - Productivity
---


I'm writing this post because it seems as though a large language model (LLM) has hit a milestone that impresses me. It wrote a nontrivial app based on a prompt I wrote that 'just worked' and then also throughout the conversation was able to make modifications as I prodded it with suggestions and even correct its own coding errors in the process. I've been using these models since early 2024 and actually using them has been working as a good check on the immense hype surrounding where AI is going.

I remain a skeptic about the most fanstastic of these claims, such as that realizing the holy grail of artificial intelligence research that's been going on since the 1950s will be achievable within the next year or even 5, or that the algorithms we have are sufficient to get us to that point so long as we scale them enough. There is an impressive amount of momentum and funding behind trying right now which is going to help the cause anyway unless a failure to reach it at this level of dogmatic belief fails and brings about not only a winter but an abandonment of hope for the whole enterprise. 

I haven't seen sufficient evidence or had adequate explanation from a 'how it works' perspective as a companion to these LLM releases to make a hardcore believer that we are on the precipice of the goalpost, even with recent efforts to use reinforcement learning on top of what the self-supervised transoformers approach could already do. I'm honestly not sure where I stand on reaching that goalpost anyway. Being able to solve some of our hardest problems in an accelerated way sounds incredible, but some of the fears of some very smart and hard-thinking people around alignment problems give me pause.

That was a tangent, and none of that is to say that I don't find LLMs to be extraordinary and very impressive for what they are. That is what I wanted to celebrate here. What I'm about to share isn't a developer job-killer so much, but then again I guess if you built an agent that could loop until it got functionality working on a much more complex task then I could see it. 

Here's what impressed me. I've been interested in learning about learning for awhile, and there are some techniques I want to put into practice. Normally what I do is just try to ad-hoc do these things, but I figured it might be nice to have an app that helps with that. So I asked Claude 3.7 to build me an app for that, and it actually did! A pretty one. the app worked inline in the anthropic console window, and it was pretty trivial to spin it off into a react application. This application was only front end, but some more conversations with Claude and I have suggestions to have it persist to local storage or a local database. It's pretty useful out of the box, core functionality I'd say is that it provides a schedule and checkboxes and I think has some logic to consider when I've studied something for spaced repetition. That may not have taken me long as an exercise to write at its core, but then populating the intiial list with data and all of the aesthetics of it would have taken much longer. This version was done in less than 2 minutes for the first iteration, and then the successive prompts probably totaled less than 8.

Here is a link to it in a public repository in case you would like to see what it built for me. It is usable if you want to for your own purposes also, Claude built in ways to add and expand on the app. 