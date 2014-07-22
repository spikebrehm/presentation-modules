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

### Syntax (copied from square/es6-module-transpiler)

This syntax is in flux and is closely tracking the module work being
done by TC39.

### Named Exports

There are two types of exports. *Named exports* like the following:

```javascript
// foobar.js
var foo = 'foo', bar = 'bar';

export { foo, bar };
```

This module has two named exports, `foo` and `bar`.

You can also write this form as:

```javascript
// foobar.js
export var foo = 'foo';
export var bar = 'bar';
```

Either way, another module can then import your exports like so:

```js
import { foo, bar } from 'foobar';

console.log(foo);  // 'foo'
```

### Default Exports

You can also export a *default* export. For example, an ES6ified jQuery might
look like this:

```javascript
// jquery.js
var jQuery = function() {};

jQuery.prototype = {
  // ...
};

export default jQuery;
```

Then, an app that uses jQuery could import it with:

```javascript
import $ from 'jquery';
```

The default export of the "jquery" module is now aliased to `$`.

A default export makes the most sense as a module's "main" export, like the
`jQuery` object in jQuery. You can use default and named exports in parallel.


### Transpilation

* https://github.com/square/es6-module-transpiler