```markdown
---
title: "Getting Back into React"
date: 2025-02-22
description: "I'm trying to get back into React after having not used it for years. This post will cover my journey, challenges, and the new features I've discovered."
tags: ["React", "JavaScript", "Web Development"]
---
```

# part one, the docs way

- simple enough, for the uninitiated, talks about how react apps are made form components and those are chunks fof the UI that have discrete logic and appearance to them. small as a button, large as a page (that is right off the main site docs)
- components are functions that return markup. 
- you can nest them using jsx
- convention is to use caps and html tags are lowercase
- dipping into javascri pt syntax, they use the export default
    - export and import are module style for ecmascript
    - you can export variables, funcitons or classes
    - you can define first and export later
    - you can import within {} or you can import *, most build tools will remove unused imports when compiling your JS
    - you can export line by line what is public, or you can just export default the one main thing from your module. you can only have one of these per file and when you import it it needs no curly braces. 
    - 'named' vs 'default' exports is what we are talking about. 
    - default doesn't need a name
    - you can use the default keyword to import something that is ambiguous from a module tha thas a default export and named exports or to export later what was defined up top
    - in favor of named exports they tell you exactly what htey are exporting and you ahve to import them by name. convention is that default exports anmes in iimports should coincide with name of the file. 
    - you can 're-export' someting from anohter module within your module. export {name} from 'file.js' this is mainly useful when you want to exposed a common interface for a more hierarchical package that has a lot of modules.
    - when re-exporting, if your module has named and default exports, you have to specify separately with the re-export syntax, {default} and * both to get named and default 
    - there are such things as dynamic imports, but not getting to them right now. other ponit is you can hoist imports and exports so they can be anywher ein teh file so long as it isn't inside of {}
- react uses JSX for markup. it is supported well for all local development react tooling. 
    - jsx requires closing your tags, html doesn't. Components can't return more than one JSX tag so you need parent <div> or <> element. 
    - css classes seem to work the same way in both jsx and html
    - inside jsx you can use {} to inject javascript inline into it so you can embed variables and such
    - this can also be done in attributes with the {} surroudning it
    - you can conditionally supply jsx attriburtes with javascript around jsx, and some operators just work within jsx like ?, : and &&. 
    - you have for and map control structures in javascript that you can use to generate jsx components. you need to have unique identifiers to ensure taht when things change react can specifically chase that element. 
    - there are event handler funcitons taht can be invoked via the onClick attribute of interactive components. you don't invoke them you just pass them. 
    - useState is a way to declare state variables. when you deconsgtruct the state variable you get two things, the current state and a way to update it with a function handler. state is set up uniquely for each time the components is instanticated and not shared between instances of the components. 
    - hooks can either be using the built in hooks or you can compose new ones out of the existing ones. you can only use hooks at the top of a component or other hook. so if you ewnat to use one you will need to extract a new component for the contents of that loop and put the hook at the top of that. 
    - you share state between components instead of using local state when you need them to share state and always update together (so like they both have a reference to some underlying thing that can be updated. the simplest way to do this is to omve teh state up to the closest parent component that has all of them in it. 'lifting state up' is the typical name given to this approach. 
    -  this is more or less where the getting started guide stops. 


    # part 2, the conceptual underpinning approach


    thinking in react document: 
    - flow is break apart user interface into components then describe the visual states then connect them so that data flows throiugh. 
    - api + wireframe is a start. then you can draw boxes around them and name them. designers may have done this already. then apply these lenses: 
        - single responsibily principle a component should do one thing or else be decomposed. 
        - css class selectors as a guide
        - design layers (this one makes less intuitive sense to me)
    -  draw the boxes, labelt them, put htem into a hierarchy, they can evolve so do what makes sense now, not later. 
    - the first step after designing the component structure is to impelment it as a static, no interaction one way data flow. communication between components would come later, instead you have no state in any components and you just have the top level copmonent get a version of the data model and send it down through props to the rest of the app. 
    - when designing for state, the guidance is to start from minimal possible state and derive everything else on the fly from that state. there are 3 rules to look at to figure out if somethign shoudl be stored in state: 
        - is unchanging? NOT STATE
        - is it passed to a component throiugh a parent? NOT STATE
        - can it be derived from state or props? NOT STATE
    - figure out where state should live: 
        - identify that common parent table of components sharing state
        - you can put the state in the common parent or a parent of it. 
        - if state doesn't seem to fit in the parent or a parent of a parent, consider putting it up in a new component just to hold it. 
    - hooks are how you impolement state now in react, special functions that lest you hook into react . the deconstructed values as explained in getting started are the current state and the setter function handler. 
    - you can set up the hooks so that if you set them properly to a default they will be passed down as props and displayed, however to do it when the user is interacting with a form, you need to pass the function handlers as props to the child that has the form in it. 

    ## rules of react

### encountering them as I go through rpgmylife code. 

hooks are cool and special and different 

useEffect is a special type of hook designed to help reach out to external sources
you do set up and teardown and then you list the dependencies and the effect will trigger again whenever one of thse dependencies change. 

the effect will update state.

same rule as other hooks, you can't call it in a loop for some reason or other control structure. 
