# Conditional Expressions & Equality

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

