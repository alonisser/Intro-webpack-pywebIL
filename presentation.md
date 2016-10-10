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

* Transpiling: ES6, ES6+, ES7, coffeescript die hards, Typescript for angular 2.0 aficionados, elm for gizra folks, probably another one written while we are speaking

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

* Source maps?

* concatenating, Minifying, uglifying, inlining static assets

* cache handling/breaking (revvying)
 
---

# The Why: Multiple targets 

* Pure front end apps don't access ENVIRONMENT Variables

* We need to build them with different targets, white labels,  configurations
 
---

# Prior art - build tools

* Grunt: The first of his name. tasks, file transformations. Lots of core plugins abandoned. **Configuration files**

* Gulp: streams! Faster for certain use cases, more "node.js". **Code over configuration**

* Various hipster stuff: [broccoli](https://github.com/broccolijs/broccoli), [brunch](http://brunch.io/), [mimosa](http://mimosa.io/), npm scripts, make files and ...

* No, I'm not making up any of this.
 
---

# Webpack - the new kid in town

* Webpack is actually a module bundler, but provide a powerful frontend assets handling.

* Gained a lot of traction with the react stack (also [commonly associated](http://stateofjs.com/2016/flavors/) with es6 transpiling, babel and jsx handling. more on that later)

* A leading contender in "the worst documentation ever" contest
 
* Wait, A module bundler? 

---

# Concepts - modules

What kind of modules js has:

* the "new" es6 modules `export default foo` and `import {bar} from foo;`

* AMD async modules `define(['./foo'],function(foo){foo()}`
 
* commonjs require style modules `module.exports = {foo:function(){console.log('hello')}` and `require ./foo`. (sync)

* script tags (everything is global, just use it)

* [UMD](https://github.com/umdjs/umd) tried so solve this all. you know how [that ends](https://xkcd.com/927/)

---
# Concepts - moduels

.img-container[![](http://imgs.xkcd.com/comics/standards.png)]

---
# Modules

.img-container[![](modules_everywhere.jpg)]
---

# Module bundling - "prior" art

* [Browserify](http://browserify.org) Invented the idea. Supposedly can achieve everything webpack does with enough configuration and plugins

* [rollup](http://rollupjs.org/)

* [systemjs](https://github.com/systemjs/systemjs) not a bundler but a universal module loader
 
---

# Concepts - modules

* In webpack **everything is a module**

* Everything: js, ts, css, img

* But what does that mean?
 
---

# Concepts - modules

.img-container[![](./what-is-webpack.png)]

---
# Concepts - modules
For example (stolen from webpack-your-bags):

```javascript
import stylesheet from 'styles/my-styles.scss';
import logo from 'img/my-logo.svg';
import someTemplate from 'html/some-template.html';

console.log(stylesheet); // "body{font-size:12px}"
console.log(logo); // "data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5[...]"
console.log(someTemplate) // "<html><body><h1>Hello</h1></body></html>"
```

---

# Concepts - loaders

* Part of a transform pipe line for processing "modules" into importable and requireable modules

* Usually associated with a specific filetype

* The final loader is expected to return JavaScript

* [Lots and Lots of loaders](https://github.com/webpack/docs/wiki/list-of-loaders) 

---

# Concepts - loaders

* loaders can have extra configuration with query params. 

```javascript
'babel-loader?presets[]=babel,presets[]=es2015'
```
* Chainable, The final loader is expected to return JavaScript.

* Can be specified in the config file, but also inline in specific files:

```javascript
const homeTemplate = require("jade!./template.jade")
```
* Loaders can be synchronous or asynchronous

* Loaders are npm installable (and use Node.js)

* A module won't be loaded by a loader unless required

* Can be scoped by "test" regex and include/exclude rules

---

# Concepts - loaders
Lots of utility loaders to solve common problems:

* import-loader
* script-loader
* export-loader

Also not complicated to write one.. 
---

# Concepts - loaders

## I think loaders are a good reason to choose webpack as a build tool. looks like a correct abstraction for a transform pipeline

---

# Concepts - plugins

* More powerful then loaders (Hooks into webpack)

* Generally: **work on bundles** or add functionality

* [Lots of plugins](https://github.com/webpack/docs/wiki/list-of-plugins)

* A lot of the **Production** workflow heavy lifting is done by plugins.
---

# Example: A react jsx es6 project

* First we need an entry point
 ```javascript
 {
    entry: {
         app: './public/main.js',
     
   },
 
 }
 
 ```
 
---

# webpack + babel

* Babel: previously 6to5. Hottest transpiler currently.
 
* presets are not a webpack feature! It's babel

* configuring a loader: query notation or object notation (More readable to me)

```javascript

{
    test: /.jsx?$/, //regex getting js or jsx files
    exclude: path.resolve(__dirname, 'node_modules/'), // We don't need those
    loader: 'babel-loader',
    query: { //another way to would be using in loader: babel?presets[]=react,presets[]=es2015 using the query params array syntax
            presets: ['es2015', 'react'], // a babel preset is preset of babel plugins loaded together, not a webpack feature
            // compact:false
            //babel configuration can also be stored seperatly in .babelrc file
     }
     
}
```

* Here we use the babel presets for handling react JSX and es2015 syntax (ES6 :) ) 

* Other presets with more functionality are available

---

# Multiple target files

* I don't like having  react in the same bundle as my code

* I don't need the transpilers to work it, and It can be probably cached much much longer.

```javascript
{
entry: {
    app: './public/main.js',
    vendor: ['react', 'react-dom'] //Now we added another bundle, we are still using the same entry point, but specifying what packages should be seperated  }
 }

//later down
plugins: [
    new webpack.optimize.CommonsChunkPlugin("vendor", "vendor.bundle.js"),
    ]
```

---
# Images

```javascript
img.src = require('./path/to/image.png')

\\and in config

{
        test: /\.(jpe?g|gif|png|svg)$/i,
        loader: 'image-webpack',
        query: {
          progressive: true,
          optimizationLevel: 3,
          interlaced: false
        }
      }
```
* url-loader

* Automatic inline as base64 for smaller images

---
# A loader pipeline

The css example allows to examine a loader pipeline.

* Resolved from right to left :( 

* Lets take a look: 

```javascript
"style!css!sass"
```

* Sass loader would run first, outputting css

* css loader loads css as a module (webpack readable sortof)

* style loader takes the css module and turns into inline style tag

* So the style is now inlined in a style tag

---

# Postcss

```javascript
"style!css!postcss!sass"
```
* Lets add another loader into they css handling pipeline

* postcss pre/post processors would take over and transform it, outputting changed css

* Here we use autoprefixer and cssnano, lots and lots more

* Yes also works with a standalone cli, gulp, grunt, you name it. 
 
---

# Hot module reload

* A big selling point, lets demo it

* Tries to avoid reloading the page, just sends a diff json and replaces only what needs replacing

* Think fixing state sensitive forms, while already filled, without losing info.
 
* Lets demo it
 
---
# Moving to production

```bash
NODE_ENV=production webpack -p --config webpack.prod.config.js
```

* Adding production plugins:

    minify, uglify, cssnano, dedupe, bundle hash for cache breaking, console.log removal
* NOT using hot module reload

* Adding text extraction for css - instead of inline style (no HMR, but parallel css download)

---

# Moving to production

```javascript
{
        test: /.jsx?$/, //regex getting js or jsx files
        exclude: path.resolve(__dirname, 'node_modules/'),
        loader: combineLoaders([{

          loader: 'babel-loader',

          query: {
            presets: ['es2015', 'react'],
            compact:false
          }
        },
          {
            loader: 'strip-loader',// cleans debugging perhaps not needed since can be done by uglify?
            query: {
              strip: ['debug', 'console.log']
            }
          }
        ])
      },
```

---

# Moving to production

```javascript
plugins: [
    new CleanWebpackPlugin(['dist']),
    new CopyWebpackPlugin([{'from': 'public'}]),
    new webpack.ProvidePlugin({ //Workaround to support jquery plugins (if needed)
      $: "jquery",
      jQuery: "jquery",
      _:"lodash",
      lodash:"lodash",
      marked:"marked"
    }),
    new webpack.optimize.CommonsChunkPlugin("vendor", "vendor.bundle.js"),

    new webpack.optimize.UglifyJsPlugin({mangle: false}),
    new webpack.optimize.DedupePlugin(), //removing double modules
    new webpack.EnvironmentPlugin(["NODE_ENV", "API_ADDRESS"]),
    new HtmlWebpackPlugin({

      template: path.resolve(__dirname, 'src/prod/index.html'),  //different html template

    }),
    new ExtractTextPlugin("styles.[hash].css") //Using a css file with a hash instead of inline styles
```
---
# Moving to production - dependencies from cdn

* Surprisingly hard to find how to do :(

* In development npm install dependencies but in production I would want to get react, lodash, jquery, etc from cdn 

* We remove it from the vendor bundle, add it to the html template, but we need a way to tell our code that the global objects exists (for example $). externals to the rescue:
 ```javascript
 externals: {
     // require("jquery") is external and available
     //  on the global var jQuery
     "jquery": "jQuery",
     "$": "jQuery",
     "_": "_",
     "lodash":"_",
     "marked":"marked"
   },
 
 ```
 
---
# Moving to production

```bash
NODE_ENV=production webpack -p --config webpack.prod.config.js
```
* Specifiy different target (dist) in config, runs once,files are actually created.

* COMMON PRACTICE: `base.webpack.config.js` required in both `dev.webpack.config.js` and `prod.webpack.config.js`

* we might need the `webpack merge plugin` to help us merge configurations

---

# We only scratched the surface of webpack.
Lots we didn't get into including:

* splitting chunks (using `require.ensure` to signal to webpack to break module)  with async "on demand" loading 

* Tree shaking: [un required modules removal](http://www.2ality.com/2015/12/webpack-tree-shaking.html)

* Those are major selling points for webpack.. I told you I'm abusing it

* Also lots of solutions for working around various problems, modules that don't conform to any standrad, that expect global objects, etc

---

# Problems with webpack

* As Usual with JS, the always present feeling that **everything is broken**. API has changed for both webpack and prominent plugins, rendering many blogposts obsolete.

* A 2.0beta is already in master branch, and looks like more is going to break.  Some plugins already moved to 2.0 and the current webpack 1 is referred only in the legacy section.

* The docs. (Or whatever you can call that)


---

# What you should take from this talk

* give your frontend build process some love. 

* Pray for frontend build technology would stabilize sometime..

* Use what works for you. I have a project I'm using `awk` and `sed` as part of a frontend build flow :(
 
* If you don't.. you'll get to be featured in [sad front end IL](http://sadfrontendil.tumblr.com/)

---

# Interesting resources and further reading

* [webpackbin](http://www.webpackbin.com/) think jsfiddle for the age of webpack.

* [The docs](http://webpack.github.io/docs/) Really terrible state currently. At least for learning to use webpack. **Maybe** good if you want to write a plugin/loader

* [Pete hunt (Instagram) webpack-howto](https://github.com/petehunt/webpack-howto) self described *start with this as your webpack docs, then look at the official docs for clarification*

* [webpack the confusing parts](https://medium.com/@rajaraodv/webpack-the-confusing-parts-58712f8fcad9#.4rqmb9t4w) a very good write up.

* [A big survivejs tutorial](http://survivejs.com/webpack/developing-with-webpack) very thorough.

* [beginners guide to webpack](https://medium.com/@dabit3/beginner-s-guide-to-webpack-b1f1a3638460#.4fzjj8l9d)

* [webpack yours bags](https://blog.madewithlove.be/post/webpack-your-bags/) Good for understanding the gist

* [Using webpack with an existing codebase](http://blog.threatstack.com/using-webpack-build-system-in-existing-codebases)

---

class: center, middle

# Open source rocks!
## Thanks for listening!

---
