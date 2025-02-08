---
title: "When Event Sourcing Feels Like a Good Fit"
date: 2025-02-07
draft: true
description: "An example of when event sourcing is a good fit, why I'm using it, and a brief overview of what it is."
tags: ["Event Sourcing", "Architecture", "CQRS", "Microservices"]
author: "Taylor Bird"
---

# When Event Sourcing Feels Like a Good Fit

In this article, I will cover an example of when event sourcing feels like a good fit and why I'm using it. Additionally, I will provide a brief overview of what event sourcing is and its benefits.

-- draft

I'm convinced Event sourcing is an intuitive concept made more difficult by the need to precisely define it and possibly because of how much it changes the standard way we are often taught to approach persisting data. 

the typical database setup behind an app that you might be familiar with is a relational model where you have entities and you can think of each of those entities geting a row in the relation, then after initial insert you update it and sometimes delete it. 

This way of doing things is very familiar and intuitive. There are issues that arise in the way that tables and their relationships in a relational model can't be straightforwardly represented in an object-oriented model, but otherwise there are a lot of benefits in terms of working with existing query languages and frameworks and not having to store a lot of information other than whatever the current state of each of your rows is. 

The event sourcing solution is different. You identify objects or groups of objects that have responsibilities in your application and instead of storing that representation in a row and mutating it over time, overwriting the records of what has happened to instances of that object as they've been created, modified and destroyed, you keep an event store and have a sequence of events for each instance of that model that serves as a record-over-time of every change that has occurred with it.

There are a lot of advantages to doing things this way, a lot of them stemming from the fact that you don't lose changes that would get overwritten in the familiar way of storing things. Some of the most straightforward ones are that you can reliably replay these events and recreate the state. Also, if things get messed up by a bad transaction, you can figure out where. You can also restore to anywhere in the timeline of events. This is one of the reasons I opted to do it. with productivity tracking, I thought it might be interesting for analysis to have that capability of 'rewinding' and viewing one's progress at earlier milestones.

There are things that I don't fully understand as I dive into this pattern, and one is this idea of aggregates. It is is more of a domain design idea that I'm honestly still getting a grip on. In the example I'm describing here, I treated aggregates a litle bit like entities. I am making an RPG-like game for habit trackign and productivity, and have three entities in my core domain
