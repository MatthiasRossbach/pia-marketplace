# scaffold-project

Claude Code plugin that scaffolds new TypeScript + pnpm projects with opinionated defaults.

## Installation

Install via the pia marketplace:

```
/plugin marketplace add MatthiasRossbach/pia-marketplace
/plugin install scaffold-project@pia
```

## Usage

### `/pia:scaffold-project`

Scaffolds a new TypeScript project with:

- `package.json` with TypeScript, ESLint, Prettier
- `tsconfig.json` with strict settings
- `eslint.config.js` (flat config)
- `.prettierrc`
- `src/index.ts` entry point
- `.gitignore`
- `CLAUDE.md` with project conventions
- `.github/workflows/ci.yml` for lint + type-check + build
- Initialized git repository with initial commit

The skill will prompt you for a project name and target directory, then generate all files, run `pnpm install`, and initialize git.

## License

MIT
