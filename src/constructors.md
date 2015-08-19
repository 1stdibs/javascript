# Constructors

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

