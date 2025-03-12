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

I'm writing this post because I was recently impressed when the output of a large language model (LLM) reached a milestone I hadn't personally witnessed yet. Claude 3.7 wrote a nontrivial app based on a prompt I provided that 'just worked.' Throughout the conversation, it was able to make modifications as I suggested and even correct its own coding errors. I've been using these models since early 2024, and so far I have to admit that they have been a good check on the immense hype surrounding AI.

I remain skeptical about the most fantastic claims towards how quickly AI is developing with LLMs, such as achieving the holy grail of artificial intelligence within the next year or even five. The algorithms we have might not be sufficient to get us there, even with significant scaling. There's a lot of momentum and funding behind AI right now, which as an AI proponetn I like and I feel will help the cause of using AI effectively unless a failure to reach some of the loftier aspirations by stated estimates creates disillusionment.

I haven't seen sufficient evidence or had adequate explanations from a 'how it works' perspective to become a hardcore believer that we are on the precipice of achieving AGI or at least the traditionally sought version of it. Recent efforts to incorporate reinforcement learning on top existing LLM architectures are impressive, but I'm unsure where I stand on reaching that ultimate goal. Finally, from an expectations standpoint, solving some of our hardest problems in an accelerated way sounds incredible, but concerns about alignment problems give me pause.

That tangent aside, I do find LLMs to be extraordinary and very impressive for what they are. This post is a celebration of that. What I'm about to share isn't exactly a developer job-killer, but it is impressive.

I've been interested in the field of meta-learning for a while and wanted an app to help me orient a goals list into a metalearning-aware guide. Normally, I try to ad-hoc these things, but I asked Claude 3.7 to build me an app, and it did! The app worked inline in the Anthropic console window and was easy to spin off into a React application. It was only front-end, but through more conversations with Claude, I got suggestions to persist data to local storage or a local database. The app provides a schedule, checkboxes, and logic for spaced repetition. While it might not have taken me long to write the core functionality, populating the initial list with data and aesthetics would have taken much longer. Claude did it in less than 2 minutes for the first iteration, with successive prompts totaling less than 8 minutes.

[Here is a link to the public repository](https://github.com/nsubordin81/learning-helper) in case you would like to see what it built for me. It is usable for your own purposes, and Claude built in ways to add and expand on the app.

The prompts I gave it were something along the lines of 

> I'm currently working on refreshing my knowledge of updates to react and learning react three fiber for threejs as well as event-driven architecture and distributed computing concepts in system design. I'm simultaneously trying to learn godot and how to design effective game mechanics.

and a simple follow up prompt,

> can you build me a simple app that will guide me through the process?
