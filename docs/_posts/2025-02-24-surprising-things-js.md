I've been modernizing my JavaScript knowledge lately after some professional time away. I am running into interesting idiosyncrasies in the conventions of usage and I thought I'd catalog some. Here is the first one.

I've been building a full stack JS project for personal use and I noticed that in the React front end of my project, most of the docs are pointing me towards using the ES Modules syntax and its `import` keyword to bring in new modules. Meanwhile, on the backend of my project, most sources I follow are using CommonJS modules and their `require` syntax.

I found this odd because, to my untrained eye, it seemed like two ways to do the same thing. A lot of Node sample code still pushes the use of CommonJS modules, but from what I understand after a little digging, ECMAScript Modules (ESModules) are intended to be the way forward whether your JavaScript is running on the server-side or the client-side.

I saw some references providing reasons why you might still use CommonJS modules for Node:
- Synchronous module loading in CommonJS could be seen as a benefit. Even though the main thread is blocked until all modules are loaded, on the server all of these modules are in the filesystem and can be loaded very fast. What you get for this benefit is that you have to worry less about issues that async module loading may present. If you load async, the order of your imports can decieve and confuse, making it harder to spot when circular imports or other bugs arise. For this one, it is worth noting that CommonJS modules were never a good fit for the browser due to the synchronous loading because of how long you'd be blocking waiting for them, so a bundler tool was necessary before ESModules to make sure load times were snappy and the browser experience was unaffected.
- Caching of modules is on by default in CommonJS, so you don't risk forgetting to turn it on or configure it and have multiple network calls for the same module.

The intent of ESModules is to be the de facto standard for making JavaScript available outside its module. The few things that CommonJS still provides are easily mitigated, and ESModules bring a lot of advantages, like tree-shaking where they remove dead code from potential imports to save on performance, a more versatile and fairly intuitive syntax for imports and exports, working in the front end and being the default there, static analysis, and compatibility with CommonJS (though it doesn't go in the other direction).


> **Note:** When working with Node.js and ES modules, remember to specify `"type": "module"` in your `package.json` to ensure proper ES module support. This small detail can save you from potential headaches when managing your project dependencies and module imports. For more information, check out [these docs](https://nodejs.org/docs/latest-v13.x/api/esm.html#esm_enabling).

## Sources

- [CommonJS vs ES Modules](https://betterstack.com/community/guides/scaling-nodejs/commonjs-vs-esm/)
- [Node.js Modules](https://nodejs.org/api/modules.html)
- [Node.js ES Modules](https://nodejs.org/api/esm.html)
- [Syncfusion Blog: CommonJS vs ES Modules](https://www.syncfusion.com/blogs/post/js-commonjs-vs-es-modules)
- [MDN Web Docs: Tree Shaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking)
