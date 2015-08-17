# UMD Conventions

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

[avoid-circular-dependencies]: https://github.com/1stdibs/1stdibs-admin-v2/wiki/CommonJS-module-guidelines#avoid-circular-dependency-problems-pattern
[example-umd-avoid-circular-dependencies]: https://github.com/1stdibs/1stdibs-admin-v2/wiki/CommonJS-module-guidelines#example-umd-pattern-that-avoids-circular-dependencies
[example-umd]: https://github.com/1stdibs/1stdibs-admin-v2/wiki/CommonJS-module-guidelines#example-umd-pattern

