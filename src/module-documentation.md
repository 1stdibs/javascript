# Module Documentation

In order to be able to generate JSDoc pages to describe our modules, it's important to correctly document your modules. [Visit usejsdoc.org](http://usejsdoc.org/) to see all available tags and an overview of each.

**Type Specifiers**
Type specifiers are the contents of the braces sometimes next to @param, @property, @return annotations (such as "{string}"). Please make sure it follows the patterns below:

* Simple types : {number}, {string}, {boolean}
* Plain objects (i.e. simple objects declared with '{}' ) : {object}
* Instances of another pseudoclass use the name of the pseudoclass as described by the '@class' annotation : {SoaModel}
* Arrays of one of the above: {SoaModel[]}, {object[]}, {string[]}
* Multiple possible types for a single param or return value should be separated by a pipe "|" : {string|number}
* An unknown return type should use an asterisk '\*' (meaning 'mixed') : {\*}
* A jQuery promise should use {$.Deferred.promise} as the type specifier
* An html element should use 'HTMLElement' as the type specifier. A jQuery element is just an instance of jQuery, so you can use {$}

**Backbone BaseView / FormView / SoaModel / SoaCollection extensions and other Pseudoclass modules**

* Add [@class](http://usejsdoc.org/tags-class.html) and [@extends](http://usejsdoc.org/tags-augments.html) annotations directly above the definition of the module:

```javascript
    /**
     * @class MyModel
     * @extends SoaModel
     */
    var MyModel = SoaModel.extend();
```

* For public properties add [@property](http://usejsdoc.org/tags-property.html) tags above the module definition as well

 ```javascript
     /**
      * @class MyView
      * @extends BaseView
      * @property {string} foo Optional description of foo
      * @property {object} thing Optional description of thing
      * @property {string} thing.bar
      *     Optional description of property 'bar' of property 'thing
      */
     var MyView = BaseView.extend();
 ```

* If you have a parameter to the initialize or constructor function, list it with the module definition with the [@param](http://usejsdoc.org/tags-param.html) tag

 ```javascript
     /**
      * @class MyView
      * @extends BaseView
      * @param {object} options
      * @param {string} options.fieldName
      *     Description of required param (params without square braces are required)
      * @param {boolean} [options.someFlag=false]
      *     Description of 'someFlag' option with a default value of false.
      */
     var MyView = BaseView.extend();
 ```

* If your module uses a mixin, add an [@mixes](http://usejsdoc.org/tags-mixes.html) tag with the name of the mixin. See the section on documenting mixins below for more on that.

```javascript
     /**
      * @class MyView
      * @extends BaseView
      * @mixes fooMixin
      */
     var MyView = BaseView.extend().marinate(fooMixin);
```

* For public / protected methods add @param and [@return](http://usejsdoc.org/tags-returns.html) tags directly above the method. You can add descriptions for the param and return if it is unclear what they should be.

```javascript
    /**
     * @param {string} foo Description of foo (descriptions for tags optional)
     * @param {number} [baz]
     *     Optional parameter example. Parameters with square braces are optional
     * @return {SoaModel} Description of the return value (descriptions are optional)
     */
    myMethod: function (foo) {
        // do stuff
        return bar;
    }
```

* For protected methods, please add the '[@protected](http://usejsdoc.org/tags-protected.html)' annotation:

```javascript
    /**
     * @protected
     * @return {boolean}
     */
    myProtectedMethod: function () {
        // do stuff
        return bar;
    }
```

* For private methods, prefix your method with an '_', and then only document it with '//' comments. That way they aren't added to the public documentation for these classes.

```javascript
   // Description of private method
   _myPrivateMethod: function () {
        // do stuff
   }
```

* For deprecated methods/properties add an '[@deprecated](http://usejsdoc.org/tags-deprecated.html)' tag. Use these for methods you do not want to use anymore.

**Mixin Modules**

* If your module is a mixin, document it with a [@mixin](http://usejsdoc.org/tags-mixin.html) tag, right above the module definition:

```javascript
/**
 * @mixin fooMixin
 */
module.exports = {
    foo: function () {
        // Do stuff
    }
}
```

**Plain object literal modules**

* Use the [@module](http://usejsdoc.org/tags-module.html) tag, as seen below, with a type tag.

```javascript
// In someModule.js
/**
 * @module {object} someModule
 * @property {string} propertyName
 */
module.exports = {
    /**
     * @param {string} myParam
     */
    methodName: function (myParam) {

    }
}
```

* **Note**: plain object literal modules still use the same public/private method documentation (they don't have protected methods) as described in the section on Pseudoclass documentation

