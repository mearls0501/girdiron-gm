# AGENTS.md

## Cursor Cloud specific instructions

### Repository layout
- The primary application is a **Next.js 16 app** living in the `gridiron-gm/` subdirectory (not the repo root). Run all `npm` commands from inside `gridiron-gm/`.
- The repo root only contains a placeholder `README.md`/`test` on `main`; the app was brought in from a feature branch.
- Stack: Next.js 16 (Turbopack), React 19, Tailwind CSS 4, TypeScript, and `@supabase/supabase-js`. Node 18.18+ is required (the VM ships Node 22, which works). Package manager is **npm** (there is a `package-lock.json`).

### Standard commands (run from `gridiron-gm/`)
- Dev server: `npm run dev` (serves http://localhost:3000)
- Lint: `npm run lint`
- Build: `npm run build`
- Production start (after build): `npm run start`

### Supabase env vars (important gotcha)
- `gridiron-gm/lib/supabaseClient.ts` **throws at import time** if `NEXT_PUBLIC_SUPABASE_URL` or `NEXT_PUBLIC_SUPABASE_ANON_KEY` are missing. This means `npm run build` and any request to the `/api/teams` route will fail hard without these vars set.
- For local dev, create a gitignored `gridiron-gm/.env.local` with these two vars (placeholder values are enough to let the app boot, lint, and build):
  ```
  NEXT_PUBLIC_SUPABASE_URL=https://placeholder.supabase.co
  NEXT_PUBLIC_SUPABASE_ANON_KEY=placeholder-anon-key
  ```
- With placeholder values, the homepage renders normally and `/api/teams` responds with HTTP 200 and JSON `{"data": null, "error": {...}}` where the error is `TypeError: fetch failed` (the placeholder URL is not a live server). This is expected — it proves the route and Supabase client are wired correctly.
- To exercise the `/api/teams` endpoint against real data, set real Supabase project credentials (and ensure a `teams` table exists) instead of the placeholders.
