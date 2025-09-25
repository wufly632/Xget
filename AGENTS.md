# Repository Guidelines

## Project Structure & Module Organization
Core worker logic sits in `src/index.js`, which drives routing helpers from `src/config/`. `index.js` exposes the default mirror map, while `platforms.js` maintains per-ecosystem transforms. Tests live under `test/`, with fixtures in `test/fixtures/`, helpers in `test/helpers/`, and integration suites alongside unit files. `scripts/test.js` centralizes local automation, and `wrangler.toml` plus root configs (`eslint.config.js`, `vitest.config.js`, `tsconfig.json`) document runtime expectations.

## Build, Test, and Development Commands
Install dependencies via `npm install`. Use `npm run dev` (or `npm start`) for a local Workers tunnel; add `--remote` when upstream services are required. Deploy with `npm run deploy` once `wrangler` credentials and routes are set. Run suites with `npm run test` or `node scripts/test.js run`; mirror CI with `node scripts/test.js ci`. `npm run lint`, `npm run format:check`, and `npm run type-check` keep code quality in sync.

## Coding Style & Naming Conventions
ESLint enforces two-space indentation, single quotes, required semicolons, and braces on all control blocks (see `eslint.config.js`). Use camelCase for functions and variables, PascalCase for classes, and screaming snake case for shared constants such as `CONFIG`. Prefer explicit named exports within this ESM project. Run `npm run format` to apply the shared Prettier profile before opening a PR.

## Testing Guidelines
Vitest drives all suites; create files as `<target>.test.js` and register new platforms with fixtures and helpers as needed. Common setup lives in `test/setup.js`, so reuse its factories for deterministic timers and caches. Uphold the 80% coverage threshold by running `node scripts/test.js coverage`; regenerate performance baselines with `node scripts/test.js bench` whenever you touch request handling or cache layers.

## Commit & Pull Request Guidelines
Write commits in the imperative mood (`Fix mirror routing`) with a concise subject under 72 characters, then reference related issues using `#123` when relevant. Squash noisy WIP commits before review. Pull requests should explain the change, note affected platforms or configuration, and attach test output (`npm run test`, coverage summaries) plus screenshots when responses differ. Keep scopes tight—new mirrors, dependency bumps, and performance work land best as separate PRs.

## Deployment & Configuration Notes
`wrangler.toml` ships with `workers_dev` disabled, so define a route or zone binding before `npm run deploy`. Manage secrets through `wrangler secret put` rather than `.env` files. When adding providers, update `src/config/platforms.js`, document any new bindings, and verify telemetry through the Cloudflare observability dashboard (`[observability]` is enabled).
