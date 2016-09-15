class: center, middle

# Webpack

###Abusing the hot module bundler as a build tool

[live presentation](https://alonisser.github.io/Intro-webpack-pywebIL) <br/>
[twitter](alonisser@twitter.com), [medium](https://medium.com/@alonisser/), 
[Sad FrontEnd IL](http://sadfrontendil.tumblr.com/)
#####you can also read my political blog: [דגל אדום](degeladom@wordpress.com)
---

# The Why?

Why do we actually need a build tool for frontend projects? Isn't this for compiled languages?

* Ok, besides running the tests (you do write tests do you?) we don't need a build tool for that
* The motivation for this talk. 

---

# The Why: JSHell

* Linting and hinting (Looks like the cool kids are using ESLint now)
* Transpiling: ES6, ES7, coffeescript die hards, Typescript for angular 2.0 aficionados, probably another one written while we are speaking
* JSX compiling
* Tree shaking (more on that later)
* Common code style
 
---
# The Why: CSS Hell

* Linting
* pre processing (sass, less)
* post processing: auto prefixing (and lots more)
 
---
# The Why: Production ready code

* Removing console.log
* concatenating, Minifying, uglifying
* cache handling (revvying)
 
---

# The Why: Multiple targets 

* Pure front end apps don't access ENVIRONMENT Variables
* We need to build them with different targets, white labels,  configurations
 
---

# Prior art

* Grunt: The first of his name. file transformations. Lots of core plugins abandoned
* Gulp: streams! Faster for certain use cases, more "node.js"
* Various hipster trends: brocolli, npm scripts, etc.
 
---

# Webpack - the new kid in town

* 
 
---

# Concepts - modules

* everything is a module
* But what does that mean?
 
---

# Concepts - modules

* What kind of modules js has:
* the "new" es6 modules `import`
* AMD
* commonjs require style modules
 
---

# Concepts - loaders

* 
 
---

# Concepts - plugins

* 
 
---

# Example: A react jsx es6 project

* 
 
---

# webpack + babel

* presets are not a webpack feature! It's babel
* configuring a loader: query notation or object notation (More readable to me)

---

# Multiple target files

* 
 
---

# Css

* 
 
---

# Sass

* 
 
---

# A loader pipeline

The css example allows to examine a loader pipeline.
* Resolved from right to left :( 

---

# Postcss

* Lets add another loader into they css handling pipeline
 
---

# Hot module reload

* 
 
---

# Moving to production

```bash
NODE_ENV=production webpack -p --config webpack.production.config.js
```


---

# We only scratched the surface of this subject.
Lots we didn't get into including:


---

# More info and further reading

* [The docs](http://webpack.github.io/docs/) Really terrible state currently. At least for learning to use webpack. Maybe works if you want to write a plugin/loader
* [webpack-howto](https://github.com/petehunt/webpack-howto)
* [beginner-s-guide-to-webpack](https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460#.4fzjj8l9d)
* [webpack yours bags](https://blog.madewithlove.be/post/webpack-your-bags/)


---

class: center, middle

# Open source rocks!
## Thanks for listening!

---
