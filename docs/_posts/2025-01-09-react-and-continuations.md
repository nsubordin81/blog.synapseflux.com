---
title: "React's Functional Roots: Understanding React Through FP Concepts"
date: 2025-01-09 12:00:00 -0500
categories:
  - Programming
  - React
tags:
  - react
  - functional programming
  - computer science
  - web development
  - javascript
excerpt: "Exploring how React's core features emerged from functional programming concepts and how understanding these origins helps us write better React code."
classes: wide
toc: true
toc_label: "FP Concepts"
toc_icon: "code"
published: false
---

Continuations. really good talk about this comiing from Sam Galson a few years ago. https://youtu.be/5OPyjrKytLU?si=Nc_q_QOA3JN7vY5h. having to do with React3 fiber. 

Zooming out, giving a formal definition to continuations. you have to look at how computers execute programs, which is always fun to do. in a traditional architecture, you have this thing called a call stack which has call frames as the individual units. the stack frames are in an actual stack data structure which has a lot of benefits for efficiency since the way things are pushed on an popped of in FIFO fashion, you will keep them in memory address order, and there is no need for overhead to do memory management on top of that. But this structure also forces sequential execution, so like you have to unstack and use everything you've unstacked before returning to the next thing at thet op of the stack. 

continuations are more like references from a called frame back tot he frame that called it so that execution of that calling frame can be resumed when the called frame returns. but a reified continuation is under control of the programmer, so I've seen different flavors of this that are less general, like I feel a block and yield from ruby or like the async await or pythyon generators or coroutines in many languages. however, I think continuations might be more general than all that and not completely overlap with any one of those concepts. 



