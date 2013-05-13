connect-modrewrite [![Build Status](https://travis-ci.org/tinganho/connect-modrewrite.png)](https://travis-ci.org/tinganho/connect-modrewrite)
========================
`connect-modrewrite` is a middleware for connect. It adds modrewrite functionality to your project.

# Getting started
Install `connect-modrewrite` with:
```bash
npm install connect-modrewrite --save
```

Require it in your Gruntfile.js file:

```javascript
var modRewrite = require('connect-modrewrite');
```
In grunt.initConfig please add the following code snippet:

```javascript
connect: {
  server: {
    options: {
      port: 9001,
      base: 'example',
      keepalive: true,
      middleware: function(connect, options) {
        return [
          modRewrite([
            '^/test$ /index.html',
            '^/test/\\d*$ /index.html [L]',
            '^/test/\\d*/\\d*$ /flag.html [L]'
          ], [
            /\/test/ //Notice that it is not a string
          ]),
          connect.static(options.base)
        ]
      }
    }
  }
}
```

# Configurations
In the example above, `modRewrite` take as an `Array` of rewrite rules as an argument.
Each rewrite rule is a string with the syntax:
`MATCHING_PATHS REPLACE_WITH [FLAGS]`.
`MATCHING_PATHS` should be defined using a regex string. And that string is passed as an argument to the javascript `RegExp Object` for matching of paths. `REPLACE_WITH` is the replacement string for matching paths. Flags is optional and is defined using hard brackets. We currently only support the last flag `[L]`. Please give suggestions to more flags that makes sense for `connect-modrewrite`. Keep in mind that `grunt-contrib-connect` is meant to be a simple static server.

## NEW IN VERSION 0.3
Connect-modrewrite normalized all URL:s that where assets before. Now in version 0.3 you need pass it as the second argument. The second argument needs to be a JS RegExp object. If a match occur a normalize urls process will begin. Normalizing assets can be convenient e.g. if you are serving JS/CSS files under the URL, http://localhost/blog/2. The browser will request the asset with e.g. `http://localhost/blog/style.css` instead of `http://localhost/style.css`. If you pass in `/\/blog/` as the second argument in `modRewrite` above, it will normalize all assets urls that begins with `/blog`

## Inverted URL matching
Begin with `!` for inverted URL matching.

## Flags
### Last [L]
If a path matches, any subsequent rewrite rules will be disregarded.

## Authors
Tingan Ho, tingan87[at]gmail.com

## License
Copyright (c) 2012 Tingan Ho
Licensed under the MIT license.
