<!-- .github/copilot-instructions.md: guidance for AI coding agents working in this repo -->

# Quick orientation

- This is a tiny Node.js codebase (CommonJS) with a single module in `src/` and Jest tests in `src/`.
- Key files to inspect first: `package.json`, `src/index.js`, `src/index.test.js`, `.github/workflows/main.yml`, and `README.md`.

# Quick commands (run locally)

- Install dependencies: `npm install`
- Run tests: `npm test` (script uses `jest` from `package.json`)

# Project-specific notes and patterns

- Module style: CommonJS `module.exports = ...` (see `src/index.js`). Tests import using relative paths (e.g. `require('./index.js')`).
- Tests are precise about string outputs. Example (discoverable):
  - `src/index.js` returns ``Hello there ${name}`` while `src/index.test.js` expects `Hello ${name}` — either code or test must be changed to keep tests green.
- Jest is declared in `package.json` under `dependencies` (not `devDependencies`). CI or packaging workflows may rely on `npm ci`/`npm install` behavior; don't assume dev-only install rules.

# CI notes

- The current workflow file `.github/workflows/main.yml` contains placeholder steps that print `echo` rather than running tests. If asked to update CI, change the `test` job to:

  - checkout (already present)
  - run: `npm ci` then `npm test`

- No external services (databases, APIs) are referenced in the repo; changes are local to the code and tests.

# Guidance for edits and PRs

- Keep tests passing locally before opening a PR. Use the exact command `npm test` to validate.
- If modifying exported behavior (e.g., `sayHi`), update the matching test under `src/` or modify behavior to match the test's intent. The test suite is the source of truth here.
- Prefer minimal, focused changes. For tiny repos like this, an acceptable PR often contains: implementation change, updated/added tests, and minor README or workflow updates.

# Examples (use these in commit messages or PR descriptions)

- "Fix sayHi to match test: return `Hello ${name}` and update unit test expectations" — ties code and test together.
- "Replace CI test placeholder with `npm ci && npm test` and run on push" — small infrastructure improvement.

# Files of interest (quick map)

- `package.json` — scripts and dependency config (test script uses `jest`).
- `src/index.js` — primary implementation (CommonJS export).
- `src/index.test.js` — unit test(s) relying on exact string equality.
- `.github/workflows/main.yml` — CI workflow; currently contains placeholders.
- `README.md` — repo-level description (currently minimal).

# When to ask for clarification

- If the intended output format for `sayHi` (or similar functions) is unclear, ask: "Should greeting include the word 'there' ("Hello there X") or not ("Hello X")?"
- If CI expectations (Node version, schedule, additional checks) are missing, ask what environments the project should support.

---
If you'd like, I can: update the CI test job to run the real tests, or fix the `sayHi` vs test mismatch—tell me which you prefer.
