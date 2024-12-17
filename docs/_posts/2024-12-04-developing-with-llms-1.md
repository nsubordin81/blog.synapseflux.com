---
title: "Working In The Open: Developing With LLMs Part 1"
date: 2024-12-04 10:43:00 -0500
# categories: [functional-programming, scala]
# tags: [scala, functional-programming, modularity, higher-order-functions, lists]
excerpt: "In this series I will be working in the open on a personal project I've dubbed 'RPGMyLife.' I'll share how I'm using LLMs to help me build it and what I'm learning about AI Assisted Programming along the way."
classes: wide
published: false
---

background on what I'm making. I like thinking about how to manage my personal productivity. However, I also think historically I have a lot of personal goals in which I've erred on using planning as a procrastination technique to protect me from the fears of putting in wasted effort, starting from a position of being behind (behind who exactly I have no idea, I guess another version of myself that was more disciplined), or failing in general despite trying because I'm somehow innately incapable. 

I've read books like Peak by Anders Ericcson, the power of habit by charles duhigg, atomic habits by james clear and range by David Epstein, and while I regret many of the important lessons and anecdotal support for their ideas probably fell through my wide retention filters, I did get some underlying nuggets and core ideas from each. Among these were the importance of tracking my behavior if I want to improve, breaking things down, and thinking about the small changes to the process instead of the long term goal as the focus. I also learned about how powerful this neurotransmitter dopamine is for helping me reshape my behavior towards what I want to do and who I want to become in the future. 

That's all very well and good, but I have trouble sticking with things that are hard and have consequences sometimes. I get afraid, I get bored, I get distracted, I get anxious, I get disrupted, I lose my faith in the stability of my situation because of unresolved childhood trauma, whatever I don't know something like that. 

Another thing that I've been thinking is I need some kind of project when learning. I need something that is going to test that I actually have the knowledge I think I might have, so I can prove to myself that I have it. I need something like that to build confidence by creating a thing, to escape imposter syndrome, to learn in a low stakes environment. 

So to combine those two ideas, I have decided to create a project that turns the process of tracking my processes in life into a role playing game. I hadn't until recently learned that someone else already did this back in 2013 and the app still exists and it is called Habitica. There are also some other competitors in this space. Interesting. 

I think I will still use this a learning project taht I can also use for myself. 

what have I done so far? created an excel spreadsheet that had what I thought i needed for a bare-bones rpg style task tool. I actually had help thinking the process through by talking to llms if I remember correctly. 

ok, none of this background or context is that interesting if I'm not myself. let's get into the meat of things. 

what is the project structure? and how did I arrive at it? 
I should look at my cursor logs to find out

stop procrastinating and just do it!

initial setup, had 3 repos, one for the backend, one for the frontend and one as a dev repo to orchestrate local development with the other two. I feel like I already overcomplicated things because the front end is a react app and the backend is node. 

apparently what I'm doing is the developer coordinator pattern. using a dedicated coordinator repository to help set up the development environment with multiople other repositories. it is flexible and good for quick iteration in a containerless setup. an alternative could be a dev container setup which would create a more 'run anywhere' system but might introduce a little more overhead for me at this early stage. 

things I should be able to talk about 
what is react? 
what is a single page application? 
what does vite do? 
what does react router do and how does it work? 

react 18, what is in it? 

do an exercise where yuou write down what you remember about react and how it works to see what you actually understand. 

the whole 3d rendering stack of threejs, react three fiber, and drei is something I know very little about, bruno simon knows more about it though and I'll learn some from his course

axios, how does that work? what are the alternatives? 

from cursor/claude: 
Uses Vite for:
Hot Module Replacement (HMR)
Dev server
Build optimization
Module bundling
Development mode: npm run dev
Build: npm run build
Preview built version: npm run preview

what is hot module replacement? guess and check this
dev server - what is it? guess and check
build optimization - what is involved? 
module bundling guess and check

