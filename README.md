# Modules in teh JavaScript

## Why use a module system?


## CommonJS vs RequireJS

### CommonJS

    // shoutify.js
    

	module.exports = function(str) {
	  return str.toUpperCase() + '!!!';
	};	

.

    // do_the_thing.js
    
	var shoutify = require('./shoutify');
	
	module.exports = function() {
	  shoutify('Hello there');
	};

* The module system used by Node.js.
* The most popular module system, thanks to its use in Node.js.
* Synchronous.
  * Simple.
  * Great for server-side JavaScript, where all dependencies are expected
    to be present and readily available.
  * A bit problematic in the client-side, because asynchronous loading of modules
    has to be handled outside of CommonJS.


### AMD

    // shoutify.js
    
	define(function(str) {
	  return str.toUpperCase() + '!!!';
	});
	
.

    // do_the_thing.js
    
	define(['./shoutify'], function(shoutify) {
	  shoutify('Hello there');
	});

* Asynchronous Module Definition.
* The reference implementation is RequireJS.
* Created primary for the browser, but also usable in Node.
* Asynchronous.
  * A bit more complex syntax and semantics.
  * Grate for client-side JavaScript, because asynchronous 
  
## Building and bundling

* For both module systems, a build step is necessary to transform the many module
  files into one, or multiple, build files.
* You need:
  * A task runner, like Grunt or Gulp
  * A module library, like Browserify (CommonJS) or RequireJS (AMD)

## ES6 Modules

* Part of the almost-ratified EcmaScript 6 spec.

### Transpilation

* https://github.com/square/es6-module-transpiler