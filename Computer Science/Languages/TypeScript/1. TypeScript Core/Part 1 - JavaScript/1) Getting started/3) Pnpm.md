# About Pnpm
* pnpm is an alternative package manager for Node.js which stands for "performant NPM". 
* It holds packages at a global level and lets projects use them too, reducing the memory needed as duplicates aren't created.
* The rest of this guide assumes the use of pnpm
* To work on existing projects all you have to do is run:
	* `git clone example.org/someproject`
	* `cd some project`
	* `pnpm install`

# Creating a project with Pnpm
Just use `pnpm init` in the main directory of your project to create the `node_modules`, `package.json` and `pnpm-lock.yaml` files.

Open the `package.json` file. 
You should see something like the following:
```json
{
  "name": "folder-name",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

We will now add axios, jest and TypeScript.
`pnpm add axios`
`pnpm add --save-dev jest`
`pnpm add -D typescript @types/node`

Additionally, replace the test error with jest:
```json
{
  "name": "conjuu-pnpm",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "jest"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@types/node": "^20.4.5",
    "jest": "^29.6.2",
    "typescript": "^5.1.6"
  },
  "dependencies": {
    "axios": "^1.4.0"
  }
}
```

This file is in the JSON format, which is pnpm's configuration format.

Note that typescript and jest are in devDependencies. This is because they are needed during development but not deployment (you don't test in prod, and don't have types in prod as typescript gets compiled to javascript).

Let's add a .gitignore file in the main directory that contains the following to it: `node-modules/`
Create an `src` directory.

We also need the `tsconfig.json` file. Create both a `tsconfig.dev.json` and a `tsconfig.json` file.

In the `tsconfig.json` file, put:
```json
// tsconfig.json
{
  "extends": "./tsconfig.dev.json",
  "include": ["src"]
}
```

And in the tsconfig.dev.json file, put:
```json
// tsconfig.dev.json
{
  "compilerOptions": {
    "strict": true,
    "strictNullChecks": true,
    "esModuleInterop": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "noUnusedLocals": true,
    "skipLibCheck": true,
    "sourceMap": true,
    "jsx": "react-jsx",
    "moduleResolution": "node",
    "outDir": "target/debug"
  }
}
```

This set up allows us to change our tsconfig easily for different builds.

Run `pnpm tsc` to compile the program.