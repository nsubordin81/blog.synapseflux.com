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
draft: true
---

I'm writing this post because Claude 3.7 recently reached a milestone that genuinely impressed me. The large language model (LLM) wrote a nontrivial app based on my prompt that "just worked." Throughout our conversation, Claude made modifications as I suggested and even corrected its own coding errors. I've been using these models since early 2024, and my experience has served as both a check on the immense hype surrounding AI and a confirmation of its genuine capabilities. This interaction with Claude demonstrated a practical use case that requires minimal creative interpretation.

my perspective on LLMs driving the AI enthusiasm wave since 2022, I remain skeptical about the most ambitious claims about AI development timelines. It also doesn't help that the way AI is evolving we maybe have a hard time of knowing what constitutes a milestone in the larger quesiton of machine intelligence. But there is a sort of intuitive 'you know it when you see it' when it comes to being impressed by an LLM that can converse like a person and disillusioned when it tells you to put something poisonous into your baked good that serves somewhat well. You can split hairs endlessly on all of the tasks that do or don't matter and whether something needs to emulate humans perfectly or not and at what level of capability you draw the line. I'll stick with human-level intelligence as a benchmark knowing this threshold of capability is debatable, but that it also serves as a useful reference point.

A more focused concern is the concept of "full job replacement" for knowledge workers, which has become a common fear given LLMs' coding and language capabilities. From a software engineer's perspective, relying on these AI models would require deferring numerous implementation decisions along with the messier questions surrounding problem domain context. Building quality software typically involves ongoing conversations about the problem domain, careful modeling, surfacing assumptions, and testing them methodically. 

An effective AI agent would need to generalize across diverse use cases without requiring significant evaluation work on a case-by-case basis. Otherwise, organizations risk creating more work monitoring the agent's output than they would have spent building applications with human developers alone. In practice, this challenge remains substantial, and even with emerging agentic AI, responsibility for this problem is largely delegated to software engineers and data scientists.

While today's LLMs can generate functional code with post-processing steps that guide them toward appropriate predictions based on training data, the current architecture likely won't be sufficient for unrestricted real-world solution development, even with additional scaling of data and compute. I find Yann LeCun's arguments compelling—token prediction alone isn't sufficient to achieve the consistent adaptability and resourcefulness of the human mind, despite how impressively it can approximate these qualities. This limitation will likely become increasingly apparent as we push current approaches further. There's significant momentum and funding behind AI research today, which will benefit the field unless failure to meet ambitious timelines causes disillusionment and another period of stagnation.

That said, I find LLMs extraordinary and impressive for what they already accomplish. This post celebrates that reality. While my example isn't a developer job-killer, it demonstrates meaningful capability.

I've been interested in meta-learning and wanted an app to help organize my goals list into a meta-learning-aware guide. Rather than creating an ad-hoc solution, I asked Claude 3.7 to build me an app—and it delivered. The app worked inline in the Anthropic console and was easily adapted into a React application. Though it was frontend-only, through additional conversations with Claude, I received suggestions to persist data using local storage or a local database. The app provides scheduling features, checkboxes, and logic for spaced repetition. While I could have written the core functionality myself, populating the initial data and creating an aesthetically pleasing interface would have taken substantially longer. Claude produced the first iteration in under two minutes, with successive refinements totaling less than eight minutes.

[Here is a link to the public repository](https://github.com/nsubordin81/learning-helper) where you can see what Claude built for me. The app is ready for your own use, and Claude incorporated ways to expand its functionality.

The prompts I gave Claude were straightforward, beginning with:

> I'm currently working on refreshing my knowledge of updates to React and learning React Three Fiber for Three.js as well as event-driven architecture and distributed computing concepts in system design. I'm simultaneously trying to learn Godot and how to design effective game mechanics.

Followed by a simple request:

> Can you build me a simple app that will guide me through the process?