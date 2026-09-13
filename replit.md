# Telegram Auto Filter Bot

Pyrogram bot that indexes Telegram channel videos/documents in MongoDB and
returns matching files from private chats or groups.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `python telegram-auto-filter-bot/main.py` — run the Telegram bot and Flask health server (port 8080)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string
- Required bot env: `API_ID`, `API_HASH`, `BOT_TOKEN`, `MONGO_URI`, `DB_CHANNEL_ID`

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)
- Bot: Python, Pyrogram, PyMongo, Flask

## Where things live

- `telegram-auto-filter-bot/` — Telegram bot, MongoDB index repository, and Flask health server

## Architecture decisions

- Channel posts are upserted by Telegram `file_id`, so repeated channel updates do not create duplicate records.
- Search input is escaped before being passed to MongoDB's case-insensitive regex query.
- Callback data contains a MongoDB record ID rather than the file ID, keeping callback payloads short and letting the bot fetch the latest record.

## Product

- Automatically indexes videos and documents from a configured Telegram database channel.
- Searches indexed filenames from private chats and groups.
- Presents matches as inline keyboard buttons and sends the selected Telegram file.
- Keeps a Flask health endpoint available for uptime monitoring.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

_Populate as you build — sharp edges, "always run X before Y" rules._

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
