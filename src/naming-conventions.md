# >Naming Conventions

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

