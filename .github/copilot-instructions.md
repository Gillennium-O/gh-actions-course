## Repo orientation (big picture)

This repository is a lesson-style collection of GitHub Actions examples. Each numbered folder (for example `01-building-blocks`, `02-workflow-events`, `04-using-actions`) contains a short lesson and, where relevant, a small sample project. The primary runnable sample is a Create React App located at:

- `04-using-actions/react-app/`

Workflows live in `.github/workflows/` and are numbered to match the lesson they demonstrate (e.g. `.github/workflows/04-using-actions.yaml`). CI jobs often use `defaults.run.working-directory` to target the sample subfolder instead of changing directories in steps.

## What matters for code changes

- The repo is not a monolithic application; it’s a set of small examples. Changes should be localized to the lesson folder unless you are improving shared docs or workflows.
- When adding a new sample, follow the existing pattern: create a number-prefixed folder with a `readme.md` and, if needed, add a workflow in `.github/workflows/` that uses `defaults.run.working-directory` to point to the sample.

## Developer workflows (how to run/build/test locally)

React sample (Windows PowerShell):

1. Open a shell in the repo root.
2. Change to the sample folder and install dependencies:

   cd 04-using-actions/react-app
   npm ci

3. Development server:

   npm start

4. Run tests (note: CRA test runner is interactive by default; to force a single CI-style run set the CI env var):

   # PowerShell example
   $env:CI = 'true'; npm test

5. Build for production:

   npm run build

CI notes (from `.github/workflows/04-using-actions.yaml`):

- Workflows use `actions/setup-node@v3` with `node-version: '20.x'` and run `npm ci` then `npm run test` from `04-using-actions/react-app`.
- When editing workflows, prefer `defaults.run.working-directory` like the existing files to keep steps concise.

## Project-specific conventions & patterns

- Lesson folders contain a `readme.md` with human-facing explanation; keep those in sync with code examples.
- Workflows are named with a leading lesson number and a short slug (e.g. `04-using-actions.yaml`). Keep the numbering consistent for new lessons.
- The React example uses Create React App (see `react-app/package.json`) and TypeScript via `tsconfig.json` in the sample.
- Tests are run via `react-scripts test` (script `test` in `react-app/package.json`). Expect watch-mode locally; CI sets `CI=true` implicitly.

## Integration points & external dependencies

- GitHub Actions: `actions/checkout@v4`, `actions/setup-node@v3` are used across workflows.
- NPM packages used by the React example are listed in `04-using-actions/react-app/package.json`. Use `npm ci` in CI to ensure reproducible installs.

## Actionable guidance for an AI coding agent

1. When modifying or adding workflows, mirror the existing YAML structure: numbered filename, `on: workflow_dispatch` (or other triggers as appropriate), and `defaults.run.working-directory` for sample-specific steps.
2. If you update the React sample dependencies, update `package.json` in `04-using-actions/react-app/` only. Run `npm ci`/`npm test` locally with `CI=true` to reproduce CI behavior.
3. Prefer minimal, localized edits: lessons are small and should remain self-contained. If you must change root-level files, explain why in the PR description.
4. Reference these example files in your changes:  
   - `.github/workflows/04-using-actions.yaml` (CI pattern)  
   - `04-using-actions/react-app/package.json` (npm scripts)  
   - `04-using-actions/react-app/README.md` (developer instructions)

## Examples (explicit snippets from this repo)

- Workflow uses Node 20 and runs tests from the sample folder:

  uses: actions/setup-node@v3
  with:
    node-version: '20.x'

- Working-directory pattern in workflow:

  defaults:
    run:
      working-directory: 04-using-actions/react-app

If anything here is unclear or you'd like more details (for example, how to add a new lesson, how PRs are expected to be structured, or any preferred branching/commit message conventions), tell me which section to expand and I will iterate.  
