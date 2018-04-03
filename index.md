# Creating a TypeScript monorepo for React and React Native with Yarn Workspaces (and optionally Webpack)

## Introduction

Creating a Yarn monorepo that works well with React and React Native can be challenging and isn't very well documented. This guide and the accompanying repository ([typescript-yarn-monorepo](https://github.com/ali-hk/typescript-yarn-monorepo)) can help you set up such a monorepo and ultimately improve your productivity.

### Example Monorepo Architecture

To demonstrate a cross-platform React and React Native app with shared components, this guide uses the following architecture and directory structure:

- `Core`: a standalone NPM package using TypeScript. It exposes `IExampleService` with a basic implementation, `BasicExampleService`.
- `Extended`: another NPM package using TypeScript. It consumes `Core` and provides a different implementation of `IExampleService`, called `ExtendedExampleService`.
- `Web`: a React web app that consumes `Core` and `Extended`.
- `Native`: a React Native app that consumes `Core` and `Extended`.

### Guide Organization

This guide is composed of three parts, each building on the previous part and mapping to a corresponding branch in the repository:

1. TypeScript React and React Native monorepo with Yarn Workspaces: [use-yarn-nohoist](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-yarn-nohoist)
2. Using Webpack for bundling components: [use-webpack](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-webpack)
3. Using Paths with TypeScript and Webpack [use-webpack-with-paths](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-webpack-with-paths)

## TypeScript React and React Native monorepo with Yarn Workspaces

### Steps

1. Create and change to a directory for your project
2. `git init`
3. Add an appropriate .gitignore
    - See [.gitignore](https://github.com/ali-hk/typescript-yarn-monorepo/blob/master/.gitignore) for a good starting point
4. `mkdir packages/core`
5. `yarn init`
6. `yarn add -D typescript`
7. `tsc --init`
8. add `es6` and `dom` libs to core/tsconfig.json

``` diff
{
    "compilerOptions": {
        ...
+       "lib": [
+           "es6",
+           "dom"
+       ],
        ...
    }
}
```

9. Set `sourceMap` to `true` in core/tsconfig.json

``` diff TypeScript
{
    "compilerOptions": {
        ...
+       "sourceMap": true,
        ...
    }
}
```

10. Enable stricter TypeScript checking

``` diff
{
    "compilerOptions": {
        ...
+       "noUnusedLocals": true,
+       "noUnusedParameters": true,
+       "noImplicitReturns": true,
+       "noFallthroughCasesInSwitch": true,
        ...
    }
}
```

### Repository Notes

- Each commit message describes the command that was run or the change that was made.