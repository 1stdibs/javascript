# Type Casting & Coercion

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

