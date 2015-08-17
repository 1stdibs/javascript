# Variables

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
        dragonBall = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonBall = 'z';
    ```

- Do not assign variables with any blocks (ie. JSON, functions, etc).  Leave them
  unassigned then assign them later in the scope.
  This makes it easier to see which variables are in scope.  If you have large blocks within the variable declarations,
  those unassigned variables can be easily lost.

    ```javascript
    // bad
    var i;
    var isItInSpringfield;
    var characters = getCharacters();
    var simpsons;
    var isItOnFox = true;
    var length;

    // bad
    var characters = getCharacters();
    var isItOnFox = true;
    var simpsons = {
        bart: 'I am so smart! S-M-R-T!',
        homer: "D'oh!",
        marge: 'Hmm...',
        lisa: 'Quit it, Bart!',
        maggie: '(pacificer)'
    };
    var isItInSpringfield;
    var length;
    var i;

    // good
    var characters = getCharacters();
    var isItOnFox = true;
    var isItInSpringfield;
    var simpsons;
    var length;
    var i;

    isItInSpringfield = function () {
        return true;
    };

    simpsons = {
        bart: 'I am so smart! S-M-R-T!',
        homer: "D'oh!",
        marge: 'Hmm...',
        lisa: 'Quit it, Bart!',
        maggie: '(pacificer)'
    };
    ```

- Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment [hoisting related issues](#hoisting).

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

