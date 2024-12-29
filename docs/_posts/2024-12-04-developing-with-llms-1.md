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

let's do a little documented learning while we work on the event store implementation. 
how is this app that I generated organzied? 

from the perspective of a vertical slice through the character actions: 
- when you make a get request to the application for either a character retrieval or a charatcter creation, you hit the router first
- threejs, there is a front end call when a user is interacting with the UI
    - using the api.get() method, getting a response back, doing this as an async method, error handling
- router exposes the routes that are acceptable. 
    - how does express managed publishing those routes with the plugin? I don't know
    - how does the browser handle this? how does the software that is lower level and standing in between your code and the browser handle this? how does it get handled on your computer? 
    - the http request is recieved and routed to a controller ok
- there is a controller involved in processing the request managed by the router
    - take for example the getCharacter method. it is an async response that takes a request and response param and then will delegate to the character service's getCharacter method and return the model object for character and convert that to json to give back, and return a 404 if the character doesn't exist. 
- there is a service layer that will handle business logic and then call the model methods and persist data. 
- there is a model method which is "current state" based in my current design, which means we are directly mutating state in the model. 

with event sourced, we will change this in the following ways: 
- new events for every change to state instead of direct state mutation
- storing of these events
- rebuilding the state by replaying events. 


concrete example: 
character experienceGained event will not just add some provided amount to the experience for the character model object and save it, it will instead: 
- create an event that is of type "GAINED_EXPERIENCE", and note that it containes the character id the payload of hwat to change and the version of the event to keep ordering. all makes sense. you save that instead. 

why don't we just call the service layer directly from the router, all the controller seems to do is delegate to the service. 

answer: 
- the main purpose of controllers in this architecture is to remove the responsibility of http handling from the service layer objects. service layers don't take a request response pair, they don't handle http errors, they don't validate the http input, they don't convert output to json format for responding to the rest reqquest. 

very cool. 

here is what I'm trying to accomplish with my conversion to event sourcing

HTTP request → Router → Controller
Controller validates input
Service loads character by replaying events
Character aggregate creates new event
Event is saved to event store
Current state is returned to client

oh boy do I love LLMs. they provided some great suggestiosn and now I just need to fill in the gaps! 

I also have a clear way forward and suggestions fromt he LLM this is a great way to learn!

documenting my larger scale plan for this: 

first step: event sourcing for just the character entity, leaving the other three as state mutation style data architecture
phase in event sourcing for each additonal after you have it working for character. that is still just encounters, quests, and store items
once everything is event sourced, get a form of permanent persistence setup, a database that isn't ephemeral, and start using the damn thing
features beyond that: 
well by that point I would have a really rough interface for creating new encounters and quests and the character would update, I already have a level system that is decent and this would also mean a gold thing. what would make thsi better? 
I would like to be able to see the abilities I've achieved over time
I would like to see the encounters I've completed over time
I would like to get data about the encounters I've completed presented in a visual and consumable way. i.e. "here are some stats about how many things you've done towards this tag of encounter in the last month
I would like to eventually add to the incentive by creating the notion of npcs that you have to battle or help 
I would like to add to the incentive by creating the notion of abilities you can use to battle better or find things better
I would like to add to the incentive by have the visuals be modern looking 
I would like to add ot the incentive by having a really fluid and responsive interface that makes entering data fun
I would like to provide the way to scroll back and forward through time on a character profile to see where they were in the past as a way to build confidence in how far they have come building their habits
I would like to build a way to have users go on 'campaigns' which coudl be preplanned towards helping them adopt new beneficial habits and structure their time
I would really like to have a lot of these things be procedurally generated with the assistance of LLMs to the extent that I can provide a theme and someone can feel like they are doing DND with an LLM dungeonmaster. 

and i think I've had a bunch of other ideas but that is jus the problem, the features aren't keeping up and I will have no idea how to do them well until I start churning out the mvp. 

so let's go. Ithink I just finished up the character part of the event sourcing with heavy help from the llm but I don't really know what is going on, so I'm going to go back and study it. that is what I'm going to do. 


the first thing that i do is define the different types of events I want to support for characters. that includes like creating a character and then allowing them to get experience and gold but also then their gold should decrease when it is spent so that was a good callout from the llm. then there is the acquisition of loot which would probably just go to gold at this point since I don't have an inventory as yet that feels like something I'll have to get later. 

this is held ins a module that is just a POJO that I export, an object literal that is a map from character event constants to strings of the character events. 

