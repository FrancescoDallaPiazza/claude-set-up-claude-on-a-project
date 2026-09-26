# CLAUDE.md

Small Express REST API (users + health check) with an in-memory store.

## Commands

- `npm run dev` — start the API with auto-reload on http://localhost:3000
- `npm test` — run tests (Node built-in `node:test` + supertest)
- `npm run lint` — ESLint; run before committing

## Conventions

- CommonJS only: `require` / `module.exports`, not `import` / `export`.
- Tests use `node:test` and `node:assert`, not Jest or Mocha. Put them in `tests/*.test.js` and hit routes through `supertest` against the exported `app`, never a real port.
- Routes never touch data directly: add a function to `db/store.js` and call it from the route.
- Errors are JSON `{ "error": "<message>" }` with the right status (400 for bad input, 404 for missing).
- Don't add dependencies without asking.

## Architecture

- `server.js` builds the Express app, mounts routers, and exports `app`; it only listens when run directly.
- `routes/` — one file per resource (`users.js`, `health.js`), each exporting an `express.Router()` mounted in `server.js`. A new resource = a new file + one `app.use` line.
- `db/store.js` — the only data layer; in-memory, resets on restart.
