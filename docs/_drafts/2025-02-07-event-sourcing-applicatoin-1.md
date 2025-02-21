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

There are things that I don't fully understand as I dive into this pattern, and one is this idea of aggregates. There will likely be further posts where I've modified the design I have as I learn more. Aggregates are more of a domain design idea that I'm honestly still getting a grip on. In the example I'm describing here, I treated aggregates a litle bit like entities. I am making an RPG-like game for habit trackign and productivity, and have three entities in my core domain


# Notes from designing data-intensive applications 2nd edition preview

data modeling impacts how we think about the problem. 

usually we layer data models on top of one another with key concern being how to represent a higher layer in terms of a lower layer. 
- app devs have an object model with interfaces as a lense against real world
    - there might be a lot of these api layers in a big application
- persistance requires these object structures to become documents, relational tables, or graphs
- the method to represenent that data on disk and in network was made by the provider of whatever persistence framework you chose the engineer behind that. 
- hardware engineering gets a little more concrete in terms of physical laws and how to convert from memory to hardware. 

some types of data are easy to express in one model and harder or more awkward in another. the chapter compares the following models for working with data:
- relational
- document
- graph based data models
- event-sourcing
- dataframes

considering query languages and also a comparison framework for deciding when to use each model. 


most query languages are declarative, where you specify a pattern and the data desired. what does that mean? it means what are the conditions that the query result needs to meet and what transformations do you want on the data. 

imperiative would be how to achieve the result, where you would specify the low level details of which index and join algorithms to use and which order to execute the query in. detailed steps, algorithms, and ordering. 

this is a sidenote but and important one, most of the time when programming you are using an algorithm which is imperative whereas query languages are using an attractive declarative structure because it obfuscates and surrenders control of the 'how' details and you get performance gains without needing to change the queries. and also it is more concise and easier to write if you know what you want.

a really good example of why this can be better is parallelism. in a declarative language, the 'how' of setting up parallel execution for the query and spreading it across CPU cores and machines is left to the underlying architecture, not something the query author is expected to set up. 

## relational

Edgar Codd 1970. there are relations which are tables and there are tuples which are rows. RDBMS (relational database management systems) are the most common implementations of this model. SQL is the most common language. Businiess Analytics still overwhelmingly use relational model for their storage, retrieval and update of data. 

there were other methods out there. hierarchical and network models were two. others that came and went were object databases and xml databases. relational model has remained the strongest and incorporated other methods like json, xml and graph data. 

NoSQL was the biggest challenge to relational to ever come out. it was not one thing but a group of concepts/technologies. these are more like new ideas and many of them have been adopted and people don't specifically call them out anymore, it may just bet hat they are present in the relational database system you are using. 

but there is a document model which was one of the ideas put forward by the NoSQL movement, which still is around and is used. usually this is data as JSON. relational databases have JSOn support but there are document dataqbases like mongodb and couchbase that are around. schema flexibility is the main attraction to these. 


### relational vs. object

#### object-relational mismatch

if you are doing your programming in the object-oriented programming language paradigm, adn you are doing the data manaagement in SQL with relational tables, you have a translation layer between them. impedance mismatch. This comes form electronics. when you connect two circuits together, it is optimal for power transfer from one circuit to another for the impedance of each match each other. 

they don't talk about the specifics of what is mismatched between object and relational models, but if you think about the fact that an object gets instantiated and can be an aggregate with lose or tight references to its dependent objects and then relationally you have rows per object but the row can only exist one time and has properties to it, and the model is changed by update statements that operate on several rows at once but objects operate directly on other objects through api calls, there are a lot of w2ays to imagine that these things are going to have issues. 

#### object-relational mapping frameworks

these are designed to stand in for that translation layer. but they have issues: 
- usually devs still have to think about some aspect of how these models differ at some point, they don't solve the mismatch
- they are biased towards being useful for OLTP systems
- you don't get good support from them if you aren't using an OLTP database
- schema generation into the ORM framework is clunky and not optimal
- inefficiant queries using the framework. example given was if you want to get comments per user, but then include the user id, you could do this with a join statement in a SQL query, but with an ORM it is liable to give you a solution where you do a select query for eaqch of the ids against the table that owns them for N+1 total queries instead of the single query with a join statement. 


but ORM frameworks have their place becasue: 
- you are going ot have to translate at some point if you are doing OOP and relational together, and ORMs do a lot of that boilerplate for you.
- there are often caching mechanisms built into ORMs which helps with efficiency and database load. 
- migrating a database schema is something that involves tracking and managing these things and so if you are using ORMs already to do that, you get this for free. 

#### Stopped right at 'the document data model fro one-to-many relationships



## Event Sourcing and CQRS