ok, so the next thing that I have is the character aggregate. so far I only had a feww things in this aggregate. it inherits from the Aggregate class which does a lot of stuff already to begin with. do I remembver what it does? let me see... I think it is doing things with processing events more generally. for example I think it has a method to load all of the ewvents for a given character by id, and I think it also has add event functionality or apply event functionality that it manages. that is all I can remember for the moment, let's see what I remember. Ok, so here is what I got righ tand what I missed: 
what I missed: 
- it has a constructor that assigns a unique identifier to the instance of the aggregae when it is created. it also assigns a version to the aggregate which gets incrementsed when a new event is processed for it. it also has a buffer array of uncompleted events that needs to be saved into the event store at any given time. 
- while I was right that it has an apply event method, this method only updates the aggregate's version to match the version of the event it is coming from, it doesn't do other things involved in applying an event like updateing the aggregate's fields because that depends on which child aggregate is getting an event applied. this makes perfect sense.
- there is also an 'applyEvents' method which is just batcdhing the applyEvent function acr4oss multiple events. 
- I think I was right that it has a way to add events, so good for me for remebering at least what the methods were, but once again, I didn't know what was really in there. the two arguments are type and payload that it accepts. then it looks like it assumes that all aggregates will have a character associated with them which is a fair assumption for the domain I'm in, then takes that type of event which is what we just defined, the payload which is what to actually adjust about the charactyer, then the timepstamp for ability to do things with the events based on order and time, and then the version which is a stricter enforcement of ordering. 
- then there is a clear changes for when that buffer of events needs to be emptied. this is interesting to me because you will call this from the child aggregate when the events are all applied, it is a lot of methods exposed and stubbed out with basic functionality to be called and orchestrated by anothe rclass. 

ok so that is aggregate, then there is the charac6er aggregate that we already had. what was in that? going from memory, I think it was gainExperience, and perhaps create character. the create functionality was to take in the name and race and class of a character, process the event by applying these directly to the aggregate fields and then persist the event to the events store. I think before you could do any of that you had to hydrate the character by loading it into memory and applying all events that the charater had in sequence. for the gain experience, this was similar except you were making that change just to the experience to add to it. you still have to hydrate the character, you still have to update the version to match what you did, but then ultimately you mutate state in that way, and you do it in sort of that loop of applying the event to the character and doing a pattern match on the event type to find out what you are going to do with this particular event. 

alright so it was surprisingly easy to implement the remainder of methods when I saw the few examples that claude provided. so the main idea is you have your events that you define as just a mapping of constants just to keep things organize3d and avoid magic strings in the code. those live centralized in one module and get exported as a javascript object. cool. then you have this aggregate system that is respon sible for managing how objects will interact with the event store at a high level. it doesn't do a lot of interesting things, it reminds me of an absetract class in java, a design helper. its main goal is to say "hey, so you are goin gto be doing these things with event sourcing right? that means you will need a way to add new events, you will need versioning of your events, and you will need ways to apply one or more events, ways to save applied events to the event store, and ways to flush a buffer of events that need to be applied snce we are doing this stuff with an event driven model and not necessarily all synchronous. then you will need a way to load all of the events to rehydrate the character instance that you want to work with. All of that makes sense. let's see how it performs. 

ok now did a few muscle memory passes towarsd fixing the routes so that the files are referencing each other correctly through imports. probably could have tried to get that automated but since Id dint' come up with th efoler structure myself this was a bit helpful to me to get in the headspace. the next part is to add some missing methods from the characterController that the response form claude left out I think it was bypassing this and going to the service when it gave responses. Ok so for the controller methods the overally conceptual pattern appears to be to take in the http request, deserialize very simply the json payload body into a simple pojo that just has character id and amount with those same field names using that destructuring operator and the fancy tool that an object literal with a variable becomes a kind of dictionary or map in the js parlance with the key being the field name and the value being the field value. so then once we have that we are going to do a little bit of input validation on it in this case maybe the amount gets validated for an add gold situation, if it fails validation then we are passing it to the bad request response and returning it back to the calling source that way via HTTP from the controller. if it gets by however, then we need to call the character service with the values and there had better be a method available to handle the specific scenario we have provided. these methods are set up to return an updated character object which is the aggregate I think but is meant to be a point in time model. I think all of those methods will replay the events to get to the latest state prior to the event happening and then they will apply that latest event and also save the event to the event store. that is how you get back a model that is correct. I don't have it figured out for two of these yet one of them is spend gold and the othe rone is get level info. there is also a get loot method that I need to figure out. going to go try that now...

ok, I need to understand the node module system a little better. so far I've been using it pretty lazily without really understanding the rules and getting surprised every once in a while when it is used a different way than the pattern I'm memorizing. I've used it the following ways: 

