# Prettier
Prettier is a formatter. These reformat your code in one pass, however they can't find or fix bugs.

1) Run the command `pnpm add --save-dev --save-exact prettier`
2) Create a json  file named `.prettierrc.json` where you will add your settings. An example set of settings are as follows
```json
{
  "semi": true,
  "trailingComma": "none",
  "singleQuote": true,
  "printWidth": 80
}
```

3) Add a `.prettierignore` file to let prettier know what files to ignore. Base your `.prettierignore` file on your `.gitignore` and `.eslintignore` files. If your project isn't ready to format html files then add `*.html` to the ignore file.
```prettierignore
node-modules/
target/
*.html
```

4) Add prettier to the script in the package.json:
```json
"scripts": {
    "format": "prettier --config .prettierrc.json src/**/*.ts --write",
    "lint": "eslint . --ext .ts",
    "build": "tsc -p tsconfig.prod.json",
    "dev": "swc src -d target/debug",
    "test": "jest"
},
```

6) run `pnpm format` to run the formatter.


# ESLint
ESLint is a linter. Linters are used to see when we are not following code conventions. They perform automated scans of your JavaScript files for syntax and style errors. They do should not make any changes to your files. You can use ESlint to change your files, but you should not do this.

1) Run the command: `pnpm add --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint typescript`
	* the `@typescript-eslint/parser` is a parser. This is needed as eslint can't work with typescript code by default.
	* The `typescript-eslint` plugin needs to be installed. 
	* We naturally need `eslint` and `typescript` installed, hence why they are there at the end.

2) Create a `.eslintrc.cjs` file in our root directory folder. Add the following to it:
```js
/* eslint-env node */  
module.exports = {  
extends: ['eslint:recommended',
  'plugin:@typescript-eslint/recommended'
],  
parser: '@typescript-eslint/parser',  
plugins: ['@typescript-eslint'],  
root: true,  
};
```

3) We now need to add eslint to our `package.json` scripts:
```json
{
  "name": "conjuu-pnpm",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "lint": "eslint . --ext .ts",
    "build": "tsc -p tsconfig.prod.json",
    "dev": "swc src -d target/debug",
    "test": "jest"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@swc/cli": "^0.1.62",
    "@swc/core": "^1.3.73",
    "@types/node": "^20.4.5",
    "@typescript-eslint/eslint-plugin": "^6.2.1",
    "@typescript-eslint/parser": "^6.2.1",
    "eslint": "^8.46.0",
    "jest": "^29.6.2",
    "typescript": "^5.1.6"
  },
  "dependencies": {
    "axios": "^1.4.0"
  }
}
```

4) Create some sus code inside the `src` folder named `main.ts`:
```ts
const amount: number = 3;
const foo:any = 29;

for (let i = 0; i < amount; i++) {
  console.log("Hello world");
}
```

5) Running `pnpm lint` will give you errors saying that foo is unused and that we are using `any`, which are both bad practices.

We could configure our typescript code to check for this by adding `"noUnusedLocals": true` and `"noUnusedParameters": true` to our `tsconfig.json` file. This would generate the same errors and stop our code from being built.

The issue is that an unused variable shouldn't break our code as we might want to use it for testing, and it is valid JavaScript code. Our Typescript would have to be 100% valid any time we want to test a production build. This makes no sense, so a linter is used to check for it instead. We can run the linter when we want to check that our code is perfect.

Go to https://typescript-eslint.io/rules/ to see all the rules that you can choose.

# SWC
SWC (speedy web compiler) is a typescript transpiler written in Rust. It only transpiles code and doesn't perform type checking, so this should only be used for dev builds. It is however much faster. As such, we will use it for our dev build.

* Run `pnpm i -D @swc/cli @swc/core`
* Change the dev settings in the `package.json` to `pnpm i -D @swc/cli @swc/core`

Now run `pnpm dev`. Notice how much quicker this is than when using `tsc`!

