---
title: 'TypeScript Course Notes'
disqus: hackmd
---

Understanding TypeScript
===

## Table of Contents

[TOC]

___

## Getting started

### What is TypeScript?

![](https://i.imgur.com/biJbBZ5.png)


* superset of JavaScript
* a language built up on JavaScript
* adds new features and advantages to JS
* browser can't execute it!
* it's also a tool, so you can compile to JS what the browser can understand

### Why TypeScript?

![](https://i.imgur.com/XGodjJN.png)

So as we can see the code above, in JS we won't get an error even if we passing strings instead of numbers, however the expected behavoir would be to return the result as 6 type of number, but JS will concatenate the string and returns '23' string instead. 
We could write more code and sanitize the user input, however TS comes handy as it's checking the types and give us error if they are not the correct ones (without the need to write extra code).

### Installing and compiling TypeScript

:::info 
In order for you to run TypeScript and compile it on your local machine, make sure you have Node and npm installed to be able to install it through CLI.

```javascript
// Install TypeScipt globally

npm install -g typescript
```
To be able to run your TS file in the browers, first you need to compile it to plain JS, which you can do with TS itself and the following command. Makes sure to navigate into the folder where you TS file is saved.
```javascript
// Compile TS file to JS

tsc [nameOfFile].ts
```
:::
### Advantages of TypeScript - Overview

![](https://i.imgur.com/Ztda4bQ.png)


## TypeScript Basics & Basic Types

### Using Types

It's only helps us before compiling our code during development. It doens't change our code at run time.

### Typescript Types vs JS Types

In JS we can check the type of a variable at runtime as it's a dynamically typed language. In the other hand TS is statically typed which we can check already in development.

### Type Casing

In TypeScript, you work with types like `string` or `number` all the times.

Important: It is `string` and `number` (etc.), **NOT** `String`, `Number` etc.

**The core primitive types in TypeScript are all lowercase!**

### Working with Numbers, Strings & Booleans

In JS and TS all numbers are float numbers, so there are no difference between 5 | 5.0.

### Type Assignment & Type Inference

Bad practice to assing a type to a variable is we already initialising it as well.

```
let number1 = 5         // good practice
let number1: number = 5 // bad practice
```

However if we only declaring the variable, without giving a value to it, it's good to write out its type, so later we can refer to its correct type, or get a compiling error to warn us incorrect typings.

```
let number1: number
let number1 = 5   // no error
let number1 = '5' // Compile error: Type of 'string' not assignable to type of 'number'
```

### Object Types

![](https://i.imgur.com/ctwC9yY.png)


#### **Objects**

We can explicitly define the types of variables in TS:

```javascript
const car: { type: string, model: string, year: number } = {
  type: "Toyota",
  model: "Corolla",
  year: 2009
};
```
however TypeScript can infer the types of properties based on their values.

```javascript!
const car = {
  type: "Toyota",
};
car.type = "Ford"; // no error
car.type = 2; // Error: Type 'number' is not assignable to type 'string'.
```

#### **Array types**

Similar to objects except the syntax.

```javascript
const names: string[] = ["Capitan", "Lily"];
names.push("Rocco"); // no error
names.push(3); // Error: Argument of type 'number' is not assignable to parameter of type 'string'.
```

If we would need a mixed array with different types, we could use ```any[]```. Then TS would allow any kind of type inside that array. We shouldn't overuse it, as we would loose the whole point of using TS. There are **union** types for that kind of problem.

#### **Typed arrays (Tuple)**

A tuple is a typed array with a pre-defined length and types for each index.
Tuples are great because they allow each element in the array to be a known type of value.
To define a tuple, specify the type of each element in the array:

```javascript!
// define our tuple
let ourTuple: [number, boolean, string];

// initialize correctly
ourTuple = [5, false, 'Coding God was here'];

// initialized incorrectly which throws an error
ourTuple = [false, 'Coding God was mistaken', 5];
```

:::warning
Even though we have a **boolean**, **string**, and **number** the order matters in our tuple and will throw an error.
:::

#### **Enums**

An enum is a special "class" that represents a group of constants *(unchangeable variables)*.

Enums come in two flavors **string** and **numeric**. Lets start with numeric.

 ##### Numeric Enums - Default
By default, enums will initialize the first value to 0 and add 1 to each additional value:

```javascript!
enum CardinalDirections {
  North, // 0
  East,  // 1
  South, // 2
  West   // 3
}
let currentDirection = CardinalDirections.North;
// logs 0
console.log(currentDirection);
// throws error as 'North' is not a valid enum
currentDirection = 'North'; // Error: "North" is not assignable to type 'CardinalDirections'.
```

##### Numeric Enums - Initialized
You can set the value of the first numeric enum and have it auto increment from that:

```javascript
enum CardinalDirections {
  North = 1,
  East,
  South,
  West
}
// logs 1
console.log(CardinalDirections.North);
// logs 4
console.log(CardinalDirections.West);
```

##### Numeric Enums - Fully Initialized
You can assign unique number values for each enum value. Then the values will not incremented automatically:

```javascript!
enum StatusCodes {
  NotFound = 404,
  Success = 200,
  Accepted = 202,
  BadRequest = 400
}
// logs 404
console.log(StatusCodes.NotFound);
// logs 200
console.log(StatusCodes.Success);
```
##### String Enums
Enums can also contain strings. This is more common than numeric enums, because of their readability and intent.

```javascript!
enum CardinalDirections {
  North = 'North',
  East = "East",
  South = "South",
  West = "West"
};
// logs "North"
console.log(CardinalDirections.North);
// logs "West"
console.log(CardinalDirections.West);
```

#### **Any type**

`any` is a type that disables type checking and effectively allows all types to be used.

:::danger
`any` can be a useful way to get past errors since it disables type checking, but TypeScript will not be able provide type safety, and tools which rely on type data, such as auto completion, will not work. Remember, it should be avoided at "any" cost...
:::

```javascript!
let v: any = true;
v = "string"; // no error as it can be "any" type
Math.round(v); // no error as it can be "any" type
```

#### **Union Types**

Union types are used when a value can be more than a single type.
Such as when a property would be string or number. Also you can accept as much as different types as you want.

##### Union ```|``` (OR)
Using the ```|``` we are saying our parameter is a string or number:

```javascript!
function printStatusCode(code: string | number) {
  console.log(`My status code is ${code}.`)
}
printStatusCode(404);
printStatusCode('404');
```

#### **Literal types**

In addition to the general types `string` and `number`, we can refer to specific `strings` and `numbers` in type positions.

One way to think about this is to consider how JavaScript comes with different ways to declare a variable. Both `var` and `let` allow for changing what is held inside the variable, and `const` does not. This is reflected in how TypeScript creates types for literals

By themselves, literal types aren’t very valuable. It’s not much use to have a variable that can only have one value!

But by combining literals into unions, you can express a much more useful concept - for example, functions that only accept a certain set of known values:

```javascript!
function printText(s: string, alignment: "left" | "right" | "center") {
  // do something
}
printText("Hello, world", "left");
printText("G'day, mate", "centre");
// Argument of type '"centre"' is not assignable to
// parameter of type '"left" | "right" | "center"'.
```

#### **Type aliases**

:::info
TypeScript allows types to be defined separately from the variables that use them.
Aliases and Interfaces allows types to be easily shared between different variables/objects.
:::

*Type Aliases allow defining types with a custom name (an Alias).*

Type Aliases can be used for primitives like `string` or more complex types such as `objects` and `arrays`.

We’ve been using object types and union types by writing them directly in type annotations. This is convenient, but it’s common to want to use the same type more than once and refer to it by a single name.

A type alias is exactly that - a name for any type. The syntax for a type alias is:

```javascript!
type Point = {
  x: number;
  y: number;
};
```

You can actually use a type alias to give a name to any type at all, not just an object type. For example, a type alias can name a union type:

```javascript!
type ID = number | string;
```

Helps to simplify code and reuse types without repetition.

```javascript!
type User = {
    name: string;
    age: number;
}
function greetUser(user: User) {
    console.log('Hi ' + user.name);
}
```

#### **Funtion return types and "void"**

```javascript!
function add(n1:number, n2:number) {
    return n1 + n2;
}

function printResult(num: number): void {
    console.log('Result' + num);
}

printResult(add(5, 7)) // 'Result: 12'
console.log(printResult(add(5, 7)) ) // undefined
```

**void represents the return value of functions which don’t return a value**

Whenever you see a function returning void, you are explicitly told there is no return value. All functions with no return value have an inferred return type of void. This should not be confused with a function returning undefined or null .


#### **Functions as Types**

Function types allow us to describe which type of functions specifically we want to use somewhere, be that an expected value in the parameter before create a function with a callback, or a variable. Function is indeed a type in typescript.

#### **Function Types and Callbacks**

```javascript!
function addAndHandle(n1: number, n2: number, cb: (num: number) => void) {
    const result = n1 + n2;
    cb(result);
};

addAndHandle(10, 20, (result) => {
    console.log(result);
});

// Output: 30
```

#### **The "unknown" type**

We can give any type to it, without have an error.
"unknown" is the type-safe counterpart of any

Anything is assignable to "unknown", but "unknown" is not assignable to anything but itself and any without a type assertion or a control flow based narrowing.

We can force the compiler to trust that an unknown varible has a specific type: const bar: string = foo as string. However, typecast might backfire us if we are not mindful using it.

```javascript!
let userInput: unknown;

userInput = 5;
userInput = 'Ted';
```

#### **The "never" type**

The `never` type is used when you are sure that something is never going to occur. For example, you write a function which will not return to its end point or always throws an exception.

```javascript!
function throwError(errorMsg: string): never { 
    throw new Error(errorMsg); 
} 

function keepProcessing(): never { 
    while (true) { 
         console.log('I always does something and never ends.')
     }
}
```

In the above example, the `throwError()` function throws an error and `keepProcessing()` function is always executing and never reaches an end point because the while loop never ends. Thus, `never` type is used to indicate the value that will never occur or return from a function.

## The TypeScript Compiler (and its Configuration)

### Using 'Watch Mode'

It's useful so we don't have to manually recompile everytime we change something in our code. Watch mode will take care it for us, and recompile our file on the fly, so we can see the live changes in the browser.

To initiate 'watch mode', instead of `tsc app.ts` in the terminal, we can write:

```bash=
tsc app.ts --watch
# OR
tsc app.ts -w
```

You can quit watch mode with `Command+C` (or `Ctrl+C` on Windows).

### Compiling the entire project/multiple files

We can tell TS to compile our whole project and recompile it every time we change something anywhere inside any files.

*You only have to make sure, you **run** this command **in the root directory** of your project, so TS will know where to look for them.*

```bash=
tsc --init
```

This will create your `tsconfing.json` file, where you can specify a bunch of different things about how TS should handle your project. However, without changing anything on your `tsconfig`, you will be able to compile all your `.ts` extension files in your project with 

```bash=
tsc
```

This you can use it together with the above mentioned 'watch mode', so you can see any changes in any files automatically. So you don't need to recompile every time.

```bash=
tsc --watch
```

### Including and excluding files

In the `tsconfig.json` file we can specify how we would like to set up the compiler, and also can add or remove files from compilation. 

#### Let's see an example below

What I really like about this approach is that it includes all of the possible values and their reasoning. I know that there is the [docs](https://www.typescriptlang.org/docs/) with all of this information, but how convinient is just read it in the codebase without jumping through apps. 

```javascript=
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */,
    "module": "commonjs" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
    // "lib": [],                             /* Specify library files to be included in the compilation. */
    // "allowJs": true,                       /* Allow javascript files to be compiled. */
    // "checkJs": true,                       /* Report errors in .js files. */
    // "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    // "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    // "declarationMap": true,                /* Generates a sourcemap for each corresponding '.d.ts' file. */
    // "sourceMap": true,                     /* Generates corresponding '.map' file. */
    // "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "./build" /* Redirect output structure to the directory. */,
    // "rootDir": "./",                       /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    // "composite": true,                     /* Enable project compilation */
    // "tsBuildInfoFile": "./",               /* Specify file to store incremental compilation information */
    // "removeComments": true,                /* Do not emit comments to output. */
    // "noEmit": true,                        /* Do not emit outputs. */
    // "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    // "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    // "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */
    /* Strict Type-Checking Options */
    "strict": true /* Enable all strict type-checking options. */,
    // "noImplicitAny": true,                 /* Raise error on expressions and declarations with an implied 'any' type. */
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
    "skipLibCheck": true /* Skip type checking of declaration files. */,
    "forceConsistentCasingInFileNames": true /* Disallow inconsistently-cased references to the same file. */,
    "resolveJsonModule": true
  },
      
  /*  <<< Here we can either include, or exclude files as an array >>>*/
      
    "include": ["./src/index.ts"],
    "exclude": [
        "node_modules" // node_modules is ignored by default
    ],
}
```

We can also use these paths as a wildcard with the asteriqs (*)

```javascript=
"exclude": [
    "*.test.ts" // all files with '.test' pattern will be ignored
    "**/*.dev.ts" // any files with '.dev' pattern in any folder will be ignored
],
```

### Setting a compilation target

Modern browsers support all ES6 features, so ES6 is a good choice. You might choose to set a lower target if your code is deployed to older environments, or a higher target if your code is guaranteed to run in newer environments.

```javascript=
{
    "target" : "es6",
}
```

The target setting changes which JS features are downleveled and which are left intact. For example, an arrow function () => this will be turned into an equivalent function expression if target is ES5 or lower.


### Understanding TypeScript Core Libs

```javascript=
{
    // "lib": [], /* Specify library files to be included in the compilation. */
}
```

TypeScript includes a default set of type definitions for built-in JS APIs (like `Math`), as well as type definitions for things found in browser environments (like `document`). TypeScript also includes APIs for newer JS features matching the target you specify; for example the definition for `Map` is available if target is ES6 or newer.

:::info
**Default** settings available when it's commented out. Once we specify the empty array of `"lib": []`, we tell TS to only include whatever we specify.

```javascript=
{
    // Exact default settings while commented out, all core JavaScript feature
    "lib": [
        "dom",
        "es6",
        "dom.iterable",
        "scripthost"
    ],
}
```
:::

*You may want to change these for a few reasons:*

* Your program doesn’t run in a browser, so you don’t want the DOM type definitions for example a NodeJS app
* Your runtime platform provides certain JavaScript API objects (maybe through polyfills), but doesn’t yet support the full syntax of a given ECMAScript version
* You have polyfills or native implementations for some, but not all, of a higher level ECMAScript version

### Working with Source Maps

Source maps help us debugging and development. When we enable it, it will generate a `.js.map` file which then act as a bridge between our JS compiled file and the TS file, which then the browser will understand.
Then in the Source Tab on our DevTools, we have both `.js`, `.ts` file and we can debug straight our `.ts` file, which is super convenient.

```javascript=
{
    "sourceMap" : true,  /* Generates corresponding '.map' file. */
}
```

### `rootDir` and `outDir`














































