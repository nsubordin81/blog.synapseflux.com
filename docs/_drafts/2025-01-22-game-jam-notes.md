    here I am, starting a game jam again. it started on january 17 but I've really done little else but plan and now it is january 22. I created a high level game design document but my mind continues to wander about what the narrative could be. 

I guess that is one important thing to internalize, if I let myself just dream on branches a narrative could take forever I'll never build a game! 

really thought a lot of it is just available time. Between my dayjob and family/house commitments I have a scarce amount of time to spend on this per day. even with the alotted 2 weeks, it is not likely I'll get to a finished product. Thor, the Pirate games founder who runs the community this jam is from, gave some encouraging advice on that front from a live stream, somethign to the effect of "this is a gift of time to yourself, it doesn't matter if you finish or submit or whatever just do it and if you learn something great." Now that is a level of commitment I can get behind.

ok, I'm using godot. I have reasons for this. unreal and unity have some friction to get started and I get bogged down customizing them. I want something a little less opinionated than I imagine gamemaekr studio to be, although that product has consistently been well reviewed and seems like maybe a better first choice. Had to pick something though.

devlog day 1 which was 1/22, for ~ 45 minutes
created a godot project. created a character node following their docs. Could have sworn I've done this before but here we are, I didn't retain it I guess. high level things I learned that I should commit to memory. 

