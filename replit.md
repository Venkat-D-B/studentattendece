# StockSense

StockSense is an inventory operations workspace for tracking stock, locations, receipts, deliveries, transfers, adjustments, and movement history.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/stocksense/src/App.tsx` — responsive app shell, routes, inventory workflows, and Clerk entry screens
- `artifacts/stocksense/src/index.css` — StockSense visual tokens and Clerk theme layer
- `artifacts/api-server/src/routes/inventory.ts` — inventory API routes and demo inventory state
- `lib/api-spec/openapi.yaml` — source-of-truth API contract

## Architecture decisions

- The first build uses an in-memory API dataset so the full inventory workflow can be explored immediately without a migration or seed step.
- API shapes are defined in OpenAPI first and generated into the shared React client and Zod validators.
- Clerk is the supported auth path; sign-in, sign-up, password recovery, and profile logout are handled by Clerk's browser session.

## Product

- Dashboard summary with category mix, pending operations, recent activity, and low/out-of-stock counts
- Product catalog with search, status filters, location stock, reorder points, and create/edit flows
- Receipts, deliveries, transfers, and physical adjustments with validate/apply actions that update stock and ledger entries
- Ledger search/type filters, warehouse and category settings, responsive navigation, and operator profile

## User preferences

No additional preferences recorded.

## Gotchas

- The inventory dataset currently resets when the API server restarts; move the state to the provisioned PostgreSQL database before treating it as production data.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
