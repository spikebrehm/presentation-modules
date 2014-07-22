# Modules in teh JavaScript

## Sprockets != module system

Sprockets asset manifest: 

    //= require ./models/base.js
    //= require ./models/todo.js
    //= require ./collections/todos.js
    //= require ./views/app_view.js

Sprockets simply concatenates these files together into one.

Order matters; if your files depend on other files, then you must make sure
you have specified the dependency before the dependent file in the manifest.
Otherwise, you will get an error, trying to access an object that doesn't exist.

## What is a module?

Think Python or Ruby -- each class or object lives in its own file and the module 
identifier is based on the filename. Modules can express dependencies on other
modules, using relative paths, or absolute paths, in the case of i.e. gems or 
libraries.

### A CommonJS example:

```js
module.exports = function(str) {
  return str.toUpperCase() + '!!!';
};
```


## Why use a module system in JavaScript?

### Explicit dependencies

With Sprockets, we rely on implicit dependencies -- if one file references another file,
it's not explicitly stated, and the only way to tease out dependencies between various
files is by grepping through to see if that object is referred to by name.

### No polluting the global scope

With Sprockets, we throw every object into the global scope, relying on namespacing to prevent
collisions. I.e. `ManageListing.CalendarView`.  Also, if you're not careful, it's easy to 
accidentally leak variables into the global scope.  For example:

```js
var selector = '.foo-bar';

function handleClick(e) {
  // ...
}

$(selector).on('click', handleClick);
```

In this case, `selector` and `handleClick` will leak out to the global scope (i.e. `window.selector`
and `window.handleClick`). The common pattern to prevent this is to wrap this in an IIFE (Immediately 
Invoked Function Expression):

```js
!function() {
  var selector = '.foo-bar';

  function handleClick(e) {
    // ...
  }

  $(selector).on('click', handleClick);
}();
```

Modules are essentially wrapped in closure functions, so you would have to explicitly assign variables
to the global scope.

### Static analysis

As a side effect of explicit dependencies, we can use static dependencies to do all sorts of cool things,
like build dependency graphs for understanding when dead code becomes "orphaned", programatically chunking
up dependencies into optimally sized chunks of code for optimal file size and caching, etc.


## CommonJS vs RequireJS

### CommonJS

```js
// shoutify.js


module.exports = function(str) {
  return str.toUpperCase() + '!!!';
};	
```

```js
// do_the_thing.js

var shoutify = require('./shoutify');

module.exports = function() {
  shoutify('Hello there');
};
```

* The module system used by Node.js.
* The most popular module system, thanks to its use in Node.js.
* Synchronous.
  * Simple.
  * Great for server-side JavaScript, where all dependencies are expected
    to be present and readily available.
  * A bit problematic in the client-side, because asynchronous loading of modules
    has to be handled outside of CommonJS.


### AMD

```js
// shoutify.js

define(function(str) {
  return str.toUpperCase() + '!!!';
});
	
```js
// do_the_thing.js
   
define(['./shoutify'], function(shoutify) {
  shoutify('Hello there');
});
```

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

* Until ES6 modules are supported by all our supported browsers, we could
  compile ES6 modules into AMD or CommonJS modules.
* https://github.com/square/es6-module-transpiler
