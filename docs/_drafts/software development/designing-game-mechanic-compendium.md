ok, so the premise is: 

you will collect a lot of data from different sources that will tell you this information: 
- lists of games, I want games that are published and peopel are playing and they have enough traction and fan base that people are writing articles about them. 
- raw text data written about these games from a bunch of different sources
- I need specific information about these games to be able to crosswalk their mechanics: 
    - what genre are the games
    - what are the core mechanics? 
    - what are the supplementary mechanics? 
    - what are the dynamics and emergent properties of them? 
    I think I may need a glossary or shared and standardized normalization of some of these concepts because mechanics can be defined different ways and can contain a lot of variety when it comes to their usage even if they are the same thing. examples of things I'd want to know which are far from comprehensive:
    - what kind of inventory systems are there? 
    - what kind of collectibles are there? 
    - what leveling system if any exists? 
    - what are the primary goals of th eplayer? 
    - are there quests? 
    - is this a casual game or a platformer or arcade style where it is literaly a high score or are there more sophisticated and obfuscated subnarratives and minigame aspects embedded into this? 
    - is it an open world game? 
    etc. 

    I bet people have gone through a lot of trouble to build these types of databases already, it would be my job to do a massive intake of all of it. 
- maybe analytics streaming data about these games, but this isn't that important for the primary use case


once I ahve this I would need to get the data in

find ways to clean it up and make sure it passes a validity test
find ways to transform it so that it is more normalized
make sure that when new data comes in I can deal with it appropriately
do data transformations to map the game data and store it in some canonical format
create a way to read it on the other side

provide views against this data, have ways for consumers to handle it and display it that are decoupled from the pipeline that ingests and transforms it. 


