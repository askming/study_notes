# [Modern JavaScript Explained](https://medium.com/the-node-js-collection/modern-javascript-explained-for-dinosaurs-f695e9747b70)

## JS package management (npm)
- why: to overcome the cumbersomeness how conventional JS program install, load 3rd party packages
- `node_modules` folder is the place where 3rd party packages are installed and located; while `packages.json` file specifies the package dependencies with corresponding versions
  - with only `package.json` file and `npm install` command, all the dependencies will be installed without sharing the `node_modules` folder, which can be very large

## JS module bundler (webpack)
- what: to assist import and export code/module/package across JS files, without resorting to global variables (as is done with the "old-school" JS way)
- on the server side, `node.js` has become the population implementation of CommonJS
- on the browser side (front end) it works differently since browser doesn't have access to the file system and this is where the module bundler comes in
 - A JavaScript **module bundler** is a tool that gets around the problem with a build step (which has access to the file system) to create a final output that is browser compatible (which doesn’t need access to the file system). It finds all `require` statements and replace them with the actual contents of each required file. The final result is a single bundled JS file.
- how it works: after installation, it can be used as follows
    ```bash
    $ ./node_modules/.bin/webpack index.js --mode=development
    ```
    - This statement runs the webpack tool installed in the `node_modules` folder, start with the `index.js` file, find any `require` statements, and replace them with the appropriate code to create a single file (by default is `dist/main.js`)
- To avoid the tediousness of running above command with all the options every time when `index.js` is updated, we can use `webpack.config.js` to specify all the options inside and only run the following command (better but still tedious)
    ```bash
    $ ./node_modules/.bin/webpack
    ```
##  Transpiling code for new language features (babel)
- Transpiling code means converting the code in one language to code in another similar language.
- This is an important part of frontend development — since browsers are slow to add new features, new languages were created with experimental features that transpile to browser compatible languages.
- For transpiling, nowadays most people use [babel](https://babeljs.io/) or [TypeScript](http://www.typescriptlang.org/)
  - We can configure webpack to use `babel-loader` through `webpack.config.js`

## npm scripts (task runner)
- why: to automate different parts of the build process, e.g. run webpack whenever the `index.js` file gets updated
- This is done through specifying some option under `scripts` section in `package.json` file
    - Note that the scripts in `package.json` can run webpack without having to specify the full path `./node_modules/.bin/webpack`, since `node.js` knows the location of each npm module path. 