- Scene in godot is like component in other engines, kind of like a recursive-hierarchy for managing and organizing discrete things.
- when creating the player node, I was instructed to click this funny looking button that looks like a weird checkerboard with dots and kind of like one of the adobe pathfinder options for like "erase overlap." What I think it does is make it so taht if the child nodes of this scene node are selected then the whole scene will be selected, kind of like a grouping thing.
- adding a node to the hierarchy is done with command + A (I'm a mac user on this project)
- godot is helpful, it tells you when a resource type has dependencies you haven't fulfilled yet with a warning icon next to it.
- almost immediately I needed assets and was leaving the godot editor to find them. Piskel has this free browser based tool to easily construct tilemaps for pixel art. helpful tutorial here https://youtu.be/FEtFgTolOL0?si=BUABXkS2BIAA6v7I, but then realized it was talkiong about tilemaps. and also realized I was already deviating from the fastest path here. I needed a public domain licensed asset pack so that I didn't start doing an art workflow in the middle of my basic setup. Mechanics first, people! at least that will be my policy because I'm the type that could just start endlessly plunking on a piano or sketching things I like the look of or thinking about plot arcs and never make a thing, as we established earlier. Know thyself, I guess.
-  ok, going to use some Kenney assets to start with, doesn't really matter which. ended up getting their all-in-one bundle because I want to support them and because I like the idea of having several assets to prototype with. that was the end of my time window. 
- went down another kind of rabbit hole trying to use a tilemap from the kenney assets for my character instead of the individual pngs that were also provided which was more of a fit to the tutorial. it is really hard to make myself stick to the easy way of doing things lolz!

devlog day 2 1/23 for ~20 minutes

man I can tell when my brain goes on autopilot learning some of this stuff. it is when I get tripped up by the smallest deviation fromt he instructions. Godot online manual is amazing, but I get this unexpected behavior from the engine. 
whoops, created a capsule collider on top of my character sprite but it is invisible. takes me a good 5 minutes to realize it is tiny and in the corner becausae the character is actually huge and outside the viewport. idk how I'd have figured that out without a little bit of gamedev background. 
next fun thing, added a script and the template GDScript that is generated is different from what the manual has. had me wondering if I was using C# for a hot second. 
very nifty that exported variables just work. 
so just monkey see, monkey do for me with the instructions on player movement until the guide casually throws in that "of course you will want to normalize the vector2 values for x and y so that diagonal movement is not faster than horizontal movement or vertical movement combined. took my tired brain a good couple of moments to register, but makes sense. X+Y can combine to be bigger than the max x or y values so velocity could end up larger, but if you normalize them, then the scale of increasing velocities will match and will total to 1 for diagonal and sraight movement
good for you godot script taking a page out of JQuery's book and making $ sugar for a commonly used 'get_node()' function. say what you want about jQuery, that was a slick thing, until it got abused
another amazing game engine feature and benefit of making your own language. They have reified delta time. a lot of engines have that living in some object or make you roll your own. it makes sense that if the engine can know how many milliseconds it takes for a frame to get replaced it should make that available to the developer
whoops! inverted the vertical movement controlls, positive y is down. good to know
whelp! thanks to Godot's ease of use despite my best efforts with two days and just under 2 hours of actual sitting in front of an editor and despite my best efforts I actually have a sprite dude walking around my screen

devlog day 3 1/24 spread across 3 25 minute pomodoros that were semi-productive
- memorize this hotkey: command + R. also F6. that is running your stuff, get that feedback loop going
- for some reason the character is pretty small, think about units and measurment. you know, after you are done
- forgot I need to reverse the direction of the character as they move. 
- already singing the praises of the godot course from gamedev.tv that I got, some of the issues I'd encountered with godot immediately addressed. saving and loading scenes and main scenes, backing out what I'd done following the godot user manual, and they point out that the default asset size (for kenney assets at least which is what I'm using) is tiny an I will need to resize them.
- first key insight for pixel art game: use global default texture filter of 'nearest' because there are anti aliasing effects being applied to the low rez art making it look blurry when we want sharp edges
- next one, needing to change the default window size to 1/4 of a typical monitor's display resolution and that makes the window larger, and then setting the stretch mode ot 'canvas items'. this is good for me to know but I'm not goitn to go into every detail like this on a blog post. 
- there are tradeoffs between scaling up the sprites v. scaling down the window size and stretching the canvas. a bit one is that fonts are going to be pixelated and hard to read
- and of course the danger of having pre-existing knowledge, I go down a 15 minute rabbit hole wondering if I should use large file storage (LFS) when I set up my github repoitory for godot, check in all assets, or just check in source code and not worry about assets. I ultimately got a good nuanced answer from Claude that I think represents public opinion alright, but this was dewfnitely not something  needed to know to finish this tutorial!!!
- ok, now discovering that there is a workflow, you close scenes and then open them to work on other ones, closing scenes and creating new ones is something I'll be doing a lot. that is command + w btw
- somehow fell into a rabbit hole and then got out but lost a good 10 minutes or so on organizing and tagging my github repositories and then one of them reminded me of something I'd heard  and wanted to confirm about openai and I watched part of a youtube video before snappign myself out of it
- wow and somethign that was so hard for me to figure out becomes intuitive an simple with this godot tutorial, turns out all we needed to do was press a little grid button and you can get the frames from a spritesheet or tilemap. cool beans
- the way that animating 2d sprites has been solved for by godot gives me hope as an indie developer with little time. once you know the tricks it is fairly step by step
- stopping midway through to make sure I have time for other priorities to get done.

devlog day 4 1/25  probably 20 minutes also watching a show
- there is an add frame frm file option for when you only have a single frame animation
- there is also a loop button that has animations loop instead of playing once, good to know
- rectangular collider is better than capsule collider for 2d character doesn't fall off platforms
- making child scene is different than making nodes, shift + command + a instead of comand + a for nodes, or the link button. 
- for some reason my character is falling but the one in the tutorial is not. I didn't attach a rigid body or anything, so maybe something different in the version of godot I'm using? 

devlog day 5 1/26 so far 20 minutes, hopefully 40 by the end, another divided attention session during a naptime
- quickly proved to myself how littele I know about godot, the script that was generated for the characterbody 2d was some kind of template, and it enabled gravity on the character and I couldn't test the animation. I fixed that but then deleted a variable and godot was complaining a bout not having it and I didn't know where to look in the UI to remove it there. will figure it out, had a nice diversion into the land of 'motion_mode" 
- ended up comenting that whole file, it was using the physics process function and I'm not sure how that works yet
- learned you have to close all your godot editor scripts when you are using vs code or you get hounded by warnings and reload requests. managed to accidentally close the scene as well and that took me a minute to figure out.
- was really confused that at first no debugger lines were going I think it was becuse my external script wasn't being used because I had it open locally and it was failing to reload. Then I got it to rn and forgot that I'd mappeed th einputs to 'd' and 'a' keys instead of arrow keys, there was no debugging info it was just silently not doing what I wanted becaue I was pressing the wrong keys. Then after that I was getting a dbug log message when I realized my mistake but the animation wasn't playing, and that was because I was jusing the wrong name for the animation.
- ok in about 10 minutes figured out how to just get the walking stuff right. 
- seems like I've been bitten a little bit by installing the latest version of Godot. this is community supported software, gotta cut them a break they are doing most everything better than the big dogs as far as I'm concerned. so look slike 4.3 has issues with the visual studio code marketplace official version of godot-tools becuase I was trying to see the available methods on my Input singleton and it wasn't telling me. that won't stand, because it takes time to google the api man pages, so spent a little diversion finding a reddit post about it (thank you Exerionius https://www.reddit.com/r/godot/comments/1aigrsz/auto_complete_in_vs_code_gd/). So downloaded a zip with the .vsix file inside to get the development branch features on my extension and it started working again.
- ok, not sure why but I"m really satisfied, I thnk in about 20 more mihutes I managed to iterate fast on learning and got myself to having a character on a platform going left and right, but the code is good and I understand it. I took anims out but had them working so I'm pretty confidenct they'll work again whenever I put them in. feeling good about all of that, and how this was tested each time. I should porbably stop now, a lot to do in the rest of life and I'm getting sick I think


devlog day 6 1/27 1 pomodoros so far
- had a few hiccups but generally ioterating pretty fast, I think I got distractewd once or twice and pulled out of my zone by whispy needing food, by investigator discussion ,generally feeling ill, but my focus and dedication were mostly good an I'm follow ing along and trying to learn
- here wa are at jump, this is the rabbit hole for platformers that I always get into. the physics feels floaty for my prototype hso I want to tweak the heck out of the jump until I either emulate mario or sonic or some other hting that I am used to, and then before I know it I sank a month into it but forgot a lot of what I learned about hbow to model the jump parabola. so this time I'm just toing to go with the flow and let the jump be floaty. I have art asset stuff to learn, music composition to do, a cool core mechanic involving environment interaction to map out, lots of cool stuff to do. 

devlog day 7 1/28 just one pomodoro probably feeling sick
- got really sick and felt discouraged about tacking these things, wich the idea of timelines and being impatient with advancing, but I shoulde embrace the concept of 'not yet' growth mindset
- learned you can use ! and 'not' , but they don't have an 'or' and 'and' equivalent in gdscript
- it turns out you can use the 'and' and 'or' operators and this person just didn't cover it. oh well.
- actually did the challenge without looking at the answer, good for me, not rushing 
- flip_h set to direction == -1 shorthand for true when direction is negative fals when it is positive
- animations happen in a separate function but I cat to name it and it seems to be pretty straightforward, taking in the direction var was a good call, something that I'll have to remember, some of this flows pretty intuitively
- 

devlog day 8, 1/29 not getting enough time today again because I'm sick and lacked the focus to get through work fast enough this time. I'm getting better at this each day. 
- renaming things so that they make sense in your project is a good idea
- death zone doesn't haven to encompass your whole level it shoul be a rectangle that the player hits if they fall below the level
- got a fast tutorial of how to set up physics layers in godot with collisions per layer, that was helpful. 
- several good lessons today that I'll need to reinforce:
    - there are layers of physics that you need to set up, I knew this from unity but godot has the same concept, collision layer is the one you are on, th mask are the ones that you can collide with, makes suer that things taht hit each other are the right things. 
    - you send signals, which is just coding in hook functions that are predefined in the editor, unfortunately for me the vscode integration here doesn't seems to be great so I'll I think have to manually check the names and write them but it will hopefully work
    - using a marker node is a way to have a visual representation of where in the world something invisible is happening so you can consisitently refer to it rather than keeping track of coordinates and work with it in your scene view
    - you can make colliders visible on the screen with debug make collisions visible option. 
    - 

devlog day 9, 1/30, feeling like in a  day full of potential I didn't get to sit down to this until the end. this is partly me, I had opportunities. I was also feeling really crappy because I'm sick,so you know, it is a victory that I did anything. 
- setting up the spring jump myself. how did that go? well I made animations and kind of wobbled through things, let's see what they did that I didn't. 
    -  I needed to use area2d for this thing
    - the collider I got right but made it too big
    - the animation playing I needed a handle to the animated sprite in the script
    - the animations were good though. 
    - gotta set those collision layers right with the masks
- the next part that I haven't started  yet is  to use the is keyword instead of a collision layer in order to set up when the player makes contact with a jump that is more specific to those two objects and more dynamic but not as simple to remember.

devlog day 10 1/31. this is the last day of the game jam. I significantly slowed down and eased things on myself becuase 1. I'm not in a position to push it, have too many competing priorities right now and 2. it is a victory to just achieve consistent work on this and be interested in it again. happy for that, going to keep up consistency and momentum for myself. I am a game creator. I'm not not a professional one. yet. I'm learning a ton by following these tutorials and I am excited for the opportunity to apply the lessons I'm learning and my own thought and problem solving abilities to my own projects. 
- ok, so lesson for today was that even though the training I'm taking says to do something it may not be right. they had the editing on a different line for the class_name part and when I looked up the syntax myself it needed to come before the extends keyword, not after it. good to know, until I did that it just wasn't working. seems like that is one risk for troublesome debugging with games. sometimes things just won't work and you have to go through a deduction to find out what didn't get set up right, which might even involve doing things differently than some source told you to. 


devlog day 11 2/3 - we are officially beyond the point of the game jam and now I'm just doing this. I should share the wealth between this an my othe rprojects more now since it is not the one I was focusing on to begin with but I'm glad I'm no longer neglecting it because it is my passion. the other two are two though, they are all important and tied to the purpose of creating interactive experiences that bring a little bit of me out to people and try to solve real problmes for people. 

today's lesson was a good one. I had a bug that was stopping me for maybe a quarter of an hour and it was to do with the fact that there was a careless typo in my function definition (function instead of func) and godot is pretty silent about telling me when I have issues with the editor code. it turne dout that I just needed to fix this but for the longest time I though tit was with another htin I was trying to do with the editor which was to get the editor to use a custom signal as was part of the lesson plan. 

devlog day 12 2/4 - very short session
- I learned that you can use the dollar sign in godot to access elements from the scene tree. so if a script belongs to a scene then it can instantiate on startup any node from that scene. 
- 

devlog day 13 2/6 - another short session, really don't have a lot of time right now, but some of that is within my control, I'd better get better about spending it
- learning that there are two animation tools. there is the sprite animator in godot and there is the animator that animates all features of the node. 
- you create animation there are basics of the timeline, you can zoom in and out and you canset the duration and there are controls for looping and auto playing and setting keyframes. 
- the keyframes areon all proprties and youc an create a keyframe for somethign like the position of one of the nodes in your scenes tree. generally not a good idea to change or animate the root node's position. they don't say why. 
u can also change the interpolation settings, cubic makes it smoother. 
- didn't try this but there is a way to call a function during the course of an animation
- connecting the signals of the traps at the start of the game involves usage of arrays in godot. arrays work like they do in C except they are not typed and you can put anything in them. they ahve subscript operator for access and modification of elements and append(), remove(index) and size() methods as well as an erase() method that deletes teh value of an element without removing it
- there are also groups that are tags you can give nodes. use the root nodes for this an dthe 'groups' tab. feels very jquery selector to use this and the scene tree to get all the nodes tagged with a specific group. efficient way to slice the available scene nodes


devlog day 14 25 minutes 2/7
- spent the first part of this finishing up the tutorial on how to get the traps working and navigate the node tree to extract an array by filtering from a set of group tags. used a for loop and also connected a signal to be able to set up relationships between the nodes and their events. 
- next part of this session is spent on tilemaps. convetion to create a scene for the tilemap, to load in a tilesheet that has a certain dimension (16x16 for example) per tile, and then use that to create a tileset and within that tileset an atlas. 
- atlas and tileset are used to set up the alignment of things, then tilemap is used to paint them into the scene. 
- quick overview of all the tools available for tilemap, you have basic kidpix toolset
- you caqn make patterns out of things you select int he tilemap and then use them to paint the pattern. 
- you can paint on the tilemap from within the level scene but make sure to have the tilemap selected and also the pointer 
- it is kind of roundabout how you get a collision shape set up for the whole tileset, you hae to kind of go in and do it from within the tileset and then apply it to all of them. 'f' key creates a collision shape. you can make smaller collision shape for your tiles. there is a way to make it so that you don't collide when cooming through the bottom but you d-o when going throiugh the top. it is called one way in the physics menu of the tileset. 
- somethign I couldn't do whatn I tried it by myselfm, creating a new physics layer, the menu is hidden in the tileset.
- 

devlog day 15 2/21 25 minutes 
- noticing that without the game jam to help me stay motivated I went back to other focuses and didn't do any godot practice for something like 2 weeks.  
- the next lesson I'm doing is about tilemap terrains. this is a way to paint tilemaps that doesn't involve having to select a pattern or a single unit and paint with it to be able to quickly build tilemaps. 
- noticed that this works fairly well, it is a lot of setup but once you have it working you can paint levels and they just know which tiles to use. it assumes you are going to hve one surface texture and then several others, but that will be generally true. 