export default class MyClass {}
export default PlainObject {}
export.thisFunc = (someParam) => {console.log("this is an arrow fucntion about about that?")}

ok, here is some key knowledge about this: 
- every file that is in a node project is a module
- modules encapsulate visibility of scope, so everything declared within them are by default private to those modules. 
- module.exports is an object that is the module. when you require a module in another file, you are going to get back that exports object. 
- exports is syntactic sugar shorthand for module.exports. if you put something on exports, you can make it available when someone else requires your export

Ok, I learned that the reason I'm seeing the different syntax is that the module paradigm has changed but the code that was generated for node is using the older module syntax and vite is using the newer syntax for module imports. the export default vs. export const part is more about how many things I want to import. I have to use named exports to get more things than one from a module and when I do that I have to use the same naem for the imports and then alias them if I want to chage them. 

one thing that shows how green I can be on http and rest based system calls is that I didn't realize that I would need to use params instead of body for the get request althought that seems obvious not that I have thoght about it and heard the answer to my question. I have worked with get requests enough int he past to know that params are the URL portion and that post requests have a body, but the fact that tyhis wasn't automatic suggests I could be applying more rigor to it. 


somethign else to record here, the difference between named and default exports and imports, you import a named export with {name1, name2} from ''; and you import a default export with import Name1 from '';

one of the things I feel is much faster with an LLM empowered ide is to make whole module edits and refactors by writing the intent in natural langauge. for example, i realized I was using the node syntax with require('') statements and exports object for all of my modules but I wanted to be consistent and use the ES6 modules supported by vite, so I was going through and asking the llm to update the files to change the syntax. even with a lot of vim ninja ing or vscode multi select magic it would have taken a while for each of these since there were multple steps for each refactor. what would be nice is to have like a recipe for this type of refactor so tha I could just be like "do the thing that I've been doing" kind of like a macro recorder for natrual language intents that are generic enough, to make it even more automated, but I'm really happy about how well it works and how few errors it makes. 

I asked the LLM to continually refine its own architecture and explain what it was doing so that I could learn some best practices for this type of node backend. it implemented a layered architecture where I woudl have a module contain and export a singleton instance for the contained class definition but then also export the class itself for testing purposes. it would then tend to have error handling implemented in the servcie classes at every layer around the business logic, so if anything went wrong in the repository/data access layer or in the business logic around that, it would be caught as somethng like a validation error or other named error and taht error would be propagated up to the controller module. 

there was dependency injection done at the top layer of the class in the constructor where dependencies were generically accepted as an object literal that was empty, then the services that this class depended on would be set if they existed on this classe's instance during construction. very flexible. 

it also did good grouping of import statements as well as strong js docs documentation of methods explaining the types taht were returned. since I'm not using typescript yet, this is crucial to debugging and contracts since I'm missing that layer of type checking that goes on.

I've foudn there is a bit of a skilsset and a learning curve to providing the right context to the llm by opening and providing the right files. in cursor this is done by adding to its index of prioritized documents to embed and add to context and sometimes just having more things open. I found I couldn't directly control and prevent files from being added with the command + enter option so I took to a more surgical approach of adding the finels manually with the plus button when I really needed to restrict what it was seeing. furthermore, I had 3 reposityories goingf at once for my project, a monorepo in the ide might have been better but it also might have been worse since I'm working on a project with mvc pattern on the front and backend and there are overlaps in controller files and service files and routes files that aren't good to be overwritten when the llm gets confused between which is whigh. I found it was a pretty useful pattern to have an overall workspace file (which cursor gets from being a clone of vscode) that gave me all of the directories I wanted in one ide project explorer, and then be more selective when I wanted to but generally giving the llm chat the ability to see whatever files I had open and navigate through them more fluently myself. not a perfect solution and the llm itself is not going to be super discerning I have to give it the context that these things aren't related sometiems but it is pretty good at learning the context iteself also so in combo we had a pretty good time of it. 

ran into another learning opportunity about es6 modules vs commonjs modules. 

look at all the cools stuff it taught me: 

Key Differences:
Loading Behavior
CommonJS: Synchronous, loads modules at runtime
ES Modules: Asynchronous, can be statically analyzed at compile time
Caching
CommonJS: Modules are cached after first load
ES Modules: Modules are singleton by design
Module Resolution
node_modules
Dynamic Loading
;
Benefits of CommonJS:
Simpler dynamic imports
Works in older Node versions
More flexible module resolution
Easier circular dependencies handling
Benefits of ES Modules:
1. Static analysis possible (better tree shaking)
Native browser support
Top-level await
Named exports are bound (live)
Better error messages
Future of JavaScript modules

