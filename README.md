# @sidisinsane/github-npm-package-template

A github npm package template with typescript.

[![License: MIT](https://img.shields.io/badge/License-MIT-white.svg?style=flat)](https://github.com/sidisinsane/github-npm-package-template/blob/main/LICENSE)

Initial commit **([Quickstart for GitHub Packages](https://docs.github.com/en/packages/quickstart))**

```bash
git init && \
git add . && \
git commit -m "Initial commit" && \
git branch -M main && \
git remote add origin https://github.com/sidisinsane/github-npm-package-template.git && \
git push -u origin main
```

Initial release **([Managing releases in a repository](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository?tool=cli))**

```bash
gh release create v0.0.1 --title "v0.0.1 (beta)" --notes "this is a beta release" --prerelease
```

## Getting started

|                        | pnpm                    | npm                    | yarn                    |
| ---------------------- | ----------------------- | ---------------------- | ----------------------- |
| Install dependencies   | `pnpm install`          | `npm install`          | `yarn`                  |
| Generate documentation | `pnpm run docs`         | `npm run docs`         | `yarn run docs`         |
| Run unit tests         | `pnpm run test`         | `npm run test`         | `yarn run test`         |
| Build                  | `pnpm run build`        | `npm run build`        | `yarn run build`        |
| Lint                   | `pnpm run lint[:fix]`   | `npm run lint[:fix]`   | `yarn run lint[:fix]`   |
| Format                 | `pnpm run format[:fix]` | `npm run format[:fix]` | `yarn run format[:fix]` |

---

## FYI

[![Language: TypeScript](https://img.shields.io/badge/Language-TypeScript-3178c6.svg?style=flat)](https://www.typescriptlang.org/) [![Bundler: tsup](https://img.shields.io/badge/Bundler-tsup-fde047.svg?style=flat)](https://tsup.egoist.dev/) [![Code Formatter: Prettier](https://img.shields.io/badge/Code_Formatter-Prettier-c596c7.svg?style=flat)](https://prettier.io/) [![Linter: ESLint](https://img.shields.io/badge/Linter-ESLint-4b32c3.svg?style=flat)](https://eslint.org/) [![Documentation: TypeDoc](https://img.shields.io/badge/Documentation-TypeDoc-9600ff.svg?style=flat)](https://typedoc.org/) [![Unit Testing Framework: Jest](https://img.shields.io/badge/Unit_Testing_Framework-Jest-c21325.svg?style=flat)](https://jestjs.io/) [![Unit Testing Framework: ts-jest](https://img.shields.io/badge/Unit_Testing_Framework-ts--jest-98435b.svg?style=flat)](https://kulshekhar.github.io/ts-jest/)

### GitHub Actions

`.github/workflows/release-package.yml`:

```yaml
name: Node.js Package

on:
  release:
    types: [created]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
      - run: npm ci
      - run: npm test

  publish-gpr:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
          registry-url: https://npm.pkg.github.com/
      - run: npm ci
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```

### Code Language & Bundling

- [TypeScript](https://www.typescriptlang.org/): TypeScript is JavaScript with syntax for types.
- [tsup](https://tsup.egoist.dev/): Bundle your TypeScript library with no config, powered by esbuild.

**Install**

```bash
pnpm add -D typescript tsup
```

**Configure**

`tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "es2020",
    "module": "commonjs",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true
  }
}
```

`tsup.config.ts`:

```typescript
import { defineConfig } from "tsup";

export default defineConfig({
  entry: ["src/index.ts"],
  target: "es2020",
  format: ["cjs", "esm"],
  splitting: false,
  sourcemap: true,
  clean: true,
  dts: true,
});
```

### Code Formatting & Linting

- [Prettier](https://prettier.io/): Prettier is an opinionated code formatter.
- [ESLint](https://eslint.org/): ESLint statically analyzes your code to quickly find problems.

**Install**

```bash
pnpm add -D @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint eslint-config-prettier prettier
```

**Configure**

`.eslintrc.cjs`:

```javascript
/* eslint-env node */
module.exports = {
  extends: [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier",
  ],
  parser: "@typescript-eslint/parser",
  plugins: ["@typescript-eslint"],
  root: true,
};
```

`.prettierrc` _(see `.editorconfig`)_:

```json
{}
```

### Documentation

- [TypeDoc](https://typedoc.org/): TypeDoc converts comments in TypeScript source code into rendered HTML documentation or a JSON model.

**Install**

```bash
pnpm add -D typedoc @mxssfd/typedoc-theme
```

**Configure**

`typedoc.config.cjs`:

```javascript
/* eslint-env node */
/** @type {import('typedoc').TypeDocOptions} */
module.exports = {
  entryPoints: ["./src/index.ts"],
  out: "docs",
  plugin: ["@mxssfd/typedoc-theme"],
  theme: "my-theme",
  cleanOutputDir: true,
};
```

### Unit Testing

- [Jest](https://jestjs.io/): Jest is a delightful JavaScript Testing Framework with a focus on simplicity.
- [ts-jest](https://kulshekhar.github.io/ts-jest/): A Jest transformer with source map support that lets you use Jest to test projects written in TypeScript.

**Install**

```bash
pnpm add -D jest ts-jest @types/jest && \
npx ts-jest config:init
```

**Configure**

`jest.config.cjs`:

```javascript
/* eslint-env node */
/** @type {import('ts-jest').JestConfigWithTsJest} */
module.exports = {
  preset: "ts-jest",
  testEnvironment: "node",
  collectCoverage: true,
  coverageDirectory: "coverage",
  coveragePathIgnorePatterns: ["/node_modules/"],
  coverageProvider: "v8",
  coverageReporters: ["json", "text", "lcov", "clover"],
};
```
