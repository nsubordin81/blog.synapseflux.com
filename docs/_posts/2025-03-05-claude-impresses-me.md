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

I'm writing this post because I was recently impressed when the output of a large language model (LLM) reached a milestone I hadn't personally witnessed yet. Claude 3.7 wrote a nontrivial app based on a prompt I provided that 'just worked.' Throughout the conversation, it was able to make modifications as I suggested and even correct its own coding errors. I've been using these models since early 2024, and so far I have to admit that my anecdotal use of these models have been just as much of a check on the immense hype surrounding where AI is going as they have been a justification of that hype. So I look for examples that require less creativity to imagine practical use cases at current levels of capability for these models. This interaction with Claude was at least one such instance.

Backing up, I feel I should expand a bit on my personal level of skepticism around the LLMs that have been driving this roughly 2022-present enthusiasm wave around AI. I remain skeptical about the most fantastic claims towards how quickly AI is developing with LLMs, such as achieving the holy grail of artificial intelligence research within the next couple of years or even five. How to define this threshold of capability is highly debatable, I've always thought of it as generalization to all human cognitive tasks at average or above human-level ability, though that may be an unfair goalpost. 

I can then narrow it down to the concept of a sort of 'full job replacement' for knowledge worker which is a current boogeyman given the coding and language capabilities LLMs demonstrate. From a software engineer's perspective, reliance on this type of ai model would require deferring a large amount of decisions that normally go into implementing the system but also the much more messy quesions surrounding the context of the problem domain. Given building software typically involves a series of continuing conversations around that software's problem domain and building good versions of this software involves a good deal of time spent modeling, surfacing assumptions and testing them. Additionally, such an AI agent would need to be able to generalize across use cases without significant work required to evaluate its performance continuously and on a case-by-case basis against the diverse set of use cases (assuming such diversity actually exists), otherwise you would risk creating more work in monitoring and evaluating the agent's inference output than you would have building the application soley with human actors. In practice I've found this to be difficult and even now with fledgling agentic ai, we are seeing responsibility for that problem pushed outwards to the software engineers and data scientist practioners.

So while we have LLMs that can generate long strings of code that will successfully run and even have kind of post-processing steps to guide it towards a good prediction around what raw and derivative training data suggests it should make, the architecture underlying what we have likely won't be sufficient to get us all the way to where we can take the guardrails off and automate real-world fidelity solutioning in an unrestricted manner, even with additional significant scaling of data and compute. I find the arguments of luminaries such as Yann LeCun very convincing if I'm understanding him right, that token prediction isn't sufficent to arrive at the same level of consistent adaptability and resourcefulness you can get with the human mind, despite how impressively it can approximate it, and at some point that will likely going to be a very apparent limiting factor of current approaches. There's a lot of momentum and funding behind AI right now, which as an AI proponent I like and I feel will help the cause of using AI effectively unless a failure to reach some of the loftier aspirations by stated estimates creates disillusionment and causes another period of stagnation in the field.

That tangent aside, I do find LLMs to be extraordinary and very impressive for what they already are. This post is a celebration of that. What I'm about to share isn't exactly a developer job-killer, but it is impressive.

I've been interested in the field of meta-learning for a while and wanted an app to help me orient a goals list into a metalearning-aware guide. Normally, I try to ad-hoc these things, but I asked Claude 3.7 to build me an app, and it did! The app worked inline in the Anthropic console window and was easy to spin off into a React application. It was only front-end, but through more conversations with Claude, I got suggestions to persist data to local storage or a local database. The app provides a schedule, checkboxes, and logic for spaced repetition. While it might not have taken me long to write the core functionality, populating the initial list with data and aesthetics would have taken much longer. Claude did it in less than 2 minutes for the first iteration, with successive prompts totaling less than 8 minutes.

[Here is a link to the public repository](https://github.com/nsubordin81/learning-helper) in case you would like to see what it built for me. It is usable for your own purposes, and Claude built in ways to add and expand on the app.

The prompts I gave it were something along the lines of 

> I'm currently working on refreshing my knowledge of updates to react and learning react three fiber for threejs as well as event-driven architecture and distributed computing concepts in system design. I'm simultaneously trying to learn godot and how to design effective game mechanics.

and a simple follow up prompt,

> can you build me a simple app that will guide me through the process?
