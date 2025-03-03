---
title: "Setting Up React with Vite: A Quick Guide"
date: 2025-03-03 12:00:00 -0500
categories:
  - Programming
  - React
tags:
  - react
  - web development
  - javascript
  - vite 
excerpt: "A concise guide on setting up a new React project using Vite as create-react-app sunsets, including step-by-step CLI instructions."
classes: wide
toc: false
---

So it looks as though the React team is saying goodbye to their create-react-app starter configuration. You can read more about the history of this configuration and the rationale for moving away from it on their blog where they announced the deprecation [here](https://react.dev/blog/2025/02/14/sunsetting-create-react-app).

I happened to run into a better way of building React apps already anyway as a student of [ThreeJS Journey](https://threejs-journey.com) for Bruno Simon's ThreeJS Journey course where he covers it in his ['first-react-application'](https://threejs-journey.com/lessons/first-react-application) lesson. I want to keep this post short and sweet so I'll condense his advice on it into these two things: 

1. install Node.js if you haven't already
2. run `npm create vite@latest` and follow the interactive CLI menu instructions.

Here is a sample of what mine looked like: 

```shell
__yourprompt__  on    main ▓▒░ npm create vite@latest

Need to install the following packages:
create-vite@6.3.1
Ok to proceed? (y) y

> threejs-journey-exercise@0.0.0 npx
> create-vite

│
◇  Project name:
│  vite-react-example
│
◇  Select a framework:
│  React
│
◇  Select a variant:
│  JavaScript
│
◇  Scaffolding project in /Users/...
│
└  Done. Now run:

  cd vite-react-example
  npm install
  npm run dev
```

and that is pretty much it! Works like a charm. If you want to know more detail about this and get a more in-depth tutorial, and if you like the idea of building interactive experiences for the web and want a single-source of education to do this, I can't recommend Bruno's course highly enough. 

I should mention I haven't tried any of the other frameworks or build tools suggested on the react.dev post, so for your needs this may not be the best and it is good to do more sleuthing, but if you need to get up and running quickly with react in a post create-react-app world, I've found this to work well.