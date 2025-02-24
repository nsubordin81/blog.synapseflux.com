---
title: "Game Jam Dabbling Log, Hopefully First Of Many"
date: 2025-02-24
categories: 
    - Game Development
    - Godot
tags: 
    - Game Jam
    - Godot Engine
    - Indie Development
    - Devlog
---

## The Setup

Here I am, diving into another game jam. I've only particiated in 2 before. It officially started January 17th, but I spent the first five days just planning and thinking about directions it 'could' go, with whatever time I had which was not much.

> "This is a gift of time to yourself, it doesn't matter if you finish or submit or whatever - just do it and if you learn something, great." 
> - Thor, Pirate Games Founder

I appreciate that self-permission angle when trying to balance a day job, family life, and that burning desire to make games. Two weeks sounds like a lot until you're living it in 20-minute chunks between real life.

## Why Godot?

I chose Godot because, even though Unreal and Unity are more popular and I have some experience with them, Godot really caught my eye. I was impressed by the courses I took, their intro videos, and the documentation. Godot seems to strike a great balance by getting out of your way when you need it to, while still providing high-quality tools. It feels like a perfect fit for indie developers who aren't aiming for incredibly complex games. Plus, the open-source nature and the free licensing (both in terms of speech and beer) are huge bonuses.

## The Daily Grind

### Day 1: Baby Steps (45 minutes of pure confusion)
* Learned that a Scene in Godot is basically like components in other engines
* Found out Command + A is my new best friend for adding nodes
* Immediately got distracted by asset hunting (classic developer move)
* Saved by Kenney's asset packs which I did purchase to support 'em, thank you!

### Days 2-3: The Movement Saga
* Turns out when your character is invisible, maybe check if it's actually in view before panicking! Some key learnings:
* `$` is Godot's jQuery moment (and I'm here for it)
* Positive Y is down, took a bit of getting used to.
* After 2 hours spread across 2 days, I had a walking sprite. Progress!

### Days 4-6: Tooling Friction
* Used VSCode with an extension for my IDE preferences. Ran into glitches where scripts opened in both VSCode and Godot got out of sync. Realized I needed to close the script in Godot to fix this.
* I managed to trick myself, jumped to conclusions since the external editor situation was fresh in my mind thinking that the reason I wasn't seeing debug logs or movement of my character was due to the 2 editor problems, but it had nothing to do with that and a lot more to do with using the wrong name for my animation and using the arrow keys when I'd mapped movement to 'WASD.'
* I had a proud moment of personal self-restraint when it comes to stripping down to the essentials when it came time to add a jump mechanic. Normally even for early stage prototyping of platformer games I've tried in the past I get caught in the beartrap of trying to make a unique and special snowflake jump parabola that will be the foundation of my whole game's personality before I even know what that is. There are a lot of rationales to work this out early:
    * It is the core mechanic of the game.
    * It is one of the things that would differentiate it from other platformers out there.
    * Given the level design and NPC behaviors have a lot to do with the character's movement abilities, it is important to address early.
    
However, I'm building to learn at this point and it is a veritable rabbit hole to try and tweak and tune and introduce funky bugs trying to overcorrect the imaginary physics of my world before I've even decided what the game is about and I'm just prototyping ideas. So I made bad floaty physics with regular gravity and moved on.

### Days 7-10 
* I was feeling the symptoms of an illness setting in by day 7 and it progressed through day 10
* I did a lot of micro-efforts based upon the Godot course I was following "Complete Godot 4 2D: Code Your Own 2D Games In Godot 4!" from GameDev.tv taught by Kaan Alpar and I was able to learn many things about how to use Godot in a short time with this practice of short sessions, such as:
    * Managing collisions with layers and the 'is' keyword
    * Decoupling interactions between scene objects with signals
    * GD script syntax
    * Implementing deadzones for typical game concerns

## Lessons Learned

1. Start simple - your brain will try to trick you into feature creep
2. Time boxes are your friend
3. When things break, check the basics first
4. The Godot community is amazing
5. Don't let perfect be the enemy of done

## What's Next?

Well, I didn't finish the game, but I learned quite a bit about Godot,  a little about game development workflows, and most importantly - how to keep moving forward even when time is tight.

I significantly slowed down and eased things on myself becuase 
 1. I'm not in a position to push it, have too many competing priorities right now and 
 2. it is a victory to just achieve consistent work on this and be interested in it again. happy for that, going to keep up consistency and momentum for myself. 
 
 I am a game creator. I'm not not a professional one. yet. I'm learning a ton by following these tutorials and I am excited for the opportunity to apply the lessons I'm learning and my own thought and problem solving abilities to my own projects. Stay tuned for more adventures in gamedev!

_P.S. I highly recommend joining a game jam even if it is in this non-commital fashion. Even when giving myself permission not to finish or push and reprioritize my life to make it happen, I did find myself making the time more just because I signed up. This is way more my speed than cutting a check to a group I hate for motivation. The motivation I did get has even carried into 5 more spread out days of gamedev at the time of this posting, so it succeeded in habituating this practice for me even a little bit, and that is a win in my book_
