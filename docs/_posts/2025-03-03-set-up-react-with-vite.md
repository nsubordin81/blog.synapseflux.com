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

The React team has announced the deprecation of their create-react-app starter configuration. You can read more about the history and the rationale behind this decision on their [blog post](https://react.dev/blog/2025/02/14/sunsetting-create-react-app).

As a student of Bruno Simon's [ThreeJS Journey](https://threejs-journey.com) course, I discovered a better way to build React apps. In his ['first-react-application'](https://threejs-journey.com/lessons/first-react-application) lesson, he covers setting up React with Vite. Here’s a condensed version of his advice:

1. Install Node.js if you haven't already.
2. Run `npm create vite@latest` and follow the interactive CLI menu instructions.

Here is a sample of what the process looks like:

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

That's it! It works like a charm. Bruno's course goes into much more depth, and I highly recommend it.

I haven't tried any of the other frameworks or build tools suggested on the react.dev post. You might find that one of these alternatives works better for you. However, if you need to get up and running quickly with React in a post create-react-app world, I hope this helps.