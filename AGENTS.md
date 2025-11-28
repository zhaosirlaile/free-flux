# Repository Guidelines

## Project Structure & Module Organization
- `app/` holds Remix routes (UI in `app/routes`, APIs like `api.image.tsx`, shared context/services under `app/context` and `app/services`).
- `public/` serves static assets; `functions/[[path]].ts` is the Cloudflare Pages Functions entry.
- `wrangler.toml` and `worker-configuration.d.ts` keep Worker config; Tailwind setup lives in `tailwind.config.ts` and `postcss.config.js`.
- Path alias `~/` points to `app/` (see `tsconfig.json`).

## Build, Test, and Development Commands
- `pnpm install` to install dependencies (Node 20+).
- `pnpm run dev` starts Remix + Vite dev server; `pnpm run build` emits client + worker bundles to `build/`.
- `pnpm run preview` serves the built worker locally with Wrangler.
- `pnpm run lint` runs ESLint for TS/React; `pnpm run typecheck` runs `tsc` with `noEmit`.
- `pnpm run deploy` builds then deploys to Cloudflare Workers; `pnpm run start` serves the built output via Wrangler Pages dev.

## Coding Style & Naming Conventions
- TypeScript in strict mode; prefer functional React components and hooks.
- 2-space indentation; ESM only (`type: module`); favor named exports unless Remix route defaults are required.
- TailwindCSS for layout/visuals; group utilities logically (layout → color → effects).
- PascalCase for components, camelCase for variables/functions, kebab-case for route files (e.g., `generate-image.tsx`).
- Keep handlers focused: validate inputs early, return `json` helpers, and use `AppError` for expected failures.

## Testing Guidelines
- No automated runner is wired yet; add coverage alongside new features when possible (lightweight request/unit tests with Remix utilities).
- For manual checks, run `pnpm run dev` and hit `/generate-image`, `/api/image`, `/api/models`, `/idioms/game`; include a sample `curl` in PRs for new endpoints and confirm 4xx/5xx responses.

## Configuration & Security Notes
- Secrets live in `wrangler.toml` (`API_KEY`, `CF_ACCOUNT_LIST`, `CUSTOMER_MODEL_MAP`, translation flags). Use JSON strings for lists/maps and keep the Worker `name` in sync with Cloudflare.
- Do not commit real keys; avoid logging credentials—use placeholders when debugging.

## Commit & Pull Request Guidelines
- Use short, imperative commit messages (e.g., `Add image loader`, `Fix model map parsing`); keep related changes together.
- PRs should cover purpose, key changes, how to verify (`pnpm run dev`, `pnpm run preview`, sample API call), and screenshots/GIFs for UI updates.
- Call out config/env changes and update `README.md` or `wrangler.toml` comments accordingly.
