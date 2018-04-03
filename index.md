# Creating a TypeScript monorepo for React and React Native with Yarn Workspaces (and optionally Webpack)

Creating a Yarn monorepo that works well with React and React Native can be challenging and isn't very well documented. This guide and the accompanying repository ([typescript-yarn-monorepo](https://github.com/ali-hk/typescript-yarn-monorepo)) can help you set up such a monorepo and ultimately improve your productivity.

## Example Monorepo Architecture

To demonstrate a cross-platform React and React Native app with shared components, this guide uses the following architecture and directory structure:

- `Core`: a standalone NPM package using TypeScript. It exposes `IExampleService` with a basic implementation, `BasicExampleService`.
- `Extended`: another NPM package using TypeScript. It consumes `Core` and provides a different implementation of `IExampleService`, called `ExtendedExampleService`.
- `Web`: a React web app that consumes `Core` and `Extended`.
- `Native`: a React Native app that consumes `Core` and `Extended`.

## Guide Organization

This guide is composed of three parts, each building on the previous part and mapping to a corresponding branch in the repository:

1. TypeScript React and React Native monorepo with Yarn Workspaces: [use-yarn-nohoist](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-yarn-nohoist)
2. Using Webpack for bundling components: [use-webpack](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-webpack)
3. Using Paths with TypeScript and Webpack [use-webpack-with-paths](https://github.com/ali-hk/typescript-yarn-monorepo/tree/use-webpack-with-paths)

## Repository Notes

- Each commit message describes the command that was run or the change that was made.