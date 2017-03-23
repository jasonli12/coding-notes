# JavaScript Modules

**JavsScript Modules**: clusters of code with distinct functionality that can be shuffled, removed or added without disrupting the system as a whole.

Benefits of Modules
+ Maintainability: A module is self-contained, so it attempts to minimize its dependency on other parts of the codebase as much as possible. This way, the module will not depend on changes made in other parts and can be improved independently.
+ Namespacing: Modules avoid namespace pollution by creating a private space for our variables.
+ Reusability: Modules can be reused in different parts of our code.

## Incorporating Modules


**Module Pattern**: The module pattern mimics the concept of classes in other languages so that we can store both public and provide methods and variables inside a single object. This allows us to create a public facing API for methods that we want to expose while still enclosing private variables and methods in a closure scope.

+ Anonymous closure: You can use local variables within this function without accidentally overwriting global variables, yet still access the global variables.

```JavaScript
var global = 'Hello, I am a global variable :)';

(function () {
  // We keep these variables private inside this closure scope

  var myGrades = [93, 95, 88, 0, 55, 91];

  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item}, 0);

    return 'Your average grade is ' + total / myGrades.length + '.';
  }

  var failing = function(){
    var failingGrades = myGrades.filter(function(item) {
      return item < 70;});

    return 'You failed ' + failingGrades.length + ' times.';
  }

  console.log(failing());
  console.log(global);
}());

// 'You failed 2 times.'
// 'Hello, I am a global variable :)'
```

+ Global Import: Similar to anonymous closures, except we pass in globals as parameters. In the example below `globalVariable` is the only variable that's global. Compared to anonymous closures, you declare the global variables upfront.

``` JavaScript
(function (globalVariable) {

  // Keep this variables private inside this closure scope
  var privateFunction = function() {
    console.log('Shhhh, this is private!');
  }

  // Expose the below methods via the globalVariable interface while
  // hiding the implementation of the method within the
  // function() block

  globalVariable.each = function(collection, iterator) {
    if (Array.isArray(collection)) {
      for (var i = 0; i < collection.length; i++) {
        iterator(collection[i], i, collection);
      }
    } else {
      for (var key in collection) {
        iterator(collection[key], key, collection);
      }
    }
  };

  globalVariable.filter = function(collection, test) {
    var filtered = [];
    globalVariable.each(collection, function(item) {
      if (test(item)) {
        filtered.push(item);
      }
    });
    return filtered;
  };

  globalVariable.map = function(collection, iterator) {
    var mapped = [];
    globalUtils.each(collection, function(value, key, collection) {
      mapped.push(iterator(value));
    });
    return mapped;
  };

  globalVariable.reduce = function(collection, iterator, accumulator) {
    var startingValueMissing = accumulator === undefined;

    globalVariable.each(collection, function(item) {
      if(startingValueMissing) {
        accumulator = item;
        startingValueMissing = false;
      } else {
        accumulator = iterator(accumulator, item);
      }
    });

    return accumulator;

  };

 }(globalVariable));
 ```

 + Object Interface: using a self-contained object interface, this approach lets you decide which variables/methods to keep private and which to expose by putting them in a return statement (i.e. returning the object).

 ``` JavaScript

 var myGradesCalculate = (function () {

  // Keep this variable private inside this closure scope
  var myGrades = [93, 95, 88, 0, 55, 91];

  // Expose these functions via an interface while hiding
  // the implementation of the module within the function() block

  return {
    average: function() {
      var total = myGrades.reduce(function(accumulator, item) {
        return accumulator + item;
        }, 0);

      return'Your average grade is ' + total / myGrades.length + '.';
    },

    failing: function() {
      var failingGrades = myGrades.filter(function(item) {
          return item < 70;
        });

      return 'You failed ' + failingGrades.length + ' times.';
    }
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.'
myGradesCalculate.average(); // 'Your average grade is 70.33333333333333.'
```

+ Revealing Module Pattern: similar to object interface. The only difference is that methods and variables are all kept private until explicitly exposed.

