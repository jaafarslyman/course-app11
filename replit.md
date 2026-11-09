# Course Learning Platform

Arabic-first English learning app for learners and administrators, backed by Supabase Auth, Postgres, and private Storage.

## Run & Operate

- `pnpm --filter @workspace/course-app run dev` — run the web app
- `pnpm --filter @workspace/api-server run dev` — run the API server
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- Supabase setup steps and SQL migration order: `artifacts/course-app/docs/supabase-setup.md`
- Required environment variables: `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, and `SUPABASE_SERVICE_ROLE_KEY`

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/course-app/src/` — learner and administrator web app
- `artifacts/course-app/supabase/` — Supabase schema, access-control functions, storage setup, and SQL updates
- `artifacts/course-app/docs/supabase-setup.md` — environment variables and setup instructions
- `artifacts/api-server/src/routes/` — protected media signing and admin learner-account endpoints
- `lib/api-spec/openapi.yaml` — API contract source; regenerate client and server validators after edits

## Architecture decisions

- Supabase is the source of truth for app data and authentication; this app does not require the workspace PostgreSQL database.
- Only the Supabase URL and anon/publishable key belong in browser configuration. Keep `SUPABASE_SERVICE_ROLE_KEY` server-only.
- Course media stays in a private Supabase Storage bucket and is delivered through short-lived signed URLs after access checks.

## Product

Learners sign in to view enrolled courses, lessons, and progress. Administrators manage learner access and build/publish courses from the web dashboard.

## User preferences

- The user will add Supabase credentials to environment variables manually.
- Do not add a separate preview artifact.

## Gotchas

- The frontend needs `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`; learner administration and protected media signing also need `SUPABASE_SERVICE_ROLE_KEY` on the API server.
- Create the administrator in Supabase Auth, then run the SQL scripts in the order documented in `artifacts/course-app/docs/supabase-setup.md`.
- The repository does not include sample courses; create and publish course content from the admin area after setup.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
