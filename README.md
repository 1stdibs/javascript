# 1stdibs JavaScript Style Guide

## <a name='TOC'>Table of Contents</a>

1. [Types](#types)
1. [Objects](#objects)
1. [Arrays](#arrays)
1. [Strings](#strings)
1. [Functions](#functions)
1. [Boolean Trap](#boolean-trap)
1. [Properties](#properties)
1. [Variables](#variables)
1. [Hoisting](#hoisting)
1. [Conditional Expressions & Equality](#conditionals)
1. [Blocks](#blocks)
1. [Comments](#comments)
1. [Whitespace](#whitespace)
1. [Commas](#commas)
1. [Semicolons](#semicolons)
1. [Type Casting & Coercion](#type-coercion)
1. [Naming Conventions](#naming-conventions)
1. [Accessors](#accessors)
1. [Constructors](#constructors)
1. [Events](#events)
1. [Modules](#modules)
1. [UMD Conventions](#umd)
1. [jQuery](#jquery)
1. [ES5 Compatibility](#es5)
1. [Testing](#testing)
1. [Performance](#performance)
1. [Resources](#resources)
1. [In the Wild](#in-the-wild)
1. [Translation](#translation)
1. [The JavaScript Style Guide Guide](#guide-guide)
1. [Contributors](#contributors)
1. [License](#license)

## <a name='types'>Types</a>

- **Primitives**: When you access a primitive type you work directly on its value

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

- **Complex**: When you access a complex type you work on a reference to its value

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#TOC)**

## <a name='objects'>Objects</a>

- Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

- Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // bad
    var superman = {
        default: { clark: 'kent' },
        private: true
    };

    // good
    var superman = {
        defaults: { clark: 'kent' },
        hidden: true
    };
    ```

- Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    var superman = {
        class: 'alien'
    };

    // bad
    var superman = {
        klass: 'alien'
    };

    // good
    var superman = {
        type: 'alien'
    };
    ```
**[⬆ back to top](#TOC)**

## <a name='arrays'>Arrays</a>

- Use the literal syntax for array creation

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

- If you don't know array length use Array#push.

    ```javascript
    var someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

- When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
        itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

- To convert an array-like object to an array, use Array#slice.

    ```javascript
    function trigger() {
        var args = Array.prototype.slice.call(arguments);
        ...
    }
    ```

**[⬆ back to top](#TOC)**

## <a name='strings'>Strings</a>

- Use single quotes `''` for strings

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

- Except when there is a single quote within the string, use double quotes`""` for readability
  
    ```javascript
    // bad
    var contraction = 'I hazn\'t had cheezburgr';
    
    // good
    var contraction = "I hazn't had cheezburgr";
    ```

- Strings longer than 120 characters should be written across multiple lines using string concatenation.
- Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // good
    var errorMessage = 'This is a super long error that ' +
        'was thrown because of Batman.' +
        'When you stop to think about ' +
        'how Batman had anything to do ' +
        'with this, you would get nowhere ' +
        'fast.';
    ```

  - When programatically building up a string, use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [
        {
            state: 'success',
            message: 'This one worked.'
        },
        {
            state: 'success',
            message: 'This one worked as well.'
        },
        {
            state: 'error',
            message: 'This one did not work.'
        }
    ];

    length = messages.length;

    // bad
    function inbox(messages) {
        items = '<ul>';

        for (i = 0; i < length; i++) {
            items += '<li>' + messages[i].message + '</li>';
        }

        return items + '</ul>';
    }

    // good
    function inbox(messages) {
        items = [];

        for (i = 0; i < length; i++) {
            items[i] = messages[i].message;
        }

        return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

**[⬆ back to top](#TOC)**


## <a name='functions'>Functions</a>

- Function expressions: <a name="named-function-expression"></a>

    ```javascript
    // anonymous function expression
    var anonymous = function() {
        return true;
    };

    // named function expression
    var named = function named() {
        return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
        console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

- Never declare a function in a non-function block (`if`, `while`, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
- **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
        function test() {
            console.log('Nope.');
        }
    }

    // good
    var test;
    if (currentUser) {
        test = function test() {
            console.log('Yup.');
        };
    }
    ```

- Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
        // ...stuff...
    }

    // good
    function yup(name, options, args) {
        // ...stuff...
    }
    ```

**[⬆ back to top](#TOC)**

## <a name="boolean-trap">Boolean Trap</a>

- Avoid using booleans as arguments

    ```javascript
    // bad
    function setImage(useShard, useHomeDir, useJpgExt) {
        var ext = '.' (useJpgExt ? 'jpg' : 'png');
        var image = 'cheezburgr' + ext;
        if (useHomeDir) {
            image = 'home/' + image;
        }
        if (useShard) {
            image = '//img-shard.domain.com/' + image;
        }
    }

    // somewhere laterz in the codez

    // what does each boolean  mean/do?!?
    var thumbnail = setImage(true, false, true);

    // good
    function setImage(options) {
        options = options || {};
        var ext = '.' + (options.ext || 'png');
        var image = 'cheezburgr' + ext;
        var useHomeDir = options.useHomeDir || false;
        var useShard = options.useShard || false;
        if (useHomeDir) {
            image = 'home/' + image;
        }
        if (useShard) {
            image = '//img-shard.domain.com/' + image;
        }
    }

    // again, somewhere laterz
    var thumbnail = setImage({ useShard: true, useHomeDir: false, ext: 'gif' });

    ```

- It may add some code bloat, but it makes it easier to instantly know what kind of settings are being passed in

**[⬆ back to top](#TOC)**


## <a name='properties'>Properties</a>

- Use dot notation when accessing properties.

    ```javascript
    var luke = {
        jedi: true,
        age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

- Use subscript/bracket notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
        jedi: true,
        age: 28
    };

    function getProp(prop) {
        return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ back to top](#TOC)**


## <a name='variables'>Variables</a>

- Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

- Use one `var` declaration for each variable on a newline.  
    This makes it easier for debugging and the multiple `var` declarations will be removed during the build process.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

- Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i;
    var len; 
    var dragonball;
    var items = getItems(),
    var goSportsTeam = true;

    // bad
    var i; 
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

- Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
        test();
        console.log('doing stuff..');

        //..other stuff..

        var name = getName();

        if (name === 'test') {
            return false;
        }

        return name;
    }

    // good
    function() {
        var name = getName();

        test();
        console.log('doing stuff..');

        //..other stuff..

        if (name === 'test') {
            return false;
        }

        return name;
    }

    // bad
    function() {
        var name = getName();

        if (!arguments.length) {
            return false;
        }

        return true;
    }

    // good
    function() {
        if (!arguments.length) {
            return false;
        }

        var name = getName();

        return true;
    }
    ```

**[⬆ back to top](#TOC)**


## <a name='hoisting'>Hoisting</a>

- Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
        console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
        console.log(declaredButNotAssigned); // => undefined
        var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope.
    // Which means our example could be rewritten as:
    function example() {
        var declaredButNotAssigned;
        console.log(declaredButNotAssigned); // => undefined
        declaredButNotAssigned = true;
    }
    ```

- Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
        console.log(anonymous); // => undefined

        anonymous(); // => TypeError anonymous is not a function

        var anonymous = function() {
            console.log('anonymous function expression');
        };
    }
    ```

- Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
        console.log(named); // => undefined

        named(); // => TypeError named is not a function

        superPower(); // => ReferenceError superPower is not defined

        var named = function superPower() {
            console.log('Flying');
        };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
        console.log(named); // => undefined

        named(); // => TypeError named is not a function

        var named = function named() {
            console.log('named');
        }
    }
    ```

- Function declarations hoist their name and the function body.

    ```javascript
    function example() {
        superPower(); // => Flying

        function superPower() {
            console.log('Flying');
        }
    }
    ```

- For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

**[⬆ back to top](#TOC)**


## <a name='conditionals'>Conditional Expressions & Equality</a>

- Use `===` and `!==` over `==` and `!=`.
- Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
        + This includes `Array`, `String`, `Date`, `RegExp`, `Boolean`, `Number` etc...
    + `undefined` evaluates to **false**
    + `null` evaluates to **false**
    + **booleans** evaluate to **the value of the boolean**
    + **numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
        // true
        // An array is an object, objects evaluate to true
    }
    ```

- Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
        // ...stuff...
    }

    // good
    if (name) {
        // ...stuff...
    }

    // bad
    if (collection.length > 0) {
        // ...stuff...
    }

    // good
    if (collection.length) {
        // ...stuff...
    }
    ```

- For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

**[⬆ back to top](#TOC)**


## <a name='blocks'>Blocks</a>

- Always use curly braces around blocks in loops and conditionals:

    ```javascript
    // bad
    if (test)
        return false;

    // bad
    if (test) return false;

    // good
    if (test) {
        return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
        return false;
    }
    ```

**[⬆ back to top](#TOC)**


## <a name='comments'>Comments</a>

- Use [JSDoc](http://usejsdoc.org/) syntax for comments whenever possible
- Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

        // ...stuff...

        return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

        // ...stuff...

        return element;
    }
    ```


- Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

- Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

- Use `// FIXME:` to annotate problems

    ```javascript
    function Calculator() {

        // FIXME: shouldn't use a global here
        total = 0;

        return this;
    }
    ```

- Use `// TODO:` to annotate solutions to problems

    ```javascript
    function Calculator() {

        // TODO: total should be configurable by an options param
        this.total = 0;

        return this;
    }
    ```

**[⬆ back to top](#TOC)**


## <a name='whitespace'>Whitespace</a>

  - Use soft tabs set to 4 spaces

    ```javascript
    // bad
    function() {
    ∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙∙∙var name;
    }
    ```
- Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
        console.log('test');
    }

    // good
    function test() {
        console.log('test');
    }

    // bad
    dog.set('attr',{
        age: '1 year',
        breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
        age: '1 year',
        breed: 'Bernese Mountain Dog'
    });
    ```
- When creating an [anonymous function expressions](#named-function-expression), place a space between the `function` and the paranthesis:

    ```javascript
    // bad
    var anonymous = function() {
        return true;
    };
    // good
    var anonymous = function () {
        return true;
    }
    ```
- For [named function expressions](#named-function-expression), there is no need to add that leading space
- TBH: This is mainly because PHPStorm is complaining at the moment :bowtie:

    ```javascript
    // bad   
    var named = function named () {
        return true;
    };

    // good
    var named = function named() {
        return true;
    };

    ```

- Use indentation when making long method chains.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // good
    $('#items')
        .find('.selected')
            .highlight()
            .end()
        .find('.open')
            .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
            .data(data)
            .enter()
            .append('svg:svg')
            .class('led', true)
            .attr('width',  (radius + margin) * 2)
            .append('svg:g')
            .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
            .call(tron.led);
    ```

**[⬆ back to top](#TOC)**


## <a name='commas'>Commas</a>

- Leading commas: **Nope.**

    ```javascript
    // bad
    var hero = {
        firstName: 'Bob'
        , lastName: 'Parr'
        , heroName: 'Mr. Incredible'
        , superPower: 'strength'
    };

    // good
    var hero = {
        firstName: 'Bob',
        lastName: 'Parr',
        heroName: 'Mr. Incredible',
        superPower: 'strength'
    };
    ```

- Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):
    - Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    var hero = {
        firstName: 'Kevin',
        lastName: 'Flynn',
    };

    var heroes = [
        'Batman',
        'Superman',
    ];

    // good
    var hero = {
        firstName: 'Kevin',
        lastName: 'Flynn'
    };

    var heroes = [
        'Batman',
        'Superman'
    ];
    ```

**[⬆ back to top](#TOC)**


## <a name='semicolons'>Semicolons</a>

- **Yup.**

    ```javascript
    // bad
    (function() {
        var name = 'Skywalker'
        return name
    })()

    // good
    (function() {
        var name = 'Skywalker';
        return name;
    })();

    // good
    ;(function() {
        var name = 'Skywalker';
        return name;
    })();
    ```

**[⬆ back to top](#TOC)**


## <a name='type-coercion'>Type Casting & Coercion</a>

  - Perform type coercion at the beginning of the statement.
  - Strings ([jsPerf](http://jsperf.com/js-native-string-object-vs-shortcut)):

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use the native `Number` method to convert strings to numbers ([jsPerf](http://jsperf.com/js-native-number-object-vs-shortcut/2)).  
    If `parseInt` is needed, remember to use the `radix` value.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age); // typeof hasAge => 'object'

    // good
    var hasAge = Boolean(age); // typeof hasAge => 'boolean'

    // good
    var hasAge = !!age;
    ```

**[⬆ back to top](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a>

- Avoid single letter names. Be descriptive with your naming.  The build process will shorten your methods, variables, etc.

    ```javascript
    // bad
    function q() {
        // ...stuff...
    }

    // good
    function query() {
        // ..stuff..
    }
    ```

- Use camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var this-is-my-object = {};
    function c() {};
    var u = new user({
        name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
        name: 'Bob Parr'
    });
    ```
- Use capitals for abbreviations in the name

    ```javascript
    // bad
    var isIe = !!window.attachEvent;
    var useUsd = true;

    // good
    var isIE = !!window.attachEvent;
    var useUSD = true;
    ```

- Use PascalCase when naming constructors or classes

    ```javascript
    // bad
    function user(options) {
        this.name = options.name;
    }

    var bad = new user({
        name: 'nope'
    });

    // good
    function User(options) {
        this.name = options.name;
    }

    var good = new User({
        name: 'yup'
    });
    ```

- Use a leading underscore `_` when naming private properties

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - When saving a reference to `this` use `self`.

    ```javascript
    // bad
    function() {
        var that = this;
        return function() {
            console.log(that);
        };
    }

    // good
    function() {
        var self = this;
        return function() {
            console.log(self);
        };
    }
    ```

- Name your [function expressions](#named-function-expression). This is helpful for stack traces.

    ```javascript
    // bad
    var log = function (msg) {
        console.log(msg);
    };

    // good
    var log = function log (msg) {
        console.log(msg);
    };
    ```
- "CONSTANTS" should be in all `UPPERCASE` with underscores (when applicable)

    ```javascript
    // bad
    var startday = 10;
    var start_hour_of_day = 20;

    // good
    var START_DAY = 10;
    var START_HOUR_OF_DAY = 20;
    ```


**[⬆ back to top](#TOC)**


## <a name='accessors'>Accessors</a>

- Accessor functions for properties are not required
- If you do make accessor functions use getVal() and setVal('hello')

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

- If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // bad
    if (!dragon.age()) {
        return false;
    }

    // good
    if (!dragon.hasAge()) {
        return false;
    }
    ```

- It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
        options || (options = {});
        var lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
        this[key] = val;
    };

    Jedi.prototype.get = function(key) {
        return this[key];
    };
    ```

**[⬆ back to top](#TOC)**


## <a name='constructors'>Constructors</a>

- Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
        console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
        fight: function fight() {
            console.log('fighting');
        },
        block: function block() {
            console.log('blocking');
        }
    };

    // good
    Jedi.prototype.fight = function fight () {
        console.log('fighting');
    };

    Jedi.prototype.block = function block () {
        console.log('blocking');
    };
    ```

- Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
        this.jumping = true;
        return true;
    };

    Jedi.prototype.setHeight = function (height) {
        this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // good
    Jedi.prototype.jump = function () {
        this.jumping = true;
        return this;
    };

    Jedi.prototype.setHeight = function (height) {
        this.height = height;
        return this;
    };

    var luke = new Jedi();

    luke.jump()
        .setHeight(20);
    ```

- The `prototype` object can also be aliased, typically to `fn`:

    ```javascript
    function Jedi() {
        console.log('new Jedi');
    }

    Jedi.fn = Jedi.prototype;

    Jedi.fn.fight = function fight () {
        console.log('fighting');
    };

    Jedi.fn.block = function block () {
        console.log('blocking');
    };
    ```

- For private method objects, use `methods` as the variable name:

    ```javascript
    // bad
    var fn = {
        getMessage: function getMessage () {},
        sendMessage: function sendMessage () {}
    };

    // good
    var methods = {
        getMessage: function getMessage () {},
        sendMessage: function sendMessage () {}
    };
    ```


- It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
        options || (options = {});
        this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName () {
        return this.name;
    };

    Jedi.prototype.toString = function toString () {
        return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ back to top](#TOC)**


## <a name='events'>Events</a>

- When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```js
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
        // do something with listingId
    });
    ```

    prefer:

    ```js
    // good
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
        // do something with data.listingId
    });
    ```

  **[⬆ back to top](#TOC)**


## <a name='modules'>Modules</a>

- The file should be named with camelCase, live in a folder with the same name, and match the name of the single export.
- Always declare `'use strict';` at the top of the module.
- [Browserify](http://browserify.org/) wraps up code for CommonJS consumption with `require`
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
- Another convention is to have a variable called `exports` and attach public methods and properties to that object

    ```javascript
    var exports = {
        name: 'Garfield'
    };

    // some private stuff going on here

    exports.likesLasagna = function () {
        return true;
    };

    exports.hatesMondays = function () {
        return true;
    };

    module.exports = exports;
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

**[⬆ back to top](#TOC)**

## <a name='umd'>UMD Conventions</a>

- UMDing files allows for files to work in both CommonJS and standard JavaScript environments. See [umdjs](https://github.com/umdjs/umd) for background.

    ```javascript
    (function (root, factory) {
        'use strict';

        if (typeof module !== 'undefined') {
            // CommonJS
            factory(module, root, require('moduleA'));
        } else {
            // browser globals
            var module = {};
            factory(module, root, root.moduleA);

            root.MyAwesomeModule = module.exports;
        }
    } (window || global, function (module, root, ModuleA) {
        'use strict';

        var MyAwesomeModule = module.exports = function () {
            // regular codez
        }
    }));

    ```

- A slightly more concise version using ```exports``` if you want to export a module with only public methods or properties. Notice that you need to set your public methods and/or properties on ```exports``` for this to work properly.

    ```javascript
    (function (root, factory) {
        'use strict';

        if (typeof module !== 'undefined') {
            // CommonJS
            factory(exports, require('ModuleA'));
        } else {
            // browser globals
            factory((root.MyAwesomeModule = {}), root.ModuleA);
        }
    } (window || global, function (exports, ModuleA) {
        'use strict';

        exports.MyAwesomeMethod = function () {
            // regular codez
        }
    }));
    ```
- [Example UMD Pattern][example-umd]
- [UMD Pattern to Avoid Circular Dependencies][example-umd-avoid-circular-dependencies]

**[⬆ back to top](#TOC)**


## <a name='jquery'>jQuery</a>

- Prefix jQuery object variables with a `$`.  This makes it quicker/easier to tell if an object can be manipulated by jQuery.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

- Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
        $('.sidebar').hide();

        // ...stuff...

        $('.sidebar').css({
            'background-color': 'pink'
        });
    }

    // good
    function setSidebar() {
        var $sidebar = $('.sidebar');
        $sidebar.hide();

        // ...stuff...

        $sidebar.css({
            'background-color': 'pink'
        });
    }
    ```

- For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
- Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#TOC)**


## <a name='es5'>ECMAScript 5 Compatibility</a>

- Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

**[⬆ back to top](#TOC)**


## <a name='testing'>Testing</a>

- **Yup.**

    ```javascript
    function() {
        return true;
    }
    ```
- [Jasmine BDD](http://jasmine.github.io/)
- [Karma Test Runner](http://karma-runner.github.io/0.12/index.html)


**[⬆ back to top](#TOC)**


## <a name='performance'>Performance</a>

- [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
- [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
- [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
- [Bang Function](http://jsperf.com/bang-function)
- [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
- [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
- [Long String Concatenation](http://jsperf.com/ya-string-concat)
- Loading...

**[⬆ back to top](#TOC)**


## <a name='resources'>Resources</a>

**Original JavaScript Style Guide**

- [Airbnb JavaScript Style Guide](airbnb/javascript)

**Read This**

- [Annotated ECMAScript 5.1](http://es5.github.com/)

**Other Styleguides**

- [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
- [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
- [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Other Styles**

- [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
- [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)
- [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript)

**Module and UMD Resources**

- [Avoiding circular dependencies][avoid-circular-dependencies]
- [Example UMD Pattern][example-umd]
- [UMD Pattern to Avoid Circular Dependencies][example-umd-avoid-circular-dependencies]

**Further Reading**

- [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
- [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer

**Books**

- [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
- [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
- [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
- [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
- [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
- [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
- [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
- [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
- [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
- [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
- [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
- [JSBooks](http://jsbooks.revolunet.com/)

**Blogs**

- [DailyJS](http://dailyjs.com/)
- [JavaScript Weekly](http://javascriptweekly.com/)
- [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
- [Bocoup Weblog](http://weblog.bocoup.com/)
- [Adequately Good](http://www.adequatelygood.com/)
- [NCZOnline](http://www.nczonline.net/)
- [Perfection Kills](http://perfectionkills.com/)
- [Ben Alman](http://benalman.com/)
- [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
- [Dustin Diaz](http://dustindiaz.com/)
- [nettuts](http://net.tutsplus.com/?s=javascript)

**[⬆ back to top](#TOC)**

## <a name='in-the-wild'>In the Wild</a>

This is a list of organizations that are using this style guide. Send us a pull request or open an issue and we'll add you to the list.

- **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
- **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
- **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
- **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
- **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
- **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
- **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
- **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
- **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
- **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
- **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
- **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
- **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
- **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
- **Userify**: [userify/javascript](https://github.com/userify/javascript)
- **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
- **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## <a name='translation'>Translation</a>

Airbnb style guide is also available in other languages:

- :de: **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
- :jp: **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
- :br: **Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
- :cn: **Chinese**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
- :es: **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)

## <a name='guide-guide'>The JavaScript Style Guide Guide</a>

- [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='authors'>Contributors</a>

- [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>License</a>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#TOC)**

# };

<!---
re-usable links
-->

[avoid-circular-dependencies]: https://github.com/1stdibs/1stdibs-admin-v2/wiki/CommonJS-module-guidelines#avoid-circular-dependency-problems-pattern
[example-umd]: https://github.com/1stdibs/1stdibs-admin-v2/wiki/CommonJS-module-guidelines#example-umd-pattern
[example-umd-avoid-circular-dependencies]: https://github.com/1stdibs/1stdibs-admin-v2/wiki/CommonJS-module-guidelines#example-umd-pattern-that-avoids-circular-dependencies