``` JavaScript

var myGradesCalculate = (function () {

  // Keep this variable private inside this closure scope
  var myGrades = [93, 95, 88, 0, 55, 91];

  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item;
      }, 0);

    return'Your average grade is ' + total / myGrades.length + '.';
  };

  var failing = function() {
    var failingGrades = myGrades.filter(function(item) {
        return item < 70;
      });

    return 'You failed ' + failingGrades.length + ' times.';
  };

  // Explicitly reveal public pointers to the private functions
  // that we want to reveal publicly

  return {
    average: average,
    failing: failing
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.'
myGradesCalculate.average(); // 'Your average grade is 70.33333333333333.'
```

## CommonJS vs AMD

### CommonJS

CommonJS module is a reusable piece of JavaScript which exports specific objects, making them available for other modules to `require` in their programs. Each JavaScript file stores modules in its own unique module context (like wrapping it in a closure). In this scope, we use the `module.exports` object to expose modules and `require` to import them.

``` JavaScript

function myModule() {
  this.hello = function() {
    return 'hello!';
  }

  this.goodbye = function() {
    return 'goodbye!';
  }
}

module.exports = myModule;
```

By setting `module.exports` equal to `myModule`, this lets the CommonJS module system know what we want to expose so that other files can `require` it.

When someone wants to use `myModule`

``` JavaScript
var myModule = require('myModule');

var myModuleInstance = new myModule();
myModuleInstance.hello(); // 'hello!'
myModuleInstance.goodbye(); // 'goodbye!'
```

Benefits of CommonJS over module patterns
+ Avoid global namespace pollution
+ Making our dependencies explicit

Downsides of CommonJS over module patterns
+ Reading a module from the web takes much longer than reading from disk.
+ For as long as the script to load a module is running, it blocks the browser from running anything else until it finishes loading.

### AMD (Asynchronous Module Definition)

``` JavaScript

define(['myModule', 'myOtherModule'], function(myModule, myOtherModule) {
  console.log(myModule.hello());
});

```

`define` takes as its first argument an array of each of the module's dependencies. These dependencies are loaded in the background (in a non-blocking manner), and once loaded `define` calls the callback function it is given.

The callback function then takes, as arguments, the dependencies that were loaded. Finally, the dependencies, themselves, must also be defined using the `define` keyword. `myModule` might look like the following:

``` JavaScript


define([], function() {

  return {
    hello: function() {
      console.log('hello');
    },
    goodbye: function() {
      console.log('goodbye');
    }
  };
});

```

With AMD, your modules can be objects, functions, constructors, strings, JSON, and many other types, while CommonJS only supports objects as modules.

AMD is not compatible with io, filesystem, and other server-oriented features available via CommonJS.

### UMD (Universal Module Definition)

UMD creates a way to use either AMD or CommonJS, while also supporting the global variable definition:

```JavaScript


(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
      // AMD
    define(['myModule', 'myOtherModule'], factory);
  } else if (typeof exports === 'object') {
      // CommonJS
    module.exports = factory(require('myModule'), require('myOtherModule'));
  } else {
    // Browser globals (Note: root is window)
    root.returnExports = factory(root.myModule, root.myOtherModule);
  }
}(this, function (myModule, myOtherModule) {
  // Methods
  function notHelloOrGoodbye(){}; // A private method
  function hello(){}; // A public method because it's returned (see below)
  function goodbye(){}; // A public method because it's returned (see below)

  // Exposed public methods
  return {
      hello: hello,
      goodbye: goodbye
  }
}));

```

### NativeJS (ES6/ES2015)

```JavaScript


// lib/counter.js

var counter = 1;

function increment() {
  counter++;
}

function decrement() {
  counter--;
}

module.exports = {
  counter: counter,
  increment: increment,
  decrement: decrement
};


// src/main.js

var counter = require('../../lib/counter');

counter.increment();
console.log(counter.counter); // 1

```

```JavaScript

counter.counter++;
console.log(counter.counter); // 2

```

```JavaScript

// lib/counter.js
export let counter = 1;

export function increment() {
  counter++;
}

export function decrement() {
  counter--;
}


// src/main.js
import * as counter from '../../counter';

console.log(counter.counter); // 1
counter.increment();
console.log(counter.counter); // 2
```

Source: [JavaScript Modules - A Beginner's Guide](https://medium.freecodecamp.com/javascript-modules-a-beginner-s-guide-783f7d7a5fcc#.63jsjbiuj)
