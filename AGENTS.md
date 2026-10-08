# AGENTS.md

## What this is
A single-page collaborative map board ("Recon Board") for a small FiveM group. It has no accounts and no permissions on purpose: anyone with the link can add or remove pings, shading, and categories.

## Architecture
- `index.html`, `src/main.ts`, `src/style.css`: vanilla TypeScript front end built with Vite. There's no framework; all DOM work is in `main.ts`.
- `netlify/functions/api.mts`: one function handling every route:
  - `GET /api/state` returns `{ categories, pings, strokes }` and seeds the three default categories when the table is empty
  - `POST /api/{categories|pings|strokes}`
  - `DELETE /api/{categories|pings|strokes}/:id` (deleting a category cascades to its pings)
- `db/schema.ts`, `db/index.ts`: Drizzle schema and client (Netlify Database). Migrations go in `netlify/database/migrations/` and are generated with `drizzle-kit generate`; don't hand-edit them.

## Non-obvious decisions
- **Coordinates are normalized (0..1)** relative to the map image, so pings and strokes stay in place at any zoom level or screen size. Stroke `width` is normalized to the image width too.
- **Live sync is polling** (every 3 s, `/api/state`). Polling results are ignored while a request is in flight or a stroke is being drawn, so local work isn't overwritten. After each change the client calls `refresh(true)`.
- **Shading opacity is applied per color group** (`<g opacity>`), not per path, so overlapping strokes of one color don't stack darker.
- Pins live in a separate, unscaled HTML layer that gets repositioned on each transform, so markers keep the same size on screen at any zoom.
- Category filters and the label toggle are per-browser settings saved in `localStorage`. They aren't shared.
- The map image is loaded directly from the remote URL the user provided.

## Conventions
- TypeScript with ESM; import local files with the `.js` extension in server code.
- Use snake_case column names in the schema.
- Validate all input on the server (hex colors, coordinate ranges, length limits).
- Insert user-supplied text with `textContent`, never `innerHTML`.
