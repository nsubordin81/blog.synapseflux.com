---
title: "Using LLMs: Basic Use Case With A Local Model"
date: 2024-03-17 12:00:00 -0500
excerpt: ""
classes: wide
---

I did a local setup to have a model that chats about an ebook lately. I'm sharing how I got there, and how I learned by failing along the way.

## 1. From Scratch

* out of the gate didn't know what model to use
* picked up llama 2 because I'd heard it was the most performant of the smaller local models, didn't spend a long time researching this
* I was also doing this on a new machine. I changed how I work with python for this. pyenv
* lesson one: bloat builds up fast and it is easy to be swimming in used up disk space
    * the models are huge
    * there are a lot of python packages involved in this
        * see if you can provide the exact list of dependencies you ended up needing
        * get numbers for disk space and explanation, maybe for footnotes
* lesson two: don't bring scuba gear to trench, know what you are willing to go deep on when. 
* talk about starting with the readme on hugging face and their examples and running into issues because you didn't understand model terminologies, and then talk about how you were able to get running faster with ollama and langchain
