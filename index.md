# Creating a TypeScript monorepo for React and React Native with Yarn Workspaces (and optionally Webpack)

## Introduction

Creating a Yarn monorepo that works well with React and React Native can be challenging and isn't very well documented. This guide and the accompanying repository ([typescript-yarn-monorepo](https://github.com/ali-hk/typescript-yarn-monorepo)) can help you set up such a monorepo and ultimately improve your productivity.

### Example Monorepo Architecture

To demonstrate a cross-platform React and React Native app with shared components, this guide uses the following architecture and directory structure:

* `Core`: a standalone NPM package using TypeScript. It exposes `IExampleService` with a basic implementation, `BasicExampleService`.
* `Extended`: another NPM package using TypeScript. It consumes `Core` and provides a different implementation of `IExampleService`, called `ExtendedExampleService`.
* `Web`: a React web app that consumes `Core` and `Extended`.
* `Native`: a React Native app that consumes `Core` and `Extended`.

### Guide Organization

This guide is composed of three parts, each building on the previous part and mapping to a corresponding branch in the repository:

1.  TypeScript React and React Native monorepo with Yarn Workspaces: [use-yarn-nohoist](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-yarn-nohoist)
2.  Using Webpack for bundling components: [use-webpack](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-webpack)
3.  Using Paths with TypeScript and Webpack [use-webpack-with-paths](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-webpack-with-paths)

## TypeScript React and React Native monorepo with Yarn Workspaces

### Steps

* Create and change to a directory for your project
* `git init`
* Add an appropriate .gitignore
  * See [.gitignore](https://github.com/ali-hk/typescript-yarn-monorepo/blob/master/.gitignore) for a good starting point
* `mkdir packages/core`

* `yarn init`
* `yarn add -D typescript`
* `tsc --init`
* add `es6` and `dom` libs to core/tsconfig.json

```diff
{
    "compilerOptions": {
        ...
-       // "lib": [],
+       "lib": [
+           "es6",
+           "dom"
+       ],
        ...
    }
}
```

* Set `sourceMap` to `true` in core/tsconfig.json

```diff
{
    "compilerOptions": {
        ...
-       // "sourceMap": true,
+       "sourceMap": true,
        ...
    }
}
```

* Enable stricter TypeScript checking

```diff
{
    "compilerOptions": {
        ...
-       "noUnusedLocals": true,
-       "noUnusedParameters": true,
-       "noImplicitReturns": true,
-       "noFallthroughCasesInSwitch": true,
+       "noUnusedLocals": true,
+       "noUnusedParameters": true,
+       "noImplicitReturns": true,
+       "noFallthroughCasesInSwitch": true,
        ...
    }
}
```

* Set `outDir` in core/tsconfig.json

```diff
{
    "compilerOptions": {
        ...
-       // "outDir": "./",
+       "outDir": "./dist",
        ...
    }
}
```

* Set `rootDir` in core/tsonfig.json

```diff
{
    "compilerOptions": {
        ...
-       // "rootDir": "./",
+       "rootDir": "./src",
        ...
    }
}
```

* Set `moduleResolution` to `node` in core/tsconfig.json

```diff
{
    "compilerOptions": {
        ...
-       // "moduleResolution": "node",
+       "moduleResolution": "node",
        ...
    }
}
```

* Set `skipLibCheck` to `true` in core/tsconfig.json

```diff
{
    "compilerOptions": {
        ...
+       "skipLibCheck": true,
        ...
    }
}
```

* Add the `IExampleService` interface in core/src/IExampleService.ts

``` TypeScript
export interface IExampleService {
    doExampleWork(input: string): Uint8Array;
}
```

* Implement `IExampleService` with `BasicExampleService` in core/src/BasicExampleService.ts

``` TypeScript
import { IExampleService } from "./IExampleService";

export class BasicExampleService implements IExampleService {
    doExampleWork(input: string): Uint8Array {
        let result: Uint8Array = new Uint8Array(input.length);
        for (let i: number = 0; i < input.length; i++) {
            result[i] = input.charCodeAt(i);
        }

        return result;
    }
}
```

* Export `IExampleService` and `BasicExampleService` in core/src/index.ts for easier consumption

``` TypeScript
export * from "./IExampleService";
export * from "./BasicExampleService";
```

* Specify "dist/index.js" (the output from building src/index.ts) as the main entrypoint in core/package.json

``` diff
{
    ...
    "version": "1.0.0",
-   "main": "index.js",
+   "main": "dist/index.js",
    "author": ...
    ...
}
```

* Set `declaration` to `true` in core/tsconfig.json

```diff
{
    "compilerOptions": {
        ...
-       // "declaration": true
+       "declaration": true,
        ...
    }
}
```

* Specify "dist/index" (the location of index.d.ts, the compiled declaration) as the entrypoint for typings in core/package.json

``` diff
{
    ...
    "main": "index.js",
+   "typings": "dist/index",
    "author": ...
    ...
}
```

* add .npmrc to core directory to prevent files in src/ from being included in the npm package
  * This is necessary to avoid compilation issues for consumers of the package

``` diff
src/
```

* At this point you can run `tsc` in the core directory and the compilation should succeed. You're directory structure will like this:

``` text
-root/
    -packages/
        -core/
            -src/
                -IExampleService.ts
                -BasicExampleService.ts
            -dist/
                -BasicExampleService.d.ts
                -BasicExampleService.js
                -BasicExampleService.js.map
                -IExampleService.d.ts
                -IExampleService.js
                -IExampleService.js.map
                -index.d.ts
                -index.js
                -index.js.map
            -node_modules/
            -.npmrc
            -package.json
            -tsconfig.json
```

### Repository Notes

* Each commit message describes the command that was run or the change that was made.
