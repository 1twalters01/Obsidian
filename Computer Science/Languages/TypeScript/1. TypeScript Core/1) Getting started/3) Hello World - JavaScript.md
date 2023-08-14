# Creating Files
* TypeScript files always end with the `.ts` extension.
	* `.tsx` files are used for typescript files that contain `jsx`.
* There is no official, universal, convention for naming JavaScript files, though google have a [js style guide](https://google.github.io/styleguide/jsguide.html).
	* They suggest using an underscore (`_`) to separate words and to always use lowercase.

# Example
Put the following into a file named `main.ts` in the `src` directory:
```typescript
const amount: number = 3;

for (let i = 0; i < amount; i++) {
  console.log("Hello world");
}
```

* Then run `pnpm tsc` to compile the code to JavaScript.
* Run node `target/debug/main.ts` to run the file

# Example Analysis
* Parameters go inside parentheses `()`.
* Code bodies are wrapped in `{}`.
	* It is good style to place the opening curly bracket on the same line as the function declaration, adding one space in between.
* JavaScript style is to indent with two spaces, not four.

* console.log lets you print to the console in a web browser or to the cli in a terminal.
	* The **`console`** object provides access to the browser's debugging console (e.g. the Web console in Firefox).
	* It's exposed as `Windows.console`, and can be referenced as `console`.
* You can optionally end lines with a semicolon (`;`)

# Compiling and running
TypeScript is a strongly typed programming language that builds on (and compiles down to) JavaScript. You can compile a program and give someone the `.js` file, and they can either run it on the browser by adding it to an html file, or they will need a runtime like node installed to run it on a terminal.

* TypeScript programs must be compiled to JavaScript. This can either be done before running the file, or upon running it using something like `esbuild`.
	* Note that `esbuild` does not check types
* We will be compiling our typescript code into JavaScript as this leads to faster programs.