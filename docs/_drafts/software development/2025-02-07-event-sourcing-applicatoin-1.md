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

#### Stopped right at 'the document data model for one-to-many relationships

showing that relational tables have limitations. you get a tuple for a person entity but they may have more than one of some other thing like experience at a company. for htose you need a one to many relationship in the tables and then you need to have a separate table and separate queries or join statements in a single complex query just to bring everything in. 

this is a 'data locality' issue, and with a json document you can have these other things be nested becusae they essentially belong ot the person document so you can get everything back at once just by bringing in the new person object. this is closer to representing objects like they are in OOP but not the same, so impedance mismatch is not resolved

json creates a tree representation where documents have a root adn leaves and nodes connected, and that fits a lot of data shapes. it is that one to many relationship between a single kind of parent row and its child rows. it actually is better when this is 'one to few' becuase it gets unweildy if you truly have a lot of children. 

storing things in lists instead of as plain text strings that are duplicated in a bunch of places is normalization. replacing a name with an ID.it is meaningful, there are ids to refer to everything. storing text directly is denormalized human meaningul information in every record that uses it. 

normalizing a piece of data into an ID removes the human meaningful part. this means it doesn't have to cahnge. it is a pointer to the data the is meaningful and that can change but it will also continue. 

the flipside of normalized is that you have a non human meaningful thing like and ID, you will have hto do a lookup to that table to be able to resolve it to the human meaningful aspect. 

document dbs can be stored in a normal form, but it is more often associated with denormalized fields. there aren't great supports for joins in doc dbs so normally lookups aren't happening a lot and whole docs are stored with the inline human meaningful data hardcoded. 

locality is an advantage for performance for document style data stores, as is teh fact that the schema is flexible, especially if you have3 too many types of objects represented in one table, your model naturally nests a lot, or yout don't control the schema and it is changing a lot.




## Event Sourcing and CQRS

one big difference between this and other ways of storing data is that when you save the data and retrieve it you are doing it in the same format. for example, you store data as rows in a table or multiple table and that data represents the entity, then you retrieve the data the same way by querying that entity to get those rows back. 

what if you wanted to store data in a uniform way that was not particular to any one of these methods (relational, document, graph)? one of the easiest and most expressive ways to do this that has been devised is with an 'event log' 

this is the core of event sourcing: when you write data, you do so with a string that contains everythign you need to know about it. that has a timestamp also, then append it to a sequence of events. the events are immutable. 

use case: conferencing software
requirements: 
- individual attendees pay by credit card
- companies buy seats in bulk and pay invoice then assign seats later
- seats can be reserved for VIPs
- seat reservations can be cancelled
- organizers might change the capacity of events by changing the venue

how do we figure out how many seats there are? the query is complicated. 

with an event log you can rcreate materialized views of each of your events. changes in state are recorded as an event. new events impact materialized views. 

so the usage of the event log for state changes and source of truth is event sourcing. separately, there is the concept of command query responsibility segregation

so there is a terminology to how this works, you have user interactions in the form of 
*commands* and then the commands get checked and verified and that converts them to facts. once they are facts they make it into the event log

event logs only get valid events and materialized views can only ingest events

storing events in the past tense is a good approach because it reinforces the fact that these events did happen in the past and it is an immutable record. anything that changes the facts are separate events that are recorded at a future time.

star schema fact tables of events are different from event sourcing because with event sourcing your event log has to be in the order events happened and the properties of events in the event log can have different schemas but then with the star schema fact tables they all need to have similar schemas.

advantages of event sourcing: 
- events are more human readable than updates to a relational structure or other data model
- being able to reproduce the view reliably from the events makes it better because you have a reliable log and if there are issues with the reproduction, you know it is coming form the code that maintains the view
- the views can be be made optimized for whatever query is being computed. you can make multiple views. can be any model, can be denormalized for faster reads. can even be in memory
- the events in the event log are really flexible. you can add new types of events all the time, new properties to the existing events, chaining newer event types off of older events. 
- no more irreversible actions, you just write a new event undoing the prior one and you can move forward. 
- events are an audit log for the transaction history

downsides of event sourcing and cqrs
- external sources are not immutable, so they can cause sdie effects. if an event depends on some external source of truth when processing (for example an exchange rate for currency that is dependent on when it is pulled) then it can create a nondeterministic view generation. if you want to be able to faithfully reproduce events, the events need to contain the information rather than pulling it in from outside. 
- you can have issues with user data becauws users can request that their data be deleted. the user ddata can be deleted in an event that focuses on multiple users and then deriving future data will be problematic. 
- you can run into issues if you have externally visible side effects whenever you reapply all the events.

any database that can process events in order is suitable for event sourcing, but some databases and even Kafka are suggested for doing so.



## notes from RPGMyLife implementation

ran into a decision point. for most of this time building out the app I continued using the excel spreadksheet pilot version of my app to track my tasks and goals, which meant Iwas less likely to dogfood. the functionality also just wasn't there, so it made sense. once I got the event sourcing kinda sorta working, I decided I should double track in the database and in my app to put the stuff to the test, I should have an MVP by now, and if things break or I hit something, it will be a test of the claims that the event sourcing model is flexible to change. 

problem is I was using an ephemeral container memory to store the mongo stuff, so I had some options: 
- use a persistent volume to do it
- use a commit log through a file append structure to get the events stored on the filesystem
- eventually, maybe kafka

LLM helped me out, suggested that I use docker compose and also split out my persistent volumes
between event store and everything else. It recommended this because it said that this way it was easier to manage these as spearate types of data, migrate the event data elsewhere, and independently backupo and restore teh event store. 

it also suggested that I should do the file appender and persistent volume approaches both now and table kafka because I don't have the requirements that would justify it, not being distrubited or having scale in terms of services taht would require the pub sub model on steroids that is kafka

as I learned mofre about what it was intending there, I realized that it wasn't actually a good idea because from their standpoint I either needed to bifurcate into two mongo dataqbases in anticipation of having a separate event store or I would need to use collections which I was already using and there wasn't a way to specify that the second volume mount corresopnded to that collection. so as long as I'm not migrating event store data to another data source, (maybe filesystem ok that is coming up soon) this is premature optimization and I had to tell the coding assistant to remove it for me




