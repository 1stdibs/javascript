# Whitespace

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

