---
name: "pia:scaffold-project"
description: Scaffold a new TypeScript + pnpm project with opinionated defaults
---

# scaffold-project

Scaffold a new TypeScript + pnpm project with opinionated defaults.

## Instructions

You are a project scaffolding assistant. When this skill is invoked, follow these steps exactly:

### Step 1: Gather Input

Ask the user two questions using the AskUserQuestion tool:

1. **Project name** — The name for the new project (used in `package.json` and as the directory name). Must be a valid npm package name (lowercase, no spaces, hyphens allowed).
2. **Target directory** — Where to create the project. Default: `./<project-name>` relative to the current working directory.

### Step 2: Create Project Files

Create the following files inside the target directory:

#### `package.json`

```json
{
  "name": "{{PROJECT_NAME}}",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "scripts": {
    "build": "tsc",
    "lint": "eslint .",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "typecheck": "tsc --noEmit"
  },
  "devDependencies": {
    "typescript": "^5",
    "eslint": "^9",
    "@eslint/js": "^9",
    "typescript-eslint": "^8",
    "prettier": "^3"
  }
}
```

#### `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "Node16",
    "moduleResolution": "Node16",
    "lib": ["ES2022"],
    "outDir": "dist",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

#### `eslint.config.js`

```js
import eslint from '@eslint/js';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  eslint.configs.recommended,
  ...tseslint.configs.recommended,
  {
    ignores: ['dist/'],
  },
);
```

#### `.prettierrc`

```json
{
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100,
  "semi": true
}
```

#### `src/index.ts`

```ts
export {};
```

#### `.gitignore`

```
node_modules/
dist/
.env
.env.*
!.env.example
*.tsbuildinfo
.DS_Store
```

#### `CLAUDE.md`

```markdown
# {{PROJECT_NAME}}

## Tech Stack

- TypeScript (strict mode)
- pnpm
- ESLint (flat config)
- Prettier

## Commands

- `pnpm build` — Compile TypeScript
- `pnpm lint` — Run ESLint
- `pnpm format` — Format with Prettier
- `pnpm format:check` — Check formatting
- `pnpm typecheck` — Type-check without emitting

## Conventions

- Source code lives in `src/`
- Use ES modules (`"type": "module"`)
- Strict TypeScript — no `any`, no implicit returns
- Format with Prettier before committing
```

#### `.github/workflows/ci.yml`

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: pnpm/action-setup@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: pnpm

      - run: pnpm install --frozen-lockfile
      - run: pnpm lint
      - run: pnpm typecheck
      - run: pnpm build
```

### Step 3: Install Dependencies

Run `pnpm install` in the project directory.

### Step 4: Initialize Git

Run the following commands in the project directory:

```bash
git init
git add .
git commit -m "Initial project setup

Scaffolded with pia:scaffold-project:
- TypeScript (strict)
- ESLint (flat config)
- Prettier
- GitHub Actions CI"
```

### Step 5: Summary

Output a summary to the user:

```
Project "{{PROJECT_NAME}}" created at {{TARGET_DIR}}.

Files created:
  - package.json
  - tsconfig.json
  - eslint.config.js
  - .prettierrc
  - src/index.ts
  - .gitignore
  - CLAUDE.md
  - .github/workflows/ci.yml

Next steps:
  cd {{TARGET_DIR}}
  pnpm build
```
