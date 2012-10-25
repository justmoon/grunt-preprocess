# grunt-preprocess

Preprocess HTML and JavaScript with directives based off ENV configuration
## Directive syntax

### Simple syntax

The most basic usage is for files that only have two states, non-processed and processed.
In this case, your directives for exclude are removed after the `preprocess` task

```html
<body>
    <!-- @exclude -->
    <header>Your on dev!</header>
    <!-- @endexclude -->
</body>
```

After build

```html
<body>
</body>
```

### Extended directives

 - @if VAR='value' / @endif
   This will include the enclosed block if your test passes
 - @ifdef VAR / @endif
   This will include the enclosed block if VAR is defined (typeof !== 'undefined')
 - @ifndef VAR / @endif
   This will include the enclosed block if VAR is not defined (typeof === 'undefined')
 - @exclude / @endexclude
   This will remove the enclosed block if your test passes
 - @echo VAR
   This will include the environment variable VAR into your source

### Extended html Syntax

This is useful for more fine grained control of your files over multiple
environment configurations. You have access to simple tests of any ENV variable

```html
<body>
    <!-- @if NODE_ENV!='production' -->
    <header>Your on dev!</header>
    <!-- @endif -->

    <!-- @if NODE_ENV='production' -->
    <script src="some/production/javascript.js"></script>
    <!-- @endif -->

    <script>
    var fingerprint = '<!-- @echo COMMIT_HASH -->' || 'DEFAULT';
    </script>
</body>
```

With a `NODE_ENV` set to `production` and `0xDEADBEEF` in
`COMMIT_HASH` this will be built to look like

```html
<body>
    <script src="some/production/javascript.js"></script>

    <script>
    var fingerprint = '0xDEADBEEF' || 'DEFAULT';
    </script>
</body>
```

With NODE_ENV not set or set to dev and nothing in COMMIT_HASH,
the built file will be

```html
<body>
    <header>Your on dev!</header>

    <script>
    var fingerprint = '' || 'DEFAULT';
    </script>
</body>
```


### JavaScript Syntax

Extended syntax below, but will work without specifying a test

```js
normalFunction();
//@exclude
superExpensiveDebugFunction()
//@endexclude

anotherFunction();
```

Built with a NODE_ENV of production :

```js
normalFunction();

anotherFunction();
```




## Getting Started
Install this grunt plugin next to your project's [Gruntfile][getting_started] with: `npm install grunt-preprocess`

Then add this line to your project's Gruntfile:

```javascript
grunt.loadNpmTasks('grunt-preprocess');
```

## Configuration and Usage

grunt-preprocess is a Grunt Multi Task that takes your
standard source and destination and processes a template based
around environment configuration.

Consider checking out [grunt-env](https://github.com/onehealth/grunt-env) for easing environment configuration.

```js
preprocess : {
  html : {
    src : 'test/test.html',
    dest : 'test/test.processed.html'
  },
  js : {
    src : 'test/test.js',
    dest : 'test/test.js'
  }
}
```


[grunt]: https://github.com/cowboy/grunt
[getting_started]: https://github.com/cowboy/grunt/blob/master/docs/getting_started.md

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [grunt][grunt].

## Release History

 - 1.0.0 Changed syntax, added directives
 - 0.4.0 Added support for inline JS directives
 - 0.3.0 Added insert, extended context to all environment
 - 0.2.0 Added simple directive syntax
 - 0.1.0 Initial release

## License

Copyright OneHealth Solutions, Inc

Written by Jarrod Overson

Licensed under the Apache 2.0 license.
