# Boolean Trap

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

