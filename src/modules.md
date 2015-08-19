# Modules

- The file should be named with camelCase, and if you have a local variable for the module export, it should be the same as the filename
- Always declare `'use strict';` at the top of the module.
- [Browserify](http://browserify.org/) or [Webpack](http://webpack.github.io/) wraps up code for CommonJS consumption with `require`
- No need to create closures for every file anymore
- Only `require` the modules needed for the module

    ```javascript
    var $ = require('jquery');
    var _ = require('underscore');
    var Backbone = require('backbone') // or backbone-base-and-form-view if using `marinate`
    ```
- Export the public methods for other modules to use

    ```javascript
    module.exports = {
        addEntry: function () { }
    };

    // or 
    function Entry(options) {
        this.entryId = options.id || null;
    }; 

    Entry.prototype.addEntry = function addEntry () { };

    module.exports = Entry;

    // file requiring Entry

    var Entry = new (require('Entry')({ id: 10 }));
    ```

- [Avoiding circular dependencies][avoid-circular-dependencies]

- Add a method called noConflict() that sets the exported module to the previous version and returns this one.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
        'use strict';

        var previousFancyInput = global.FancyInput;

        function FancyInput(options) {
            this.options = options || {};
        }

        FancyInput.noConflict = function noConflict() {
            global.FancyInput = previousFancyInput;
            return FancyInput;
        };

        global.FancyInput = FancyInput;
    }(this);
    ```
    

### <a name='module-conventions'>Module Conventions</a>

There are several conventions for organizing module files that we should be adhering to, when possible. This is to make it easier for people who are looking
at the module for the first time to be able to read and understand it, because they don't have to learn every person's style for defining modules.

When you define a module, it's important that we organize the modules consistently, so your module should follow this basic outline:

1. Declare/load dependencies (i.e. require statements)
2. Declare local vars, if any (variables that are only available within the scope of the module)
3. Define module.exports and static/instance properties

When you define a module that might have circular dependencies, you should follow this outline instead

1. Declare/load Superclass, if there is one
2. Set module.exports function/constructor or object, without properties or methods
3. Load remaining dependencies
4. Define module static/instance properties

####Examples

Below are examples of the main module conventions we would like everyone to follow:

**Basic objects**: if your module is an object with a straightforward set of properties, you can follow this convention:

foo.js (note that this module does not refer to itself anywhere, so no local var is necessary)
```javascript

"use strict";

// Dependencies
var dep = require('dep');

// Module Definition
/**
 * @module {object} foo The module name
 */
module.exports = {
    foo: 1,
    bar: 2,
    /**
     * @returns {object}
     */
    functionThatReturnsDep: function () {
        return dep();
    },
    /**
     * @returns {number}
     */
    functionThatReturns4: function () {
        return 2 + 2;
    }
};
```

bar.js (note that this module refers to itself, so a local var 'bar' is used for that)
```javascript

"use strict";

// Dependencies, if any
// Local vars, if any
/**
 * @module {object} bar The module name
 */
var bar = module.exports = {
    myProperty: 'This is a test property',
    getMyProperty: function () {
        return bar.myProperty;
    }
};
```

**Constructor function modules**: if your module is a constructor function, you should try to use the following convention:

MyWidget.js

```javascript

"use strict"

// Dependencies, if any
var foo = require('foo');
var _ = require('underscore');

// Local Vars, if any
var baz;

// Define Module
/**
 * @class MyWidget
 */
var MyWidget = module.exports = function (foo) {
    this.foo = foo; // Objects should be put directly in the constructor/initializer
};

// Instance Properties (do not put objects on the prototype unless it's read-only)
_.extend(MyWidget.prototype, {
    someStr: 'String shared on every instance'
    /**
     * @param {object} thing
     */
    someMethod: function (thing) {
        // do stuff

        // Methods of PseudoClasses/Constructors are allowed to use the
        // 'this' keyword to refer to specific instances
        return this;
    }
};

// Static Properties
_.extend(MyWidget, {
    SOME_CLASS_CONST: 12 // Pseudoclass/Constructor constants should use all caps and be static properties
});
```

**Backbone Models and Collections**: if your module is a Backbone.Model or Backbone.Collection constructor function, you should try to follow the next example:

MyModel.js

```javascript

"use strict";

// Load Super Class
var SoaModel = require('SoaModel');

// Define Module with <superclass>.extend method
/**
 * @class MyModel
 * @extends {SoaModel}
 */
var MyModel = module.exports = SoaModel.extend();

// Load other dependencies
var Backbone = require('dibs-backbone');
var AnotherModel = require('AnotherModel');
var _ = require('underscore');

// Define Module Instance Properties
_.extend(MyModel.prototype, {

    relations : [{
        key: 'another',
        relatedModel: AnotherModel,
        type: Backbone.One
    }],

    /**
     * @param  {string} param
     * @return {boolean}
     */
    bar: function (param) {
        var bool = false;
        // do stuff
        return bool;
    },

    // Note: I think that private properties
    // should go at the bottom of the properties
    // object

    /**
     * @protected
     * @return {object}
     */
    _utilMethod: function () {
        var thing = {};
        // do something
        return thing;
    }

});

// Define Module Static Properties
_.extend(MyModel, {
    SOME_CLASS_CONSTANT: 'foo',
    /**
     * @return {MyModel}
     */
    getInstance: function () {
        // Singleton method definition
    }
});

```

**Backbone Views**: if your module is a Backbone.View constructor function, you should try to follow the next example:

```javascript

"use strict";

// Load Dependencies
var MyParentView = require('MyParentView');
var SomeDep = require('SomeDep');

// Define Module and Instance Properties
/**
 * @class MyView
 * @extends {MyParentView}
 */
var MyView = module.exports = MyParentView.extend({

    /**
     * @param  {object} argument
     */
    foo: function (argument) {
        // body...
    },

    // Again, private/protected functions and event handlers
    // should go at the bottom of the module

    /**
     * @private
     * @param  {Backbone.Model} model
     * @return {SomeDep}
     */
    _bar: function (model) {
        return new SomeDep({ model: model });
    }
});

```

[avoid-circular-dependencies]: https://github.com/1stdibs/1stdibs-admin-v2/wiki/CommonJS-module-guidelines#avoid-circular-dependency-problems-pattern
