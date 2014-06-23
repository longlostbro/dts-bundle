# dts-bundle

[![Build Status](https://secure.travis-ci.org/grunt-ts/dts-bundle.svg?branch=master)](http://travis-ci.org/grunt-ts/dts-bundle) [![NPM version](https://badge.fury.io/js/dts-bundle.svg)](http://badge.fury.io/js/dts-bundle) [![Dependency Status](https://david-dm.org/grunt-ts/dts-bundle.svg)](https://david-dm.org/grunt-ts/dts-bundle) [![devDependency Status](https://david-dm.org/grunt-ts/dts-bundle/dev-status.svg)](https://david-dm.org/grunt-ts/dts-bundle#info=devDependencies)

> Export TypeScript .d.ts files as an external module definition

This module is a naïve string-based approach at generating bundles from the .d.ts declaration files generated by a TypeScript compiler.

The main use-case is generating definition for npm/bower modules written in TypeScript (commonjs/amd).

Source-code should following the external-module pattern (using `import/export`'s and `--outDir`).

:warning: Experimental; use with care.

This module was born out of necessity and frustration. It is a little hacky, but at least if seems to work..

- original code was extracted from an ad-hoc Grunt-task so it is fully synchronous without feedback (except maybe some thrown assertion Error's if you feed it garbage input).
- it works by line-by-line string operations so who knows what kind of funky edge-cases it burns (leave an issue if you find some).


## Usage

1) Get it from npm:

````
npm install dts-bundle
````

2) Compile your modules with the TypeScript compiler of your choice with the `--declaration` and `--outDir` flags (and probably `--module commonjs` too).

3) Run `dts-bundle`

Let's assume the project is called `cool-project` and the main module is `build/index.js` with a `build/index.d.ts`:

````js
var dts = require('dts-bundle');

dts.bundle({
    name: 'cool-project',
    main: 'build/index.d.ts'
});
````

This will traverse all references and imports for the .d.ts file of your sub-modules.

Then it replaces the `build/index.d.ts` file with the bundle of all visible imports.

It will also remove the other `.d.ts` files.

4) Extra bonus points if you link the generated definition in your package.json (or bower.json), in the typescript element:

````json
{
    "name": "cool-project",
    "version": "0.1.3"

    "typescript": {
        "definition": "build/index.d.ts"
    }
}
````

Using this makes the definition findable for tooling, for example the [TypeScript Definitions package manager](https://github.com/DefinitelyTyped/tsd) (from v0.6.x) can auto-link these into it `tsd.d.ts` bundle file (wow, so convenient).

## Todo

- add feature to create a DefinitelyTyped header (using `definition-header` package)
- add feature to auto-update package.json/bower.json with the definition link
- find time to implement a real, parser based solution


## Contributions

They are very welcome. Beware this module is a quick hack-job so good luck!


## License

Copyright (c) 2014 [Bart van der Schoor](https://github.com/Bartvds)

Licensed under the MIT license.
