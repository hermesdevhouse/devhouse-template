# devhouse-template

Dev House project template: a Hono API and a React PWA on one Cloudflare Worker (D20, D11),
TypeScript strict, ESLint and Prettier (D22), Vitest (D10) and an OpenAPI contract (O1,
contract-first).

| Path                                | What                                                                                             |
| ----------------------------------- | ------------------------------------------------------------------------------------------------ |
| `src/api/`                          | Hono app, served under `/api/*` (Worker runs first for `/api/*`)                                 |
| `src/web/`, `index.html`, `public/` | React PWA client (static assets, SPA fallback)                                                   |
| `openapi/openapi.json`              | API contract. `test/api.test.ts` checks routes ↔ spec; CI `spec-diff` checks it against the base |
| `e2e/`                              | Playwright + axe, run by CI against the branch preview only                                      |
| `tools/`                            | `card_guard.py`, `injection_tripwire.py`, `text_hygiene.py` (run locally and in CI)              |

## After creating a repo from this template

1. `git config core.hooksPath .githooks` (local card guard; CI runs the same scan).
2. Replace `devhouse-template` in `wrangler.jsonc` and `package.json`.
3. Connect **Workers Builds** (Cloudflare dashboard → Workers → Import a repository). Build command
   `npm run build`; production branch `main`; enable non-production branch builds.
   Previews use **non-production bindings with fake data only** (D25).
4. Set the repo **variable** `PREVIEW_URL_TEMPLATE` to
   `https://{branch}-<worker-name>.<subdomain>.workers.dev`. Until it is set, `preview-e2e` skips.

## Rules

- Workflows, `CODEOWNERS` and dependency manifests change only with Chanel's approval; every new
  runtime dependency needs her approval (D21). Updates arrive as Dependabot PRs; Chanel merges.
- CI never holds secrets: it runs agent-authored code (D25).
