# TypeScript Tutorial

<p align="center">
  <a href="https://www.typescriptlang.org" target="blank"><img style="border-radius: 10px;" src="./doc-images/typescript-logo.png" width="200" alt="Nest Logo" /></a>
</p>

<p align="center"><i><b>TypeScript</b> is a strongly typed programming language that builds on <b>JavaScript</b>, giving you better tooling at any scale.</i></p>

## What is TypeScript?

TypeScript is a syntactic superset of JavaScript which adds **static typing**.

This basically means that TypeScript adds syntax on top of JavaScript, allowing developers to add **types**.

> TypeScript being a "Syntactic Superset" means that it shares the same base syntax as JavaScript, but adds something to it.

## Why should I use TypeScript?

JavaScript is a loosely typed language. It can be difficult to understand what types of data are being passed around in JavaScript.

In JavaScript, function parameters and variables don't have any information! So developers need to look at documentation, or guess based on the implementation.

TypeScript allows specifying the types of data being passed around within the code, and has the ability to report errors when the types don't match.

For example, TypeScript will report an error when passing a string into a function that expects a number. JavaScript will not.

> TypeScript uses compile time type checking. Which means it checks if the specified types match **before** running the code, not **while** running the code.

[🔼 Back to top](#typescript-tutorial)

## How do I use TypeScript?

A common way to use TypeScript is to use the official TypeScript compiler, which transpiles TypeScript code into JavaScript.

The next section shows how to get the compiler setup for a local project.

Some popular code editors, such as Visual Studio Code, have built-in TypeScript support and can show errors as you write code!

## How to Setup a TypeScript + Node.js Project

▶️ [Original Blog](https://khalilstemmler.com/blogs/typescript/node-starter-project)

### Prequisites

- You should have Node and npm installed
- You should be familiar with Node and the npm ecosystem
- You have a code editor installed (preferably VS Code, it's the champ for TypeScript)

### Goals

In this short guide, I'll walk you through the process of creating a basic TypeScript application and compiling it. It's actually _really_ easy!

Afterwards, we'll setup a few scripts for hot-reloading in `development`, building for `production`, and running in `production`.

### About TypeScript

TypeScript, developed and appropriated labeled by Microsoft as "JavaScript that scales", is a superset of JavaScript, meaning that everything JavaScript can do, TypeScript can do (and more better).

TypeScript was primarily meant to solve two problems:

1. Provide JavaScript developers with an _optional type system_.
2. Provide JavaScript developers with the ability to utilize **planned features from future JavaScript editions** against _current JavaScript engines_.

We use TypeScript for most of the topics on this blog because it's a lot better suited for creating long-lasting software and having the compiler help catch bugs and validate types is tremendously helpful.

[🔼 Back to top](#typescript-tutorial)

### Initial Setup

Let's create a folder for us to work in.

```shell
mkdir typescript-starter
cd typescript-starter
```

Next, we'll setup the project `package.json` and add the dependencies.

### Setup Node.js `package.json`

Using the `-y` flag when creating a `package.json` will simply approve all the defaults.

```shell
npm init -y
```

### Add TypeScript as a dev dependency

This probably doesn't come as a surprise ;)

```shell
npm install typescript --save-dev
```

After we install `typescript`, we get access to the command line TypeScript compiler through the `tsc` command. More on that below.

### Install ambient Node.js types for TypeScript

TypeScript has Implicit, Explicit, and Ambient types. Ambient types are types that get added to the global execution scope. Since we're using Node, it would be good if we could get type safety and auto-completion on the Node apis like `file`, `path`, `process`, etc. That's what installing the [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) type definition for Node will do.

```shell
npm install @types/node --save-dev
```

[🔼 Back to top](#typescript-tutorial)

### Create a `tsconfig.json`

The `tsconfig.json` is where we define the TypeScript compiler options. We can create a tsconfig with several options set.

```shell
npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true
```

- `rootDir`: This is where TypeScript looks for our code. We've configured it to look in the `src/` folder. That's where we'll write our TypeScript.
- `outDir`: Where TypeScript puts our compiled code. We want it to go to a `build/` folder.
- `esModuleInterop`: If you were in the JavaScript space over the past couple of years, you might have recognized that modules systems had gotten a little bit out of control (AMD, SystemJS, ES Modules, etc). For a topic that requires a much longer discussion, if we're using `commonjs` as our module system (for Node apps, you should be), then we need this to be set to `true`.
- `resolveJsonModule`: If we use JSON in this project, this option allows TypeScript to use it.
- `lib`: This option adds ambient types to our project, allowing us to rely on features from different Ecmascript versions, testing libraries, and even the browser DOM api. We'd like to utilize some `es6` language features. This all gets compiled down to `es5`.
- `module`: `commonjs` is the standard Node module system in 2019. Let's use that.
- `allowJs`: If you're converting an old JavaScript project to TypeScript, this option will allow you to include `.js` files among `.ts` ones.
- `noImplicitAny`: In TypeScript files, don't allow a type to be unexplicitly specified. Every type needs to either have a specific type or be explicitly declared `any`. No implicit `any`s.

At this point, you should have a `tsconfig.json` that looks like this:

```js
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */,
    "module": "commonjs" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */,
    "lib": [
      "es6"
    ] /* Specify library files to be included in the compilation. */,
    "allowJs": true /* Allow javascript files to be compiled. */,
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "build" /* Redirect output structure to the directory. */,
    "rootDir": "src" /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */,
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    // "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true /* Enable all strict type-checking options. */,
    "noImplicitAny": true /* Raise error on expressions and declarations with an implied 'any' type. */,
    // "strictNullChecks": true,              /* Enable strict null checks. */
    // "strictFunctionTypes": true,           /* Enable strict checking of function types. */
    // "strictBindCallApply": true,           /* Enable strict 'bind', 'call', and 'apply' methods on functions. */
    // "strictPropertyInitialization": true,  /* Enable strict checking of property initialization in classes. */
    // "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    // "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    // "noUnusedLocals": true,                /* Report errors on unused locals. */
    // "noUnusedParameters": true,            /* Report errors on unused parameters. */
    // "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    // "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */
    // "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    // "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    // "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    // "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    // "typeRoots": [],                       /* List of folders to include type definitions from. */
    // "types": [],                           /* Type declaration files to be included in compilation. */
    // "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */,
    // "preserveSymlinks": true,              /* Do not resolve the real path of symlinks. */
    // "allowUmdGlobalAccess": true,          /* Allow accessing UMD globals from modules. */

    /* Source Map Options */
    // "sourceRoot": "",                      /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    // "mapRoot": "",                         /* Specify the location where debugger should locate map files instead of generated locations. */
    // "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    // "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    // "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    // "emitDecoratorMetadata": true,         /* Enables experimental support for emitting type metadata for decorators. */

    /* Advanced Options */
    "resolveJsonModule": true /* Include modules imported with '.json' extension */
  }
}
```

We can go ahead and clean the commented out stuff that we don't need. Our `tsconfig.json` should look like this:

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "lib": ["es6"],
    "allowJs": true,
    "outDir": "build",
    "rootDir": "src",
    "strict": true,
    "noImplicitAny": true,
    "esModuleInterop": true,
    "resolveJsonModule": true
  }
}
```

We're set to run our first TypeScript file.

[🔼 Back to top](#typescript-tutorial)

### 📜 References

- [W3Schools](https://www.w3schools.com/typescript)

- [TypeScript Documentation](https://www.typescriptlang.org/docs)

### 🤝 Contributors

- Mengsreang-Chhoeung [@mengsreang_dev](https://twitter.com/mengsreang_dev)