the llm pretty much told me that javascript community is deprecating commonjs modules even though older versions of node support them, is aware that I'm using newer node, and is still suggesting not to move away from commonjs so that I don't have to specify module as the type in my package.json. that is odd to me, I would prefer to be future proofed for 2026 if it is only like a one line change. so I asked it to just show me how to get the more recent support

I learned that .js extensions are required for node when it is supporting ES modules syntax but it is not required by vite or webpack or other bundlers that are designed to support esmodules as they tend to handle resolutions for the file extensions without having them explicitly stated

alrighty, looks like the llm is about to drop some great networking knowledge on me, let's see what it is saying. so I got a proxy error from my frontend trying to communicate to my backend. I have a feeling that it is becasue I changed the port that my backend was listening on, but let's see if it caught that and is telling me the thing. 

it does a great job ob breaking things down. it is saying that the front end is setup to make reqeusts to /api/v2/characer route. 
- it establishes that the expectation is that this is happening throiugh vite and that vite should know how to proxy this to the backend which is running on port 5001.  - the proxy isn't working, so then there is a proxy setting that vite needs to set on the frontend project so taht vite can use the right address to communicate to the backend. 
- there is a configuration for the vite server to proxy the /api routes all tot he backend server. handling cors headers automatically is part of this as well which is a cool feature and somethign I need to learn more about. but taht is cross origin request 

oh this is great, posting here: 

first nugget: 
Browser (localhost:3000) 
    ↓ HTTP requests
Vite Dev Server (localhost:3000)
    ↓ Proxy requests to /api/*
Backend Server (localhost:5001)

second nugget: 
```javascript
// When your frontend code makes a request:
fetch('/api/v2/character')

// Vite intercepts this because it matches the proxy config:
server: {
  proxy: {
    '/api': {
      target: 'http://localhost:5001',
      changeOrigin: true
    }
  }
}
```
I was right that vite was acting as an intermediary web servier between the browser and the backend server, that is somethign I didn't reallize was happening, I thought it was doing all of its work prior to the runtime to transpile the code and prepare to serve it statically. 

vite is acting as a reverse proxy in this case, because it is getting requests from the browswer and then forwarding them to the backend for processing. a forward proxy would be to stand in front of traffic goign to the browser I think and then do something with that or control where it is routed to. 

thisf actually gets around the same origin policy for requests scripts make from them to potentially different origins. in this case front and backend locally are both running on local host address, but if this were deployed they would likely run from different network addresses. maybe they actually already do because they listen on different ports adn that could be cross origin because a dns resolves to an ip and port combo. yeah just checked my knowledge on this and llm is right, CORS applies at the socket level not the ip lvel it really operates at the http header level which includes protocol too I think. 

the llm introduces but then explains its own bugs. for example for my event store, it was trying to get all the events using 'character' as an argument when the event store getEvents method only accepts an object id it was suppoesed to be the aggregate id of some character event which I don't have. 
so instead of querying mongo for the id that wwe don't have as if it were an aggregate type, it realized it should be querying for all of the events of the type CHARACTER_CREATED, then it should be getting the first one if there are any, then get the id for that one and replay all events associated with it using the load() function we'd implemented earlier

I was able to see how the llm addressed somethign I would need to do which was to update the error handling to have it set the character value to null so that the character creation form would show in the jsx. this was good but I encountered useeffect again and wasn't quite sure what to make of it. so I remember that use effect is related to the react 16 changes that were made to move react more towards a pure functional approacha nd away from class based using javascript classes that were stateful and mutating those within the scope of the component. instead, they were using things like useEffect to make the state changes happen in a similar way to the way that algebraic effects and monads work, but I don't really have that theory down just right so I need to think about this practically. What is the mechanism and purpose of useEffect? I should ask it

another thing, the llm injected a bunch of knowledge of the event type into the frontend even though the type of event I would be proecessing can always be inferred from which type of rest request I'm making to my backend api. so I ended up having it take it out but not before questioning whether or not it agreed with me and hearning its points for why it would have left it in and  ultimately getting it to be convinced that it shoudl remove what it added. helped me be moresure of myself in the process.

I learned soemthign form the llm about how routers work with node. the routes are interpreted in top down order, and since there was a matcher for :id for a get request that happened above the match for level-info, level-info was interpreted as teh id field at the end of that earlier route, and then mongoose was trying to query itself with that label and breaking because it wasn't the right kind of id.

having a fun new problem now, the character is always resetting when I go back tot he character screen, even in thes same session. this is not what I wanted, I want it to find the earliest character created event and then use that to load the player. I'm not seeing this, this is my next challenge to fix.