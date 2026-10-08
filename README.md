# Recon Board — Shared GTA Map

A shared, live map for a small FiveM crew. Anyone with the link can drop color-coded pings on the GTA V map, shade over areas that have already been searched, and add new color categories. Everything is saved to a database and syncs to everyone who has the page open.

## Features

- Map of Los Santos and Blaine County with mouse-wheel zoom (zooms toward the cursor) and drag-to-pan
- **Add ping**: click the map, name the ping, and pick its color/category
- **Add shading**: hold the left mouse button to paint semi-transparent shading; release to stop. You can choose the color and brush size, and undo your last stroke.
- **Erase**: click a shaded stroke to remove it
- A color key at the top of the page (Blue = Crafting, Red = Drugs, Yellow = Heists/Crime by default). Click a category to show or hide its pings.
- **+ Add category**: anyone can add a color category; a tag name is required
- Live collaboration: the page checks for changes every 3 seconds, so other people's edits show up on their own

Keyboard shortcuts: `1` Move, `2` Add ping, `3` Add shading, `4` Erase, `Esc` back to Move, `+`/`-` zoom, hold `Space` to pan while shading.

## Tech

- Vite + TypeScript (no framework) for the front end
- Netlify Functions (`netlify/functions/api.mts`) for the REST API
- Netlify Database (managed Postgres) with Drizzle ORM for storage

## Running locally

```bash
npm install
npx netlify dev
```

Then open the URL the CLI prints. Netlify Dev runs the Vite front end, the API function, and a local database together.

## Changing the database

Edit `db/schema.ts`, then run:

```bash
npx drizzle-kit generate --name describe_your_change
```

Commit the generated files in `netlify/database/migrations/`. Netlify applies them automatically when it deploys.
