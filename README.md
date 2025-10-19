# Frontend

Stack: Next.js. Renders the chat UI and consumes backend REST/SSE.

- Key endpoints:
  - `POST /conversations` (convenience for first message)
  - `POST /conversations/:cid/messages`
  - `GET /conversations/:cid/messages`
  - `GET /conversations/:cid/stream/:mid` (SSE, no replay)
  - `GET /conversations/:cid/stream` (SSE, no replay)
- Flow:
  1) Create conversation (optionally with first message)
  2) Render history via REST
  3) If active, subscribe SSE; otherwise poll status until done
- Notes:
  - Leave `NEXT_PUBLIC_BACKEND_URL` unset for local development to talk directly to `http://localhost:8000`; override it with your own backend URL as needed.
  - Handle 204 on SSE by falling back to REST polling
  - Keep UI state split into history (from DB) and live stream (from SSE)
  - SSE helper: `openConversationStream(cid, { onEvent, onError })` in `frontend/lib/api/sse.ts` wraps EventSource and forwards parsed JSON events

## OpenAPI Codegen

- Full client (axios):
  - `pnpm run openapi:client` → generates axios client under `frontend/openapi`
  - Usage:
    - `import { OpenAPI, DefaultService as Api } from '@/openapi'`
    - Or use the thin re-export: `import { Api, OpenAPI } from '@/api'`
    - `OpenAPI.BASE = process.env.NEXT_PUBLIC_BACKEND_URL ?? 'http://localhost:8000'`
  - Note: method names come from backend `operationId` (e.g., `getConversationsMessages`).

### Auto‑generation and repo hygiene

- The generated client lives in `frontend/openapi/` and is git‑ignored (see `.gitignore`).
- It is auto‑generated before `dev` and `build` via npm scripts (`predev`, `prebuild`).
- If auto‑generation fails (e.g., network), run manually:
  - `pnpm run openapi:client` (from `frontend/`)
  - or `make codegen-frontend-client` (from repo root)
- You can use the generated client directly, or re‑export via `@/api`:
  - `import { Api } from '@/api'`
  - SSE helper: `import { openConversationStream } from '@/api/sse'`
