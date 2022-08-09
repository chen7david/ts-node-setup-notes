# ts-node-setup-notes

## Introduction

This document will teach you how to install typescript in a node project. Because node cannot run native typescript code, we will require a typescript execution engine to accelerate development. We'll use <code>ts-node</code> for this because it allows us to run typescript code without having to precompile it. Restarting our server after each change slows development. As a result, we will use <code>nodemon</code> to restart our development server after each file save event. We will install these modules as dev-dependencies because they are only required during development.

## Setup

#### Dev-Dependencies:

- <code>typescript</code> - compiles ts-code to a prespecified target js (ECMAScript) version.
- <code>ts-node</code> - enables you to run typescript code without precompiling it.
- <code>nodemon</code> - watches targeted files changes and restarts server.
- <code>@types/node</code> - add type support for node.

```bash
npm i -D typescript ts-node nodemon @types/node
```

## Getting Started

After we have installed our dev-dependencies, there are a few things we have to do before we can start writing out scripts.

- create a <code>tsconfig.json</code> file. This is going to tell typescript how compile our code.
- configure the <code>tsconfig.json</code>.

#### Creating a <code>tsconfig.json</code> file

There are two ways of creating a <code>tsconfig.json</code> file:

1. Generate a <code>tsconfig.json</code>

```bash
npx tsc --init
```

2. Manually create a <code>tsconfig.json</code>

```bash
touch tsconfig.json
```

Both of these two methods are equally valid.

#### Example <code>tsconfig.json</code> file

```json
// tsconfig.json
{
  "compilerOptions": {
    "outDir": "dist", // where to put the compiled JS files
    "target": "ES2020", // which level of JS support to target
    "module": "CommonJS", // which system for the program AMD, UMD, System, CommonJS

    // Recommended: Compiler complains about expressions implicitly typed as 'any'
    "noImplicitAny": true,
    "types": ["node", "jest"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "baseUrl": ".",
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src", "__test__", "test"], // which files to compile
  "exclude": ["node_modules"], // which files to skip
  "compileOnSave": true
}
```

#### Extending base <code>tsconfig.json</code> file.

Sometimes it becomes nesecary to make changes to our <code>tsconfig.json</code> file before compiling for production. In this case we can <code>extend</code> our base <code>tsconfig.json</code> file then make the required changes. Below is an example of this where we are removing our tests from being compiled.

```json
// tsconfig.build.json
{
  "extends": "./tsconfig",
  "exclude": ["**/*.test.*", "**/__mocks__/*", "**/__tests__/*"]
}
```

#### Adding scripts to <code>package.json</code> file

Now that we have configured our <code>tsconfig.json</code> file, we can start adding scripts to our <code>package.json</code> file. Please add the following scripts.

```json
"main": "dist/app.js",
"scripts": {
  "start": "npm run build && node dist/app.js",
  "dev": "nodemon --watch './**/*.ts' --exec 'ts-node' src/app.ts",
  "rm:dist": "tsc --build --clean",
  "build": "npm run rm:dist && tsc -p tsconfig.build.json",
  "build:watch": "tsc --watch"
},
```