# Jest
Jest is a JavaScript Testing framework that is focused on simplicity. It is mostly used for unit testing. It tests all files with the extension `.test.js`. We downloaded it using `pnpm add --save-dev jest`. We will now set it up.

1) Add `"test": "jest"` to the scripts in the `package.json` file.
	 * We can add the `watchAll` flag to have jest watch our code in the background and rerun the tests any time it changes.
	 * The `coverage` flag shows our code coverage percentage.
	 * We can also add the `verbose` flag to add extra output to the terminal.
	 * It would thus look like this: `"test": "jest --watchAll --verbose --coverage`
2) Create a `test` directory and add an `example.test.js` file.
3) Add the test directory to our .eslintignore
4) To run any tests, run `pnpm test`
	* Run `pnpm test ` to see how well our tests cover our source code

## Puppeteer
Puppeteer is a library that provides a high level API to control Chrome or Chromium over the DevTools Protocol.

Most things that you can do manually in a browser can be done using Puppeteer, for example:
* Generating screenshots and PDFs of pages
* Crawl a SPA and generate pre-rendered content (i.e. "SSR")
* Automate form submission, UI testing, keyboard input, etc.

1) Install jest, puppeteer and jest-puppeteer using `pnpm add --save-dev jest-puppeteer jest puppeteer`
2) Write your preset into a `jest.config.js` file in your root directory:
```js
/** @type {import('jest').Config} */
const config = {
  preset: "jest-puppeteer",
  verbose: true,
};

module.exports = config;
```
3) Create a test that uses puppeteer, for example in test/example.js:
```js
/* globals describe, expect, it, beforeAll, page */ 

describe('Google', () => {
  beforeAll(async () => {
    await page.goto('https://google.com');
  });

  it('should be titled "Google"', async () => {
    await expect(page.title()).resolves.toMatch('Google');
  });
});
```
4) Run `pnpm test` to run the test

This will be useful for end to end tests. How to use Jest and Playwright will be shown in the respective library.

# ESBuild
JavaScript was created to run on web browsers so let's create a html file. Make a public dir and put an `index.html` file in there. Fill it with the following:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>HTML 5 Boilerplate</title>
    <!-- <link rel="stylesheet" href="style.css"> -->
  </head>
  <body>
	<script src="../target/debug/example.js"></script>
  </body>
</html>
```

You will upon opening this file on a web browser, you will see an error stating `SyntaxError: Cannot use import statement outside a module`. This is because the modules are unbundled.

Currently, our files rely on the node_modules directory, which can't be accessed from the browser. Bundling them means that everything is put together in a self contained way. ESBuild is our bundler of choice due to its speed.


1) Run `pnpm add --save-exact esbuild` to install it
2) In `package.json` change
	`"dev": "swc src -d target/debug"`
	to
	`"dev": "swc src -d target/debug && esbuild target/debug/main.js --bundle --outfile=target/debug/dist/main.js"`
3) In `package.json` change
	``"build": "tsc -p tsconfig.prod.json"``
	to
	``"build": "tsc -p tsconfig.prod.json && esbuild target/debug/main.js --bundle --outfile=target/release/dist/main.js""``
5) Change the html file's script location to either `../target/debug/dist/main.js` or `../target/release/dist/main.js`, respectively.

The code obviously doesn't work (you get a CORS policy error or it just won't work as it is a fake link), but you can clearly see that the actual JavaScript module loaded.

# Summary
We now have a:
* formatter - this ensures our typescript will be stylistic
* linter - this quickly ensures that our typescript has good syntax
* a tester - to test the code we write
* a dev builder - to quickly make a dev build of our code
*  a release builder - to ensure our code is perfect, though it takes long to do
* a bundler - to bundle our code.

Serving the http file via a server is something discussed in the Express framework section. That is not of scope in this chapter - this was just about basic tooling we will want.