# Blocks

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
