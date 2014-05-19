# demo-typescript-node-minimal 

[![Build Status](https://travis-ci.org/DefinitelyTyped/demo-typescript-node-minimal.svg?branch=source)](https://travis-ci.org/DefinitelyTyped/demo-typescript-node-minimal)

> Minimal demo showing TypeScript use in node.js

This demo shows the essentials for using node.js modules using TypeScript.

In the minimalistic example app we use the [xregexp](https://www.npmjs.org/package/xregexp) module to execute a simple RegExp on a string an log the output, and a local modules to show relative imports.

This is a very simple example, just to show it actually works.

The typings are from [DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped).


## Prerequisites

Have node.js installed, it comes with the `npm` package manager.

Have the global `tsc` TypeScript compiler:

````
npm install typescript -g
````

Clone the repos to your local workspace

## How to use


### Install npm dependencies

Use your command-line:

A) From `package.json` (recommended for existing projects):

````
npm install
````

B) Manually (when adding new modules)

````
npm install xregexp --save
````

This will create the `node_modules` and add the `xregexp` package. If you look into the `node_modules/xregexp` folder you see a lot of stuff, but we don't have to be aware of any of that:

The magic happens when Node.js loads a module: it will scan the `node_modules` folder and find the `xregexp` folder. In the `package.json` it will see the element `"main": "./xregexp-all.js"`. 

This tells node that the `xregexp` module uses that file as main-entry point. This also means you don't have to look into the folder as module resolution is all automated. 

See http://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders for the actual logic (it is very clever).


### Check the './typings' folder

It contains a folder `xregexp` with a file `xregexp.d.ts`. These names are *not* 'magic'. You have to explicitly reference them using a `/// <reference ..` tag in your code.

This particular typing defines a `xregexp` external module (notice the name matches the npm module's name)


### Check the 'index.ts' file

Note the `/// <reference ..` tag, it is a relative path from this file to the typing `.d.ts`. It will add all it's content to the compilers namespace but not do anything on itself.

Note the `import x = require('xregexp')`. This serves two purposes:

1. At compile time TypeScript will use it to attach type information, it expects to have type declaration for a `xregexp` module (this is why we had the reference mentioned above).
1. At runtime it becomes the actual module import, it will use node's standard `require` system to get the `xregexp` module from the `node_modules` folder.

Note the `import g = require('./lib/greeter');`

This will import our own TypeScript module. When we compile the `index.ts` the compiler will also compiler this sub module to it's own JavaScript file. 


### Compile the 'index.ts' file

We will compile `index.ts` to `index.js` Every file it `imports` automatically gets compiled to it's own module too.

Because we're on node.js and using `import` we will generate CommonJS modules:

````
tsc index.ts --module commonjs
````

### Check the output

Open the `index.js` and `./lib/greeter.js`. Because TypeScript is mostly syntax sugar this looks very similar to the source-code.

Notice how the `import`'s are transformed in a plain `var` assignment. 

The `require`'s still have the same values. One has the npm module name and will be resolved by node, the other has the relative path to our own module.


### Run the code

````
node ./index.js
````

It will run the code, in our case we see the output from the console in the example code (a simple RegExp result and a greeting).


## Next steps

* Add more modules, both from npm as well as your own.
* Define your own typing for a module. Look at the pattern in the example or visit the guide on [DefinitelyTyped](http://definitelytyped.org/guides/creating.html).
* Use [TSD](http://www.tsdpm.com) to pull definitions from DefinitelyTyped and work with the `tsd.d.ts` bundle to manage your references.


## Contributions

Fixes and clarifications are very welcome. Send a pull request or leave a [ticket](https://github.com/DefinitelyTyped/demo-typescript-node-minimal/issues) if you found any problems.

## License

Copyright (c) 2014 DefinitelyTyped

Licensed under the MIT license.