ES modules - you are learning about them now, learn about what alternatives are
vite modern js featurs, don't know what they are and how vite supports them

things to go deep on: 
what is the difference between a web application and a typical application you write for your operating system? 


ok you officially have motion without action here, or I guess action but more in support of motion rather than the other way around. so commit to writing a backstory for this and covering how you got started later. for now, you will take notes as you learn how to make this thing an event sourced app. you will not shy away from understanding, you will not get lost in theory, you will not be afraid to try

goal: take working rpg my life app and convert it to a version of that which is event sourced. it is still a full stack javascript application

event sourcing. when instead of saving the application state directly and then overwriting it every time soomething changes, you save the sequence of events that occur to influence the state. its like if you were to record a video instead of taking a sequence of pictures.

event sourcing has a simple enough premise, for as long as the application has been running historically, make sure all of the changes to state are recorded as events. 

event sourcing starts out being easy, you can think of it as doing that left fold over all of the sequenced events to apply them and build up your application state, but there are always devils in the details. so like when you do that, yoiu might find the reconstruction each time to get kind of slow as the number of events grows. so the answer thwere is to cache the applicaiton state at more recent times with a snaphot of it. however, your event log could still remain the source of truth. 

ok, plan. you will use event sourcing for rpgmylife. this will allow you to take advantage of the ability to query the events, reconstruct events, rewind, etc.

pretty sure domain objects are encounters, quests, game store itmes, characters, abilities
need to figure out events, but seems like
- add new encounter
- update existing encounter
- complete encounter
- delete encounter

- add new quest
- quest finished
- update question information
- delete quest

- create new character
- update character information
- remove character
- get character information

should a get request be modeled into these domain objects? 

encounter
name - what it is for
category tags - idea here is to get some identity values in here as well as some goals
applicable quests
how much experience you get for completing it
how much gold you get for completing it
state? new, in progress, abandoned, completed, redefined
priority
target completion date

quest
name
category tags
why am I doing it
how much experimence you get for completing it
how much gold you get for completing it
state? new, in progress, abandoned, completed, redefined
priority
target completion date

store item
name
cost (in points, eventually the points system will be different but for me it is 100 points = 1 USD)
minimum character level to purchase
state? created, ineligible to purchase, eligible to purchase, purchased, deprecated, deleted

character 
name
kind - this would be like race if you are doing fantasy or sci fi, what kind of creature are you elf, troll, gnome, dwarf, wizard, etc. 
class - warrior, rogue, thief, mage, etc. 

ok, so starting down the path of transforming my app, which follows the pattern I'm used to, towards a more event sourcing friendly pattern. I want the benefits of being able to rewind my state and replay it if necessary. I want that event log book that I can use to recreate everything. 

So, first thing that my llm tells me to do is start defining an event store and aggregates. 
WTF are these? Event store becomes easy enough to understand, at least at a high level. 
It looks like this will exist for each type of thing I can have events about in my data schema. 
I'm starting with the character and gold accumulation, because between experience and gold updates those are the most importaqnt things to track to get the state at any given point. So for the Character in this case I will have an event store. there are more than one Characters and all of their events will live in the store.

the aggregate part is starting to get a little bit more arcane. I did a bit of side reading. not super helpful. It seems like a powerful construct but I'm not sure of the utility here yet. 

here is my attempt to put aggregates in my own words: 
- you create a group of related things. this group is looked at like one thing from the outside, and there is one thing in the group which represents the group. within the group, there are relationshipos and all of hte things are unique from each other. things that are in the group can have duplicates outside the group, which is ok because from the outside you will never see those internal things. they call these things 'entities' and the entity that identifies the group is called the 'aggregate identitiy' 

ah, ok, so reading a bit more I think I get why these are useful for event sourcing. aggregates are a tool for enforcing consistency. in event sourcing, things you do to an aggregate result in events, and these operations have to take place all or nothing for the whole aggregate or no event occurs and nothing changes. 
